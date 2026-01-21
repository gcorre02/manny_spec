

# Plasticity & Learning Physics — Specification (v0)

**Status:** Canon-adjacent (normative)  
**Purpose:** Define what learning (plasticity) is in Manny, what it may and may not do, and how it remains local, bounded, auditable, and reversible.

This spec constrains the **PlasticityEngine**, **Promotion/Decay**, and the learning-related parts of **Consolidation**. It complements:
- Routing & Anchoring Spec (pressure entry)
- Traversal Physics Spec (motion law)
- Lens Spec (temporary bias)
- Gravity & Emergence Spec (what attraction must not become)

---

## 1. Core Claim

> Learning is curvature. Plasticity is the only mechanism that mutates κ.

Learning occurs only as a **local, trace-bounded deformation** of manifold geometry following traversal.

---

## 2. Definitions

- **κ (curvature):** edge- and/or node-associated strength that shapes traversal cost.
- **Δκ (curvature delta):** bounded update applied to κ as a result of a completed trace.
- **PlasticityEngine:** the component that computes and applies Δκ.
- **Decay:** reduction of κ for unused or low-utility structure.
- **Promotion:** creation of higher-order structure (e.g., motif) from repeated traversal under constraints.
- **Consolidation:** offline maintenance tasks (prune, promote, recompute proxies) that may mutate κ only via Plasticity rules.

---

## 3. Non-Negotiables

1. **Mode gating:** κ may be mutated only in **LEARN** or **STAGE+commit**.
2. **Trace-bounded:** all Δκ must reference an explicit trace id; no free-form updates.
3. **Locality:** Δκ and decay operate only on the trace path and a bounded local neighborhood.
4. **Bounded updates:** Δκ must be clamped; no runaway curvature.
5. **No semantic authority:** LLM outputs may not directly set κ; only experiences and traces can influence updates.
6. **Auditability:** every κ mutation must emit artifacts sufficient for replay, inspection, and rollback.
7. **Reversibility:** any durable change must be reversible via artifacts (delta report + prior snapshot).
8. **No learning by observation:** PlasticityEngine must be unreachable in OBSERVE by construction.

---

## 4. Learning Pipeline (Canonical Order)

Learning may occur only in this order:

1) **Traversal completes** and emits `trace.json`  
2) **PlasticityEngine computes Δκ** from trace + signals  
3) **Δκ is applied** (LEARN) or **buffered** (STAGE)  
4) **Artifacts emitted** (`delta_report.json`)  
5) **Snapshot updated** (`snapshot.json`) if committed

Plasticity must never run during traversal.

---

## 5. Allowed Inputs to Plasticity

PlasticityEngine may use only:
- the completed trace (nodes/edges visited, step costs)
- valence channels (importance/novelty/affect) as *scalars that modulate Δκ magnitude*
- lens identifiers (read-only) for logging and optional modulation
- bounded local statistics (recent activation counts, local mass proxy values)

**Forbidden:**
- direct LLM scoring
- global graph metrics computed at runtime
- any lookahead or future simulation
- external planners or objectives

---

## 6. Update Rules (Contract-Level)

This spec does not prescribe a single formula, but constrains the *shape* of updates.

### 6.1 Reinforcement (trace edges)
- Edges traversed in the selected trace may receive positive Δκ.
- Δκ magnitude must be bounded and scaled by valence.

### 6.2 Correction / negative reinforcement
- If an experience marks a trace as incorrect or harmful, Δκ may be negative on the responsible subpath.
- Negative reinforcement must be bounded and logged explicitly.

### 6.3 Neighborhood decay (local-only)
- Edges not used may decay, but only within a bounded neighborhood of the trace.
- Decay rate must be conservative and deterministic.

### 6.4 Clamps and stability guards
- κ must be clamped to [κ_min, κ_max].
- A stability guard must detect runaway variance and halt learning if thresholds are breached.

---

## 7. Promotion & Compression (Motifs)

Promotion is allowed only via **repeated traversal evidence**.

### 7.1 Candidate criteria (minimum)
A motif candidate must satisfy:
- recurrence: observed across ≥ N traces (N configurable)
- benefit: reduces traversal cost/steps when reused
- stability: persists across varied contexts/lenses

