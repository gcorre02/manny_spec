# Gravity & Emergence — Placeholder Specification (v0)

**Status:** Canon-adjacent placeholder (non-binding)  
**Purpose:** Formally state what *gravity*, *emergence*, and *attraction* mean in Manny’s physics — and, critically, what they are **not allowed to become**.

This document exists to prevent future drift as higher-level metaphors (gravity, mass, attraction, emergence) are explored or implemented.

---

## 1. Scope and Intent

This specification does **not** introduce new mechanics.

It exists to:
- reserve conceptual territory for future work
- define hard boundaries around misuse
- ensure compatibility with existing routing, lens, and learning specifications

Nothing in this document authorizes implementation.

---

## 2. Definitions (Constrained)

### Gravity (Manny-native)

Gravity refers to the *emergent tendency* of traversal to flow toward regions of high curvature and semantic mass under the existing motion law.

Gravity is:
- descriptive, not directive
- emergent, not configured
- a consequence of compression, not a force applied externally

Gravity may eventually be expressed through signed potential effects or metric deformation, but only after stable curvature has emerged through learning. Gravity must not be approximated via additive cost penalties or lens escalation.

### Emergence

Emergence refers to stable, compressive structure that arises from repeated traversal under constraint.

Emergence is detected, not declared.

---

## 3. What Gravity Is NOT

Gravity must **never** be implemented as:

- a goal signal
- an objective function
- a reward
- a global optimizer
- a planner heuristic
- a shortcut around traversal

If an implementation *selects* destinations rather than allowing motion to *fall*, it is not gravity.

---

## 4. Relationship to Lenses (Critical Boundary)

Lenses are **not** gravity.

### Lenses may:
- locally reweight traversal cost
- bias motion temporarily
- surface different perspectives on the same topology

### Lenses may NOT:
- create persistent attraction
- accumulate across sessions
- define long-range pull
- simulate gravity through weight escalation

Any lens that behaves like gravity violates this spec.

Resistance terms (e.g., mass or friction penalties) oppose motion locally; gravity describes global attraction that emerges from shaped geometry and must not be simulated by such penalties.

---

## 5. Relationship to Anchoring & Routing

Gravity must not influence:
- anchor selection
- anchor expansion
- re-anchoring decisions

Gravity emerges *after* anchoring and *during* traversal only.

If gravity affects where pressure enters the system, routing has been corrupted.

---

## 6. Relationship to Plasticity & Learning

Gravity does not learn.

Learning reshapes curvature; gravity is the *effect* of that reshaping.

Gravity must not:
- modify κ
- modify mass proxies
- trigger consolidation
- influence promotion decisions

If gravity writes state, learning has been bypassed.

---

## 7. Emergence vs Control

Emergence must remain:
- bottom-up
- falsifiable
- reversible

Emergence must not be:
- injected
- accelerated artificially
- protected from decay

Any mechanism that preserves structure without continued pressure is a violation.

---

## 8. Forbidden Failure Modes

The following are explicitly disallowed:

- “Gravitational goals”
- “Attractor targets”
- “Emergent intent”
- “Soft planners disguised as gravity”
- “Long-range attraction fields”

These patterns are incompatible with Manny’s physics.

---

## 9. Allowed Future Exploration (Strictly Non-Binding)

Future work *may* explore:
- measuring gravitational effects (post-hoc)
- visualizing basin depth and flow
- comparing gravitational patterns across domains

Such work must:
- operate on artifacts
- remain observational
- introduce no new motion laws

---

## 9.a Candidate Diagnostic Metrics for Gravity Emergence (Non-Binding)

The following metrics are **descriptive signals** that may be used to *observe* whether gravity-like behavior has emerged. These metrics are diagnostic only and must not be used as control signals, thresholds, or optimization targets.

### Basin Capture Rate
- Definition: proportion of traversals from diverse anchors that terminate within the same region or subgraph.
- Interpretation: increasing capture rate suggests emergent attraction.

### Mean Path Convergence
- Definition: reduction in variance of path endpoints and intermediate trajectories across repeated runs.
- Interpretation: convergence indicates shaped geometry rather than exploratory spread.

### Effective Distance Compression
- Definition: decrease in average shortest-path length to a region over time (post-learning).
- Interpretation: reflects motif formation and curvature shaping.

### Escape Energy
- Definition: average additional cost required to traverse away from a region once entered.
- Interpretation: higher escape energy suggests basin depth.

### Motif Dominance Persistence
- Definition: stability of motif reuse patterns across sessions and after consolidation/sleep.
- Interpretation: persistent motifs indicate structural reinforcement consistent with gravity.

### Lens Independence Check
- Definition: confirmation that observed attraction persists when all lenses are disabled.
- Interpretation: distinguishes intrinsic gravity from extrinsic field effects.

All metrics must be computed post-hoc from artifacts. No metric listed here may influence traversal, anchoring, learning, or consolidation decisions.

---

## 10. Summary Constraint

> Gravity in Manny is what motion *does* in a shaped space — not something the system *uses* to decide.

This document reserves the concept of gravity for emergent explanation only, unless explicitly promoted through a future canonical process.