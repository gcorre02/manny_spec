# Traversal Physics — Specification (v0)

**Status:** Canon-adjacent (normative)  
**Purpose:** Define what traversal (motion) is in Manny, what it may and may not do, and how it remains local, auditable, and consistent across modes.

This spec constrains the **Thread Runner** and the **Energy/Cost Evaluator**. It complements:
- Routing & Anchoring Spec (where pressure enters)
- Lens Spec (temporary bias after anchoring)
- Gravity & Emergence Spec (what attraction must not become)

---

## 1. Core Claim

> Reasoning is motion. Traversal is the only reasoning step.

Traversal is a local process that selects successive activations from an active frontier under a deterministic, logged energy/cost law.

---

## 2. Definitions

- **Thread:** a single traversal instance producing one trace.
- **Trace:** an ordered list of visited nodes/edges with per-step cost breakdown.
- **Frontier:** the local k-hop neighborhood considered for next-step selection.
- **Energy/Cost:** the local bias function used to select the next step.
- **Termination:** the condition under which a thread ends.

---

## 3. Non-Negotiables

1. **Locality:** traversal may only examine a bounded neighborhood (k-hop frontier). No global graph scans.
2. **Determinism:** given snapshot + seed + lens configuration, traversal must be reproducible.
3. **Auditability:** each step must log node, edge, and cost component breakdown.
4. **Mode invariance:** traversal behavior is identical across OBSERVE/LEARN/STAGE up to trace termination.
5. **No planners:** traversal must not execute search controllers, heuristics that simulate planning, or global optimization.
6. **No semantic shortcuts:** traversal may not use semantic inference or LLM scoring to choose steps.

---

## 4. Frontier Semantics

- The frontier is an *active front*: a set of candidate nodes/edges reachable within k hops from the current node(s).
- k must be explicitly configured and logged.
- Multi-anchor traversals maintain a combined frontier; no anchor may be privileged by traversal logic.
- Traversal may maintain a small working set of candidate states, but must remain bounded.

---

## 5. Energy/Cost Composition (Contract)

Traversal selects steps using a local energy/cost function. This spec does not define the precise formula, but constrains inputs and exclusions.

### 5.1 Allowed inputs
- κ (curvature) along candidate edges / at candidate nodes
- lens weighting terms (relation-type and tag weighting)
- semantic mass proxies (read-only)
- valence traces (read-only)
- novelty terms (e.g., revisit penalties)
- bounded stochasticity (ε) derived from seed (temperature τ)

### 5.2 Forbidden inputs
- direct LLM scoring or similarity rankings as decision criteria
- global centrality measures computed at runtime
- unlogged heuristics
- external planners or goal stacks

### 5.3 Logging requirements
For every step, the trace must include:
- selected node/edge
- candidate count in frontier
- cost components (κ term, lens term, mass term, valence term, novelty term, ε)

---

## 6. Motion Law (Discrete Free-Fall)

Traversal must behave as discrete free-fall:

- At each step, the thread evaluates candidates in the frontier.
- The next step is chosen by sampling biased toward lower energy/cost.
- Mild stochasticity is allowed, but must be seed-derived and logged.
- Traversal must not guarantee global optimality; it must guarantee locality and reproducibility.

This preserves the principle:
> You do not compute trajectories. You follow gradients locally.

---

## 7. Termination Rules

A thread may terminate only by explicit, logged conditions:

- **Energy convergence:** no candidate offers a meaningful energy decrease (threshold configured + logged).
- **Goal reach (optional):** a designated target node is reached (if provided as an explicit artifact).
- **Step cap:** maximum steps reached (configured + logged).
- **Frontier collapse:** no candidates remain (dead-end).

Termination must be recorded in the trace.

---

## 8. Multi-Thread Behavior

Traversal may spawn multiple threads only under explicit policy:
- ambiguity (multiple anchors)
- stuckness (re-anchoring authorized by governance)

Constraints:
- threads must be independent and bounded
- thread spawning policy must be logged
- no global coordination that amounts to planning

