# Manny Manifolds — Scientific Proof Validation (Implementation Spec)

Purpose: implement a falsifiable, repeatable validation process that turns Manny’s core hypotheses into executable suites and an authoritative `validation_report.json`. A green report is the definition of “Manny is functioning.”

This framework is subordinate to Canonical v1.1; it operationalizes, but does not redefine, the physics or contract. Validation harness and packs are executed in `implementation/v1.1/` (see milestone docs and packs referenced there).

Scope: this document specifies what must be implemented in code (CLI + harness + pack format + report schema + CI rule). It intentionally avoids tuning constants and domain content beyond small seed packs.

## 1) Hypotheses → Gates
- H1 Learning exists: Δκ changes persist and matter.
- H2 Reasoning is path-based: every answer has a trace; `/why` is faithful.
- H3 Transfer exists: explicit reuse events on analogies yield efficiency.
- H4 Stability: κ stays bounded; consolidation doesn’t erase mastery.
- H5 LLM is a lens: runtime reasoning remains manifold-driven (LLM usage bounded).

## 2) What is automated vs manual

### Automated (required)
- The suites (Gates A–D + Efficiency) are **implemented in the validation harness** and run via `mm validate`. They are not manual checklists.
- Each suite produces machine-computable metrics and pass/fail gate outcomes.
- The harness writes standard run artifacts and an authoritative `validation_report.json`.


### Manual (optional)
- Manual exploration (e.g., ad-hoc CLI sessions, inspection of `/why`, spot-checking plots) is allowed, but **never substitutes** for gates.
- Manual steps may be documented as “investigation playbooks,” but the source of truth remains the report.

## 2.1 User-in-the-loop validation (structured, repeatable)

Manny’s hypotheses explicitly include the effect of *user interaction* (queries, corrections, valence). Validation must therefore include **structured user-in-the-loop runs** that are repeatable and auditable.

User interaction is not treated as noise; it is a first-class input to learning and must be modeled explicitly.

### What is validated
- How user queries drive traversal paths.
- How user corrections reshape geometry (Δκ persistence).
- How valence influences learning *intensity* without selecting outcomes.
- That observation-only interactions do not mutate learning state.

### How it is implemented
- User runs are captured as scripted interaction sequences in `interactions.jsonl`.
- Each step corresponds to an explicit user action (`query`, `teach_correct`, `review_valence`).
- Interactive CLI sessions may be recorded and exported into `interactions.jsonl` for replay.

### What is forbidden
- Unlogged, free-form user interaction influencing learning.
- Validation that depends on live, unreplayable human input.

The validation harness treats user interaction scripts exactly like automated runs, producing the same artifacts and gate computations.

## 3) Implementation requirements

### 3.1 `mm validate` command (contract)
`mm validate` is the single entry point for proof validation.

Required arguments:
- `--pack <path>` (dataset pack folder)
- `--profile <name>` (runtime profile)
- `--seed <int>` (deterministic seed)
- `--state <path>` (SQLite DB file / state location)

Optional arguments:
- `--out <path>` (override output directory)
- `--ablations <name|all>` (run paired ablations)
- `--ann <auto|brute|hnsw>` (force ANN strategy for the run)
- `--llm <on|off>` (force runtime reasoning LLM usage)

Exit codes:
- `0` = all required gates passed
- `1` = one or more gates failed
- `2` = harness error (invalid pack, missing artifacts, nondeterminism)

### Step-by-step user run process (reference)
1. Start from a known state snapshot.
2. Execute a scripted user interaction sequence (`interactions.jsonl`).
3. Capture traversal, Δκ, and metrics after each step.
4. Optionally apply consolidation (`mm sleep`) if specified by the pack.
5. Compute gates and assertions.
6. Emit `validation_report.json` and artifacts.

This process mirrors an interactive user session but is fully deterministic and replayable.

### 3.2 Run registration (pre-register)
Each validation run must write a `run_manifest.json` before execution and never mutate it.

