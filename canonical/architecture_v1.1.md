# Manny Manifolds — Architecture (High-Level Infrastructure)
> Version: v1.1

> **Purpose:** Freeze the *shape* of the system and the *tooling choices* early, without locking implementation details.
> 
> This document complements:
> - `foundations_v1.1.md` (physics / non-negotiables)
> - `design_document_v1.1.md` (contract, gates, and drift control)

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
- **Primitive Registry:** manages primitive types/metadata (provisional atoms), versioning, decay hooks, and promotion status.
- **Interaction Orchestrator:** coordinates one interaction; never reasons.
- **Thread Runner:** produces a `Trace` (the only reasoning step); discrete “free-fall” law: local downhill steps in k-hop neighborhood with mild noise; stop on energy convergence/goal; always log path.
- **Energy / Cost Evaluator:** computes the local traversal bias from κ (curvature), lens weighting, semantic mass proxy, valence channels, and novelty; used exclusively by the Thread Runner; local-only.
- **Energy / Cost Evaluator (audit):** cost components must be recorded per step in `trace.json` (κ term, lens term, mass term, valence term, novelty term) to make `/why` a faithful replay.
- **Energy / Cost Evaluator (determinism):** for a given state snapshot + seed + lens config, energy/cost evaluation must be deterministic.
- **Lens Engine:** applies lens weightings over relation types/node tags/ANN bias; lenses never fork the graph.
- **Plasticity Engine:** applies local, bounded Δκ; emits `DeltaReport`; reachable only in LEARN mode.
- **Valence & Executive Thermostat:** computes valence (importance/novelty/affect heuristics) and regulates parameters (τ temperature, η learning gain, ζ lens friction) based on mass metrics, convergence trends, instability flags, and motif reuse stats; never routes paths or injects reasoning.
- **Motif Miner & Reuser:** mines motifs (frequency × valence × benefit) during consolidation; reuse is explicit (reuse_count > 0) and logged in traces.
- **Virtual Stage:** isolated run-mode for simulation; curvature writes buffered and committed only via explicit gate; default is discard. Virtual Stage is an isolation boundary (write-buffer), not a cognitive mode; it must not change traversal rules, only write behavior.
- Commit gate: `stage_commit.json` artifact must be produced to merge buffered curvature into the canonical manifold.

### 2.3 Offline pipeline
- **Consolidation (“sleep”):** prune / normalize / motif mining / index rebuild.
- **Observation pipeline:** dashboards, diffs, dry-run merges, QA reports.

### 2.4 Runtime loop (discrete “free-fall” law)
Input → parse → candidate nodes → spawn thread(s) → traverse locally downhill (k-hop, mild noise) → log path → if RunMode=LEARN apply plasticity → respond.

### 2.5 Run-modes & write barriers
- RunMode = `LEARN | OBSERVE | STAGE`.
- `LEARN`: traversal + plasticity allowed.
- `OBSERVE`: read-only traversal; plasticity engine unreachable; artifacts still written.
- Write barrier: Plasticity Engine and any κ mutation pathways must be unreachable in OBSERVE mode by construction (not by convention).
- `STAGE`: isolated buffer for simulation; no writes to canonical manifold unless an explicit commit gate is invoked.

### 2.6 Perception-to-Manifold (ingest contract)
- No opaque blobs as knowledge: artifacts are references; knowledge must be manifold-native (nodes/edges/fields).
- External encoders/LLMs may propose primitives/edges; manifold physics decides acceptance; proposals logged with provenance.
- Ingest produces: (a) artifact reference, (b) proposed primitives/relations, (c) optional lens/dimension tags.

### 2.6.1 MVP multimodal ingestion example (non-binding)
- Image/audio ingestion uses external encoders to propose mid-level primitives (e.g. patches/keypoints/phonemes) plus candidate edges to existing concepts.
- The raw artifact is stored as a reference with provenance; no opaque embedding blob is treated as knowledge.
- Acceptance occurs only through standard interaction traversal + promotion rules; proposals are never authoritative.

### 2.7 Semantic mass / density
- Region mass proxy = f(activation counts, κ distribution, valence history) over a window.
- Traversal bias: prefer coherent/high-mass basins unless novelty/drive weighting pushes exploration; bias logged.
- Consolidation: prune low-mass noise; promote stable dense clusters; expose mass metrics to Executive and motif miner.
- Computation: mass proxies are computed during consolidation and exposed read-only to the Thread Runner, Executive, and Motif Miner.

### 2.8 Derived indices are advisory
- Embeddings/ANN indices may propose neighbors; traversal and learning remain governed by manifold edges/physics.
- LLMs are not planners/routers; may assist with proposals/realization only.

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

**Artifact references:** Raw media/documents are stored as references with provenance; they are not semantic authority unless decomposed into manifold-native structure.

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

- Ensure `design_document_v1.1.md` and this file remain consistent.
- Implement M0 (DB + determinism + trace persistence) with the validation harness as the merge gate.

---

> **Principle:** Freeze the shape, not the internals.
