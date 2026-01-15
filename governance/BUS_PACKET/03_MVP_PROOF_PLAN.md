# MVP Proof Plan

Definition of “Manny works” (v1.1):
- **Explainability:** path coverage = 1.0; `/why` fidelity = 1.0 (Gate A).
- **Convergence:** ≥20% mean path-length reduction on repeat tasks (local/directional) (Gate B).
- **Transfer / Motif utility:** ≥30% reuse events on analogies; ≥15% latency/step gain on motif hits (Gate C).
- **Stability:** κ bounded; consolidation preserves mastered paths; rollback works (Gate D).
- **Efficiency:** runtime scales with active k-hop region; runtime LLM ≤5% (reasoning only).

Required artifacts per run:
- `runs/<run_id>/run_manifest.json`
- `validation_report.json` + `.md`
- Per interaction: `interaction.json`, `trace.json`, `delta_kappa.json`, `metrics.json`

Command:
```
mm validate --pack <pack> --profile dev --seed 1 --state state.sqlite
```

Packs (MVP examples):
- `packs/toy_m1/` for Gate A
- `packs/toy_m2/` for persistence determinism
- `packs/toy_m3/` for convergence (Δκ)
- `packs/toy_m4/` for motif reuse
- `packs/toy_m5/` for consolidation/rollback
