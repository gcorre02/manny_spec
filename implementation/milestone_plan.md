# Manny Manifolds — Implementation Milestones (Atomic Slices)

Purpose: implement Manny in vertical slices where every milestone ends with a runnable system, a passing subset of validation, and archived artifacts. Specific per-milestone task files will be added later; this is the scaffold.

## Rules for atomic progress
1) One new capability per slice.  
2) Each slice must include code, tests, at least one validation pack update (or new pack), and a passing `mm validate` subset.  
3) Each slice produces new run artifacts: `run_manifest.json`, `validation_report.json` (and `.md` summary).  
4) No interface drift: CLI and API call the same engine from day 1 (API may be stubbed).

5) Each milestone must be runnable by a new developer using only the repository, the dataset packs, and the `mm validate` command (no undocumented setup or manual steps).

## Milestones (M0 → M6)

### M0 — Skeleton that can validate itself (harness alive)
Goal: prove harness + artifact plumbing exist before intelligence.  
Deliverables: stub `run_interaction(...)` writes `interaction.json`, `trace.json` (schema-valid, can be empty), `delta_kappa.json` (empty), `metrics.json`; `mm validate` runs on a tiny pack and emits `run_manifest.json`, `validation_report.json/.md`.  
Pass: gate computation executes; deterministic output with fixed seed.

### M1 — Thread runner + trace fidelity (Gate A core)
Goal: reasoning is traversal and can replay.  
Deliverables: minimal graph seeded (e.g., `seed_graph.json`); thread runner produces real `trace.json`; `/why` replays trace; CLI `mm run`, `mm why`.  
Pass: `path_coverage == 1.0`; `why_fidelity == 1.0`.

### M2 — Persistence + snapshot/fork discipline
Goal: state is durable, inspectable, replayable.  
Deliverables: SQLite store with schema version; `mm state save|load|fork` (diff optional); atomic writes for state + session artifacts.  
Pass: same input/seed/snapshot → identical trace; forked states diverge cleanly (no cross-contamination).

### M3 — Plasticity (Learning-as-curvature) (Gate B)
Goal: Δκ is local, bounded, persistent, and changes traversal.  
Deliverables: plasticity applier with local Δκ on traversed edges, clamp + per-interaction budget; populated `delta_kappa.json`; repeat tasks show task-local convergence.  
Pass: `delta_kappa_persists == true`; local convergence meets pack threshold; κ bounds/variance stable on small runs.

### M4 — Motifs + explicit reuse events (Gate C)
Goal: transfer is a logged reuse event, not overlap.  
Deliverables: motif candidate detection (repeat traces), motif commit store, motif reuse with `reuse_count > 0` and trace segments marked as reuse; pack (e.g., apple → pear) yields reuse event.  
Pass: `reuse_event_pct >= threshold`; motif latency/step gain ≥ threshold.

### M5 — Consolidation + rollback (Gate D)
Goal: offline work improves structure, is reversible.  
Deliverables: `mm sleep` checkpoints state, prunes/normalizes, (optional) mines motifs, rebuilds indices, performs atomic swap + rollback; rollback test in harness.  
Pass: consolidation preserves mastered paths; rollback works; κ variance bounded under stress pack.

### M6 — Service mode (optional, later)
Goal: API server wraps engine with zero semantic drift.  
Deliverables: FastAPI calling same engine functions; CLI still runs in-process; optional CLI client mode to hit API.  
Pass: same interaction via CLI vs API → identical trace/metrics given same seed/state snapshot.

## Folder layout (implementation scope)
- `core/` — engine modules (`engine.py`, `threading/`, `plasticity/`, `motifs/`, `lenses/`, `state/` for SQLite + migrations).
- `cli/` — thin adapter only.
- `validation/` — harness (`mm validate`), pack loader, gate calculators, report writer.
- `packs/` — `toy_cooking/`, `facts_updates/`, `stress_pathology/` (grow as needed).
- `runs/` — artifacts (gitignored).

## Checkpoint ritual (end of each milestone)
1) Run: `mm validate --pack ...`  
2) Save: `runs/<run_id>/validation_report.json` (and `.md`), plus `run_manifest.json`.  
3) Commit + tag (e.g., `m0-harness-alive`, `m1-trace-fidelity`, `m3-learning-curvature`, …).  
4) Optionally store one canonical “golden run” artifact directory per milestone.
