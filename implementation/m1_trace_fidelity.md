# M1 — Trace Fidelity (Gate A)

Objective  
Implement real thread traversal and exact `/why` replay so Gate A becomes mechanically verifiable. M1 proves: reasoning is motion (every answer has a trace) and transparency is physics (`/why` replays execution, not narrative).

M1 explicitly excludes learning and plasticity; all κ values are treated as read-only placeholders. Any κ mutation in M1 is a bug.

## M1.0 Implementation contract
Must conform to canon:
- `foundations.md`: threads-as-thought, no hidden planners, explanation == path.
- `design_document.md`: Gate A (path coverage + `/why` fidelity).
- `architecture.md`: core engine is source of truth; interfaces are transport only.
- `validation/validation_framework.md`: pack format, artifacts, report schema.

Success = `mm validate` computes and passes Gate A:
- `path_coverage == 1.0`
- `why_fidelity == 1.0`

## M1.1 Repo scaffolding (minimum)
- Folders: `core/threading/`, `core/explain/`, `tests/unit/`, `packs/toy_m1/` (or extend `packs/toy_m0/`).
- Files: `core/threading/thread_runner.py`, `core/threading/trace_types.py`, `core/explain/why_replay.py`, `validation/gates.py` (if not present).

## M1.2 Dependencies
- Required: Python 3.11+, pytest.
- Optional: networkx (only if it simplifies toy traversal; must remain replaceable).
- Hard rule: no framework introduces semantics; traversal + trace stay in `core/`.

## M1.3 Core behavior
Thread runner:
- Deterministic trace given seed + state.
- Traversal must be deterministic for a given `(state snapshot, runtime profile, seed)` triple.
- Traversal is edge-backed (neighbors from explicit edges).
- No LLM calls in M1.

`/why` fidelity:
- `/why <trace_id>` loads `trace.json` and renders exact ordered steps, same node/edge IDs, same reasons/metadata.
- `/why` must not re-run traversal, consult ANN/LLM, or read live graph state beyond the stored `trace.json`.

## M1.4 Minimal trace schema (contract)
`trace.json` must include at minimum:
- `trace_id`
- `interaction_id`
- `seed`
- `start_nodes` (list)
- `goal_nodes` (list; may be empty)
- `steps`: ordered list of:
  - `step_index`
  - `node_id`
  - `edge_id` (nullable for step 0)
  - `reason` (e.g., start, edge_follow, terminate)
  - `cost` (number; can be 0 in M1)
  - `kappa_used` (number; can be 0 in M1)

Later milestones may add fields but must not break these.

Additional constraints:
- `trace_id` and `interaction_id` must be globally unique per run.
- `step_index` must be contiguous and zero-based.
- Replaying steps in order must be sufficient to reconstruct the path without consulting live state.

## M1.5 CLI surfaces
- `mm run "<prompt>"` → executes one interaction and prints `{response_text, trace_id}`.
- `mm why <trace_id>` → prints replay from stored trace.
- CLI must call only core engine + explain functions; no traversal logic in CLI.

## M1.6 Validation harness updates
Pack: create `packs/toy_m1/` (or evolve `toy_m0`) with:
- `seed_graph.json` containing a tiny graph with at least one non-trivial path.
- `interactions.jsonl` with 2–3 queries.

Assertions (`assertions.json`):
- `path_coverage: 1.0`
- `why_fidelity: 1.0`

Gate computation (`validation/gates.py`):
- `path_coverage = (# interactions with stored trace_id and trace file) / (total interactions)`.
- `why_fidelity = 1.0` only if replay payload equals trace content. Easiest: have `why_replay.py` emit a machine-readable replay object and compare to `trace.json`.

Gate computation is owned exclusively by the validation harness. The core engine must not compute, assume, or short-circuit gate outcomes.

## M1.7 Tests (must ship)
- Unit: `test_trace_schema_min_fields()`, `test_thread_runner_deterministic_given_seed()`, `test_why_replay_matches_trace_exactly()`.
- Integration: `mm validate --pack packs/toy_m1 ...` returns exit code 0 (Gate A passes).

## M1 “Done” definition
✅ New dev can run:
```
python -m cli.main validate --pack packs/toy_m1 --profile dev --seed 1 --state state.sqlite
```
and gets:
- exit code 0 for Gate A,
- artifacts per interaction including real `trace.json`,
- `/why` fidelity proven by automated checks.

If Gate A fails, the milestone is not complete even if manual inspection appears correct.

## Deferred intentionally (not in M1)
- Plasticity / Δκ updates
- Motifs and reuse
- Lenses and switching
- Consolidation
- ANN / embeddings

M1 is about making traversal and transparency real and verifiable.
