Promotion & Motif Emergence Spec (how higher-order structure is allowed to stabilize)
# Promotion & Motif Emergence — Specification (v0)

**Status:** Canon-adjacent (normative)  
**Purpose:** Define how higher-order structure (motifs and promotions) may emerge and stabilize in Manny without introducing planners, semantic shortcuts, or manual injection.

This spec constrains **MotifMiner**, **MotifRegistry**, and the promotion-related aspects of **Consolidation**. It complements:
- Routing & Anchoring Spec (pressure entry)
- Traversal Physics Spec (motion law)
- Plasticity & Learning Physics Spec (κ mutation, decay, reversibility)
- Lens Spec (temporary bias)
- Gravity & Emergence placeholder (prevents attractor targets)

---

## 1. Core Claim

> Motifs are compressed, reusable structure that emerges from repeated traversal under constraint.

Motifs are not authored, not injected, and not treated as goals. They are **discoveries** made from trace evidence.

---

## 2. Definitions

- **Promotion:** the act of stabilizing higher-order structure derived from lower-order evidence (usually via consolidation).
- **Motif:** a reusable higher-order representation of a frequently traversed subpath or subgraph.
- **Motif Candidate:** a proposed motif derived from traces that has not yet been accepted.
- **Reusable Subpath:** an ordered subsequence of edges that appears across traces.
- **Motif Reuse Event:** an explicit event where traversal uses a motif instead of expanding all underlying steps.

---

## 3. Non-Negotiables

1. **Evidence-only:** motifs may be created only from trace artifacts (trace.json). No manual injection.
2. **Repeatability:** a motif must be supported by repeated occurrences across independent traces.
3. **Benefit requirement:** a motif must demonstrate measurable benefit (cost/steps reduction) when reused.
4. **Expandable explainability:** motifs must be expandable into underlying steps for `/why`.
5. **No semantic authority:** LLMs may name motifs or propose candidates, but may not authorize creation.
6. **No planners:** motifs must not become goals, routes, or long-range attractor targets.
7. **Locality:** motif discovery must operate on bounded windows and local neighborhoods, not global optimization.
8. **Reversibility:** motif creation and pruning must be reversible (artifact-backed).

---

## 4. Motif Life Cycle

Motifs progress through explicit states:

1) **Candidate** (discovered)
2) **Validated** (meets evidence and benefit criteria)
3) **Active** (eligible for reuse in traversal)
4) **Deprecated** (no longer beneficial or stable)
5) **Pruned** (removed/disabled; reversible)

All state transitions must be logged.

---

## 5. Candidate Discovery (Allowed Mechanisms)

MotifMiner may propose candidates using only trace-derived signals:

- frequency of subpaths (n-grams of edges)
- recurrence across different starting anchors
- recurrence across different lenses/contexts
- stability of local cost profile
- locality of the pattern (bounded length)

**Forbidden:**
- clustering or optimization requiring full-graph scans at runtime
- motif creation from single traces
- motif creation from LLM conclusions

---

## 6. Acceptance Criteria (Promotion Gates)

A motif candidate may be promoted to **Validated** only if all conditions hold:

### 6.1 Recurrence gate
- appears in ≥ N traces (N configurable)
- appears across ≥ C distinct contexts (lens ids or session ids)

### 6.2 Benefit gate
- reuse reduces:
  - step count OR
  - cumulative energy/cost
- benefit must be demonstrated on replay or held-out traces

### 6.3 Stability gate
- motif remains valid under small perturbations:
  - mild τ changes
  - lens weight variation
  - input paraphrases

### 6.4 Explainability gate
- motif can expand to a concrete underlying sequence of nodes/edges
- `/why` must surface both:
  - motif use
  - expanded steps (on demand)

If any gate fails, candidate remains candidate or is discarded.

---

## 7. Motif Representation (Contract)

