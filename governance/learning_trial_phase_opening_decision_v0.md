# Learning Trial Phase — Opening Decision Record (v0)

**Status:** ⬜ **PENDING** (default) / ☑ **APPROVED** / ☑ **REJECTED**  
**Date:** [YYYY-MM-DD]  
**Decision Maker(s):** [Name(s)]

⸻

## Decision
**⬜ PENDING** — Decision not yet made  
**☑ APPROVED** — Learning Trial Phase is authorized to open  
**☑ REJECTED** — Learning Trial Phase opening is denied

Only one option may be selected. Approval requires that all required preconditions below are verified and evidenced.

⸻

## Required Preconditions Review

Before a Learning Trial Phase may be opened, the following must be satisfied:

### 1. Observational Emergence Phase Completion

**Status:** ☑ Complete / ⬜ Incomplete

- Baseline diagnostics captured and replayed successfully
- Perturbation effects understood and documented
- Negative controls validated (no spurious emergence signals)
- Diagnostic behavior sufficiently characterized to inform learning experiments
- Diagnostic review conducted without thresholds, pass/fail labels, or rankings
- If diagnostic sensitivity has been validated via a bounded calibration probe, results may be included as supporting evidence but do not substitute for completion of the Observational Emergence Phase

**Evidence:**
- [ ] Observational Emergence Phase clearance record: `canonical/validation/observational_emergence_clearance_record_v0.md`
- [ ] Diagnostic reports demonstrating baseline stability, perturbation responses, and negative control validation
- [ ] Concrete run identifiers and artifact paths: [list run IDs and paths]

**Reviewer Notes:**
[Space for reviewer to document evidence review]

Completion here refers to formal governance clearance of the Observational Emergence Phase, not merely execution of runs.

---

### 2. Canonical Stability

**Status:** ☑ Verified / ⬜ Not Verified

- Foundations, Design, and Architecture v1.1 remain frozen
- No canonical changes have occurred since the Observational Emergence Phase was frozen
- Validation framework operational

**Evidence:**
- [ ] Confirmation that canonical freeze v1.1 remains in effect
- [ ] No canonical changes since Observational Emergence Phase freeze date

**Reviewer Notes:**
[Space for reviewer to document stability verification]

---

### 3. RunMode Discipline Proven

**Status:** ☑ Verified / ⬜ Not Verified

- OBSERVE/LEARN/STAGE boundaries enforced
- Write barriers verified
- At least one OBSERVE-only run replayed successfully with identical results
- Negative enforcement verified: forbidden writes in OBSERVE mode are rejected

**Evidence:**
- [ ] Replay validation reports demonstrating RunMode isolation
- [ ] Write barrier test results
- [ ] Negative enforcement test results

**Reviewer Notes:**
[Space for reviewer to document RunMode discipline verification]

---

### 4. Diagnostic Infrastructure Operational

**Status:** ☑ Operational / ⬜ Not Operational

- `emergence_diagnostics_v0.md` implemented and producing outputs
- `diagnostic_dashboard_spec_v0.md` implemented and compliant
- Dashboard immutability during experiments is enforced

**Evidence:**
- [ ] Operational diagnostic tools producing reproducible outputs
- [ ] Dashboard compliance verification
- [ ] Dashboard immutability enforcement verification

**Reviewer Notes:**
[Space for reviewer to document infrastructure verification]

⸻

## Governance Decision Criteria

Opening a Learning Trial Phase requires explicit governance approval based on:

1. **Completion of Observational Phase** — all exit criteria met
2. **No premature emergence claims** — no gravity or learning effects declared during observation
3. **Infrastructure readiness** — all diagnostic and validation tools operational
4. **Explicit rationale** — documented reason for transition (not "we want to try learning")

**Completion of all preconditions does not automatically trigger opening of the Learning Trial Phase; an explicit governance decision is always required.**

Governance approval must be based on evidence review, not anticipated outcomes or expected learning benefits.

