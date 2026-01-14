# Manny Manifolds — Architecture (High-Level Infrastructure)

> **Purpose:** Freeze the *shape* of the system and the *tooling choices* early, without locking implementation details.
> 
> This document complements:
> - `foundations.md` (physics / non-negotiables)
> - `design_document.md` (contract, gates, and drift control)

---

## 1) Guiding constraints (non-negotiable)

- **One reasoning engine:** path traversal / threads only (no hidden planners).
- **Learning is geometry:** durable learning is expressed as κ/edge updates and logged deltas.
- **Observation ≠ Learning:** diagnostics, dry-runs, and simulation never mutate learning state.
- **Reproducibility:** every run is replayable via runtime profile + seed + state snapshot.

---

## 2) System shape (modules)

### 2.1 Surfaces
- **CLI (primary):** interactive REPL (developer + power user surface).
- **HTTP API (secondary):** thin wrapper over the same engine calls (future UI / services).
- **No interface drift:** all surfaces (CLI, API, future UIs/agents) must call the same core engine functions; interfaces are transport only and must not replicate reasoning, learning, or state semantics.
- **Contract boundary:** surfaces may not mutate durable state except by invoking core engine commands that produce standard artifacts (`interaction.json`, `trace.json`, `delta_kappa.json`, `metrics.json`). Any direct state mutation in an interface is forbidden.

### 2.2 Core engine
- **Interaction Orchestrator:** coordinates one interaction; never reasons.
- **Thread Runner:** produces a `Trace` (the only reasoning step).
- **Plasticity Engine:** applies local, bounded Δκ; emits `DeltaReport`.
- **Motifs & Lenses:** reuse + context projection; never override traversal physics.

### 2.3 Offline pipeline
- **Consolidation (“sleep”):** prune / normalize / motif mining / index rebuild.
- **Observation pipeline:** dashboards, diffs, dry-run merges, QA reports.

---

## 3) Data & persistence (centralized + in-memory-first)

### 3.1 Durable store
- **SQLite (v0):** single-file DB for portability and deterministic testing.
- Clean upgrade path to **Postgres** if/when multi-user or larger scale requires it.
- **Canonical store remains relational:** SQLite is the durable source of truth for geometry and learning state; graph/document databases may be used later only as derived read models (visualization/analysis), not as canonical truth.

### 3.2 In-memory hot path
- Load durable state into memory for interactive traversal.
- Maintain session-scoped caches (ANN index handle, hot-node cache, memoized neighborhoods).
- Session caches are disposable; durable state is explicit.

### 3.3 Artifact-first observability
Each interaction writes artifacts under `runs/<run_id>/interactions/<interaction_id>/`:
- `interaction.json` (input, runtime hash, ids)
- `trace.json` (exact traversal)
- `delta_kappa.json` (learning deltas)
- `metrics.json` (gate metrics)

The validation harness consumes artifacts only.

**Authority boundary:** Artifacts are authoritative for *what happened during a run* (replay, audit, validation), but they are not a second source of durable truth. The centralized data store (DB) remains the sole source of durable learning state and geometry.

**κ storage strategy (v0):** store current κ as relational state for speed and simplicity, and optionally persist Δκ as an append-only log (in DB or artifacts) for audit/replay. If richer history is needed later, event-sourcing can be introduced without changing core engine semantics.

---

## 4) Tooling choices (what we standardize)

### 4.1 Language and runtime
- **Python 3.11+**
- Packaging: **pyproject.toml** (preferred), with `requirements.txt` kept minimal if needed.

### 4.2 CLI
- **Typer** (or Click) for command structure
- **Prompt Toolkit** for REPL UX (history, Ctrl+R, navigation)

### 4.3 API
- **FastAPI** (thin wrapper, no business logic)

### 4.4 Persistence
- **sqlite3** (stdlib) or **SQLAlchemy** (only if it reduces complexity)
- Migrations: **Alembic** if SQLAlchemy is used; otherwise a minimal migrations folder with versioned SQL.
- **κ representation (v0):** prefer state tables (current κ) plus optional append-only Δκ logs; avoid full event-sourcing until needed for long-horizon audits.

### 4.5 Embeddings & LLM integration (scoped)
- LLMs are used for **proposal generation** (primers, candidate edges, labels) and **language realization**.
- LLMs are never used to plan traversal or apply learning.
- Embeddings are used for neighbor proposal; traversal remains edge-backed.

### 4.6 ANN / similarity search
- **hnswlib** when available
- **Brute-force** fallback as truth-mode for correctness and deterministic tests
- Active strategy logged in runtime artifacts

