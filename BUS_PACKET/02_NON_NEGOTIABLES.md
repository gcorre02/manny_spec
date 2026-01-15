# Non-Negotiables (Drift Killers)

- No planners or rule engines outside manifold traversal.
- No opaque blobs as knowledge; artifacts are references only.
- Observation ≠ Learning: RunMode `OBSERVE/STAGE` cannot mutate κ/motifs/lenses; only `LEARN` can.
- Edge-backed traversal only; every answer has a trace; `/why` replays execution (no narrative rewrites).
- LLM/ANN propose-only: they may suggest primitives/edges but never route or decide learning.
- Single engine of truth; derived indices are advisory only.
- Determinism: seed + snapshot → reproducible traces; state save/load/fork required.
- Reuse is explicit (reuse_count > 0); overlap alone is not reuse.
- Convergence is local/directional; no global ASPL promises.