`run_manifest.json` must include:
- `run_id` (time + hash)
- runtime profile name + hash
- seed
- state snapshot/hash
- pack name + version
- feature toggles (LLM on/off, ANN mode, merge policy)
- code version (git commit hash if available)

### 3.3 Dataset pack format (deterministic)
A dataset pack is a folder with versioned, deterministic inputs.

**Pack authoring note:** Dataset packs are intentionally small, explicit, and human-auditable. They are hypotheses about learning behavior, not benchmarks. Packs should prefer clarity and falsifiability over realism or scale.

Required files:
- `pack.json` (metadata: name, version, description)
- `seed_graph.json` (nodes/edges/primers sufficient to start)
- `interactions.jsonl` (scripted interaction sequence)
- `assertions.json` (gate thresholds and expected events)

Optional files:
- `notes.md` (human description)
- `ablations.json` (paired-run definitions)

`interactions.jsonl` schema (minimum):
- `id`
- `type` one of: `query` | `teach_correct` | `review_valence`
- `input` (text)
- `valence` (optional)
- `expect` (optional references to assertion IDs)

### 3.4 Standard artifacts (must be written)
For each run:
- `runs/<run_id>/run_manifest.json`
- `runs/<run_id>/validation_report.json`
- `runs/<run_id>/validation_report.md`

For each interaction:
- `runs/<run_id>/interactions/<interaction_id>/interaction.json`
- `runs/<run_id>/interactions/<interaction_id>/trace.json`
- `runs/<run_id>/interactions/<interaction_id>/delta_kappa.json`
- `runs/<run_id>/interactions/<interaction_id>/metrics.json`

Artifacts are the source of truth for **what happened** in the run.

### 3.5 `validation_report.json` (authoritative schema)
Top-level keys (required):
- `run_id`, `ts_utc`, `profile`, `seed`, `pack`, `state_hash`
- `gates` (A–D + Efficiency): pass/fail + reasons
- `metrics` (computed): path coverage, /why fidelity, Δκ persistence, κ bounds/variance, convergence (task-local), reuse events %, motif latency gain, LLM token %, active-region scaling signals
- `ablations` (if run): per-ablation gate deltas + summary
- `failures` (if any): invariant violations, missing artifacts, nondeterminism flags
- `links` (paths to key artifacts)

### 3.5.1 Minimal examples (copy/paste)

`validation_report.json` (minimal skeleton):
```json
{
  "run_id": "2026-01-14T19-00-00Z_ab12cd",
  "ts_utc": "2026-01-14T19:00:00Z",
  "profile": {"name": "profileB", "hash": "…"},
  "seed": 123,
  "pack": {"name": "toy_cooking", "version": "0.1"},
  "state_hash": "…",
  "gates": {
    "A_foundations": {"pass": true, "reasons": []},
    "B_learning": {"pass": true, "reasons": []},
    "C_transfer": {"pass": false, "reasons": ["reuse_event_pct below threshold"]},
    "D_stability": {"pass": true, "reasons": []},
    "efficiency": {"pass": true, "reasons": []}
  },
  "metrics": {
    "path_coverage": 1.0,
    "why_fidelity": 1.0,
    "delta_kappa_persists": true,
    "kappa_bounds_ok": true,
    "convergence_pct": 0.23,
    "reuse_event_pct": 0.18,
    "motif_latency_gain_pct": 0.11,
    "kappa_variance": 0.42,
    "runtime_llm_token_pct": 0.03,
    "active_region_scaling_ok": true
  },
  "ablations": [],
  "failures": [],
  "links": {
    "run_manifest": "runs/<run_id>/run_manifest.json",
    "report_md": "runs/<run_id>/validation_report.md",
    "artifacts_root": "runs/<run_id>/"
  }
}
```

