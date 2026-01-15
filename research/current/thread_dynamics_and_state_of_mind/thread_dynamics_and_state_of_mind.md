

# Thread Dynamics & State of Mind (Research – Non‑Canonical)

**Status:** Research / exploratory
**Relation to canon:** Downstream of Manny v1.1. Nothing in this document is binding.

## Purpose
This document explores *mechanistic hypotheses* for how experiential "states of mind" may emerge as properties of thread dynamics in the Manny manifold. These ideas aim to explain locality, cascade behavior, lateral thinking, and memory texture **without introducing cognitive modes, planners, or new reasoning engines**.

The core claim under investigation:
> States of mind are not control modes; they are emergent properties of how threads move, branch, and deposit curvature under different dynamical conditions.

---

## 1. Locality as the Primary Scalability Mechanism

Manny must scale without global scans. Thread dynamics therefore operate on **local activation fronts**:

- Each thread maintains an *active frontier* limited to a k‑hop neighborhood around its current node.
- Expansion is driven by local signals only: edge curvature (κ), recent activation, valence, lens weighting.
- The frontier evolves as the thread moves; inactive regions of the manifold remain untouched.

**Hypothesis:**
Local activation fronts are sufficient for global coherence because high‑mass regions amplify and stabilize passing activity, while low‑mass regions naturally dampen it.

---

## 2. Emergent Cascades (“Neurons firing for no reason”)

Biological cognition exhibits spontaneous activations that occasionally cascade into full thoughts. Manny can approximate this via **background stochastic seeding**:

- A low‑rate stochastic process (temperature τ) occasionally seeds micro‑threads at random or weakly activated nodes.
- Most micro‑threads decay immediately.
- When a micro‑thread intersects a high semantic‑mass basin, amplification occurs and a full traversal emerges.

**Interpretation:**
- Curiosity, sudden insight, and associative leaps emerge without planners.
- Lateral thinking arises from rare survival of noisy activations in fertile regions.

---

## 3. “Neurons fire according to weights” — Manny analogue

Manny does not simulate spikes, but can approximate firing dynamics probabilistically.

### Activation rule (conceptual)
A node *n* becomes active with probability proportional to its **local potential**:

```
P(activate n) ∝ f(κ_in(n), recent_activation(n), valence(n), lens_weight(n))
```

Where:
- κ_in(n): sum of incoming curvature from the active frontier
- recent_activation(n): short‑term trace (recency)
- valence(n): positive/negative modulation
- lens_weight(n): contextual bias

A **thread** is then a *path of successive activations* chosen from the evolving frontier.

This preserves:
- locality
- stochasticity
- determinism under fixed seed + state

---

## 4. Direct ↔ Flexible as Motion Characteristics

What psychology labels as *direct* vs *flexible* thinking can be reframed as traversal dynamics:

- **Direct:** low τ, high cost sensitivity, narrow frontier → straight, efficient paths
- **Flexible:** higher τ, broader frontier → cascades, side‑effects, lateral exploration

These are *properties of motion*, not modes or intentions.

---

## 5. Free ↔ Bound as Curvature Viscosity

Another axis emerges from how curvature is deposited:

- **Free:** curvature updates diffuse across regions, decay faster, memories are nuanced and less sticky
- **Bound:** curvature updates are localized, decay slower, strong attractors form (rumination, fixation)

This affects:
- how quickly memories form
- how emotionally “colored” they are
- how hard they are to escape

---

## 6. State of Mind as an Experiencer Phenomenon

These dynamics are best understood as **children of the Experiencer**:

- Different threads may exhibit different motion characteristics simultaneously
- No global state of mind exists
- The Executive may *observe aggregate statistics* (e.g., average τ, curvature spread) but must not impose them

**Key constraint:**
> State of mind is inferred from motion; it is never set.

---

## 7. Implications for Memory Formation

Memory is affected not only by *what* is traversed, but *how*:

- Flexible / free motion → broader associative memory
- Direct / bound motion → sharp, durable attractors

This suggests that emotional nuance and context arise naturally from traversal dynamics, not from memory annotations.

---

## 8. Relationship to Primitives

For MVP:
- Primitives are symbolic units (words, percept patches)
- Their **activation dynamics** approximate firing

Future possibility (non‑binding):
- Introduce a low‑level percept manifold where primitives are represented as activation patterns
- Such patterns may *propose* activations but must never bypass standard traversal or curvature rules

---

## 9. Explicit Non‑Goals

- No spiking neural simulation
- No global activation sweeps
- No planners, goal stacks, or mode switches
- No psychological labels injected into cognition

---

## 10. Promotion Criteria (Meta)

Any mechanism described here may only be promoted if:
1. It preserves locality and free‑fall traversal
2. It introduces no new reasoning engines
3. It can be expressed as a parameterization of existing dynamics
4. It is empirically supported by MVP traces

---

**Status:** Research sandbox. May evolve rapidly. Not part of Manny canon.