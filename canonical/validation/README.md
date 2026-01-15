# Validation (Canonical)

- Entry point: `validation_framework.md`
- Purpose: hypothesis-driven validation (Gate A–D + efficiency), packs, report schema, artifacts.
- Artifacts per run: `run_manifest.json`, `validation_report.json/.md`; per interaction: `interaction.json`, `trace.json`, `delta_kappa.json`, `metrics.json`.
- Packs: `packs/` (toy_cooking, facts_updates, stress_pathology, etc.) defined in implementation scope; validation harness consumes packs only.
- Reports: include run_id, ts_utc, profile, seed, pack, state_hash, gates, metrics, failures, links.
- Automation vs manual: gates/computation automated; manual exploration is non-authoritative.
