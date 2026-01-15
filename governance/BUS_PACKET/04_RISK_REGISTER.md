# Risk Register (Starter)

| Risk | Mitigation | Owner |
| --- | --- | --- |
| Runaway learning / κ explosion | κ clamps, per-interaction budget, variance checks; consolidation with rollback | TBD |
| Mode drift / hidden engines | RunModes enforced; single engine; no external planners; validation gates | TBD |
| LLM/ANN bypass | LLM propose-only; advisory ANN; logs capture proposals vs. chosen | TBD |
| Opaque knowledge | No blobs; ingestion must decompose artifacts into manifold-native structure | TBD |
| Loss of determinism | Seed + snapshot discipline; state save/load/fork; reproducible packs | TBD |
| Motif misuse | Only mined motifs; explicit reuse events; log reuse_count | TBD |
| Consolidation regression | Checkpoint + rollback; Gate D tests | TBD |
| Self-modification risk | STAGE + validation replay before commit; approval required | TBD |