A motif must be represented as:
- a **named subpath** (ordered list of underlying edges/nodes)
- optional metadata:
  - recurrence counts
  - observed contexts
  - benefit stats
  - creation snapshot id

A motif must not be represented as:
- an opaque embedding blob
- an external plan
- a black-box macro action

---

## 8. Motif Reuse (Traversal Integration)

Traversal may reuse motifs only if:
- motif is **Active**
- reuse is chosen by the same energy/cost law (no overrides)
- reuse is explicitly logged as a **Motif Reuse Event**

### 8.1 Reuse logging requirements
A reuse event must log:
- motif id
- expanded path length vs motif step length
- cost delta (before/after)
- lens + run mode

### 8.2 No hidden compression
If a motif is reused, it must still be possible to replay traversal with motifs disabled and obtain an equivalent expanded trace.

---

## 9. Motifs vs Lenses (Boundary)

- Lenses reweight traversal temporarily.
- Motifs compress structure persistently.

A motif must not become a lens.
A lens must not behave like a persistent attractor.

---

## 10. Motifs vs Gravity (Boundary)

Motifs may create efficient channels, but must not become long-range attraction targets.

Forbidden pattern:
- “motif pull” as a goal-like force that overrides local cost.

Allowed:
- motifs reduce local cost as a consequence of learned structure.

---

## 11. Decay, Deprecation, and Pruning

Motifs must be subject to decay and removal.

A motif may be **Deprecated** if:
- benefit drops below threshold over time
- reuse frequency collapses
- it increases error or instability

A deprecated motif may be **Pruned** if:
- it remains unused or harmful across a window

All deprecations and prunes must be reversible and artifact-backed.

---

## 12. Consolidation Rules

Motif discovery and promotion occur during consolidation only, unless explicitly enabled in a controlled maintenance window.

Consolidation must:
- be deterministic given snapshot + seed
- emit artifacts describing:
  - candidates proposed
  - promotions accepted
  - motifs deprecated/pruned

---

## 13. Failure Modes (Hard Stops)

Promotion/motif logic is invalid if:
- motifs are created from a single trace
- motifs are injected manually
- motifs cannot be expanded for `/why`
- motif reuse occurs without explicit logging
- motif selection bypasses energy/cost law
- motif mining requires global scans in the hot path

Any failure requires abort and rollback.

---

## 14. Conformance Tests (Outline)

A motif system passes this spec if:
- motifs require repeated trace evidence
- motifs demonstrably reduce cost/steps
- motifs are expandable and explainable
- motifs do not change traversal rules
- motif lifecycle events are logged and reversible

(Implement test harness separately.)

---

## 15. Summary Constraint

> Motifs are earned compression. They emerge from repeated motion, remain explainable, and may decay.

This spec prevents motifs from becoming manual macros, planners, or hidden controllers.

---

## Appendix A — Minimal Motif Mining Sketch (Non-binding)

1. Collect a window of recent traces (bounded).
2. Extract frequent edge n-grams (length 2..L).
3. Filter candidates by:
   - recurrence across contexts
   - minimum benefit estimate
4. Validate on replay/held-out traces.
5. Promote to Active only when gates pass.

---

## Appendix B — Motif Conformance Tests

### B.1 Recurrence Gate Test
- Provide traces with repeated subpaths.
- Assert motif candidate appears only after N traces.

### B.2 Single-Exposure Prohibition Test
- Provide one trace.
- Assert no motif created.

### B.3 Benefit Test
- With motifs enabled, assert reduced step count or cost.
- With motifs disabled, expanded trace must still exist.

### B.4 Explainability Test
- Motif reuse event must expand to underlying steps.

### B.5 No Planner Test
- Motif presence must not override cost law.

### B.6 Lifecycle Reversibility Test
- Promote then prune; rollback restores prior state.

These tests should be implemented as automated checks against artifacts (trace.json, motif_registry.json, promotion_report.json, snapshot.json).