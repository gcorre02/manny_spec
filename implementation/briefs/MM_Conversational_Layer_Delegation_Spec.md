# Manny Manifolds — Conversational Layer (MVP + Lateral Thinking)
**Phase 7.5/8 Delegation Spec (High‑Level, Implementation‑Agnostic)**

> **Purpose:** Equip a dedicated implementation agent with a clear, contract‑first blueprint to deliver the **offline, hot‑swappable conversational layer** in two complementary streams — (1) a **Reflective Narrator** that explains what the manifold learned, and (2) an **Elicitor/Story‑Capture** stream that invites user stories/metaphors and stores them as structured, geometry‑ready artifacts for later reprocessing. The design is **agnostic to the underlying model/tooling**, aligns with MM’s scientific goals (learning‑as‑curvature, transfer, explainability), and plugs into existing project surfaces without changing core learning physics.

- **Alignment with Vision & Design Laws:** Conversational outputs are built from/into the geometry (paths, Δκ, motifs, lenses) with **provenance and determinism**; they remain **offline**, preserving MM’s locality, stability, and dual‑dynamics principles.
- **Integration Scope:** Uses existing MM surfaces for paths (`runner/message.py`, `/why`), lenses (`io/lens_io.py`, `handlers/lens.py`), motifs (`orchestration/mining.py`, `handlers/motifs.py`), nightly diffs/plots/heatmaps (`orchestration/nightly.py`, `handlers/plots.py`) and the **Phase 7.5 hot‑swap gravity/lens exporters**. No modifications to `core/plasticity.py` or learning loops.

---

## 1) Objectives & Outcomes

### 1.1 Reflective Narrator (System → Human)
**Goal:** Convert the day’s learned geometry into faithful, concise narratives with **provenance** (what changed, where κ deepened, which motifs/lenses activated, why paths shortened).  
**Value:** Makes “understanding as curvature” observable and reviewable; supports Phase 7/7.5/8 validation and demos.  
**Artifacts:** `narrative.json` (structured, provenance) + `narrative.txt` (human‑readable).

### 1.2 Elicitor / Story‑Capture (Human → System)
**Goal:** Elicit **explanations, metaphors, and alternative perspectives** around a focused subject to surface lateral associations and candidate structures (paths/motifs/lenses) for future reprocessing.  
**Value:** Broadens semantic coverage, seeds **candidate motifs/mind‑map edges** without touching hot‑path learning; supports transfer and later research.  
**Artifacts:** `story_<id>.json` **StoryPack** (structured) + `story_<id>.txt` (user narrative).

### 1.3 Non‑Goals (for this phase)
- No changes to core plasticity/update rules (`core/plasticity.py`) — learning on κ remains **as is** (Phase 8 needs stability).
- No real‑time updates to κ/edges from conversations; all story‑derived structures are queued for offline review.
- No complex UI; dashboard adds a simple Narratives/Stories panel linking to generated files (full interactive viewers remain Phase 8/9+).

---

## 2) High‑Level Architecture

```
            (nightly artifacts)                         (user prompts)
      ┌──────────────────────────┐                  ┌──────────────────┐
      │  paths / why.json        │                  │   user input      │
      │  Δκ diffs / motif logs   │                  │   (explain, etc.) │
      │  lens exports / heatmaps │                  └─────────┬────────┘
      └───────┬──────────────────┘                            │
              │                                                │
              ▼                                                ▼
      [ArtifactCollector]                               [Elicitor]
              │                                           │
              ▼                                           ▼
      [Interpreter]  →  deltas: key concepts,             [StoryPack]
       (top Δκ, reuse, motif changes, lens              (structured; queued)
       coverage, path selections)
              │
              ▼
      [NarratorAdapter] (rules/llm) → narrative.txt + narrative.json (w/ provenance)
              │
              ▼
      [QA & Persist] → save to `logs/viz/narratives/…` & emit telemetry
```

