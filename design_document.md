# Manny Manifolds - Design & Drift Control (Canonical)

> Purpose: one authoritative design document that captures the user-facing contract, success criteria, requirements, architecture, and build plan. The foundations live in `foundations.md`; this file assumes and enforces them. If anything in other docs disagrees with this one, this document wins.

---

## 1) User-visible contract
- Conversational assistant in one or more domains.
- Improves with use (shorter/faster/more direct on repeats).
- Explains itself with inspectable paths (`/why` matches traversal).
- Remembers what it learned inside explicit boundaries.
- Not every conversational turn causes learning; durable learning occurs only when traversal produces a trace and explicit merge/learning criteria are met.
- If a user cannot observe improvement, explanation, and persistence, Manny has not manifested.

## 2) Binding definitions
- Manifold: persistent knowledge state (nodes, edges, kappa, motifs, lens stats); excludes session caches and LLM hidden state.
- Graph: discrete substrate; becomes a manifold only with curvature and traversal dynamics.
- Curvature (kappa, κ): bounded, decayed, local scalar on edges (optionally nodes) that biases traversal; updated by experience.
- Thread: one trajectory through the manifold; every answer corresponds to at least one thread.
- Motif: frequently traversed subpath (procedural memory); discovered, not authored.
- Reuse is an explicit event (e.g., reuse_count > 0 via motif reuse/merge); structural overlap alone is not counted as reuse.
- Lens: emergent projection that alters metrics/affinity without copying or partitioning the graph.
- Valence: multi-channel energy signal (importance/affect/novelty) that scales plasticity, not answers.
- Energy (here) refers to traversal cost / curvature bias, not a reward scalar.
- Experiencer: spawns and runs threads; all cognition originates here.
- Executive: regulates parameters (eta, tau, zeta); never routes paths or injects reasoning steps.
- Interaction: query / teach-correct / review-valence; always yields a path trace and deterministic log.
- Session: bounded runtime context that can bias traversal but is not persistent memory.
- Persistence: explicit, inspectable state that survives beyond a session; must be resettable/forkable.

## 3) Success criteria and gates
- S1 Learning: non-zero delta kappa persists across sessions; deleting manifold collapses performance.
- S2 Reasoning: 100% of answers have path IDs; `/why` equals traversal logs.
- S3 Convergence: >=20% mean path-length reduction on repeats within a domain/task family (local/directional convergence; not global ASPL); traversal correlates with kappa.
- S4 Transfer: >=30% motif/edge reuse on analogies (counted via explicit reuse events, not mere overlap); >=15% latency reduction on motif hits.
- S5 Stability: kappa bounded; consolidation does not erase mastery.
- S6 Efficiency: runtime scales with active k-hop region; LLM usage budgeted and scoped to runtime reasoning (offline ingestion/clarification/embeddings are out of scope for this budget).
- Gates: A Foundations (path coverage, `/why` fidelity, delta kappa persistence, kappa bounds); B Learning (>=20% local/directional convergence, no catastrophic regression); C Transfer (>=30% reuse and >=15% latency gain); D Drift (no metric gain may violate foundations).

## 4) Requirements, invariants, forbidden patterns
- Core requirements: single persistent manifold store; path-centric reasoning only; local bounded plasticity; explicit persistence boundaries; `/why` replays exact traversal; Observation ≠ Learning: observation/diagnostics (dry-run merges, guarded applies, lens/temperature swaps) must not alter learning physics.

Training model: Manny Manifolds does not undergo a traditional global training phase. Learning occurs exclusively through interaction-driven traversal (edge-backed traces) and explicit, gated merges that deform geometry. Any offline process that mutates curvature must be treated as consolidation and remain subject to Observation ≠ Learning constraints and rollback.
- Invariants: I1 every answer has a path ID; I2 kappa bounded; I3 deleting manifold collapses performance; I4 motifs emerge only from traffic; I5 lenses never fork the graph; I6 executive never routes.
- Forbidden: LLM planning/routing; answers without paths; learning outside geometry; global hot-path recompute; hard modes/duplicate graphs; non-reproducible or non-inspectable state.
- Data quality sensitivity: experience density and structure (e.g., conversational vs definitional data) materially affect κ formation, convergence, and transfer; validation must account for this dependency.

-### Implementation patterns worth preserving
## 4.2 Implementation principles by concept

This section defines **cross-cutting implementation constraints** for core concepts. These are not features; they govern how code must be written and how components may interact.

