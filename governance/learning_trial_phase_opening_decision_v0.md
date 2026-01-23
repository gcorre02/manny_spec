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
- [x] Observational Emergence Phase clearance record: `canonical/validation/observational_emergence_clearance_record_v0.md`
- [x] Implementation execution/clearance readiness record (runtime repo): `/Users/guilherme.ribeiro/dev/Manny_v3/docs/validation/observational_emergence_clearance_record_v0.md`
- [x] Observational Phase completion report (runtime repo): `/Users/guilherme.ribeiro/dev/Manny_v3/implementation_plans/status/observation_emergence_report.md`
- [x] Diagnostic report bundles (runtime repo, golden): `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_baseline_v0`, `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_novelty_v0`, `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_negative_control_v0`, `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_flatten_kappa_v0`, `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_calibration/`
- [x] Concrete run identifiers and artifact paths (runtime repo):
  - Baseline `54b06f0f4f9fd64f62735a1c813e5d0cdd042e4214a73b7e7c9a72eaaa9197ad` — `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_baseline_v0` and `/Users/guilherme.ribeiro/dev/Manny_v3/runs/audit/emergence_baseline_v0`
  - Perturbation (novelty) `43dc7911da6b7a912e2c25df8a1c06dabafc4d5b05c9d083d9d1230b5167e4a5` — `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_novelty_v0`
  - Negative control (randomize traversal) `cf1d12648fe79b1d677b65b20c9269bc06a361b07e48a65e86957195604ba968` — `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_negative_control_v0`
  - Negative control (flatten κ/mass) `2a6957172a1d945fb2990a4d0bb9dd80bca1b5858f1c8c6d4b37ee91088cdcc8` — `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_flatten_kappa_v0`
  - Calibration probe (bounded LEARN) `7b80c10a0a5c65c27f8d6268be4b3889e5d61fe976ae08f3a95b6ef2d6f5b722` — `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_calibration/learn`

**Reviewer Notes:**
- 2026-01-23 audit: evidence exists in runtime repo and is archived as golden runs + phase report; observational diagnostics are stable/non-discriminative under frozen physics (no learning/gravity authorized).
- Fresh rerun 2026-01-23: `emergence_baseline_v0` produced `emergence_report.json` and dashboard bundle under `/Users/guilherme.ribeiro/dev/Manny_v3/runs/audit/emergence_baseline_v0`.
- Issue to investigate: emergence diagnostics report lacks a recorded spec pin (`spec_pin` null) in runtime artifacts; provenance should include the pinned spec commit/version.

Completion here refers to formal governance clearance of the Observational Emergence Phase, not merely execution of runs.

---

### 2. Canonical Stability

**Status:** ☑ Verified / ⬜ Not Verified

- Foundations, Design, and Architecture v1.1 remain frozen
- No canonical changes have occurred since the Observational Emergence Phase was frozen
- Validation framework operational

**Evidence:**
- [x] Confirmation that canonical freeze v1.1 remains in effect: `governance/canonical_freeze_v1.1.md`
- [x] Runtime repo spec pin and verification date: `/Users/guilherme.ribeiro/dev/Manny_v3/SPEC_VERSION.md` (pins spec submodule at `c262cc3964d9e55cd68b0add5a4c9408bfd1f1b5`, last verified 2026-01-20)
- [x] Runtime repo spec submodule is clean (no local modifications): `/Users/guilherme.ribeiro/dev/Manny_v3/spec/` (submodule at pinned commit)

**Reviewer Notes:**
- 2026-01-23 audit: runtime repo is pinned to spec commit `c262cc3…` and `spec/` shows no local drift.
- Issue to investigate: runtime spec copy `spec/governance/opening_learning_trial_phase_note.md` still claims the observational phase is “not yet cleared” and references `experiential_learning_curriculum_v0.md`; reconcile via manny_spec update or explicit divergence note in runtime repo.

