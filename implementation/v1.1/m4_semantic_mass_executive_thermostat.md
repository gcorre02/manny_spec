# M4 — Semantic Mass & Executive Thermostat

## Objective
Introduce **semantic mass** as a first-order property of the manifold that influences motion, and an **executive thermostat** that regulates global parameters *without routing or planning*.

M4 proves:
- Motion responds to accumulated structure (mass/density), not just κ.
- Semantic mass participates in the cost/energy function and is logged per step.
- The executive regulates parameters only (τ, η, ζ), never paths or decisions.
- Learning semantics (κ updates) remain unchanged from M3.

---

## M4.0 Implementation contract (what guides the build)

M4 must conform to canonical specs:
- **Foundations v1.1:** cognition as motion in a weighted field; regulation without control.
- **Design v1.1:** semantic mass as signal; executive as thermostat, not planner.
- **Architecture v1.1:** cost/energy evaluator is authoritative; core engine owns regulation.
- **Validation framework:** participation and logging are required; correctness is heuristic v0.

**RunMode (M4):** Semantic mass computation and executive regulation are permitted in all modes; durable learning (κ mutation) remains restricted to `LEARN` only.

---

## M4.1 Scope (explicit inclusions / exclusions)

### Included
- Semantic mass proxy (heuristic v0) computed from local statistics.
- Integration of mass term into the cost/energy evaluator.
- Executive thermostat adjusting global parameters only.
- Trace-level logging of mass and regulation inputs.

### Explicitly excluded
- Changes to κ semantics (M3 remains authoritative).
- Motif promotion or reuse (M5).
- Consolidation or decay application (M6).
- Any executive routing or path selection logic.

---

## M4.2 Core concepts

### Semantic mass
- **Definition:** A scalar proxy representing accumulated structure/density in regions of the manifold.
- **Sources (v0 heuristic):** local activation frequency, κ distribution summaries, optional valence aggregation.
- **Rule:** Mass influences motion cost but does not constitute learning.

### Executive thermostat
- **Definition:** A regulator that adjusts global parameters to maintain stability and exploration/exploitation balance.
- **Allowed controls:** temperature (τ), learning rate scalar (η), exploration bias (ζ).
- **Forbidden:** selecting nodes/edges, overriding traversal decisions, injecting content.

---

## M4.3 Engine interfaces (stable shapes)

### Mass computation API
- `compute_semantic_mass(state, local_context) -> MassReport`
- Must be deterministic for a given `(state snapshot, seed, local_context)`.

### Cost/Energy evaluator
- Extends the M1 evaluator to include a mass term.
- Per-step cost breakdown must include: κ term, mass term, lens term (if present), novelty term (if present).

### Executive thermostat API
- `update_parameters(metrics) -> ParameterDelta`
- Parameter deltas must be logged and bounded.

---

## M4.4 Artifacts (required)

Per interaction/run, emit:
- `mass_report.json` — mass values consulted during traversal.
- `executive_adjustments.json` — parameter deltas with reasons.
- Updated `trace.json` entries including per-step mass contribution.
- Standard artifacts: `interaction.json`, `metrics.json`, `delta_kappa.json` (unchanged from M3 semantics).

Artifacts must be sufficient to prove:
- Mass participated in motion.
- Executive adjusted parameters without routing.

---

## M4.5 Validation harness updates

### Packs
Create or extend packs:

1) `packs/mass_participation/`
- Queries traverse regions with different accumulated structure.
- Asserts mass term is non-zero and logged in traces.

2) `packs/executive_regulation/`
- Induces instability signals (e.g., oscillation, saturation).
- Asserts parameter adjustments occur and are logged.

### Assertions (minimum)
- `mass_term_logged == true`
- `executive_adjustments_logged == true`
- `no_routing_by_executive == true`
- `kappa_semantics_unchanged == true`

Implementation notes:
- Validation checks *participation and boundaries*, not numerical optimality.

---

## M4.6 Tests (must ship)

Unit tests:
- `test_mass_computation_deterministic()`
- `test_cost_includes_mass_term()`
- `test_executive_only_adjusts_parameters()`

Integration tests:
- `mm validate --pack packs/mass_participation ...` passes.
- `mm validate --pack packs/executive_regulation ...` passes.

---

## M4 “Done” definition

✅ A new developer can run:

```bash
python -m cli.main validate --pack packs/mass_participation --profile dev --seed 1 --state state.sqlite
python -m cli.main validate --pack packs/executive_regulation --profile dev --seed 1 --state state.sqlite
```

…and observe:
- Mass terms present in trace cost breakdowns.
- Executive parameter adjustments logged.
- No κ semantic changes beyond M3.
- Validation reports pass deterministically.

---

## Notes on forward compatibility

- Motifs (M5) may use mass as an eligibility signal but must not alter mass semantics.
- Consolidation (M6) may recompute mass offline but must remain reversible.
- Future refinements may change the mass heuristic but must preserve the contract defined here.

---

> **Principle:** The manifold has weight, but no will.

---