`interactions.jsonl` (one line example):
```json
{"id":"q001","type":"query","input":"How do I make an apple tart?","valence":null,"expect":["A.path_coverage","B.convergence"]}
```

`assertions.json` (minimal schema + example):

Schema (keys expected by the harness):
- `thresholds`: numeric thresholds for gates
- `expect_events`: expected explicit events (e.g., reuse events) and minimum counts
- `invariants`: required boolean checks

Example:
```json
{
  "thresholds": {
    "convergence_pct": 0.20,
    "reuse_event_pct": 0.30,
    "motif_latency_gain_pct": 0.15,
    "runtime_llm_token_pct": 0.05,
    "kappa_variance_max": 1.00
  },
  "expect_events": {
    "reuse_events_min": 1,
    "motif_hits_min": 1
  },
  "invariants": {
    "path_coverage": 1.0,
    "why_fidelity": 1.0,
    "delta_kappa_persists": true,
    "kappa_bounds_ok": true
  }
}
```

`seed_graph.json` (minimal example):
```json
{
  "nodes": [
    {"id": "apple", "label": "apple"},
    {"id": "pear", "label": "pear"},
    {"id": "tart", "label": "tart"},
    {"id": "bake", "label": "bake"}
  ],
  "edges": [
    {"src": "apple", "tgt": "tart", "weight": 0.10},
    {"src": "pear", "tgt": "tart", "weight": 0.05},
    {"src": "tart", "tgt": "bake", "weight": 0.20}
  ],
  "notes": "Minimal seed graph for toy_cooking pack. Weights are initial hints only; learning occurs via traversal and κ updates."
}
```

### 3.6 Gate computation (must be automated)
- Gate A (Foundations):
  - `path_coverage == 1.0`
  - `why_fidelity == 1.0`
  - `delta_kappa_persists == true`
  - `kappa_bounds_ok == true`
- Gate B (Learning):
  - `convergence_pct >= threshold` (task-local / directional)
  - `no_catastrophic_regression == true`
- Gate C (Transfer):
  - `reuse_event_pct >= threshold` (explicit events)
  - `motif_latency_gain_pct >= threshold`
- Gate D (Stability):
  - `kappa_variance <= threshold`
  - `consolidation_preserves_mastery == true`
  - `rollback_ok == true` (if consolidation applied)
- Efficiency:
  - `runtime_llm_token_pct <= threshold`
  - `active_region_scaling_ok == true` (heuristic)

### 3.7 Ablations (paired runs for causality)
Ablations are implemented as paired runs with one toggle changed.

Required ablations (Phase A/B):
- motifs on/off
- valence on/off
- consolidation on/off
- ANN brute vs HNSW (where available)
- LLM runtime on/off

Ablation outputs must be summarized in `validation_report.json` under `ablations` and must not change the baseline run artifacts.

## 4) Falsification tests (must be implemented)
- Learning is geometry: repeat a task → shorter path; zero out/remove highest-κ edges → performance drops. If removal doesn’t hurt, learning isn’t in geometry.
- `/why` is faithful: `/why` exactly replays the executed trace; re-run with same seed/state yields same trace. Otherwise, it’s storytelling.
- Reuse is real: transfer suite shows `reuse_count > 0` events (not just overlap) plus efficiency gain.

These tests are automated checks integrated into the validation harness and produce gate pass/fail outcomes.

## 5) MVP checklist (deliverables)

Must ship:
1. `mm validate` command implementing Gates A–D + Efficiency.
2. Dataset packs:
   - `packs/toy_cooking` (repeat + analogy)
   - `packs/facts_updates` (contradiction/correction persistence)
   - `packs/stress_pathology` (noise + conflicting valence + consolidation)
3. Deterministic run artifacts and `validation_report.json` schema.
4. CI rule: merges are blocked unless `mm validate` returns exit code 0 and the report is attached.

Optional (later):
- HTML dashboard renderer for reports
- Elastic/Kibana indexing for large artifact corpora (derived only)