⸻

## Explicit Rationale

**Why is the Learning Trial Phase being opened now?**

[Document the explicit reason for transition. Examples:
- "Sufficient baseline established to enable controlled learning experiments"
- "Diagnostic behavior sufficiently characterized to distinguish learning effects from artifacts"
- "Observational phase complete; infrastructure ready; governance approval obtained"]

**What learning experiments are proposed?**

[Reference or describe the proposed learning window plan following `experiential_learning_curriculum_v0.md`]

Rationale must reference concrete evidence artifacts and governance records; speculative or aspirational justifications are insufficient.

⸻

## Required Documentation for Approval

To approve opening a Learning Trial Phase, governance must receive:

1. **Observational Phase Completion Report** — evidence that all exit criteria are met
2. **Baseline Diagnostic Summary** — characterization of pre-learning manifold state
3. **Infrastructure Readiness Statement** — confirmation that diagnostic tools are operational
4. **Explicit Rationale** — documented reason for transition
5. **Learning Window Plan** — proposed learning experiments following `experiential_learning_curriculum_v0.md`
6. **Concrete run identifiers and artifact paths** — for all evidence dossiers

**Evidence Dossier:**
- [ ] Observational Phase Completion Report: [path]
- [ ] Baseline Diagnostic Summary: [path]
- [ ] Infrastructure Readiness Statement: [path]
- [ ] Learning Window Plan: [path]
- [ ] Run identifiers and artifact paths: [list]

⸻

## What This Decision Authorizes

If **APPROVED**, this decision authorizes:

- Controlled learning windows as defined in `experiential_learning_curriculum_v0.md`
- LEARN-mode interactions with explicit boundaries
- Before/after diagnostic comparisons
- Hypothesis-constrained learning experiments

It does **not** authorize:
- Intrinsic gravity mechanisms
- Motif promotion beyond staging
- Parameter tuning to influence outcomes
- Canonical promotion of learning results

- Any automatic transition to subsequent phases or promotion decisions

**Opening a Learning Trial Phase authorizes experimentation only; it does not authorize claims of learning success, improvement, or emergence.**

⸻

## Decision Record

**Decision:** [APPROVED / REJECTED / PENDING]  
**Date:** [YYYY-MM-DD]  
**Decision Maker(s):** [Name(s)]  
**Rationale:** [Summary of decision rationale]

**If APPROVED:**
- Learning Trial Phase opening date: [YYYY-MM-DD]
- Learning window plan reference: [path]
- Expected duration: [if known]
- Evaluation criteria: [reference to `experiential_learning_curriculum_v0.md`]

**If REJECTED:**
- Reason for rejection: [explanation]
- Required actions before reconsideration: [list]

⸻

## Relationship to Other Documents

This decision record depends on:
- `canonical/validation/observation_emergence_phase_v0.md` — defines what must be completed
- `canonical/validation/observational_emergence_clearance_record_v0.md` — clearance evidence
- `canonical/candidate/training/experiential_learning_curriculum_v0.md` — defines how learning trials would be conducted
- `governance/opening_learning_trial_phase_note.md` — defines conditions and rationale requirements
- `canonical/governance/gravity_promotion_checklist_v0.md` — defines when gravity may be discussed (not yet authorized)

This decision record does **not** authorize implementation by itself. It records the governance decision to open the phase.

In case of conflict, the canonical specifications and clearance records take precedence over this decision record.

⸻

## Next Steps (If Approved)

1. Update Learning Trial Phase status to ACTIVE
2. Begin learning window experiments per `experiential_learning_curriculum_v0.md`
3. Maintain strict diagnostic discipline throughout
4. Record all learning window activities and outcomes
5. No claims of learning success until curriculum completion and evaluation

⸻

> **Principle:** Opening a Learning Trial Phase is a governance act that authorizes controlled experimentation only. Learning, emergence, or improvement must be demonstrated empirically and evaluated separately.