---

## 9. Relationship to Learning

Traversal does not learn.

- In LEARN mode, PlasticityEngine may apply Δκ **after trace completion**.
- In OBSERVE mode, PlasticityEngine must be unreachable by construction.
- In STAGE mode, Δκ is buffered and committed only via explicit gate artifact.

Traversal behavior itself remains invariant across modes.

---

## 10. Failure Modes (Hard Stops)

Traversal is invalid if:
- it performs global scans or unbounded expansion
- it uses LLM scoring to choose steps
- it changes behavior across modes
- it produces output without a trace
- it terminates without a logged reason
- it privileges anchors in a multi-anchor run

Any failure requires abort and review.

---

## 11. Conformance Tests (Outline)

A traversal implementation passes this spec if:
- Replay determinism holds (snapshot + seed)
- Trace logs include required fields
- Frontier size remains bounded
- Mode invariance holds up to termination
- No semantic or planner inputs influence step selection

(Implement test harness separately.)

---

## 12. Summary Constraint

> Traversal is the only reasoning step. It is local, replayable, bounded, and trace-grounded.

This spec prevents traversal from becoming planning, inference, or hidden control.

---

## Appendix A — Minimal Traversal Algorithm Sketch (Non-binding)

This appendix provides a minimal, illustrative sketch of traversal behavior consistent with the Traversal Physics Specification v0. It is intended to clarify execution order and invariants, not to prescribe implementation details.

### A.1 Inputs
- Anchor set A = {a1, a2, …} (from Routing & Anchoring)
- Snapshot S (immutable manifold state)
- Seed r (deterministic randomness source)
- Lens configuration L (read-only, session-scoped)
- Traversal parameters (k, τ, step cap)

### A.2 Initialization
- Initialize one thread per anchor ai ∈ A
- For each thread:
  - set current node = ai
  - initialize empty trace

### A.3 Step Loop (per thread)
Repeat until termination:
1. Construct frontier F from current node using k-hop expansion
2. For each candidate c ∈ F:
   - compute local energy/cost E(c) using allowed inputs only
3. Sample next step c* from F biased toward lower E(c)
4. Append step (current → c*) and cost breakdown to trace
5. Update current node = c*

### A.4 Termination
Terminate thread only if:
- energy convergence criterion met
- goal artifact reached (if applicable)
- step cap exceeded
- frontier collapses

### A.5 Output
- Emit trace.json for each thread
- Do not modify manifold state

Notes:
- No global optimization is performed
- No traversal parameter may change mid-thread
- All stochasticity must be derived from seed r

---

## Appendix B — Traversal Conformance Tests

The following tests define whether a traversal implementation conforms to the Traversal Physics Specification v0.

### B.1 Determinism Test
- Given identical snapshot, anchors, lenses, and seed
- Traversal must produce identical traces

### B.2 Locality Test
- Traversal must not inspect nodes or edges outside the configured k-hop frontier
- Fail if any global scan or unbounded expansion occurs

### B.3 Mode Invariance Test
- Given OBSERVE, LEARN, and STAGE runs with identical inputs
- Traversal behavior and traces must be identical up to termination
- Plasticity effects (if any) must occur only after trace completion

### B.4 Cost Composition Test
- Every step must log κ, lens, mass, valence, novelty, and ε terms
- Fail if any unlogged heuristic influences step selection

### B.5 No Planner Test
- Traversal must not evaluate future paths, scores, or destinations
- Fail if lookahead, backtracking, or goal-directed search is detected

### B.6 Multi-Anchor Neutrality Test
- In multi-anchor runs, traversal must not privilege one anchor
- Fail if anchor identity affects cost or frontier construction

### B.7 Termination Validity Test
- Every trace must terminate with an explicit, logged reason
- Fail if termination is implicit or unexplained

### B.8 Replay Audit Test
- Given trace.json + snapshot + seed
- A replay must reconstruct identical traversal behavior

These tests should be implemented as automated checks against trace artifacts.