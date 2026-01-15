# Manny Manifolds — Canonical Foundations

> **Status:** Canonical / Non-Negotiable  
> This document defines the **irreducible foundations** of Manny Manifolds. Any component, experiment, feature, or implementation that violates these foundations is **out of scope** for Manny Manifolds, regardless of usefulness or novelty.

---

## 0. Core Claim (The One Sentence That Must Remain True)

**Manny Manifolds is a cognitive system where:**

> **Knowledge is geometry.**  
> **Reasoning is motion.**  
> **Learning is curvature.**

Meaning emerges as stable curvature configurations produced by repeated, coherent interaction within the manifold.

This is not metaphorical framing. It is an **operational definition**.

If knowledge, reasoning, or learning occur anywhere *outside* this geometric substrate, the system is no longer Manny Manifolds.

---

## 1. Knowledge Representation — The Manifold

### Principle
Knowledge is represented as a **living, deformable manifold**.

- Nodes represent concepts, entities, or atomic ideas
- Edges represent relationships or transitions
- **Curvature encodes learned association strength**

There is no separation between *model* and *memory*.

> The manifold **is** the learned state.

### Non-Negotiables
- Geometry must directly influence traversal
- Learned state must persist across interactions
- No external symbolic controller may override manifold structure
- No opaque artifacts (files, blobs, attachments) may constitute knowledge unless decomposed into manifold-native structure

### Violations
- Treating embeddings as the “real” knowledge
- Using the graph only as a visualization layer
- Maintaining a separate learned policy outside the manifold

---

## 2. Cognition — Threads as Thought

### Principle
All cognition occurs via **threads**.

A thread is an **active trajectory** through the manifold:
- Spawned by experience, query, or exploration
- Proceeds via local decisions only
- Terminates when energy converges or budget is exhausted
- Motion follows a local energy/cost bias derived from curvature and valence (no global search)

> A thread *is* a thought.

### Non-Negotiables
- No central planner
- No global reasoning pass
- No hidden chain-of-thought outside the manifold

### Violations
- Reasoning steps not traceable to a path
- External planners dictating traversal

---

## 3. Learning Law — Curvature ↔ Motion Feedback

### Principle
Learning emerges from a **bidirectional feedback loop**:

> **Threads change curvature.**  
> **Curvature changes future threads.**

This loop defines learning itself.

### Operational Requirements
- Local Hebbian-style updates
- Valence-modulated plasticity
- Hard bounds and decay
- Two-phase dynamics:
  - Online plasticity (hot path)
  - Offline consolidation (sleep)
- Promotion and decay must be supported as first-class outcomes of repeated traversal

### Non-Negotiables
- Learning must be local
- Updates must be incremental
- Stability must emerge without freezing plasticity

### Violations
- Global backpropagation
- Full retraining to correct errors
- Unbounded weight growth

---

## 4. Valence — Energy Modulation, Not Reward

### Principle
Valence is an **energy-shaping signal**, not a reward function.

Valence modulates:
- *How strongly* learning occurs
- *Where* attention and exploration bias

Valence does **not**:
- Select actions
- Dictate answers
- Encode goals directly

### Characteristics
- Multi-channel (importance, affect, novelty)
- Scales plasticity
- Signals coherence or surprise

### Violations
- Treating valence as a scalar reward
- Allowing valence to override traversal physics

---

## 5. Motifs — Emergent Procedural Memory

### Principle
Motifs are **emergent, reusable subpaths**.

They represent **procedural knowledge** (“knowing how”), not facts.

### Properties
- Discovered via repeated traversal and consensus
- Compress frequent reasoning patterns
- Enable analogical transfer

> A motif exists because it *worked*, repeatedly.

### Non-Negotiables
- Motifs must not be hand-authored
- Motifs must remain inspectable

### Violations
- Hard-coded macros
- Hidden procedural shortcuts

---

## 6. Lenses — Context as Projection

### Principle
Context is a **projection of the same manifold**, not a mode switch.

Lenses:
- Modify local metrics
- Emphasize dimensions
- Emerge from repeated use


Lenses are weightings over dimensions of constraint, not domain switches or modes.

Infrastructure-level run isolation (e.g. write-barriers, simulation buffers, or staging contexts) is permitted where it enforces invariants; such mechanisms do not constitute cognitive modes.

### Non-Negotiables
- No explicit mode flags
- No context-specific data copies

### Violations
- Hard context switches
- Separate reasoning engines per mode

---

## 7. Bicameral Regulation — Thermostat, Not Controller

### Principle
Manny has two interacting subsystems:

- **Experiencer** — explores and traverses
- **Executive** — regulates parameters

The executive **never routes paths**.

### Executive May
- Adjust temperature
- Modulate learning gain
- Trigger consolidation
- Detect instability

### Executive May Not
- Select answers
- Override paths
- Inject reasoning steps

---

## 8. Transparency — Geometry as Explanation

