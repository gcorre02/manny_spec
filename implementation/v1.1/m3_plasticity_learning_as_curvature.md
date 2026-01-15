 
# M3 — Plasticity: Learning as Curvature (Gate B)

## Objective
Introduce **durable learning** as local, bounded curvature updates, and prove that learning exists *only* as geometry that changes future motion.

M3 proves:
- Learning occurs **only** in `LEARN` mode.
- Learning is expressed as **local Δκ on traversed edges**.
- Learning persists across runs and measurably alters traversal.
- Removing learned curvature degrades performance (falsification).

---

## M3.0 Implementation contract (what guides the build)

M3 must conform to canonical specs:
- **Foundations v1.1:** learning is curvature; locality; bounded change; transparency via replay.
- **Design v1.1:** LEARN-only mutation; explicit reuse semantics; task-local convergence.
- **Architecture v1.1:** core engine owns mutation; DB is durable truth; artifacts are run truth.
- **Validation framework:** Gate B computed from artifacts; ablations required for causality.

**RunMode (M3):** Durable mutation is permitted **only** when `RunMode == LEARN`. Any mutation in `OBSERVE` or `STAGE` is a bug.

---

## M3.1 Scope (explicit inclusions / exclusions)

### Included
- Plasticity engine applying Δκ to traversed edges.
- Bounded, local updates with clamps and per-interaction budgets.
- Persistence of κ across sessions.
- Validation packs proving convergence and falsification.

### Explicitly excluded
- Motif promotion or reuse (M5).
- Semantic mass influence (M4 comes later).
- Consolidation/decay application (M6).

---

## M3.2 Core concepts

### Curvature (κ)
- **Definition:** Scalar weight on edges that biases traversal cost.
- **Rule:** κ is the *only* durable learning signal.
- **Constraint:** κ updates must be local to edges actually traversed.

### Plasticity
- **Definition:** Mapping from experience (trace + valence) to Δκ.
- **Constraint:** Plasticity may not inspect global graph state beyond the traversed path.

---

## M3.3 Engine interfaces (stable shapes)

### Plasticity API
- `compute_delta_kappa(trace, signals) -> DeltaReport`
- `apply_delta_kappa(delta_report) -> None`

Rules:
- `compute_delta_kappa` must be pure (no mutation).
- `apply_delta_kappa` must enforce RunMode == `LEARN`.
- Δκ must be clamped and logged per edge.

### Signals
- Minimum signals available in M3:
  - traversal frequency (implicit)
  - optional valence marker (if present in interaction)

---

## M3.4 Artifacts (required)

Per interaction/run, emit:
- `delta_kappa.json` — list of `{edge_id, delta_kappa, reason, clamp_applied}`
- Updated durable state in DB (edges table κ values)
- Standard artifacts: `interaction.json`, `trace.json`, `metrics.json`

Artifacts must be sufficient to:
- Prove which edges changed.
- Prove updates were local and bounded.

---

## M3.5 Validation harness updates (Gate B)

### Packs
Create or extend packs:

1) `packs/learning_convergence/`
- Repeatedly query the same task.
- Assert task-local convergence (shorter paths / lower cost).

2) `packs/learning_falsification/`
- Learn on a task.
- Remove or zero out the highest-κ edges involved.
- Re-run the task and assert degraded performance.

### Assertions (minimum)
- `delta_kappa_persists == true`
- `convergence_pct >= threshold` (task-local / directional)
- `kappa_bounds_ok == true`
- `learning_only_in_learn_mode == true`

---

## M3.6 Tests (must ship)

Unit tests:
- `test_delta_kappa_local_only()`
- `test_kappa_clamped_and_bounded()`
- `test_no_mutation_outside_learn_mode()`

Integration tests:
- `mm validate --pack packs/learning_convergence ...` passes Gate B.
- `mm validate --pack packs/learning_falsification ...` demonstrates degradation when κ removed.

---

## M3 “Done” definition

✅ A new developer can run:

```bash
python -m cli.main validate --pack packs/learning_convergence --profile dev --seed 1 --state state.sqlite
python -m cli.main validate --pack packs/learning_falsification --profile dev --seed 1 --state state.sqlite
```

…and observe:
- κ changes only in LEARN mode.
- κ persists across runs.
- Traversal improves with learning and degrades when curvature is removed.
- Gate B passes deterministically.

---

## Notes on forward compatibility

- Semantic mass (M4) will add additional terms to cost but must not alter κ semantics.
- Motifs (M5) will consume κ/traffic to promote reusable structure.
- Consolidation (M6) may apply decay to κ but must respect bounds and rollback.

---

> **Principle:** If learning does not change geometry, it does not exist.

---