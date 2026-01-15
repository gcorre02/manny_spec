# Manny Unified Manifold — Gap Analysis (Primitive→Compression→Semantic Mass Spine)

Scope: quick gap scan against the new spine (primitives → compression ladder → semantic mass → dimensions→taxonomy-as-projection) versus current canon (`foundations.md`, `design_document.md`, `architecture.md`). vII is newer than v1; this scan assumes vII principles.

## P0 (will block the proof if unaddressed)

- **Missing Primitive Contract**
  - Absent: canonical definition of primitive (fields, lifecycle, decay), distinction from concepts/motifs, decomposability rules.
  - Fix: add “Primitive Contract” (types, required metadata, versioning, decay, promotion hooks).

- **Missing Compression Ladder / Promotion Rules**
  - Absent: resonance vs novelty signals; promotion criteria (percept → concept → motif); what stays ephemeral.
  - Fix: add promotion thresholds + “resonance gate” definition (even heuristic).

- **No Perception-to-Manifold Contract (multimodal ingestion)**
  - Absent: how non-text becomes manifold-native; artifact pointer vs manifold nodes/edges; how external encoders feed Manny without becoming a second brain.
  - Fix: add “Perception-to-Manifold Contract” (encoders may propose; manifold decides; artifacts are references only).

- **Semantic Mass / Information Density (operational proxy) not represented**
  - Absent: region-level mass metric (ρ_info), effect on traversal and consolidation.
  - Fix: define mass proxies (activation count + κ distribution + valence history), and how they bias cost/traversal and pruning/promotion.

- **Missing Thread Energy / Cost Contract**
  - Absent: canonical definition of the local energy/cost function governing traversal (inputs, invariants, exclusions).
  - Fix: define energy inputs (κ, valence, semantic mass proxy, lens weighting, novelty term), locality constraints, and explicit exclusions (no direct LLM scoring, no global planners), without fixing equations.

## P1 (important, but not day-1 blockers)

- **Executive/Drives absent in architecture.md**
  - Foundations has it; architecture lacks the module/loop (τ, η, ζ surfaces).
  - Fix: add Executive Thermostat as a module with inputs/outputs.

- **Virtual Stage missing from architecture.md**
  - Design doc mentions it; architecture omits it.
  - Fix: add STAGE mode (isolated, no learning writes) with commit gate.

- **Observation ≠ Learning enforcement not explicit**
  - Absent: run-modes and write barriers.
  - Fix: add RunMode = {LEARN, OBSERVE, STAGE}; plasticity unreachable in OBSERVE.

- **Lens semantics lack “dimensions ≠ domains”**
  - Lenses are treated as context, not dimension weighting.
  - Fix: define lens = weighting over relation types/node tags/ANN bias; taxonomy as projection.

- **Free-fall cognition not nailed as a discrete motion law**
  - Implicit only.
  - Fix: state a local, discrete motion law: k-hop neighborhood descent with mild stochasticity; stop on energy convergence or goal reach; always log the chosen path.

## P2 (note now, land later)

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
