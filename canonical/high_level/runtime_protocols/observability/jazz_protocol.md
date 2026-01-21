# Jazz Session Protocol

**Status:** Candidate / Non-canonical (training & observation aid)  
**Purpose:** Define a safe, exploratory conversational protocol that allows Manny to engage in open-ended interaction (“jazz”) while preserving trace fidelity, explainability, and architectural invariants.

---

## 1. Core Intent

A Jazz Session is **not a task, benchmark, or training milestone**. It is an *observational environment* designed to:
- expose emergent basins and interference patterns
- reveal traversal styles (direct/flexible, bound/free)
- surface motifs and decay behavior
- make Manny’s stream of thought legible via traces

Conversation is treated as **experience**, not instruction.

---

## 2. Safety & Invariants (Non-negotiable)

- **RunMode:** OBSERVE by default (LEARN only if explicitly enabled)
- **No goals:** No objective functions, success criteria, or rewards
- **No planners:** Traversal remains local free-fall under the standard cost law
- **LLM role:** Stimulus generator only; never decides traversal or content
- **Determinism:** Snapshot + seed required; replay must be possible
- **Run manifest:** Each session must write a `run_manifest.json` capturing snapshot/state hash, seed, runtime profile, runmode, and tool toggles (LLM on/off, ANN mode).

---

## 3. Roles & Separation

### Manny
- Receives conversational turns as experiences
- Runs internal traversal and selects a response trace
- Produces a *narrated response* derived from the trace
- Logs all paths, costs, and frontier dynamics

### LLM (Conversation Partner)
- Generates conversational stimuli (prompts, reflections, topic shifts)
- May paraphrase Manny’s selected trace *after* selection
- Has no access to internal state beyond the chosen response text

---

## 4. Session Parameters (Configurable)

- **τ (temperature):** exploration bias (default: slightly elevated)
- **Frontier breadth (k):** neighborhood size
- **Lens weighting:** optional (e.g., associative, reflective)
- **Valence bias:** neutral by default
- **Turn limit:** soft cap (e.g., 20–50 turns)

Parameters bias motion; they do not set modes.

---

## 5. Jazz Loop (Canonical)

```
LLM proposes conversational move
→ Manny ingests as experience
→ Manny runs local traversal
→ Manny selects a response trace
→ Manny produces narrated response
→ Log trace + metrics
→ Repeat
```

No step may be skipped.

---

## 5.1 Required artifacts (per turn and per session)

A Jazz Session is only valid if it emits the standard artifacts so it can be replayed and compared.

Per session (run root):
- `run_manifest.json`
- `session_summary.json` (turn count, parameters, runmode, notes)
- `validation_report.json` (optional; if run through `mm validate`)

Per turn:
- `interaction.json` (input, runmode, ids)
- `trace.json` (ordered steps + per-step cost components)
- `metrics.json` (frontier stats, basin revisits, novelty)
- `delta_kappa.json` (must exist; empty in OBSERVE)

If any of these are missing, the session is not considered analyzable.

---

## 5.2 Recording to packs (how sessions become replayable training/observation data)

Jazz Sessions can be exported into a deterministic pack form:
- Each turn becomes a line in `interactions.jsonl` (`query`/`teach_correct`/`review_valence` as appropriate).
- The session’s initial snapshot and seed become `seed_graph.json` + pack metadata.

Rule: exporting a session must be observation-only. Converting a session into a pack must not mutate geometry.

---

## 6. Narrated Manny Responses (Definition)

> **Detailed specification:** See `canonical/candidate/jazz/jazz_narration_spec_v1_1.md` for comprehensive narration constraints and invariants.

A *narrated response* is **not free text generation**. It is a constrained rendering of Manny’s chosen trace.

### 6.1 Required Properties

- **Trace-grounded:** Every sentence maps to nodes/edges in the trace
- **Reflective, not authoritative:** Expresses association and motion, not certainty
- **Open-ended:** Invites continuation rather than closure
- **Minimal:** Avoids verbosity; favors fragments over exposition

### 6.2 Allowed Forms

- Associations ("This connects to…")
- Reflections ("I’m noticing a pull toward…")
- Questions ("That makes me wonder about…")
- Metaphors (if present in trace)

### 6.3 Disallowed Forms

- Definitive answers
- Claims of truth or correctness
- Hidden reasoning or unexplained jumps
- Content not present in the trace

### 6.4 Trace → sentence mapping (required)

To preserve trace fidelity, the narration layer must provide a lightweight mapping:
- Each sentence (or fragment) references one or more `trace.steps[*]` ids.
- If a sentence cannot be mapped to the trace, it must be omitted.

This mapping can live in `metrics.json` or a separate `narration_map.json`.

---

## 7. Example Narrated Responses

### Example A — Association

> “When you mention *apple*, my attention slides toward food and eating. There’s a smaller pull toward stories I’ve seen before, but it doesn’t hold as strongly.”

*(Trace shows strong food basin, weak narrative ridge.)*

---

### Example B — Lateral Jump

> “This thought branches unexpectedly toward *promise* — not because they’re similar, but because both sit near ideas of trust and expectation.”

*(Trace shows stochastic hop between adjacent basins.)*

---

### Example C — Bound Motion

> “I keep circling the same idea here. It’s hard to move past it without effort.”

*(Trace shows repeated localized traversal.)*

---

## 8. What to Observe During a Session

- Frontier expansion vs collapse
- Basin revisitation frequency
- Motif candidates
- Cost reduction over turns
- Noise activation and decay
- Valence propagation patterns
- RunMode compliance (OBSERVE: `delta_kappa.json` is empty; STAGE: any deltas remain buffered and must not reach the canonical manifold without an explicit commit gate)

These observations are descriptive, not evaluative.

---

## 9. Replay & Comparison

Jazz Sessions should be replayable:

- Same snapshot + seed → same motion
- Alter τ or lens → observe change in style
- Compare two sessions to study interference dynamics

Replay is the primary analysis tool.

Comparison is done by diffing artifacts, not by subjective transcript review. Minimum comparisons:
- same snapshot+seed, same τ → identical trace
- same snapshot+seed, τ changed → different trace with preserved invariants
- OBSERVE vs LEARN (only if enabled) → κ deltas appear only in LEARN

---

## 10. Termination Conditions

End a session if:
- Traces stop changing meaningfully
- Basin collapse or runaway repetition occurs
- Curiosity is exhausted (human-in-the-loop)
- Any write-barrier violation occurs (any mutation in OBSERVE, or any canonical write in STAGE without an explicit commit gate)

There is no notion of success or failure.

---

## 11. Explicit Non-Goals

- Demonstrating intelligence
- Optimizing conversational quality
- Passing benchmarks
- Training to convergence

---

## 12. Summary

A Jazz Session is a **listening instrument**. It allows Manny to move freely within constraints while we observe how meaning emerges, interferes, and stabilizes.

> Conversation is not how Manny proves it can think.  
> Conversation is how we watch Manny think.
