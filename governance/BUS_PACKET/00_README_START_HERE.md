# BUS PACKET — START HERE

Note: this repository is **documentation-only**. Any `mm ...` commands and `packs/` paths referenced here are expected to exist in the runtime/implementation repository, not in this folder.
AI assistant guidance: `AGENTS.md` (Codex) and `.cursorrules` (Cursor).

What Manny is (v1.1): a unified geometric mind where **knowledge is geometry, reasoning is motion, learning is curvature**. All cognition is edge-backed traversal; learning is Δκ; transparency is intrinsic (`/why` replays the trace).

What Manny is not:
- No planners/rule engines outside the manifold
- No opaque blobs as knowledge
- No silent learning (observation ≠ learning)
- No interface drift (CLI/API call the same engine)
- No hidden LLM/ANN planners (LLM/ANN propose-only)

Canon lives in `canonical/`:
- `canonical/foundations_v1.1.md` — invariants + provisional primitives addendum
- `canonical/design_document_v1.1.md` — normative contract (definitions, gates, RunModes, promotion/ingestion)
- `canonical/architecture_v1.1.md` — module/flow spec (motion law, run-modes, ingest contract)
- `canonical/validation/validation_framework.md` — hypothesis-driven validation
- High-level refs in `canonical/high_level/`

How to prove the MVP (Gate A..D):
- Run `mm validate --pack <pack> --profile dev --seed 1 --state state.sqlite`
- Produce artifacts: `trace.json`, `delta_kappa.json`, `metrics.json`, `validation_report.json`
- Gates: path_coverage=1.0; why_fidelity=1.0; convergence≥20% (local); reuse≥30% + latency gain≥15%; κ bounded; runtime LLM budgeted
