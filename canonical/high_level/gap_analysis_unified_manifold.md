# Manny Unified Manifold — Gap Analysis & Closure Log (v1.1)

Scope: quick gap scan against the new spine (primitives → compression ladder → semantic mass → dimensions→taxonomy-as-projection) versus current canon (`foundations.md`, `design_document.md`, `architecture.md`). vII is newer than v1; this scan assumes vII principles.

## Status Summary (v1.1)
This document now serves as a closure log for gaps identified against the vII spine. Items marked **Closed in v1.1** have been explicitly addressed in `foundations_v1.1.md`, `design_document_v1.1.md`, and `architecture_v1.1.md`. Remaining items are intentionally deferred and tracked below.

## P0 (will block the proof if unaddressed)

- **Missing Primitive Contract** — **Closed in v1.1**
  - Absent: canonical definition of primitive (fields, lifecycle, decay), distinction from concepts/motifs, decomposability rules.
  - Fix: add “Primitive Contract” (types, required metadata, versioning, decay, promotion hooks).

- **Missing Compression Ladder / Promotion Rules** — **Closed in v1.1**
  - Absent: resonance vs novelty signals; promotion criteria (percept → concept → motif); what stays ephemeral.
  - Fix: add promotion thresholds + “resonance gate” definition (even heuristic).

- **No Perception-to-Manifold Contract (multimodal ingestion)** — **Closed in v1.1**
  - Absent: how non-text becomes manifold-native; artifact pointer vs manifold nodes/edges; how external encoders feed Manny without becoming a second brain.
  - Fix: add “Perception-to-Manifold Contract” (encoders may propose; manifold decides; artifacts are references only).

- **Semantic Mass / Information Density (operational proxy) not represented** — **Closed in v1.1**
  - Absent: region-level mass metric (ρ_info), effect on traversal and consolidation.
  - Fix: define mass proxies (activation count + κ distribution + valence history), and how they bias cost/traversal and pruning/promotion.

- **Missing Thread Energy / Cost Contract** — **Closed in v1.1**
  - Absent: canonical definition of the local energy/cost function governing traversal (inputs, invariants, exclusions).
  - Fix: define energy inputs (κ, valence, semantic mass proxy, lens weighting, novelty term), locality constraints, and explicit exclusions (no direct LLM scoring, no global planners), without fixing equations.

## P0 Closure Notes
All P0 gaps identified in the initial scan have been resolved in v1.1 through explicit contracts and architectural modules:
- Primitive Contract (design) + Primitive Registry (architecture)
- Compression ladder and promotion/decay gates (design)
- Perception-to-Manifold contract with artifact/reference separation (design + architecture)
- Semantic mass defined as an operational proxy and wired into traversal, consolidation, and executive regulation
- Thread Energy / Cost Contract formalized and enforced as local, deterministic, and auditable

## P1 (important, but not day-1 blockers)

- **Executive/Drives absent in architecture.md** — **Closed in v1.1**
  - Foundations has it; architecture lacks the module/loop (τ, η, ζ surfaces).
  - Fix: add Executive Thermostat as a module with inputs/outputs.

- **Virtual Stage missing from architecture.md** — **Closed in v1.1**
  - Design doc mentions it; architecture omits it.
  - Fix: add STAGE mode (isolated, no learning writes) with commit gate.

- **Observation ≠ Learning enforcement not explicit** — **Closed in v1.1**
  - Absent: run-modes and write barriers.
  - Fix: add RunMode = {LEARN, OBSERVE, STAGE}; plasticity unreachable in OBSERVE.

- **Lens semantics lack “dimensions ≠ domains”** — **Closed in v1.1**
  - Lenses are treated as context, not dimension weighting.
  - Fix: define lens = weighting over relation types/node tags/ANN bias; taxonomy as projection.

- **Free-fall cognition not nailed as a discrete motion law** — **Closed in v1.1**
  - Implicit only.
  - Fix: state a local, discrete motion law: k-hop neighborhood descent with mild stochasticity; stop on energy convergence or goal reach; always log the chosen path.

## P1 Closure Notes
All P1 gaps have been resolved in v1.1 to remove architectural ambiguity and prevent drift:
- Executive Thermostat added with explicit parameter surfaces (τ, η, ζ)
- Virtual Stage introduced as an isolation boundary with explicit commit gate
- RunModes and write barriers enforced by construction
- Lenses redefined as dimension-weighting projections
- Free-fall cognition formalized as a discrete, local motion law

## P2 (note now, land later)
These items are intentionally deferred beyond v1.1 and do not block the MVP proof; they are tracked here for future iterations.

- **Episodic/time anchors undefined**
  - Add timestamps/session/episode nodes/links; artifact pointers as episodic anchors.

- **Motif emergence algorithm not locked**
  - Need MVP mining strategy: frequency + consensus + benefit rule; explicit reuse events.

## Minimal edits that close most gaps (do these first)
1) Add Primitive Contract (types + versioning + decay + promotion hooks).  
2) Add Compression Ladder (L0–L3) + promotion thresholds + resonance/novelty gates.  
3) Add Perception-to-Manifold Contract (no opaque blobs; artifacts are references).  
4) Add Executive module to architecture.md (τ/η/ζ surfaces).  
5) Add Virtual Stage stub + RunModes enforcing Observation ≠ Learning.  
6) Add Semantic Mass metrics and wire into traversal + consolidation.

## Explicit non-solutions (guardrails)
- No global planners or search controllers outside the manifold.
- No rule engines or symbolic solvers that bypass traversal.
- No modality-specific reasoning modules.
- No direct LLM-to-answer paths (LLMs may propose primitives/edges only).

## Cross-doc ownership (where to fix)
- **Design doc (normative):** Primitive Contract; Compression Ladder/Promotion; Perception-to-Manifold; RunModes; Semantic Mass metrics; lenses as dimension weighting; motif reuse invariants and gate metrics; Thread energy/cost contract (inputs, invariants, exclusions).
- **Architecture:** Modules (Executive, Virtual Stage, Primitive Registry, Lens Engine); runtime loop; run-modes/write barriers; ingest pipeline; derived indices advisory only; motif miner MVP; motion law; Energy/cost computation module or function used by Thread Runner (local-only).
- **Foundations (minimal add):** Provisional primitives; “no opaque artifacts as knowledge”; compression survives via promotion/decay (non-implementation).

## “Done” for this gap pass
1) Foundations gains only irreducible clauses; no implementation.  
2) Design doc owns behavioral contract + gates/drift killers.  
3) Architecture aligns modules/flows to both.  
4) Shared vocabulary across all three (primitives, lenses, executive, observation≠learning, semantic mass).  
5) MVP proof mapped explicitly to gates (convergence, transfer, motif utility, stability, explainability, runtime LLM budget).

## Canon v1.1 Outcome
With v1.1, Manny’s canonical documents now fully encode the Primitive→Compression→Semantic Mass→Free-Fall cognition spine. The system is canon-complete for MVP implementation, with all proof-blocking gaps closed and remaining items explicitly deferred.
