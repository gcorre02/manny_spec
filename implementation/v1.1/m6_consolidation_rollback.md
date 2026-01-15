

# M6 — Consolidation & Rollback (Gate D)

## Objective
Introduce **offline consolidation** as a privileged, reversible maintenance phase that improves structure without breaking learned behavior.

M6 proves:
- Consolidation is **offline-only** and never required for hot-path correctness.
- Consolidation is **reversible** (checkpoint → apply → atomic swap → rollback).
- Consolidation maintains stability (bounded κ variance) and preserves mastered paths.
- Consolidation maintains the compression ladder (decay/promotion bookkeeping).

---

## M6.0 Implementation contract (what guides the build)

M6 must conform to canonical specs:
- **Foundations v1.1:** dual dynamics (online motion vs offline maintenance); stability without freezing learning.
- **Design v1.1:** consolidation never alters learning physics beyond bounded maintenance; rollback is mandatory.
- **Architecture v1.1:** DB is durable truth; artifacts are run truth; no interface drift; derived indices are advisory.
- **Validation framework:** Gate D computed from artifacts; rollback must be tested.

**RunMode (M6):** Consolidation is a privileged durable mutation operation executed explicitly via `mm sleep` (or harness-controlled step). It must never run implicitly during `mm run`.

---

## M6.1 Scope (explicit inclusions / exclusions)

### Included
- Checkpointing durable state before consolidation.
- Offline consolidation pipeline:
  - prune low-signal edges
  - normalize κ distribution
  - apply decay/promotion bookkeeping (ladder maintenance)
  - (optional) motif mining refinement
  - rebuild derived indices (ANN)
- Atomic swap of consolidated state.
- Rollback to last checkpoint.

### Explicitly excluded
- Any hot-path global recomputation.
- Any consolidation that cannot be rolled back.
- Any consolidation that mutates state without emitting artifacts.

---

## M6.2 Core concepts

### Checkpoint
- A durable snapshot of the DB (or export) taken immediately before consolidation.
- Must be uniquely identified and referenced in run artifacts.

### Atomic swap
- Consolidation writes to a new state (e.g., new DB file or transactionally swapped tables) and swaps only on success.

### Rollback
- Revert to the checkpointed state deterministically.
- Rollback must be verifiable by state hash equality with the checkpoint.

---

## M6.3 Engine interfaces (stable shapes)

### Consolidation API
- `checkpoint_state(reason) -> checkpoint_id`
- `consolidate(checkpoint_id, policy) -> new_state_id`
- `atomic_swap(new_state_id) -> active_state_id`
- `rollback(checkpoint_id) -> active_state_id`

Constraints:
- Consolidation must be idempotent given the same checkpoint + seed + policy.
- Consolidation must not consult live interfaces for semantics; it operates on durable truth.

---

## M6.4 Artifacts (required)

Per consolidation run, emit:
- `sleep_manifest.json` — includes checkpoint_id, policy hash, seed, and timing.
- `sleep_diff.json` — summary of changes (counts, thresholds applied).
- `sleep_report.md` — human-readable summary with provenance.
- `rollback_report.json` (if rollback executed) — includes before/after hashes.

Durable references:
- checkpoint reference (file path or DB snapshot id)
- new state reference
- active state reference after swap

Validation artifacts must be sufficient to prove:
- consolidation ran offline and explicitly
- consolidation was reversible
- key behaviors preserved

---

## M6.5 Validation harness updates (Gate D)

### Packs
Create or extend packs:

1) `packs/consolidation_stability/`
- Run learning to establish mastered paths.
- Run `mm sleep`.
- Re-run mastered queries.
- Assert no catastrophic regression and κ bounds/variance constraints.

2) `packs/consolidation_rollback/`
- Run learning to establish mastered paths.
- Run `mm sleep`.
- Execute rollback.
- Assert state hash equals checkpoint and mastered queries still pass.

### Assertions (minimum)
- `consolidation_offline_only == true`
- `rollback_ok == true`
- `consolidation_preserves_mastery == true`
- `kappa_variance <= threshold`
- `compression_ladder_maintained == true`

Implementation notes:
- Mastery preservation should be checked via path/cost similarity and correctness of known packs.
- Derived indices (ANN) must be rebuildable; failures must not corrupt durable truth.

---

## M6.6 Tests (must ship)

Unit tests:
- `test_checkpoint_created_before_consolidation()`
- `test_atomic_swap_only_on_success()`
- `test_rollback_restores_exact_state_hash()`

Integration tests:
- `mm validate --pack packs/consolidation_stability ...` passes Gate D.
- `mm validate --pack packs/consolidation_rollback ...` passes rollback assertions.

---

## M6 “Done” definition

✅ A new developer can run:

```bash
python -m cli.main validate --pack packs/consolidation_stability --profile dev --seed 1 --state state.sqlite
python -m cli.main validate --pack packs/consolidation_rollback --profile dev --seed 1 --state state.sqlite
```

…and observe:
- Consolidation never runs implicitly.
- Consolidation emits checkpoint + diff + report artifacts.
- Rollback restores the exact state.
- Gate D passes deterministically.

---

## Notes on forward compatibility

- Consolidation may later incorporate richer motif mining and ladder maintenance, but must preserve rollback semantics.
- Any future distributed/service deployment must preserve atomic swap and rollback guarantees.

---

> **Principle:** Sleep is maintenance, not memory invention.

---