### 7.2 Prohibitions
- motifs must not be injected manually
- motifs must not be created from single exposures
- motifs must not bypass trace logging

### 7.3 Motif effects
- motifs may introduce a new higher-order node/edge structure
- motifs must remain expandable into underlying steps for `/why`

---

## 8. STAGE Buffering & Commit Gate

In STAGE mode:
- PlasticityEngine computes Δκ but writes only to a **buffer**.
- Canonical manifold remains unchanged.

Commit requires an explicit gate artifact:
- `stage_commit.json` must exist and reference the buffered deltas.

Absence of commit artifact guarantees discard.

---

## 9. Consolidation Rules

Consolidation may:
- prune low-κ noise
- promote motifs
- recompute semantic mass proxies

Consolidation may mutate κ **only** by invoking the same Plasticity rules and must emit artifacts.

Consolidation must be deterministic given snapshot + seed.

---

## 10. Failure Modes (Hard Stops)

Learning is invalid if:
- κ changes without a trace id
- learning occurs in OBSERVE
- Δκ exceeds clamps or stability guards are bypassed
- LLM output directly sets κ
- consolidation mutates κ without artifacts
- commit occurs without `stage_commit.json`

Any failure requires abort and rollback.

---

## 11. Conformance Tests (Outline)

A plasticity implementation passes this spec if:
- **Mode gating:** κ never changes in OBSERVE
- **Trace-bounded:** every Δκ references a trace id
- **Determinism:** same snapshot + seed + experience → identical deltas
- **Boundedness:** κ remains within clamps; variance remains within guardrails
- **Rollback:** prior snapshot + delta report can restore state
- **Promotion discipline:** motifs require repeated evidence and reduce cost

(Implement test harness separately.)

---

## 12. Summary Constraint

> Plasticity is the only writer of κ. It runs only after traversal, is local, bounded, replayable, and reversible.

This spec prevents learning from becoming hidden optimization or semantic injection.

---

## Appendix A — Minimal Plasticity Algorithm Sketch (Non-binding)

This appendix provides a minimal sketch consistent with the spec.

### A.1 Inputs
- trace.json (selected trace)
- run_mode
- valence channels (importance/novelty/affect)
- κ clamps and stability thresholds

### A.2 Compute deltas
1. Identify the set of traversed edges E_trace.
2. For each e in E_trace:
   - compute Δκ_e = g(valence, step_cost, reuse_count)
3. Identify local neighborhood N_local around E_trace.
4. Apply conservative decay to edges in N_local \ E_trace.
5. Clamp all Δκ to safe bounds.

### A.3 Apply / buffer
- If LEARN: apply Δκ immediately.
- If STAGE: buffer Δκ; do not write to canonical κ.
- If OBSERVE: do not run.

### A.4 Emit artifacts
- delta_report.json: (trace_id, per-edge Δκ, clamps applied, summary stats)
- update snapshot.json only if commit is permitted.

---

## Appendix B — Plasticity Conformance Tests

### B.1 OBSERVE No-Write Test
- Run identical experience in OBSERVE.
- Assert κ unchanged; assert no delta_report emitted.

### B.2 Trace-Bounded Mutation Test
- Run LEARN.
- Assert every Δκ references a trace id and only touches trace path + bounded neighborhood.

### B.3 Determinism Test
- Re-run with same snapshot + seed.
- Assert identical deltas.

### B.4 Clamp & Guard Test
- Force high valence inputs.
- Assert κ remains within clamps; learning halts if guard triggers.

### B.5 STAGE Commit Gate Test
- Run STAGE; assert κ unchanged.
- Provide stage_commit.json; assert κ changes.
- Omit stage_commit.json; assert discard.

### B.6 Rollback Test
- Apply LEARN updates.
- Roll back using prior snapshot + delta report.
- Assert state equivalence.

### B.7 Motif Promotion Discipline Test
- Provide repeated traces meeting recurrence criteria.
- Assert motif created and reuse reduces traversal cost.
- Assert no motif from single exposure.

These tests should be implemented as automated checks against artifacts (trace.json, delta_report.json, snapshot.json, stage_commit.json).