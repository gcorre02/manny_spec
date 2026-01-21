# Experiential Learning Curriculum (v0 — Candidate)

**Status:** Candidate / Non-canonical  
**Purpose:** Define a hypothesis-constrained learning protocol that treats learning as an experiment, not optimization.

This is not an implementation plan and not a training script. It is a hypothesis-constrained learning protocol that answers: under what conditions is learning allowed, what is held constant, what constitutes evidence vs noise, and how do we compare before/after.

⸻

## Core Hypothesis

Learning occurs through controlled experience under observable conditions. We can measure whether learning happened, but we cannot optimize for it without corrupting the measurement.

⸻

## Authority & Dependencies

This document defines a **candidate learning protocol** and does not authorize implementation by itself.

It depends on, and must not redefine:
- `emergence_diagnostics_v0.md` (authoritative measurement vocabulary)
- `diagnostic_dashboard_spec_v0.md` (permitted observation surfaces)
- Canonical Foundations, Design, and Architecture v1.1

Any learning curriculum that conflicts with these documents is invalid.

⸻

## Preconditions

Before any learning curriculum may be applied:

1. **Diagnostics baseline must exist**
   - `emergence_diagnostics_v0.md` must be operational
   - `diagnostic_dashboard_spec_v0.md` must be implemented
   - Baseline measurements must be recorded

2. **Canonical invariants must be frozen**
   - Foundations, design, and architecture v1.1 must be stable
   - No canonical changes during learning experiments

3. **RunMode discipline must be proven**
   - OBSERVE/LEARN/STAGE boundaries must be enforced
   - Write barriers must be verified

4. **Baseline reproducibility must be demonstrated**
   - At least one no-learning (OBSERVE-only) run must be replayed successfully
   - Diagnostics must be stable across identical seeds and conditions

⸻

## Learning Window Definition

A learning window defines:
- **What is allowed to change** — κ values, motif emergence, mass proxies
- **What must remain constant** — traversal physics, cost function structure, RunMode semantics
- **Duration** — explicit start/end conditions
- **Scope** — which interactions/domains are included

Learning windows are **bounded experiments**, not open-ended training.

⸻

## Learning Window Invariants

During a learning window:
- Anchoring and routing semantics must remain unchanged
- Lenses may be applied only if explicitly logged and disabled in control runs
- No new cost terms may be introduced
- Learning must be local and incremental; no global recomputation

Any violation invalidates the experiment.

⸻

## Negative Controls

Every learning curriculum must include:
- **No-learning runs** — identical conditions in OBSERVE mode
- **Baseline comparisons** — pre-learning snapshot measurements
- **Control groups** — parallel runs with different seeds but same structure

Negative controls prove that observed changes are due to learning, not measurement artifacts.

⸻

## Commit Discipline

Learning experiments must follow explicit commit rules:

1. **Baseline snapshot** — captured before learning window
2. **Learning window** — bounded set of LEARN-mode interactions
3. **Post-learning snapshot** — captured after learning window
4. **Comparison artifacts** — before/after diagnostics
5. **Decision point** — explicit gate: accept, reject, or extend

No learning may be committed without explicit comparison and decision.

⸻

## Expected Falsification Outcomes

This curriculum must be falsifiable. Expected outcomes:

- **If learning occurred:** measurable κ changes, path length reduction, motif emergence
- **If learning did not occur:** no κ changes, no path improvements, no structural emergence
- **If measurement is corrupted:** inconsistent diagnostics, non-reproducible results
- **If apparent learning is lens-dependent:** results are invalid
- **If learning disappears when replayed:** results are invalid

The curriculum must be able to **fail** and still be valid.

⸻

## Diagnostic Referencing Rule

All claims of learning, improvement, or structural change must reference specific diagnostics by name as defined in `emergence_diagnostics_v0.md`.

Narrative descriptions without diagnostic grounding are not admissible evidence.

⸻

## Relationship to Gravity

If gravity appears during a learning experiment, it must appear under these conditions:

- Observable via emergence diagnostics
- Measurable via diagnostic dashboard
- Reproducible across runs with same conditions
- Independent of specific learning content

This curriculum enables the statement:

> "If gravity appears, it appeared under these conditions."

⸻

## Relationship to Other Candidate Specifications

- Gravity & Emergence Specification — defines how gravity may be discussed
- Promotion & Motif Emergence Stub — defines how motifs may be promoted
- Jazz Protocol — defines observational surfaces prior to learning

This curriculum does not override any of the above.

⸻

## Promotion Path

This curriculum remains candidate until:
- It has been applied successfully
- Diagnostics show clear before/after differences
- Negative controls confirm learning causality
- Results are reproducible

Promotion to canonical would require evidence that this curriculum reliably produces observable learning without corrupting measurement.

⸻

> **Principle:** Learning is an experiment. Experiments must be falsifiable.
