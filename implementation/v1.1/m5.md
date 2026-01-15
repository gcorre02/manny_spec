

# M5 — Motifs as Promotion (Gate C)

## Objective
Introduce **motifs as promoted structure** in the compression ladder (L3), and prove that transfer occurs through **explicit reuse events**, not overlap or caching.

M5 proves:
- Motifs are **emergent promotions** derived from repeated traffic.
- Reuse is an explicit, logged event (`reuse_count > 0`).
- Motifs accelerate traversal and improve efficiency without altering κ semantics.

---

## M5.0 Implementation contract (what guides the build)

M5 must conform to canonical specs:
- **Foundations v1.1:** motifs are emergent procedural memory; reuse must be observable.
- **Design v1.1:** motifs are L3 promotions; overlap ≠ reuse.
- **Architecture v1.1:** core engine owns promotion; artifacts prove reuse.
- **Validation framework:** Gate C computed from artifacts; efficiency gains required.

**RunMode (M5):** Motif detection may occur in any mode; motif **commit** is allowed only in `LEARN` and must respect write barriers.

---

## M5.1 Scope (explicit inclusions / exclusions)

### Included
- Motif candidate detection from repeated traces.
- Motif commit as L3 promotion.
- Explicit reuse during traversal with logged events.
- Efficiency measurement (latency / step reduction).

### Explicitly excluded
- Manual motif authoring.
- Global pattern mining unrelated to traversal.
- Any learning outside κ semantics.

---

## M5.2 Core concepts

### Motif
- **Definition:** A reusable procedural subpath promoted from repeated traffic.
- **Layer:** L3 in the compression ladder.
- **Rule:** Motifs do not replace geometry; they compress it.

### Reuse event
- **Definition:** A traversal step that explicitly invokes a motif.
- **Requirement:** Must be logged in `trace.json` with a `reuse_event` marker.

---

## M5.3 Engine interfaces (stable shapes)

### Motif detection API
- `observe_motif_candidate(trace) -> None`
- Accumulates frequency/benefit signals only.

### Motif commit API
- `commit_motif(candidate_id) -> motif_id`
- Allowed only in `LEARN` mode.

### Traversal reuse
- Traversal may select a motif when beneficial.
- Reuse must emit `reuse_event` with `{motif_id, step_range}`.

---

## M5.4 Artifacts (required)

Per run/interaction, emit:
- `motifs.json` — committed motifs and metadata.
- `motif_candidates.json` — observed candidates and signals.
- Updated `trace.json` entries with `reuse_event` markers.
- Standard artifacts: `interaction.json`, `metrics.json`, `delta_kappa.json`.

Artifacts must be sufficient to prove:
- Motifs emerged from traffic.
- Reuse occurred explicitly.
- κ semantics remain unchanged.

---

## M5.5 Validation harness updates (Gate C)

### Packs
Create or extend packs:

1) `packs/transfer_analogy/`
- Learn on a base task.
- Run an analogical task.
- Assert explicit motif reuse.

### Assertions (minimum)
- `reuse_event_pct >= threshold`
- `reuse_events_logged == true`
- `motif_latency_gain_pct >= threshold`
- `overlap_without_reuse_not_counted == true`

---

## M5.6 Tests (must ship)

Unit tests:
- `test_motif_detected_from_repeated_traces()`
- `test_motif_commit_only_in_learn_mode()`
- `test_reuse_event_logged_in_trace()`

Integration tests:
- `mm validate --pack packs/transfer_analogy ...` passes Gate C.

---

## M5 “Done” definition

✅ A new developer can run:

```bash
python -m cli.main validate --pack packs/transfer_analogy --profile dev --seed 1 --state state.sqlite
```

…and observe:
- Motifs committed only after sufficient traffic.
- Explicit reuse events in traces.
- Efficiency gains measured and reported.
- Gate C passes deterministically.

---

## Notes on forward compatibility

- Consolidation (M6) may refine motif storage but must preserve reuse semantics.
- Motifs may influence semantic mass eligibility but not κ semantics.
- Future motif heuristics must respect explicit reuse logging.

---

> **Principle:** Reuse must be visible, or it does not exist.

---