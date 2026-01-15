# Manny Manifolds - Design & Drift Control (Canonical)
> Version: v1.1

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
- Interaction: query / teach-correct / review-valence; yields a path trace and reproducible log (deterministic given state snapshot + seed), unless explicitly declared non-deterministic.
- Session: bounded runtime context that can bias traversal but is not persistent memory.
- Persistence: explicit, inspectable state that survives beyond a session; must be resettable/forkable.

## 2a) Primitive Contract (P0)
- Primitive types (versioned): v1 = tokens/words; future = percept primitives (patches, phonemes) and structured fields. All primitives carry `origin`, `layer`, `version`, `decay_rate`, `provisional=true`.
- Distinction: primitives are provisionally atomic units; concepts are stabilized clusters; motifs are procedural subpaths; episodes are anchored aggregates.
- Decomposition: a primitive may be refined into sub-primitives without breaking higher-level links; promotion/decomposition must preserve traceability and avoid opaque blobs.
- Decay/promotion hooks: primitives carry decay rules; promotion eligibility is logged (resonance counts, valence-weighted recurrence).

## 2b) Compression Ladder & Promotion Rules (P0)
- Layers (conceptual MVP): L0 microstructure (raw percept features); L1 percept primitives; L2 concepts; L3 motifs/macros; L4 episodes.
- Resonance gate: promotion triggers on frequency × valence-weighted recurrence and local energy reduction; novelty gate preserves low-frequency, high-surprise items without premature promotion.
- Promotion criteria (MVP): hit frequency threshold, show latency/energy reduction, and pass stability window; demotion/decay for low-reuse + low-valence over windows.
- Ephemeral allowance: items may remain ephemeral if they lack resonance; they must not pollute the canonical manifold unless promoted.

## 2c) Perception-to-Manifold Contract (P0)
- No opaque blobs: artifacts (files, media) are stored as references only; knowledge must be decomposed into manifold-native nodes/edges/fields to count.
- External encoders/LLMs may **propose** primitives/edges but may not decide traversal or learning; manifold physics decides.
- Ingestion outputs: (a) artifact reference with provenance; (b) proposed primitives + relations; (c) optional lens/dimension tags. All subject to RunMode and promotion rules.

## 2d) Thread Energy / Cost Contract (P0)
- Energy/cost is the local bias function used by the Thread Runner to choose the next step; it is not a reward and must be computable from k-hop neighborhood information only.
- Inputs (MVP): κ (curvature), lens weighting (relation-type + tags), valence channels (importance/novelty/affect), semantic mass proxy, novelty term.
- Exclusions: no direct LLM scoring may enter the energy/cost function; no global graph scans; no external planners/routing.
- Determinism: for a given state snapshot + seed + lens configuration, traversal must be reproducible.
- Logging: every step logs (node, edge, cost components, lens, runmode) so `/why` is a faithful replay of the motion law.

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

### Training model (contract)
Manny Manifolds does not undergo a traditional global training phase. Learning occurs through interaction-driven traversal (edge-backed traces) plus explicit, gated merges that deform geometry. Any offline process that mutates curvature is treated as consolidation and remains subject to RunModes, Observation ≠ Learning constraints, and rollback.
- Invariants: I1 every answer has a path ID; I2 kappa bounded; I3 deleting manifold collapses performance; I4 motifs emerge only from traffic; I5 lenses never fork the graph; I6 executive never routes.
- Forbidden: LLM planning/routing; answers without paths; learning outside geometry; global hot-path recompute; hard modes/duplicate graphs; non-reproducible or non-inspectable state.
- Data quality sensitivity: experience density and structure (e.g., conversational vs definitional data) materially affect κ formation, convergence, and transfer; validation must account for this dependency.

## 4a) RunModes and Observation ≠ Learning (P0)
- RunMode = `LEARN | OBSERVE | STAGE`.
- `OBSERVE`: read-only; plasticity engine unreachable; artifacts/logs still produced.
- `STAGE` (Virtual Stage): isolated curvature buffer; no writes to canonical manifold until an explicit commit gate; default is discard.
- All diagnostics, validation harness runs, and simulations must execute in `OBSERVE` or `STAGE`; only `LEARN` may mutate κ/motifs/lenses.

## 4b) Semantic Mass / Density Metrics (P0)
- Define region mass proxy: function of activation counts, κ distribution, and valence history over a window.
- Traversal bias: prefer coherent/high-mass basins unless novelty/drive weighting pushes exploration; log the bias used.
- Consolidation: prune low-mass noise; promote stable dense clusters; expose mass metrics to Executive and motif miner.

## 4c) Lens Semantics (dimensions ≠ domains) (P1)
- Lens = weighting over relation types, node tags, and ANN neighborhood bias; lenses do not fork the graph.
- Taxonomy is a projection (lens view), not topology; domains are emergent basins.
- Lens friction (ζ) and energy benefit must be logged; lens activation correlates with energy reduction.

## 4d) Motif Emergence and Reuse (P1)
- MVP mining: frequent subpath detection with thresholds (frequency × valence), stability window, and benefit rule (latency/energy gain).
- Motif commit only via miner (no hand-authored motifs).
- Reuse is an explicit logged event (reuse_count > 0 with trace segment markers); structural overlap alone is not reuse.

### Implementation patterns worth preserving
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
  - Domains are emergent basins (clusters) in the manifold; “domain packs/primers” are scaffolds that seed structure but do not define boundaries.
  - Domain evolution occurs through interaction-driven traversal plus validated merges; domains must remain forkable and reversible via snapshot semantics.
  - Domains are evaluated by gates/metrics (convergence, transfer, stability), not by declaration.

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

## 5a) Explicit non-solutions (guardrails)
- No global planners or search controllers outside the manifold.
- No rule engines or symbolic solvers that bypass traversal.
- No modality-specific reasoning modules.
- No direct LLM-to-answer paths (LLMs may propose primitives/edges only).

## 6) Minimal architecture and data flow
- Components: client (CLI/API), interaction orchestrator (coordination only), thread runner (only reasoning engine), plasticity engine (local delta kappa), manifold store (persistent truth), `/why` + metrics (exact replay), offline consolidation engine (sleep).
- Interaction artifacts (minimum): `interaction.json` (ids, input, runtime hash, response, metrics), `trace.json` (nodes, edges, costs, kappa, lens context), `delta_kappa.json` (edge delta kappa, valence, provenance).
- Flow: load state -> set seed -> run thread -> persist trace -> apply plasticity -> persist state -> invariant checks -> metrics; abort run if any invariant fails.

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
