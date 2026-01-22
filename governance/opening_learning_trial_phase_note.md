# Opening the Learning Trial Phase — Conditions and Rationale

**Status:** Prepared / Not Yet Executed / Non-authorizing  
**Date:** 2026-01-15  
**Purpose:** Define conditions and rationale required to transition from Observational Emergence Phase to Learning Trial Phase

This document governs authorization only; it does not define experimental design, learning mechanics, or success criteria.

⸻

## Context

The Observational Emergence Phase (frozen as `canonical/validation/observation_emergence_phase_v0.md`) defines the OBSERVE-only period during which Manny's manifold is observed for signs of emergent structure prior to any autonomous learning or intrinsic gravity.

This note prepares the governance decision to open a Learning Trial Phase, but does not authorize it.

⸻

## Required Preconditions

Before a Learning Trial Phase may be opened, the following must be satisfied:

### 1. Observational Emergence Phase Completion

- Baseline diagnostics captured and replayed successfully
- Perturbation effects understood and documented
- Negative controls validated (no spurious emergence signals)
- Diagnostic behavior sufficiently characterized to inform learning experiments
- Diagnostic review conducted without thresholds, pass/fail labels, or rankings
- If diagnostic sensitivity has been validated via a bounded calibration probe, results may be included as supporting evidence but do not substitute for completion of the Observational Emergence Phase.

**Evidence required:** Diagnostic reports demonstrating baseline stability, perturbation responses, and negative control validation.

### 2. Canonical Stability

- Foundations, Design, and Architecture v1.1 remain frozen
- No canonical changes have occurred since the Observational Emergence Phase was frozen
- Validation framework operational

**Evidence required:** Confirmation that canonical freeze v1.1 remains in effect.

### 3. RunMode Discipline Proven

- OBSERVE/LEARN/STAGE boundaries enforced
- Write barriers verified
- At least one OBSERVE-only run replayed successfully with identical results
- Negative enforcement verified: forbidden writes in OBSERVE mode are rejected

**Evidence required:** Replay validation reports demonstrating RunMode isolation.

### 4. Diagnostic Infrastructure Operational

- `emergence_diagnostics_v0.md` implemented and producing outputs
- `diagnostic_dashboard_spec_v0.md` implemented and compliant
- Dashboard immutability during experiments is enforced

**Evidence required:** Operational diagnostic tools producing reproducible outputs.

**Note on Phase Status**  
At the time of writing, the Observational Emergence Phase is frozen at the specification level but has not yet been cleared in implementation. Detailed implementation gaps and execution status are recorded in `canonical/validation/observation_emergence_phase_v0.md`.

⸻

## Governance Decision Criteria

Opening a Learning Trial Phase requires explicit governance approval based on:

1. **Completion of Observational Phase** — all exit criteria met
2. **No premature emergence claims** — no gravity or learning effects declared during observation
3. **Infrastructure readiness** — all diagnostic and validation tools operational
4. **Explicit rationale** — documented reason for transition (not "we want to try learning")

Completion of all preconditions does not automatically trigger opening of the Learning Trial Phase; an explicit governance decision is always required.

⸻

## What a Learning Trial Phase Authorizes

If approved, a Learning Trial Phase would permit:

- Controlled learning windows as defined in `experiential_learning_curriculum_v0.md`
- LEARN-mode interactions with explicit boundaries
- Before/after diagnostic comparisons
- Hypothesis-constrained learning experiments

It does **not** authorize:
- Intrinsic gravity mechanisms
- Motif promotion beyond staging
- Parameter tuning to influence outcomes
- Canonical promotion of learning results

Opening a Learning Trial Phase authorizes experimentation only; it does not authorize claims of learning success, improvement, or emergence.

⸻

## Required Documentation for Approval

To approve opening a Learning Trial Phase, governance must receive:

1. **Observational Phase Completion Report** — evidence that all exit criteria are met
2. **Baseline Diagnostic Summary** — characterization of pre-learning manifold state
3. **Infrastructure Readiness Statement** — confirmation that diagnostic tools are operational
4. **Explicit Rationale** — documented reason for transition (e.g., "sufficient baseline established to enable controlled learning experiments")
5. **Learning Window Plan** — proposed learning experiments following `experiential_learning_curriculum_v0.md`
6. Concrete run identifiers and artifact paths for all evidence dossiers

⸻

## Relationship to Other Documents

This note depends on:
- `canonical/validation/observation_emergence_phase_v0.md` — defines what must be completed
- `canonical/candidate/training/experiential_learning_curriculum_v0.md` — defines how learning trials would be conducted
- `canonical/governance/gravity_promotion_checklist_v0.md` — defines when gravity may be discussed (not yet authorized)

This note does **not** authorize implementation. It only prepares the governance decision.

⸻

## Decision Record

**Status:** Not yet executed  
**Decision:** Pending  
**Rationale:** Awaiting completion of Observational Emergence Phase

When a decision is made, this section will be updated with:
- Approval/rejection decision
- Date of decision
- Rationale
- Evidence dossier reference

⸻

> **Principle:** Transition to learning requires explicit governance approval based on evidence, not desire. The Observational Phase must be complete before learning begins.
