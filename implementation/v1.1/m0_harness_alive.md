# M0 — Harness Alive (Executable Checklist)

Objective  
Prove the validation spine exists before intelligence:
- `mm validate` runs deterministically
- artifacts are written
- a report is emitted
- exit codes work
- no hidden manual steps

**RunMode (M0):** All M0 executions run in `OBSERVE` mode by default. Any durable state mutation (κ, edges, motifs) during M0 is a harness or engine bug.

## M0.0 Implementation contract (what guides the build)

This M0 checklist is a *success-criteria and deliverables* view. The implementation must conform to the project’s canonical specs:

- **Foundations:** `canonical/foundations_v1.1.md` (non-negotiables: geometry-first, threads, Observation ≠ Learning)
- **Design contract:** `canonical/design_document_v1.1.md` (gates, invariants, CLI/API no drift, Virtual Stage constraints)
- **Architecture shape:** `canonical/architecture_v1.1.md` (authority boundaries, artifact discipline, run-modes)
- **Validation harness spec:** `canonical/validation/validation_framework.md` (pack format, report schema, exit codes)

Durable learning state is authoritative only in the centralized store; run artifacts are authoritative only for replay and validation.

M0 is considered correct only if the implemented code produces artifacts and reports that satisfy `canonical/validation/validation_framework.md`.

## M0.1 Repo scaffolding (minimum)

Create folders
- `core/`
- `cli/`
- `validation/`
- `packs/toy_m0/`
- `runs/` (gitignored)

Create files
- `cli/__init__.py`
- `cli/main.py`
- `core/engine.py`
- `validation/runner.py`
- `validation/report.py`
- `validation/pack_loader.py`
- `packs/toy_m0/pack.json`
- `packs/toy_m0/seed_graph.json`
- `packs/toy_m0/interactions.jsonl`
- `packs/toy_m0/assertions.json`

.gitignore
- add: `runs/`, `__pycache__/`, `.venv/`, `*.sqlite`

### Dependencies / frameworks (M0)

Required (keep minimal):
- Python 3.11+
- CLI framework: Typer (preferred) or Click
- Testing: pytest

Optional (may be deferred until REPL work in later milestones):
- Prompt Toolkit (REPL UX)

Hard rule: frameworks must not embed semantics. All reasoning/learning/state transitions live in `core/` and are invoked by `cli/` and `validation/`.

## M0.2 CLI command stub: `mm validate`

Requirement  
Running this should work from a clean checkout:  
`python -m cli.main validate --pack packs/toy_m0 --profile dev --seed 1 --state state.sqlite`

Exit codes
- 0 only if all required gates pass (won’t happen yet)
- 1 gates fail (expected for M0)
- 2 harness error (pack missing, artifact missing, nondeterminism)

For M0 we accept exit code 1, but not 2.

### CLI implementation requirements (M0)

- `cli/main.py` must expose a `validate` command that calls **only** `validation.runner.run_validation(...)`.
- CLI must not implement pack parsing, gate logic, or artifact writing; those belong to `validation/`.
- CLI must return the harness exit code unchanged.

## M0.3 Pack loader (deterministic)

Pack format (minimal)
- Pack loading must be deterministic and side-effect free; loading a pack must not mutate state.
- `pack.json` includes `{name, version, description}`
- `seed_graph.json` can be empty graph (still schema-valid)
- `interactions.jsonl` contains at least 1 interaction
- `assertions.json` contains thresholds (even if not met)

toy_m0 recommended contents
- `seed_graph.json`: empty nodes/edges
- `interactions.jsonl`: one query interaction with any input
- `assertions.json`: minimal invariants (path_coverage, why_fidelity) but allow failing

## M0.4 Engine stub: `run_interaction`

In `core/engine.py`, implement:

`run_interaction(state, runtime, interaction) -> EngineResult`

For M0:
- return a placeholder response (e.g., `"M0 stub"`)
- but write standard artifacts via the validation runner

No reasoning required yet.

### Core engine interface (M0)

Implement a stable interface even in stub form:

- `core/engine.py` exports `run_interaction(state, runtime, interaction) -> EngineResult`
- `EngineResult` must include at minimum: `response_text`, `trace`, `delta_report`, `interaction_metrics`

The engine must not compute gate outcomes, thresholds, or pass/fail status.

M0 allows placeholder values, but shapes must be consistent so later milestones do not refactor signatures.

## M0.5 Artifact writer (non-negotiable)

When `mm validate` runs, it must create:

Per run
- `runs/<run_id>/run_manifest.json`
- `runs/<run_id>/validation_report.json`
- `runs/<run_id>/validation_report.md`

Per interaction
- `runs/<run_id>/interactions/<interaction_id>/interaction.json`
- `runs/<run_id>/interactions/<interaction_id>/trace.json`
- `runs/<run_id>/interactions/<interaction_id>/delta_kappa.json`
- `runs/<run_id>/interactions/<interaction_id>/metrics.json`

Schema rules (M0)
- `trace.json` may be empty but must be valid JSON with: `trace_id`, `steps: []`
- `delta_kappa.json` may be empty but must include: `trace_id`, `deltas: []`
- `metrics.json` must include at least: `path_coverage` (0.0 or 1.0), `why_fidelity`, `errors: []|[...]`

## M0.6 Report generator (authoritative)

`validation_report.json` must include required top-level keys:
- `run_id`, `ts_utc`, `profile`, `seed`, `pack`, `state_hash`
- `gates` (A–D + efficiency) with pass/fail and reasons
- `metrics` (at least the basics)
- `failures` (empty unless harness error)
- `links` (paths to key artifacts)

For M0:
- Gates will fail because reasoning isn’t implemented.
- That is fine; the report must still be produced.

### Report/gate computation ownership

- Gate computation and report writing are owned by `validation/` (not CLI, not core).
- The core engine emits per-interaction artifacts; the harness computes gates from artifacts.
- This separation prevents interface drift and preserves scientific falsifiability.

## M0.7 Determinism check (M0 proof)

Run twice:
1. `mm validate ... --seed 1`
2. `mm validate ... --seed 1`

Pass if:
- both runs succeed in generating artifacts + reports
- run manifests are consistent in structure
- outputs are stable except for run_id and timestamps

Optional but recommended:
- include `runtime_hash` in manifest to prove inputs are identical

## M0 “Done” definition

✅ A new developer can do:

```
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt   # or pip install -e .
python -m cli.main validate --pack packs/toy_m0 --profile dev --seed 1 --state state.sqlite
```

…and it always produces a run folder with artifacts + reports.

No manual steps. No hidden setup. No editing required.

If `mm validate` exits with code `2` (harness error), M0 is not complete regardless of produced artifacts.
