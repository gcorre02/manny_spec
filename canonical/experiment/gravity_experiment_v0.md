# Gravity v0 — Experiment Plan (Canon-Compatible)

**Status:** Experiment protocol (non-canonical)  
**Purpose:** Design a falsifiable experiment to detect whether **gravity-like behavior** emerges in Manny under the existing physics (routing + traversal + plasticity + motifs) without adding new mechanics.

This plan is explicitly compatible with:
- Gravity & Emergence Placeholder Spec (v0), including pre-existence criteria
- Emergence Diagnostics Spec (v0)
- Routing/Traversal/Plasticity/Motif specs

> The goal is not to make gravity. The goal is to determine whether gravity *appears* under repeatable conditions.

---

## 0. Preconditions

- Routing & Anchoring Spec is implemented and passing conformance tests.
- Traversal Physics Spec is implemented and passing conformance tests.
- Plasticity Spec is implemented and passing conformance tests.
- Motif Emergence Spec is implemented and passing conformance tests.
- Emergence Diagnostics v0 instrumentation exists (or is approximated by logs).

**RunModes:**
- OBSERVE for diagnostics and measurement runs
- LEARN only for controlled learning windows
- STAGE optional (recommended for safety) with explicit commit artifacts

---

## 1. Experimental Hypotheses

### H0 (Null)
No gravity emerges. Traversal does not show persistent, lens-independent, anchor-independent basin capture beyond what can be explained by routing artifacts, lenses, stochasticity, or incidental topology.

### H1 (Gravity)
Gravity emerges as defined in the Gravity spec: persistent, lens-independent, path-level attraction toward certain regions caused solely by accumulated curvature and compression.

---

## 2. Experimental Design Overview

The experiment proceeds in three phases:

1) **Baseline (Flat)** — establish non-gravity baseline under OBSERVE
2) **Learning (Pressure)** — apply controlled experiential pressure under LEARN/STAGE
3) **Post-Learning (Detection)** — run OBSERVE-only diagnostic suite to test the pre-existence gravity criteria

All phases are replayable and must log snapshot + seed.

---

## 3. Test Environments (Graph Seeds)

Use small, synthetic manifolds to eliminate semantic confounds.

### Environment A — Symmetric Graph
- Two equal basins with equal connectivity
- Expectation: no persistent preference (any preference indicates bias or bug)

### Environment B — Asymmetric Graph
- One region has slightly more structural connectivity (controlled)
- Expectation: mild preference possible, but must not satisfy gravity criteria unless reinforced through learning

### Environment C — Hidden Cross-Link Graph
- Two distant regions connected only via a mid-level corridor
- Expectation: corridor traversal only becomes common after repeated pressure (tests metric bending)

### Environment D — Negative Control Graph
- Randomized graph / flattened κ distribution
- Expectation: no stable basins, no convergence signals

Each environment should have a deterministic generator and a `seed_graph.json` artifact.

---

## 4. Phase 1 — Baseline (OBSERVE-only)

### Objective
Establish baseline distributions for:
- path convergence
- basin capture rate
- escape energy
- subpath reuse frequency

### Procedure
For each environment:
1. Load snapshot S0
2. Select a set of diverse anchors (hub + periphery + unrelated)
3. Run N Jazz-style traversals in OBSERVE:
   - lenses disabled
   - fixed k, τ
   - vary only random seed across runs
4. Record:
   - trace.json
   - emergence_report.json (or equivalent metrics)

### Expected outcome
No gravity should be detected under the pre-existence criteria.

---

## 5. Phase 2 — Learning (Controlled Pressure)

### Objective
Apply repeated experience to create curvature accumulation and motif consolidation pressure.

### Principles
- Learning must occur only through traces + plasticity
- No manual injection of motifs or gravity
- Use STAGE where possible and commit explicitly

### Procedure
For each environment:
1. Start from baseline snapshot S0
2. Define a training curriculum of experiences that repeatedly traverse certain subpaths:
   - e.g., repeatedly start near region X and traverse corridor to region Y
   - include varied anchors to avoid anchor bias
3. Run a learning window of M interactions:
   - LEARN or STAGE
   - with explicit valence (importance) for the repeated corridor
4. Run consolidation after the learning window:
   - motif mining enabled
   - pruning enabled
5. Produce snapshot S1

### Negative controls
- Repeat the same learning window with:
  - LEARN disabled (OBSERVE)
  - or flattened valence
Expect no structural stabilization.

---

## 6. Phase 3 — Post-Learning Gravity Detection (OBSERVE-only)

### Objective
Test the gravity criteria strictly, with lenses disabled first.

### Procedure
For each environment:
1. Load snapshot S1
2. Run the same diagnostic suite as Phase 1:
   - lenses disabled
   - diverse anchors
   - multiple seeds
3. Evaluate the **Necessary Conditions** from the gravity definition:
   - lens independence
   - anchor independence
   - path-level behavior (no teleportation)
   - persistence under perturbation
   - cost-based explanation
   - earned through experience

### Additional perturbation tests
- repeat with small τ changes
- repeat with small lens weight changes (if lenses are enabled for probing)
- repeat with motifs disabled (sanity check)

---

## 7. Pass/Fail Criteria (Strict)

### Gravity is *not* present unless all hold:
- lens independence confirmed
- anchor independence confirmed
- path-level attraction across multiple routes
- persistence under perturbation
- cost-based explanation (trace cost breakdown supports it)
- earned-through-experience validated by baseline comparison

### Explicit fail conditions
- attraction disappears when lenses are removed
- convergence depends on anchor privilege
- behavior is explained by a single motif shortcut only
- diagnostics produce similar signals in negative control graphs

---

## 8. Required Artifacts

For each environment and phase:
- `seed_graph.json`
- `snapshot_S0.json`, `snapshot_S1.json` (and any intermediate)
- `trace.json` (per run)
- `emergence_report.json` (per batch)
- `delta_report.json` (learning windows)
- `promotion_report.json` (motif mining)

All artifacts must include:
- snapshot id
- seed
- runmode
- lens config (or explicit “none”)

---

## 9. Interpretation Rules

- If gravity criteria are not met, that is **information**, not failure.
- If partial signals appear, treat them as *pre-gravitational* and refine diagnostics.
- Only claim gravity when the strict criteria are satisfied.

---

## 10. Minimal Implementation Notes (Non-binding)

- Start with Environment C (hidden cross-link) as the most informative.
- Keep graphs small (50–500 nodes) to ensure full observability.
- Prefer STAGE for learning windows until rollback is proven.

---

## 11. Summary

This experiment plan provides a disciplined way to test whether gravity emerges from:
- κ accumulation
- motif consolidation
- resistance/decay interplay

without introducing planners or faking attraction.

> We measure first. If gravity appears, we document it. If it doesn’t, we learn why.
