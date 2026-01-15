# M2 — RunModes & Virtual Stage Isolation

## Objective
Make **Observation ≠ Learning** a physical property of the system by enforcing write barriers.

M2 proves:
- `OBSERVE` cannot mutate durable geometry.
- `STAGE` can simulate/buffer changes but cannot commit without an explicit gated apply.
- `LEARN` is the only mode where durable learning (κ/edges/motifs/promotions) may occur.

---

## M2.0 Implementation contract (what guides the build)

M2 must conform to canonical specs:
- **Foundations v1.1:** learning is curvature; transparency is replay; Observation ≠ Learning.
- **Design v1.1:** RunModes, Virtual Stage, no interface drift, artifact authority.
- **Architecture v1.1:** core engine is source of truth; artifacts are run truth; DB is durable truth.
- **Validation framework:** packs are deterministic; gates computed from artifacts.

**Priority rule:** If a mutation can occur in `OBSERVE`, M2 has failed.

---

## M2.1 Scope (explicit inclusions / exclusions)

### Included
- RunMode enum and enforcement in the core engine: `OBSERVE | STAGE | LEARN`.
- Write barrier that prevents durable state mutation outside `LEARN`.
- Virtual Stage buffering for `STAGE` with explicit commit artifact.
- Validation packs proving non-mutation and gated commit semantics.

### Explicitly excluded
- Plasticity implementation details (M3).
- Motif promotion logic (M5) beyond commit gating.
- Consolidation (M6).

---

## M2.2 Core concepts

### Durable geometry
- Canonical learning state stored in SQLite (nodes/edges/κ/motifs/lenses/primitives).

### Observation artifacts
- Run artifacts are authoritative for replay and validation, but not durable truth.

### Virtual Stage
- A protected, non-learning context that may accumulate *proposed* deltas.
- Stage deltas are buffered and must not affect durable geometry unless explicitly committed.

---

## M2.3 Engine interfaces (stable shapes)

### RunMode
- `RunMode.OBSERVE`: traversal + metrics + artifacts only.
- `RunMode.STAGE`: traversal + metrics + buffered deltas; produces `stage_delta.json`.
- `RunMode.LEARN`: traversal + metrics + durable mutation (plasticity/promotion allowed).

RunMode must be recorded in:
- `run_manifest.json`
- each `interaction.json`

### Stage buffering API
- `stage_begin(run_id, interaction_id) -> stage_id`
- `stage_record_delta(stage_id, delta) -> None`
- `stage_commit(stage_id, reason) -> commit_id` (allowed only via explicit gated apply)
- `stage_discard(stage_id, reason) -> None`

Notes:
- In M2, stage deltas may be empty placeholders; semantics matter more than magnitude.

### Write barrier
- Any attempt to call durable mutators while RunMode != `LEARN` must raise an error and be recorded in `failures`.
- Such violations must fail the run deterministically; silent blocking or best-effort suppression is forbidden.
- The barrier must cover:
  - κ updates
  - edge creation/deletion
  - motif commit
  - promotion/decay apply

---

## M2.4 Artifacts (required)

Per run:
- `run_manifest.json` includes `run_mode_default` and per-pack overrides.
- `validation_report.json/.md`

Per interaction:
- `interaction.json` includes `run_mode`.
- `trace.json` (as per M1)
- `metrics.json`
- `delta_kappa.json`:
  - OBSERVE: must exist, `deltas: []`
  - STAGE: must exist, `deltas: []` or placeholders
  - LEARN: may be populated later (M3)

Stage-specific:
- `stage_delta.json` (STAGE only): buffered proposed deltas
- `stage_commit.json` (only if commit occurs): commit metadata + references to applied deltas

---

## M2.5 Validation harness updates

### Packs
Create or extend packs:

1) `packs/observe_no_mutation/`
- Runs the same queries twice in OBSERVE.
- Asserts no durable mutation and deterministic traces.

2) `packs/stage_no_commit_no_mutation/`
- Runs STAGE interactions that generate buffered deltas.
- Does **not** perform a commit.
- Asserts no durable mutation.

3) `packs/stage_commit_gated/`
- Runs STAGE interactions.
- Executes an explicit gated commit step (harness-controlled).
- Asserts durable mutation occurs only after commit artifact.

### Assertions (minimum)
- `no_kappa_mutation_in_observe == true`
- `no_durable_mutation_without_commit == true`
- `stage_commit_requires_explicit_artifact == true`
- `run_mode_recorded_everywhere == true`

Implementation notes:
- Durable mutation checks compare `state_hash_before` vs `state_hash_after` for relevant tables.
- `stage_commit.json` must include references to the stage deltas applied.

---

## M2.6 Tests (must ship)

Unit tests:
- `test_write_barrier_blocks_mutation_in_observe()`
- `test_stage_buffers_without_mutating_durable_state()`
- `test_commit_requires_explicit_commit_artifact()`

Integration tests:
- `mm validate --pack packs/observe_no_mutation ...` passes.
- `mm validate --pack packs/stage_no_commit_no_mutation ...` passes.
- `mm validate --pack packs/stage_commit_gated ...` passes.

---

## M2 “Done” definition

✅ A new developer can run:

```bash
python -m cli.main validate --pack packs/observe_no_mutation --profile dev --seed 1 --state state.sqlite
python -m cli.main validate --pack packs/stage_no_commit_no_mutation --profile dev --seed 1 --state state.sqlite
python -m cli.main validate --pack packs/stage_commit_gated --profile dev --seed 1 --state state.sqlite
```

…and observe:
- OBSERVE runs never mutate durable geometry.
- STAGE runs never mutate durable geometry without explicit commit.
- Commits require `stage_commit.json` and occur only through gated apply.
- Validation reports reflect pass/fail deterministically.

---

## Notes on forward compatibility

- Plasticity (M3) will populate Δκ in `LEARN` only.
- Motifs/promotions (M5) must respect the same write barriers.
- Consolidation (M6) must be treated as a privileged durable mutation operation with rollback.

---

> **Principle:** If observation can change memory, the system cannot be trusted.

---