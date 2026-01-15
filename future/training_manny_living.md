# Training Manny — Living Document (Beyond MVP)

Purpose: define how “training” works for Manny as curriculum/experience design (not model fitting), how to use LLMs safely, and how the plan evolves beyond MVP. This is non-canonical but aligned to v1.1.

---

## 1) Reframe “training” for Manny
- Manny is not trained by global loss/epochs; there is no convergence target.
- Training = shaping geometry through controlled experience: threads move → motion deposits curvature → curvature reshapes motion → stability emerges via repetition + decay.
- Think curriculum design, environment shaping, experience scheduling.

---

## 2) MVP: Safe, useful LLM involvement
Golden rule: **LLMs may propose structure; Manny must earn it.**

LLMs are excellent for:
- **Proposal generation:** candidate relations, paraphrases, varied examples → ingested as low-κ suggestions; only repeated traversal + confirmation increases κ.
- **Curriculum generation:** simple→complex, concrete→abstract, conflicting lenses, analogies, counterfactuals.
- **Variation/noise:** rephrases, near-misses, contradictions to form interference patterns.
- **Explanation rendering:** turn an existing trace into fluent language without influencing reasoning.

LLMs must never:
- Decide traversal paths, assign κ directly, resolve ambiguity, collapse conflicting basins, inject “correct answers” as authority, or create goals silently.

MVP training loop (safe pattern):
1) Seed lightly: small label set, weak initial edges.
2) LLM proposes experiences (facts/queries/contrasts/analogies/counterexamples) as scenarios, not truths.
3) Manny traverses: edge-backed, local energy/cost; traces logged.
4) Validate through repetition: reinforcement via repeats; conflicts create ridges; one-offs decay.
5) Consolidate: promotion, motif mining, decay. LLM involvement ends before traversal.

---

## 3) Beyond MVP: evolving the training plan
- **Promotion ladder:** enforce resonance/novelty gates for moving from primitives → concepts → motifs → episodes; log promotions/demotions.
- **Semantic mass-aware curriculum:** bias experience toward high-value/underexplored basins; use mass metrics to steer consolidation and exploration.
- **Ablations and paired runs:** motifs on/off, valence on/off, ANN brute vs. HNSW, LLM off vs. on (runtime reasoning) to prove causality.
- **Counterfactual practice:** use STAGE mode to simulate alternative paths; never write to canonical curvature without LEARN + validation.
- **Drive-aware scheduling (later):** adjust temperature/novelty based on executive signals; keep executive from routing paths.
- **Multi-modality (later):** apply ingestion contract—no opaque blobs; decompose artifacts; external encoders propose only.
- **Goal/agency tracks (non-canonical):** keep in research_tracks with promotion rules and validation gates before touching canon.

---

## 4) Operational safeguards (tie back to v1.1)
- RunModes: OBSERVE/STAGE cannot mutate κ/motifs/lenses; only LEARN can.
- Determinism: seed + snapshot discipline; state save/load/fork; reproducible packs.
- Artifacts: per interaction (interaction.json, trace.json, delta_kappa.json, metrics.json); per run (manifest, validation_report).
- Gates: path_coverage/why_fidelity, convergence, reuse/latency gain, κ bounds, runtime LLM budget.
- Provenance: changelog, contributors, license; tag canon releases.

---

## 5) Living updates
- Record changes to training practices here with date + rationale.
- If a change alters canon behavior, propose a new canon version; otherwise, keep as guidance aligned to v1.1.
