# Manny Manifolds — Implementation Milestones (Atomic Slices)

Purpose: implement Manny in vertical slices where every milestone ends with a runnable system, a passing subset of validation, and archived artifacts. Specific per-milestone task files will be added later; this is the scaffold.

## Rules for atomic progress
1) One new capability per slice.  
2) Each slice must include code, tests, at least one validation pack update (or new pack), and a passing `mm validate` subset.  
3) Each slice produces new run artifacts: `run_manifest.json`, `validation_report.json` (and `.md` summary).  
4) No interface drift: CLI and API call the same engine from day 1 (API may be stubbed).

5) Each milestone must be runnable by a new developer using only the repository, the dataset packs, and the `mm validate` command (no undocumented setup or manual steps).

## Milestones (M0 → M7)
Validation entrypoint: Gates and artifact contracts are defined in `canonical/validation/validation_framework.md`.

### M0 — Skeleton that can validate itself (harness alive)
Goal: prove harness + artifact plumbing exist before intelligence.
Deliverables: stub `run_interaction(...)` writes `interaction.json`, `trace.json` (schema-valid, can be empty), `delta_kappa.json` (empty), `metrics.json`; `mm validate` runs on a tiny pack and emits `run_manifest.json`, `validation_report.json/.md`.
Pass: gate computation executes; deterministic output with fixed seed.

### M1 — Motion law + trace fidelity (Gate A core)
Goal: reasoning is traversal governed by an explicit, logged cost/energy evaluator and can replay exactly.
Deliverables: minimal graph seeded (e.g., `seed_graph.json`); thread runner produces real `trace.json` **with per-step cost components** (distance/κ/mass/lens/novelty terms as available); `/why` replays trace (no re-execution); CLI `mm run`, `mm why`.
Pass: `path_coverage == 1.0`; `why_fidelity == 1.0` (fidelity means replay of stored motion-law inputs and step ordering, not narrative).

### M1.5 — Primitives + compression scaffolding (representation spine)
Goal: lock the unified manifold representation spine early so later learning/motifs/lenses are promotions, not bolt-ons.
Deliverables: Primitive Registry (versioned primitive types); compression ladder states (L0–L4) declared in code; promotion/decay hooks exist as stubs (eligibility counters + logs), without requiring actual promotion yet. M1.5 must not mutate κ; promotion/decay hooks are observational only in this milestone.
Pass: packs can seed primitives deterministically; promotion/decay hooks emit artifacts/logs without mutating durable geometry in OBSERVE mode.

### M2 — RunModes + Virtual Stage isolation (Observation ≠ Learning in code)
Goal: enforce write barriers so observation, diagnostics, and staging cannot mutate durable learning state.
Deliverables: engine RunModes (`OBSERVE | LEARN | STAGE`) enforced; plasticity unreachable in OBSERVE; STAGE writes buffered and committed only via explicit gated apply; stage commit produces explicit commit artifact.
Pass: OBSERVE pack asserts no κ mutation; STAGE pack asserts no durable mutation without explicit commit.

### M3 — Plasticity (Learning-as-curvature) in LEARN only (Gate B)
Goal: Δκ is local, bounded, persistent, and changes traversal; learning only possible in LEARN mode.
Deliverables: plasticity applier with local Δκ on traversed edges, clamp + per-interaction budget; populated `delta_kappa.json`; repeat tasks show task-local / directional convergence.
Pass: `delta_kappa_persists == true`; convergence meets pack threshold; κ bounds/variance stable on small runs.

### M4 — Semantic mass proxy + executive thermostat (free-fall dynamics)
Goal: make semantic mass/density a first-order, logged signal in traversal and regulation.
Deliverables: mass proxy computed (activation/traffic + κ/valence summaries); cost/energy evaluator includes mass term (logged in trace); executive thermostat adjusts only {τ, η, ζ} based on stability signals (never routes). Mass proxy is heuristic v0 at this stage; validation asserts participation/logging, not perfect correctness.
Pass: traces show mass term participation; thermostat adjustments are logged and do not alter path semantics beyond parameter changes.

### M5 — Motifs as promotion (Gate C)
Goal: motifs are L3 promotions from repeated traffic; transfer is a logged reuse event, not overlap.
Deliverables: motif candidate detection (repeat traces + benefit); motif commit store; motif reuse with `reuse_count > 0` and trace segments marked as reuse; analogy pack (e.g., apple → pear) yields explicit reuse event and efficiency gain.
Pass: `reuse_event_pct >= threshold`; motif latency/step gain ≥ threshold.

### M6 — Consolidation + rollback (Gate D)
Goal: offline work improves structure, remains reversible, and maintains the compression ladder (decay/promotion maintenance).
Deliverables: `mm sleep` checkpoints state, prunes/normalizes, maintains decay/promotion bookkeeping, (optional) mines motifs, rebuilds indices, performs atomic swap + rollback; rollback test in harness.
Pass: consolidation preserves mastered paths; rollback works; κ variance bounded under stress pack.

### M7 — Service mode (optional, later)
Goal: API server wraps engine with zero semantic drift.
Deliverables: FastAPI calling same core engine functions; CLI still runs in-process by default; optional CLI client mode to hit API.
Pass: same interaction via CLI vs API → identical trace/metrics given same seed/state snapshot.

## Folder layout (implementation scope)
- `core/` — engine modules (`engine.py`, `threading/`, `cost/`, `plasticity/`, `motifs/`, `lenses/`, `primitives/`, `stage/`, `executive/`, `state/` for SQLite + migrations).
- `cli/` — thin adapter only.
- `validation/` — harness (`mm validate`), pack loader, gate calculators, report writer.
- `packs/` — `toy_cooking/`, `facts_updates/`, `stress_pathology/` (grow as needed).
- `runs/` — artifacts (gitignored).

## Checkpoint ritual (end of each milestone)
1) Run: `mm validate --pack ...`  
2) Save: `runs/<run_id>/validation_report.json` (and `.md`), plus `run_manifest.json`.  
3) Commit + tag (e.g., `m0-harness-alive`, `m1-trace-fidelity`, `m3-learning-curvature`, …).  
4) Optionally store one canonical “golden run” artifact directory per milestone.