---

### 3. RunMode Discipline Proven

**Status:** ☑ Verified / ⬜ Not Verified

- OBSERVE/LEARN/STAGE boundaries enforced
- Write barriers verified
- At least one OBSERVE-only run replayed successfully with identical results
- Negative enforcement verified: forbidden writes in OBSERVE mode are rejected

**Evidence:**
- [x] Replay validation reports demonstrating RunMode isolation (runtime repo, golden):
  - OBSERVE no-mutation `0bc3ffbf437913ffb5c7ee20f3ba5e7d315e86f9d777753c60fb26371491ec6f` — `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/m2_runmodes_obs`
  - STAGE no-commit `47ce29ccfdd194d4344460589efc26c8c112a5b4beb2bd2c4356d7d2d8032904` — `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/m2_runmodes_stage`
- [x] Write barrier test results (runtime repo): `/Users/guilherme.ribeiro/dev/Manny_v3/tests/unit/test_write_barrier.py`, `/Users/guilherme.ribeiro/dev/Manny_v3/tests/integration/test_mode_isolation.py`
- [x] Negative enforcement test results (runtime repo): `/Users/guilherme.ribeiro/dev/Manny_v3/tests/integration/test_barrier_violation_reporting.py`
- [x] Fresh reruns 2026-01-23 (runtime repo):
  - OBSERVE control `aaaccfa365616840ce84ddf5b367c179bf68e77c02545b0554c4b39b309fe789` — `/Users/guilherme.ribeiro/dev/Manny_v3/runs/audit/toy_m1`
  - LEARN commit (persistence pack) `aaa07eccc61f7f9537f148a6bf088e084b6711fc668d829306b360547c9a5b97` — `/Users/guilherme.ribeiro/dev/Manny_v3/runs/audit/learn_commit_persistence`

**Reviewer Notes:**
- 2026-01-23 audit: RunModes are enforced end-to-end; OBSERVE runs produce traces without durable κ mutation; LEARN commits produce explicit `stage_commit.json` and applied `delta_kappa` artifacts.
- Note: runtime `run_manifest.json` is intentionally immutable after preregistration; post-commit state hashes live in `validation_report.json` (use report for after/rollback evidence).

---

### 4. Diagnostic Infrastructure Operational

**Status:** ☑ Operational / ⬜ Not Operational

- `emergence_diagnostics_v0.md` implemented and producing outputs
- `diagnostic_dashboard_spec_v0.md` implemented and compliant
- Dashboard immutability during experiments is enforced

**Evidence:**
- [x] Operational diagnostic tools producing reproducible outputs (runtime repo):
  - `emergence_report.json` and dashboard bundle (fresh rerun): `/Users/guilherme.ribeiro/dev/Manny_v3/runs/audit/emergence_baseline_v0`
  - Golden dashboard bundles: `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_baseline_v0/dashboard`, `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_novelty_v0/dashboard`, `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_negative_control_v0/dashboard`, `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_flatten_kappa_v0/dashboard`
- [x] Dashboard compliance verification (runtime repo): `/Users/guilherme.ribeiro/dev/Manny_v3/src/cli/main.py` (`emergence-report`, `emergence-dashboard`), `/Users/guilherme.ribeiro/dev/Manny_v3/tests/integration/test_emergence_phase.py`
- [x] Dashboard immutability enforcement verification: dashboards are derived from artifacts only (no state writes); OBSERVE/isolated runs keep state unchanged (`state_mode=isolated` in run manifests for emergence packs).

**Reviewer Notes:**
- 2026-01-23 audit: diagnostic tooling exists and was executed end-to-end (`mm emergence-report`, `mm emergence-dashboard`) on an OBSERVE run root; outputs are deterministic and non-authoritative (no thresholds/scores).

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

[Reference or describe the proposed learning window plan following `canonical/candidate/training/experiential_learning_curriculum_v0.md`]

