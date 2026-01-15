

# Training Track — Responsibilities, Observation, and Validation

**Status:** Supporting document (non-canonical)  
**Purpose:** Define how training supports each implementation milestone (v1.1 plan) without becoming a milestone itself.

---

## Core Principle
Training shapes geometry through controlled experience. Milestones assert emergent behaviors; training provides the conditions under which those behaviors can appear. Training is continuous, bounded by invariants, and audited.

## Always-on training responsibilities
- Learning only via traversal + local plasticity (no external planners/rule engines).
- Observation ≠ Learning enforced (RunModes; plasticity unreachable in OBSERVE/STAGE).
- Determinism: seed + snapshot discipline; trace logging/replayable runs.
- Decay and consolidation enabled; one-offs decay.
- LLMs/ANNs propose only; never decide traversal or κ.

---

## Milestone-aligned training map (v1.1)

### M0 — Harness alive
- Claim: validation spine exists.  
- Training role: minimal; ensure packs run deterministically and artifacts are written.  
- Signals: gate computation runs; deterministic outputs with fixed seed.

### M1 — Motion law + trace fidelity (Gate A)
- Claim: reasoning is traversal with explicit cost; `/why` replays.  
- Training role: small seeded graph; varied queries to exercise traversal; ensure traces include cost components.  
- Signals: path_coverage=1.0; why_fidelity=1.0.

### M1.5 — Primitives + compression scaffolding
- Claim: representation spine declared (primitives, ladder states, hooks).  
- Training role: seed primitives deterministically; exercise promotion/decay hooks in OBSERVE (no κ writes).  
- Signals: hooks emit logs/artifacts; no durable geometry change in OBSERVE.

### M2 — RunModes + Virtual Stage isolation
- Claim: write barriers enforced.  
- Training role: OBSERVE packs assert no κ change; STAGE packs assert no durable writes without commit.  
- Signals: plasticity unreachable in OBSERVE; STAGE buffer only commits via gate.

### M3 — Plasticity (Learning-as-curvature) (Gate B)
- Claim: Δκ local, bounded, persistent; affects traversal; only in LEARN.  
- Training role: repeat tasks to show local/directional convergence; ensure clamps/budgets.  
- Signals: delta_kappa_persists; convergence meets threshold; κ bounds/variance stable.

### M4 — Semantic mass + executive thermostat
- Claim: mass/density is logged and influences cost; executive adjusts {τ, η, ζ} only.  
- Training role: supply traffic to create/cohere basins; vary novelty; observe mass term in traces.  
- Signals: traces log mass term; executive adjustments logged; no path routing by executive.

### M5 — Motifs as promotion (Gate C)
- Claim: motifs mined from repetition; reuse is explicit and beneficial.  
- Training role: provide repeatable procedural sequences; analogy cases (e.g., apple→pear); avoid hand-authoring motifs.  
- Signals: reuse_event_pct and latency/step gain meet thresholds; reuse_count > 0 with trace markers.

### M6 — Consolidation + rollback (Gate D)
- Claim: offline work improves structure, reversible, maintains ladder.  
- Training role: run sleep with checkpoints; stress packs; verify rollback; ensure decay/promotion bookkeeping.  
- Signals: mastered paths preserved; rollback works; κ variance bounded under stress.

### M7 — Service mode (optional)
- Claim: API wraps engine with zero semantic drift.  
- Training role: ensure same packs/runs via CLI vs API yield identical traces/metrics with same seed/state.  
- Signals: equivalence between surfaces.

---

## Observation guide
- Log: trace.json, delta_kappa.json, metrics, semantic mass proxy, motif events, run_manifest, validation_report.
- Review: compare early vs late traces for cost reduction/reuse; confirm no learning in OBSERVE/STAGE.
- Do not use: benchmark accuracy/ground-truth scores as primary signals.

## LLM/ANN usage during training
- Allowed: propose experiences, curriculum variations, render explanations from existing traces.
- Forbidden: decide traversal, assign κ, collapse ambiguity, create goals.


## Red flags (stop/inspect)
- Behavioral improvement without trace change.
- κ changes without traversal or in OBSERVE/STAGE.
- LLM/ANN output influences decisions directly.
- Motifs appear without repetition or benefit.

## Training → Pack Promotion Rule (how exploratory training becomes replayable proof input)

**Purpose:** Convert exploratory training sessions into deterministic dataset packs without contaminating learning physics.

### Core rule
- **Training runs are never proof.** Only **packs** executed via `mm validate` can contribute to gate outcomes.
- Promotion is **observation-only**: exporting a training session into a pack must not mutate κ, edges, motifs, or any durable state.

### When a training session is eligible for promotion
A session may be promoted only if all are true:
1. **Artifact completeness:** required artifacts exist for every turn (`interaction.json`, `trace.json`, `metrics.json`, `delta_kappa.json`), plus `run_manifest.json`.
2. **Replayability:** the session can be replayed from the same `(snapshot/state_hash, seed, profile)` to produce identical traces in `OBSERVE`.
3. **RunMode integrity:** no write-barrier violations occurred; `delta_kappa.json` is empty for OBSERVE/STAGE turns.
4. **Signal value:** the session contains one of:
   - a reproducible convergence pattern (local/directional),
   - an explicit reuse event candidate (repeatable subpath),
   - a staged commit candidate (STAGE deltas worth reviewing),
   - a stability/rollback stress sequence.

### Promotion process (deterministic)
1. **Select window:** choose a contiguous slice of turns that demonstrates a single phenomenon (avoid mixing goals).
2. **Normalize inputs:** convert each turn into `interactions.jsonl` entries (`query` / `teach_correct` / `review_valence`).
3. **Freeze seed + state:** record the starting `state_hash` and seed in `pack.json` and embed the initial structure in `seed_graph.json`.
4. **Write assertions:** create `assertions.json` capturing only the measurable expectations (e.g., `why_fidelity`, `reuse_events_min`, `convergence_pct`).
5. **Dry-run verify:** run `mm validate --pack <new_pack>` in `OBSERVE` to confirm determinism and artifact completeness.
6. **Register:** assign pack name + version; store `notes.md` with provenance links to the original run.

### What is forbidden during promotion
- Editing the pack to "make it pass" without updating the underlying phenomenon.
- Including unreplayable human-only steps that cannot be represented as scripted interactions.
- Using free-form LLM output as ground truth; LLM outputs may only appear as proposals or user text inputs.

### Promotion outputs (minimum)
A promoted pack must contain:
- `pack.json`, `seed_graph.json`, `interactions.jsonl`, `assertions.json`
- optional `ablations.json` (recommended when the pack supports causality tests)

**Result:** the promoted pack becomes a permanent, replayable proof input that can be used in CI and regression tests.

## Reminder
If a milestone can be passed without training, it isn’t a Manny milestone. Training exists to shape geometry; milestones prove emergent behaviors under the invariants.
