# Gravity Promotion Checklist (v0)

**Status:** Governance / Promotion gate  
**Purpose:** Define when it is allowed to say "gravity exists" and what evidence must be present.

This is not physics and not an experiment. It is a permission document that acts as a guardrail preventing premature canonization of gravity.

⸻

## Core Principle

Gravity promotion is a **governance act**, not a technical one. This checklist must be unreasonably strict. That is a feature, not a bug.

⸻

## Required Diagnostic Signals

Before gravity may be promoted, the following must be present in `emergence_diagnostics_v0.md` outputs:

1. **Basin formation** — persistent attractor regions observable across runs
2. **Mass correlation** — semantic mass correlates with observed "pull"
3. **Distance effects** — observed effects scale with manifold distance
4. **Persistence** — effects persist across consolidation cycles
5. **Reproducibility** — effects reproducible with same snapshot + seed

All diagnostic signals must be:
- Measurable via `diagnostic_dashboard_spec_v0.md` compliant tools
- Traceable to artifact sources
- Independent of specific content domains

All required signals must be demonstrated *prior* to any intrinsic gravity mechanism being implemented. Diagnostic presence authorizes *discussion* of gravity, not implementation.

⸻

## Required Negative Controls

Gravity promotion requires evidence that gravity is **not**:
- A measurement artifact
- A lens effect
- A traversal bias
- A learning artifact

Required negative controls:
- **No-gravity baseline** — runs where gravity should not appear
- **Lens isolation** — gravity effects present without lens activation
- **Traversal independence** — gravity effects not explained by traversal heuristics
- **Learning independence** — gravity effects present in OBSERVE mode
- **Anchor invariance** — gravity effects must persist under different anchoring strategies (goal-directed, wander, fallback), holding other conditions constant.

⸻

## Persistence Requirements

Gravity must demonstrate:
- **Cross-session persistence** — effects survive snapshot save/load
- **Consolidation stability** — effects preserved through `mm sleep` cycles
- **Domain generality** — effects not limited to specific content areas
- **Scale invariance** — effects observable at different manifold sizes

⸻

## Lens-Independence Requirements

Gravity must be demonstrably **not** a lens effect:
- Effects present without active lenses
- Effects not correlated with lens activation patterns
- Effects not modifiable by lens parameter changes
- Effects observable in lens-free baseline runs

⸻

## Explicit Disqualifiers

Gravity promotion is **forbidden** if any of the following are true:

1. **Planner-like behavior** — if gravity appears to route paths or select answers
2. **Goal-directed effects** — if gravity appears to optimize toward objectives
3. **Hidden mechanisms** — if gravity cannot be explained via observable physics
4. **Measurement corruption** — if gravity only appears when measurement is altered
5. **Single-domain effects** — if gravity only appears in one content domain
6. **Non-reproducible** — if gravity cannot be reproduced with same conditions
7. **Lens-dependent** — if gravity disappears when lenses are disabled
8. **Learning-dependent** — if gravity only appears during learning windows
9. **Early parameterization** — if gravity can be tuned, weighted, or switched on/off via a parameter rather than observed as an effect, promotion is forbidden.

**If any disqualifier is true, do not promote.**

⸻

## Promotion Process

If all requirements are met and no disqualifiers apply:

1. **Evidence dossier** — compile all diagnostic outputs, negative controls, persistence tests
2. **Cross-spec review** — verify no conflicts with existing canonical specs
3. **Governance review** — explicit approval via governance process
4. **Version increment** — promotion requires new canonical version (e.g., v1.2)
5. **Documentation update** — update foundations/design/architecture as needed
6. **Public record** — record the promotion decision, evidence dossier, and dissenting opinions (if any) in governance logs.

Promotion is **not automatic** even if all checkboxes pass. It requires explicit governance decision.

⸻

## Relationship to Other Documents

This checklist depends *normatively* on:
- `canonical/validation/emergence_diagnostics_v0.md` — defines what to measure
- `canonical/validation/diagnostic_dashboard_spec_v0.md` — defines how to observe
- `canonical/candidate/training/experiential_learning_curriculum_v0.md` — defines learning conditions
- `canonical/candidate/gravity/gravity_emergence_spec.md` — defines gravity hypothesis

This checklist is the **gate** that prevents premature promotion.

⸻

## Conservative Bias

This checklist is intentionally conservative. It is better to:
- Wait too long to promote gravity
- Require too much evidence
- Be overly cautious

Than to:
- Promote gravity prematurely
- Canonize a measurement artifact
- Corrupt the physics suite

⸻

> **Principle:** When in doubt, do not promote. Gravity can wait. Canon cannot be undone.