### 4.6.1 Search and analytics (optional)
- **Elasticsearch is not required** for core Manny operation; it is an optional indexing/analytics layer.
- Prefer SQLite queries + file-based artifacts for early phases; add Elasticsearch only if full-text search over large artifact corpora or operational dashboards become a bottleneck.
- If introduced, Elasticsearch must be treated as a derived index (rebuildable) and must not become a source of truth for learning semantics.

### 4.7 Testing & validation
- **pytest** for unit/integration tests
- A deterministic **validation harness** that emits `validation_report.json` and gate results
- CI blocks merges unless `mm validate` passes

### 4.8 Visualization (initial)
- Text-based `/why` and `/map` in CLI
- Optional static exports (JSON/PNG) for dashboards (Phase 7.5 style)

---

## 4.9 Technology decision principles (future-forward)

These principles guide **all technology choices** to ensure Manny Manifolds can evolve fluidly across iterations without architectural churn.

### Determinism over cleverness
- Prefer deterministic, replayable systems over opaque optimizations.
- Any non-deterministic component must be optional, logged, and replaceable.
- Determinism is a prerequisite for validation, debugging, and trust.

### Replaceability by interface
- Every major dependency (DB, ANN, LLM provider, embedding model) must be swappable behind a narrow interface.
- No implementation choice may leak semantics into the core learning or reasoning logic.
- Migration paths must be cheaper than rewrites.

### Explicit state boundaries
- Durable state, session state, cache state, and observation artifacts must be clearly separated.
- Crossing a boundary must be intentional and observable.
- Hidden state is treated as a bug.

### Observation-first instrumentation
- Instrumentation is built-in, not added later.
- All critical decisions should already be observable via artifacts and metrics.
- If a behavior cannot be observed, it cannot be trusted.

### Scale as an opt-in, not a requirement
- Systems must work correctly at small scale (brute force, local DB, single process).
- Scale-oriented components (ANNs, background workers, distributed stores) are additive layers, not prerequisites.
- Correctness must not depend on scale.

### Future modality neutrality
- Architecture must not assume text-only input or output.
- New modalities (vision, audio, sensor streams, actions) should attach as proposal generators and observation sources.
- The manifold remains the unifying substrate regardless of modality.

### Human-in-the-loop by design
- The system must support inspection, correction, rollback, and comparison as first-class operations.
- Manual intervention is expected, not exceptional.
- Tooling should make teaching Manny easier over time, not harder.

### Slow change, fast iteration
- Core abstractions change rarely.
- New behavior should emerge from recomposition of existing pieces, not wholesale replacement.
- Prefer additive evolution over refactors.


### No interface drift (single source of truth)
- The core engine is the single source of truth for reasoning, learning, and state transitions.
- CLI, API, and future interfaces are thin adapters: they translate I/O and invoke engine contracts, nothing more.
- Any business logic, caching, heuristics, or state that changes semantics belongs in the core engine (or explicitly marked observation-only tooling).

### Derived indices are advisory, not authoritative
- Derived indices (ANNs, Elasticsearch, analytics stores) may be consulted to *propose* candidates or accelerate lookup.
- The core engine always decides traversal, learning, and state mutation using canonical geometry and rules.
- No derived index may introduce semantics, override decisions, or become a source of truth.

---

## 5) Deployment shape (early vs later)

### 5.1 Local dev (Phase A/B)
- Single process REPL + local SQLite
- Artifacts written to `runs/`

**Execution model (default):** In local development and early phases, the CLI and core engine run **in the same process**, sharing a single in-memory manifold instance. This is the primary and preferred execution model, as it maximizes determinism, observability, and debuggability.

### 5.2 Service mode (Phase C+)
- API server wraps the same engine
- Background worker for consolidation and exports

**Service mode (later, optional):** Running the core engine behind an API as a long-lived service is a secondary, future option. In this mode, the CLI becomes a client, but must still call the same core engine contracts. Service mode must not introduce semantic differences, hidden state, or interface drift.

---

## 6) What we are deliberately NOT deciding yet

- Exact DB schema details (beyond table categories)
- Exact cost function constants and heuristics
- Exact lens taxonomy or motif mining algorithm
- UI beyond CLI and minimal dashboards

---

## 7) Next steps

- Ensure `design_document.md` and this file remain consistent.
- Implement M0 (DB + determinism + trace persistence) with the validation harness as the merge gate.

---

> **Principle:** Freeze the shape, not the internals.