- **Collector:** Reads existing JSON/CSV/PNG refs (no mutation) — `orchestration/nightly.py`, `handlers/plots.py`, `handlers/map_why.py` surfaces.
- **Interpreter:** Deterministic summarization of geometry (top deltas, trends, lens coverage).
- **Adapter layer:** Strategy pattern for generation (rules/LLM); identical I/O signature; can **hot‑swap** implementations without touching the pipeline.
- **Elicitor:** Prompt presets (explain/metaphor/perspective/story); stores responses as **StoryPack** for future reprocessing.

---

## 3) Interfaces & Contracts (stable)

### 3.1 Artifact Bundle → Interpretation
```python
@dataclass
class ArtifactBundle:
    paths: Dict[str, Any]          # from handlers/map_why.py (why playback JSON)
    diffs: Dict[str, Any]          # nightly Δκ summaries
    motifs: List[Dict[str, Any]]   # mined motifs & reuse stats
    lenses: Dict[str, Any]         # lens exports, coverage metrics
    gravity: Dict[str, Any]        # gravity JSON meta (strategy, overlap, monotonicity)

@dataclass
class Interpretation:
    highlights: List[Dict]         # e.g., {"type":"delta","id":"motif_123","delta_k":0.42,...}
    transfer: Dict                 # {reuse_pct, convergence, top_bridge_edges, …}
    lens_summary: Dict             # {coverage, new_tags, overlaps}
    why_slices: List[Dict]         # curated steps with ΔE/Δκ and lens tags
    viz_refs: Dict                 # links to PNGs/JSONs for provenance
```

### 3.2 NarratorAdapter (strategy pattern)
```python
class NarratorAdapter(Protocol):
    def generate(self, interpretation: Interpretation, *, style: str, max_len: int, seed: int) -> Tuple[str, Dict]:
        """Return (text, meta). Must not mutate inputs. meta includes {adapter, seed, token_usage?, warnings}."""
```
- **RulesAdapter**: Jinja/static templates; fully deterministic; ≤2 s on standard bundle.
- **LLMAdapter**: Optional; same signature; strict token/time budgets; must pass the same QA gate.

### 3.3 StoryPack Schema (v1)
```json
{
  "id": "story_2025-10-25T23-59-59Z_abcdef",
  "source": {"user_id":"…","session_id":"…","ts":"…"},
  "text": {"raw":"…","clean":"…"},
  "extraction": {
    "concepts":[{"label":"…","confidence":0.92,"hint_domain":"…"}],
    "relations":[{"src":"…","tgt":"…","type":"causes|part_of|analogy","evidence":[[start,end]]}],
    "sequence":["A","→","B","→","C"],
    "lenses":[{"tag":"process","score":0.81}],
    "candidate_motifs":[{"path":["…","…","…"],"score":0.73}],
    "mindmap_edges":[{"src":"…","tgt":"…","weight":0.2,"note":"from story"}]
  },
  "provenance": {"inputs":["why.json","nightly_diff.json"],"adapter":"rules","deterministic":true,"seed":0},
  "queue": {"status":"pending","priority":"normal"}
}
```
**Important:** Story‑derived edges/motifs are **proposals** only; no κ mutation in this phase.

### 3.4 CLI & Runtime Keys
- `/converse [--style operator|explainer] [--adapter rules|llm] [--report-out PATH]`  
  **Reads:** `why.json`, nightly diffs, lens/motif exports. **Writes:** `logs/viz/narratives/…`
- `/story --mode {elicit|metaphor|perspective|stitch} [--adapter …] [--out PATH]`
- `runtime.json` keys (TTL‑reloaded):
```json
{
  "conversation": {"enabled": true, "adapter": "rules", "style": "operator", "seed": 0,
    "max_len": 800, "run_on_nightly": false },
  "gravity_equation": "log_monotone", "shadow_gravity_equation": null,
  "gravity_params": {"tile_size":64,"ema_window":3,"beta_rep":0.1}
}
```
*(Gravity strategy keys shown for context; handled by 7.5 nightly exporter; not used to compute narratives, only to read viz meta.)*