Rationale must reference concrete evidence artifacts and governance records; speculative or aspirational justifications are insufficient.

⸻

## Required Documentation for Approval

To approve opening a Learning Trial Phase, governance must receive:

1. **Observational Phase Completion Report** — evidence that all exit criteria are met
2. **Baseline Diagnostic Summary** — characterization of pre-learning manifold state
3. **Infrastructure Readiness Statement** — confirmation that diagnostic tools are operational
4. **Explicit Rationale** — documented reason for transition
5. **Learning Window Plan** — proposed learning experiments following `canonical/candidate/training/experiential_learning_curriculum_v0.md`
6. **Concrete run identifiers and artifact paths** — for all evidence dossiers

**Evidence Dossier:**
- [x] Observational Phase Completion Report: `/Users/guilherme.ribeiro/dev/Manny_v3/implementation_plans/status/observation_emergence_report.md`
- [x] Baseline Diagnostic Summary (artifact bundle): `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/emergence_baseline_v0/dashboard/emergence_dashboard.md` (and fresh rerun: `/Users/guilherme.ribeiro/dev/Manny_v3/runs/audit/emergence_baseline_v0/dashboard/emergence_dashboard.md`)
- [x] Infrastructure Readiness Statement: `/Users/guilherme.ribeiro/dev/Manny_v3/docs/validation/observational_emergence_clearance_record_v0.md`
- [ ] Learning Window Plan: [TBD — produce a concrete learning-window plan aligned to `canonical/candidate/training/experiential_learning_curriculum_v0.md`]
- [x] Run identifiers and artifact paths (fresh audit 2026-01-23): `/Users/guilherme.ribeiro/dev/Manny_v3/runs/audit/` (toy_m1, learn_commit_persistence, transfer_analogy_v0, transfer_negative_control_v0, emergence_baseline_v0, jazz_preview, jazz_export_pack, jazz_replay)
- [x] Run identifiers and artifact paths (golden evidence): `/Users/guilherme.ribeiro/dev/Manny_v3/evidence/golden_runs/` (M0–M6, Jazz, observational emergence tracks)

**Issues / Further Investigation (from 2026-01-23 audit):**
- Runtime validation harness gate scoping: `learn_commit_persistence` fails due to Gate C being evaluated for all LEARN runs (`/Users/guilherme.ribeiro/dev/Manny_v3/src/validation/gates.py`); consider making Gate C not-evaluated unless motifs/reuse are in-scope for the pack.
- Runtime Gate B semantics: `delta_kappa_persists` is forced true for non-LEARN/commitless runs (`/Users/guilherme.ribeiro/dev/Manny_v3/src/validation/runner.py`), which can make OBSERVE packs appear to “pass” Gate B; consider separating “not evaluated” vs “true”.
- Spec/governance mismatch in runtime spec pin: `spec/governance/opening_learning_trial_phase_note.md` status text is stale vs. implemented/cleared observational evidence; reconcile via spec update (manny_spec) or explicit divergence note.
- Provenance gap: emergence diagnostics report currently omits spec pin (`spec_pin` null) in runtime `emergence_report.json`; add spec pin for auditability.

⸻

## What This Decision Authorizes

If **APPROVED**, this decision authorizes:

- Controlled learning windows as defined in `canonical/candidate/training/experiential_learning_curriculum_v0.md`
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
- Evaluation criteria: [reference to `canonical/candidate/training/experiential_learning_curriculum_v0.md`]

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
2. Begin learning window experiments per `canonical/candidate/training/experiential_learning_curriculum_v0.md`
3. Maintain strict diagnostic discipline throughout
4. Record all learning window activities and outcomes
5. No claims of learning success until curriculum completion and evaluation

⸻

> **Principle:** Opening a Learning Trial Phase is a governance act that authorizes controlled experimentation only. Learning, emergence, or improvement must be demonstrated empirically and evaluated separately.