### Tokens
- **Role:** atomic semantic anchors; lowest-level addressable units.
- **Principles:** deterministic, hygienic, versioned.
- **Constraints:**
  - Tokenization must be deterministic and versioned.
  - Token hygiene (e.g., stopword filtering; controlled multi-word promotion) is enforced at ingestion.
  - Tokens never learn directly; learning occurs only via edge-backed traversal.

### Nodes
- **Role:** persistent concepts/entities.
- **Principles:** sparse creation, durable identity.
- **Constraints:**
  - Nodes are created only via gated ingestion or explicit proposal acceptance.
  - Node identity is stable across sessions and snapshots.
  - Nodes do not carry learning state; edges do.

### Edges
- **Role:** relations and transitions; primary learning substrate.
- **Principles:** edge-backed traversal, local plasticity.
- **Constraints:**
  - Traversal must always be backed by an edge; provisional edges may be created before traversal.
  - All learning that counts is expressed as Δκ on edges.
  - Updates are local, bounded, and logged.

### Domains
- **Role:** scoped regions of meaning and evaluation.
- **Principles:** hypothesis-driven, reversible.
- **Constraints:**
  - Domains are introduced via gated ingestion (packs/primers).
  - Domain evolution occurs through interaction plus validated merges.
  - Domains are evaluated by metrics (gates), not belief, and are forkable/rollbackable.

### Motifs
- **Role:** reusable procedural knowledge ("knowing how").
- **Principles:** emergent, consensus-based.
- **Constraints:**
  - Motifs are discovered from repeated traffic; never hand-authored.
  - Reuse is an explicit event (counted and logged), not inferred overlap.
  - Motifs accelerate traversal but never bypass geometry or validation.

### Lenses
- **Role:** contextual projections over the same manifold.
- **Principles:** emergent, non-partitioning.
- **Constraints:**
  - Lenses modify metrics and affinities only; they never duplicate data.
  - Lens switching has friction and is logged.
  - Lenses bias traversal; they do not encode learning.

### Sessions
- **Role:** short-term continuity and interaction context.
- **Principles:** ephemeral, disposable.
- **Constraints:**
  - Sessions may persist history and annotations.
  - Sessions must never implicitly modify durable geometry.

### LLMs
- **Role:** proposal generator and language lens.
- **Principles:** suggestive, never authoritative.
- **Constraints:**
  - LLM outputs are proposals or labels only.
  - LLMs never plan routes or apply learning.
  - Runtime reasoning budgets apply only to traversal-time usage.

### Observation & Validation
- **Role:** safety, inspection, falsification.
- **Principles:** learning-neutral.
- **Constraints:**
  - Observation, diagnostics, dry-runs, and simulation must not modify learning state.
  - Only explicit, gated applies may commit changes to geometry.
- **Persistent CLI as a guardrail:** the interactive REPL is not just UX; it is an enforcement surface that makes state transitions explicit and observable, preventing hidden learning or drift.
- **Session vs. manifold separation:** session state (history, temporary lenses, valence annotations) may persist across runs, but must remain strictly separable from durable manifold state.
- **Atomic persistence:** graph and session saves must use atomic writes to avoid partial or corrupt state.
- **Configurable error strategies:** pipeline stages (LLM calls, embedding lookup, traversal) must support RETRY, FALLBACK, SKIP, or ABORT strategies without corrupting learning state.
- **Graceful degradation:** failure in one component must not cascade; observation and interaction should continue even if learning is temporarily disabled.

## 5) Build rules (drift killers)
- No merge without a passing `validation_report.json`.
- Any answer without a persisted trace is a hard failure.
- LLM calls are optional and budgeted; LLM cannot plan or route.
- Persistence is centralized and explicit; session caches are disposable.
- Every run is reproducible (runtime profile, seed, state snapshot).

## 6) Minimal architecture and data flow
- Components: client (CLI/API), interaction orchestrator (coordination only), thread runner (only reasoning engine), plasticity engine (local delta kappa), manifold store (persistent truth), `/why` + metrics (exact replay), offline consolidation engine (sleep).
- Interaction artifacts (minimum): `interaction.json` (ids, input, runtime hash, response, metrics), `trace.json` (nodes, edges, costs, kappa, lens context), `delta_kappa.json` (edge delta kappa, valence, provenance).
- Flow: load state -> run thread -> persist trace -> apply plasticity -> persist state -> invariant checks -> metrics; abort run if any invariant fails.

### Virtual Stage (protected, non-learning context)
- Proposals, simulations, diagnostics, and dry-run merges operate in a protected observation context prior to any merge. They must never modify kappa, motifs, or durable learning state. Only explicit, gated applies may commit validated changes to the manifold.