---

## 4) Acceptance Criteria & QA (for delegation)

### 4.1 Reflective Narrator
- **Determinism:** Given identical artifacts, `RulesAdapter` yields bit‑identical `narrative.json/.txt` in ≤ 2s.
- **Provenance ≥ 95%:** Each claim in `narrative.json` references at least one input offset/id; the QA stage rejects ungrounded sentences.
- **Explainability:** `/why` step‑alignment = 100% (every referenced step exists in the input playback); include Δκ/ΔE where available.
- **No side‑effects:** No writes to `core` data; only new files in `logs/viz/narratives/…`.

### 4.2 Elicitor / Story‑Capture
- **Prompt coverage:** Each mode (elicit/metaphor/perspective/stitch) produces a `StoryPack` with populated `extraction.sequence` or `relations` and optional `lenses`.
- **Safety & scope:** StoryPack entries are stored with **very low weights** or as explicit review tasks; **no κ mutation**.
- **Reprocess readiness:** StoryPacks load via `io/story_io.py`; can be iterated by a future batch job without changing schema.

### 4.3 Observability & Metrics
- `/metrics --viz` (or new `/metrics` block) includes:
  - last conversation run timestamp, adapter, runtime (ms)
  - counts of narratives & stories generated, average length
  - (later) user feedback aggregates (clarity/usefulness)
- Telemetry row per run: `{ts, adapter, style, seed, chars, ms, ok}`

---

## 5) Integration with Existing Surfaces (No Contract Breaks)
- **Paths & /why:** `handlers/map_why.py` (playback JSON), `core/viz.py` (overlay hooks) — read‑only usage.
- **Motifs:** `orchestration/mining.py`, `handlers/motifs.py` exports consumed for motif deltas — read‑only.
- **Lenses:** `io/lens_io.py::load_conversational()`, `handlers/lens.py` for pack management; narrator/elicitor read tags & coverage only.
- **Nightly tail:** `orchestration/nightly.py` — optional call to `/converse` after plots & gravity export; **non‑blocking**.
- **Hot‑swap gravity/lens export:** gravity/lens registries & PNGs already emitted post‑plots; narrator reads metadata; **no feedback into κ/optimiser**.

---

## 6) Work Breakdown (for the implementation agent)

**Milestone M1 — Foundations (5–7 person‑days)**  
1. **Define contracts:** `ArtifactBundle`, `Interpretation`, `Narrative`, `StoryPack` (as above).  
2. **Collector:** implement `ArtifactCollector` to load `why.json`, nightly diffs, motif/lens/gravity exports (paths from runtime or `PROJECT_MAP`), with unit tests.  
3. **Interpreter:** deterministic summarizers (top‐k Δκ, motif reuse deltas, lens coverage deltas, path excerpts).  
4. **RulesAdapter:** Jinja‑based `NarratorAdapter` impl; deterministic seeding; max_len; tests.

**Milestone M2 — Reflective Narrator (3–5 person‑days)**  
1. **Narrator pipeline:** `narrative.txt/json` generation + provenance linking and QA pass.  
2. **CLI:** `/converse` (style, adapter, report‑out), wired via `handlers/system.py` or new `handlers/conversation.py`.  
3. **Nightly integration (opt‑in):** hook into `orchestration/nightly.py` post‑plots; non‑fatal, measured.

**Milestone M3 — Elicitor & Story Capture (4–6 person‑days)**  
1. **StoryPack schema & I/O:** `io/story_io.py` (persist/reload); tests for empty/minimal cases.  
2. **Elicitor CLI:** `/story --mode elicit|metaphor|perspective|stitch [--adapter rules|llm]` → StoryPack.json + .txt saved to `stories/`.  
3. **(Optional) Motif‑hint lens wiring:** if present, surface motif labels in playback and story extraction; else stub.