### Principle
Explainability is a **by-product of physics**, not a reporting feature.

- `/why` exposes the actual path taken
- Explanations must correspond to execution

### Non-Negotiables
- No post-hoc rationalization
- No explanation without a path

### Violations
- Narrative explanations detached from traversal

---

## 9. Locality & Energy Efficiency

### Principle
Computation scales with the **active region**, not total knowledge.

- Sparse updates
- Event-driven dynamics
- Near-linear scaling in active subgraph size
- Traversal energy/cost must be computable from local neighborhood information only

This property enables:
- Lifelong learning
- Interpretability
- Neuromorphic viability

### Violations
- Global recomputation in the hot path
- Reasoning cost tied to total graph size

---

## 10. Foundation Lock

Any proposed addition must answer **yes** to all of the following:

1. Does it operate *inside* the manifold?
2. Does it manifest as geometry, motion, or curvature?
3. Is it local and incremental?
4. Can it be traced as a path?
5. Does it preserve stability without freezing learning?

If not, it is **not Manny Manifolds**.

---

## 11. Foundations → Metrics Mapping (What Proves Each One)

This section defines **observable, testable signals** that demonstrate each foundation is upheld in practice.

### Foundation 0 — Core Claim (Geometry = Knowledge)
- ΔCurvature (Δκ) per successful interaction is non-zero
- Learned associations persist across sessions
- Failure signals: answers improve without measurable geometric change; resetting the manifold does not degrade performance

### Foundation 1 — Knowledge as a Deformable Manifold
- Graph state and κ distribution evolve over time
- No separate learned parameters outside the manifold
- Audit: delete manifold → performance collapses

### Foundation 2 — Threads as Thought
- 100% responses have an associated path trace
- Mean path length decreases on repeats
- Failure signals: answers produced without path IDs; cached answers bypass traversal

### Foundation 3 — Curvature ↔ Motion Feedback
- ≥20% average path-length reduction on **task-local / domain-local / transfer-directional** repeats (not global ASPL).
- Edge selection probability correlates with κ (kappa)
- Guardrails: bounded curvature variance; no monotonic κ explosion
- Observation-only flows (dry-run merges, diagnostics, hot-swap lens/temperature) must not alter κ or learning physics

### Foundation 4 — Valence as Energy Modulation
- |Δκ| scales with valence magnitude
- Zero-valence interactions still produce valid paths
- Failure signals: valence directly selects answers; removing valence breaks reasoning instead of just learning
- Energy is used as traversal cost / curvature bias (not a reward scalar).

### Foundation 5 — Motifs (Procedural Memory)
- Motif reuse ≥30% on analogical tasks
- ≥15% latency reduction when motifs are used
- **Reuse is an explicit event** (e.g., reuse_count > 0 via motif reuse/merge); **structural overlap alone ≠ reuse**.
- Failure signals: motifs never trigger; motifs encode answers instead of procedures

### Foundation 6 — Lenses (Contextual Projection)
- Lens activation correlates with local energy reduction
- Same node participates in multiple lenses
- Failure signals: hard mode switches; context-specific duplicate graphs

### Foundation 7 — Bicameral Regulation
- Executive only changes {τ, η, ζ}
- Removing executive increases instability but preserves reasoning
- Failure signals: executive selects or edits paths; executive injects content

### Foundation 8 — Transparency
- /why paths exactly match traversal logs
- Removing a high-κ edge alters the answer
- Failure signals: post-hoc explanations; explanations without causal impact

### Foundation 9 — Locality & Energy Efficiency
- Runtime ∝ k-hop neighborhood size
- <5% **runtime reasoning** compute in LLM calls (explicitly excludes offline ingestion, clarification, and embedding generation).
- Failure signals: global graph scans during reasoning; latency grows with total graph size

---

### Observation ≠ Learning (Non‑Negotiable)
- Observation, diagnostics, validation, and simulation **must not alter learning physics**.
- Dry‑run merges, guarded applies, observation‑only diagnostics, and lens/gravity hot‑swaps are **learning‑neutral**.

### Data Quality Sensitivity
- Experience density and structure (e.g., conversational vs definitional) materially shape curvature formation, convergence, and transfer; validation must account for this.

---

## Provisional Primitives & Compression (Foundational Addendum)
- Primitives are the smallest useful units at the current layer (provisionally atomic) and may be decomposed later without breaking manifold invariants.
- Knowledge is what survives promotion/decay within the manifold; opaque artifacts (files, blobs) are references only and do not constitute knowledge unless decomposed into manifold-native structure.
- Systems must support promotion/decay without creating external authoritative stores; manifold-native structure remains the sole learned state.
- Domains and taxonomies are emergent projections of manifold structure under specific lenses; they are not foundational constructs

---

## Canonical Summary

> **Manny Manifolds is a self-evolving geometric mind where experience reshapes space, and space shapes thought.**

Everything else is implementation detail.