### Ingestion & embedding infrastructure (preserved)
- **Gated ingestion:** domain packs, primers, or validated vocabularies gate what may become learnable structure. Ungated inputs may be observed or proposed but must not alter curvature.
- **Embedding abstraction:** embeddings propose neighbors and candidate relations only; they do not encode learning or decision authority.
- **ANN parity:** approximate ANN (e.g. HNSW) may be used for scale, but a brute‑force fallback must exist to ensure determinism, correctness, and testability on small graphs. The active strategy is logged in runtime and validation artifacts.

## 7) Interfaces
- CLI (must): interactive REPL with a **shared, stateful manifold instance**. Supports command history (↑/↓), cursor navigation, reverse search (Ctrl+R), pagination (`--limit`), and case‑insensitive lookup. Commands operate against the same in‑memory graph to enforce *no hidden state*. Required commands:
  - `mm init`
  - `mm run "<prompt>"` / interactive REPL mode
  - `mm why <trace_id>`
  - `mm map [node|topic]`
  - `mm status`
  - `mm reset` (session only)
  - `mm learn <domain_pack>` / `learn ingest=... --merge ...`
  - `mm state save|load|fork|diff`
  - `mm sleep`

## 8) Persistence and repo structure
- Persistence: centralized DB (SQLite baseline, upgradeable to Postgres) for nodes, edges, kappa, motifs, lenses, interactions, traces, deltas. Ephemeral session caches for ANN index, hot node cache, memoized neighborhoods. Persisted state must be inspectable, resettable, forkable.
- **Snapshot semantics:** save/load/fork/diff are first‑class operations used for drift control, regression testing, and domain experimentation. Forking a manifold to compare learning trajectories is expected behavior, not an edge case.
- **Session persistence:** sessions may optionally persist query history and valence annotations to support multi‑session continuity, but must never implicitly modify durable geometry.
- **Domain lifecycle (explicit):** domains are introduced via gated ingestion (packs/primers), evolved through interaction and validated merges, measured by gates/metrics, and remain forkable and reversible via snapshot semantics.
- Repo skeleton (illustrative; adapt per language/runtime):
```
mm/
  core/ (state, graph, threading, plasticity, motifs, lenses, explain, consolidate)
  services/ (engine, validation)
  api/
  cli/
  packs/
  tests/
  runs/
  state/
```

## 9) Validation harness and test suites
- Harness responsibilities: deterministic interaction sequences, continuous invariant checks, Gates A-D computation, emits `validation_report.json` and `gates.json` (authoritative for CI).
- Report sections: foundations (path coverage, `/why` fidelity), learning (delta kappa persistence, convergence), transfer (motif reuse + latency gain), stability (kappa bounds/variance), efficiency (active-region scaling + LLM budget).
- Minimum suites: procedural domain, structured facts with updates, stress/pathology cases.
- CI rule: no merge without passing `mm validate` and an attached report.
- **Guided demos:** curated demo scripts (e.g. apple→pear transfer) are preserved as onboarding and regression tools; they provide concrete, inspectable examples of convergence, transfer, and stability.
- **Manual test harness:** interactive edge‑case testing is supported alongside automated tests to validate behavior under unusual sequences.
- **Extensive automated tests:** unit and integration tests must cover traversal, plasticity, consolidation, persistence, and error handling; selective deselection and fast paths are encouraged to keep feedback loops tight.

## 10) Milestones (validation-first)
- M0 Skeleton: DB + determinism + trace persistence; CLI `init`, `state save|load`, `run` (stub), `validate` (stub). Exit: no-op interaction still emits artifacts.
- M1 Gate A: ThreadRunner + `/why`; invariants 100% path coverage and `/why` fidelity. Exit: Gate A passes on tiny deterministic suite.
- M2 Gate B: ValenceResolver + PlasticityApplier with clamps and per-turn kappa budget; demonstrate >=20% path-length reduction on repeats. Exit: Gate B passes.
- M3 Gate C: Motif detection/commit/reuse with logs; demonstrate >=30% reuse and >=15% latency reduction on motif hits. Exit: Gate C passes on analogy suite.
- M4 Gate D: Consolidation with checkpoint/apply/atomic swap/rollback; kappa distribution monitoring and stress suite. Exit: Gate D passes on stress suite.

## 11) Canonical summary
- Manny Manifolds is a self-evolving geometric mind where experience reshapes space, and space shapes thought. Everything else is implementation detail.