**Milestone M4 — Metrics & Dashboard (2–3 person‑days)**  
1. **Metrics export:** extend `/metrics` with conversation block, telemetry writers; add to `logs/metrics_log.csv` (timestamp, adapter, runtime, chars, count).  
2. **Dashboard card:** lightweight “Narratives/Stories” panel showing latest files + key stats; link to PNG heatmaps and why‑playbacks.

**Milestone M5 — Documentation & Ops (1–2 person‑days)**  
1. **Docs:** `docs/conversation_mvp.md` (how to run, contracts, toggles, examples).  
2. **Fixtures:** sample bundles and golden outputs for deterministic tests.  
3. **Feature flags:** default off for nightly; enable via runtime key when ready.

---

## 7) Risks & Mitigations
- **Model drift / hallucination (LLM mode):** Keep LLM optional; enforce provenance; default to RulesAdapter.  
- **Performance surprises:** Profile exporter and adapter separately; cap runtime; keep adapter stateless.  
- **Scope creep into learning loop:** Guard with “read‑only” policy; StoryPack queued for later review.  
- **UI creep:** Resist building full interactive editors until Phase 8/9; start with file outputs & a simple dashboard card.

---

## 8) Acceptance & Handover
- **Technical sign‑off** after M1–M4 acceptance tests pass.  
- **Operational sign‑off** after 2 nightlies confirm stable, deterministic outputs and no hot‑path regressions.  
- **Handover package:** code + contracts + test fixtures + docs; runbook for ops; sample narratives & StoryPacks; configuration reference.

---

## 9) References & Surfaces (for implementers)
- **Project map (authoritative module list & entry points):** `mmvp/core/*`, `mmvp/handlers/*`, `mmvp/orchestration/*`, `mmvp/io/*`, `mmvp/strategies/*`, `tools/*`.  
- **Phase 7.5 implementation notes:** gravity/lens hot‑swap; registry, runtime keys, PNG renderer, experiment manifests.  
- **Design laws & success metrics:** explainability, transfer, dual dynamics, KPIs for convergence/reuse/stability.

---

### Appendix A — Example RulesAdapter Template (sketch)
```
Title: What I Learned Today (Operator View)

– Top curvature changes:
{% for d in interpretation.highlights %}
• {{ d.label }}: Δκ={{ d.delta_k|round(3) }} at {{ d.location }}  [evidence: {{ d.provenance_id }}]
{% endfor %}

– Motif reuse: {{ interpretation.transfer.reuse_pct }}% (Δ {{ interpretation.transfer.delta_reuse_pct }})
– Cross‑domain bridges: {{ interpretation.transfer.top_bridges|join(', ') }}
– Lens coverage: {{ interpretation.lens_summary.coverage }} (new: {{ interpretation.lens_summary.new_tags|join(', ') }})
– Representative path: {{ interpretation.why_slices[0].tokens|join(' → ') }}  [ΔE={{ interpretation.why_slices[0].delta_E }}]
```

### Appendix B — StoryPack Example (excerpt)
```json
{
  "id":"story_2025-10-25T22-10Z_ab12cd",
  "text":{"raw":"Cooking pasta is like planning bus routes…","clean":"…"},
  "extraction":{
    "concepts":[{"label":"pasta","confidence":0.91,"hint_domain":"cooking"},
                {"label":"bus route","confidence":0.88,"hint_domain":"transit"}],
    "relations":[{"src":"pasta","tgt":"water","type":"requires","evidence":[[10,25]]}],
    "sequence":["boil water","add pasta","stir","drain","serve"],
    "lenses":[{"tag":"process","score":0.84},{"tag":"timing","score":0.62}],
    "candidate_motifs":[{"path":["boil","cook","drain"],"score":0.7}],
    "mindmap_edges":[{"src":"pasta","tgt":"bus route","weight":0.15,"note":"metaphor mapping"}]
  },
  "provenance":{"inputs":["why_2025-10-25.json"],"adapter":"rules","deterministic":true}
}
```
