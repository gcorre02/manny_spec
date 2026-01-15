# Manny Manifolds — High-Level Implementation Spec (Post-Milestones)

**Status:** Implementation-oriented, canon-aligned  
**Scope:** System behavior after core milestones (M0–M6) are satisfied  
**Non-goal:** This is not a low-level API or performance spec  
**Use:** This spec guides implementation by defining module boundaries, run contracts, and required artifacts. It is the post-milestones runtime target the milestone docs converge to.

## 1. System Posture After Milestones

Once milestones are in place, Manny operates as a continuously learning cognitive substrate, not a project in “build mode”.

Key properties:
	•	No “training phase” vs “inference phase”
	•	No frozen model
	•	No batch retraining
	•	No global optimizer
	•	Cognition always occurs via:
	•	local traversal
	•	trace logging
	•	optional plasticity
	•	periodic consolidation

The system is always alive, even when not learning.

## 1.1 Relationship to milestones (how to use this spec)

- The milestone docs (M0–M6) define *atomic proof steps*; this document defines the *steady-state runtime posture* once those proofs exist.
- Implementation should follow milestones, but must maintain this runtime shape throughout.
- If any milestone implementation introduces a second reasoning engine, hidden state, or interface-specific semantics, it violates this spec and must be rejected.

## 2. Core Runtime Loop (Canonical)

This is the invariant runtime loop Manny follows:

Input (experience)
→ Ingestion & proposal
→ Local traversal (thread)
→ Trace selection
→ Response generation
→ (Optional) Plasticity
→ Logging & metrics
→ (Periodic) Consolidation

Nothing bypasses this loop.

### 2.1 One-page system diagram (reference)

```mermaid
flowchart LR
  A[Experience Input]
  B[Ingestion & Proposal]
  C[Thread Runner]
  D[Energy/Cost Evaluator]
  E[Trace Selection]
  F[Narration Layer]
  G[Response]
  H[Plasticity Engine]
  I[Logs & Metrics]
  J[Consolidation]
  K[(Manifold State)]
  L[LLM (Stimulus Only)]

  A --> B --> C
  C --> D --> C
  C --> E --> F --> G
  C --> I

  %% RunModes
  C -. OBSERVE .-> I
  C -. LEARN .-> H --> K
  C -. STAGE .-> H -. buffered .-> K

  %% Offline
  I --> J --> K

  %% LLM boundaries
  L -. proposes .-> A
  L -. polish text .-> F
```

**Legend:**
- Solid arrows: canonical execution path
- Dashed arrows: conditional (RunMode) or advisory
- LLM has no access to traversal, κ, or state

### 2.2 Ownership boundaries (no drift)
- **Core engine is the source of truth:** traversal, mutation rules, runmodes, and state transitions live only in core.
- **Interfaces are transport:** CLI/API/UIs invoke core engine commands and must not replicate semantics.
- **Validation is external:** gates are computed by the harness from artifacts; the engine never decides pass/fail.

## 3. Execution Modes (Operational)

3.1 OBSERVE (default)
	•	Traversal only
	•	No κ updates
	•	No durable mass updates (mass may be computed/logged as observational signal)
	•	Used for:
	•	Jazz Sessions
	•	replay
	•	explanation
	•	debugging
	•	analysis
	•	writes `delta_kappa.json` as an empty deltas file

3.2 LEARN
	•	Traversal + local plasticity
	•	Used for:
	•	curriculum exposure
	•	environment interaction
	•	controlled training experiences
	•	writes populated `delta_kappa.json` and updates durable κ

3.3 STAGE
	•	Traversal + buffered plasticity
	•	No write to canonical manifold
	•	Commit only via explicit gate artifact
	•	writes `stage_delta.json`; durable mutation only after `stage_commit.json`

RunMode is infrastructure, not cognition.

## 4. Ingestion Layer (Post-Milestones)

4.1 Experience Ingestion

All inputs are converted into experience proposals:
	•	conversational turns
	•	percept inputs (future)
	•	environment events
	•	internal questions (future)

Each proposal includes:
	•	entry anchors (labels)
	•	optional lens hint
	•	valence signals (weak, provisional)
	•	run mode

4.2 LLM Integration (Strict Boundary)

LLMs may:
	•	generate experience proposals
	•	vary phrasing and context
	•	act as conversational partners

LLMs may not:
	•	choose traversal
	•	assign κ
	•	collapse ambiguity
	•	inject answers

LLMs never see Manny’s internal state beyond surfaced responses.

## 5. Traversal Engine (Thinking)

Traversal is:
	•	local
	•	energy-biased
	•	stochastic (seeded)
	•	frontier-based

Mechanically:
	•	active k-hop frontier
	•	next step sampled by energy/cost
	•	energy derived from:
	•	curvature
	•	semantic mass proxy
	•	valence traces
	•	lens weighting
	•	novelty

Traversal produces:
	•	a trace
	•	motion statistics
	•	candidate motifs

Traversal is thinking.

## 6. Response Generation (Narrated Cognition)

Responses are derived from traces, not generated independently.

6.1 Response Selection

Manny selects:
	•	one trace (or a small set)
	•	one perspective on that trace

6.2 Narration Layer

Narration:
	•	maps trace elements → language
	•	expresses motion (“pull”, “drift”, “resistance”)
	•	avoids authority or finality

LLMs may polish language after trace selection.

## 7. Learning & Plasticity (When Enabled)

Plasticity is:
	•	local
	•	incremental
	•	trace-bounded

Updates may include:
	•	κ increase along repeated paths
	•	decay elsewhere
	•	mass proxy update

Learning is never global.

## 8. Consolidation (Always Offline)

Runs periodically, not per interaction.

Consolidation may:
	•	promote stable subpaths → motifs
	•	decay unused edges
	•	recompute mass proxies
	•	clean noise

Consolidation is deterministic given snapshot.

## 9. Observability & Instrumentation

Every run produces artifacts:
	•	trace.json
	•	cost breakdown
	•	frontier dynamics
	•	motif events
	•	mass evolution

Primary analysis tools:
	•	replay
	•	comparison
	•	delta inspection

Accuracy is never the metric.
Motion is.

Standard artifacts (authoritative):
- per run: `run_manifest.json`, `validation_report.json` (optional), `validation_report.md` (optional)
- per interaction: `interaction.json`, `trace.json`, `metrics.json`, `delta_kappa.json`
- stage (when used): `stage_delta.json`, `stage_commit.json`

## 10. Jazz Sessions (Operational Mode)

Jazz Sessions are first-class operational mode, not demos.

Purpose:
	•	observe stream of thought
	•	inspect interference
	•	stress-test locality
	•	study emergence

They:
	•	run in OBSERVE by default
	•	are replayable
	•	are annotatable
	•	are never graded

Jazz Sessions are how you listen to Manny.

## 11. Scaling & Size Management

Scalability relies on:
	•	locality (active frontier only)
	•	decay
	•	motif compression
	•	no global scans

No step requires:
	•	full graph traversal
	•	full re-embedding
	•	global optimization

This is why Manny can grow.

## 11.1 Persistence authority boundary

- The centralized store (SQLite) is the sole source of durable learning state and geometry.
- Artifacts are the source of truth for *what happened during a run* (replay/audit/validation).
- Derived indices (ANN, search) are advisory only and rebuildable; they must not become semantic truth.

## 12. Safe Extension Points (Future-Facing)

Post-milestones, Manny may be extended via new experience sources, not new engines:
	•	agency → action experiences
	•	world models → episodic experiences
	•	ToM → other-agent experiences
	•	self-modification → staged configuration experiences

All extensions must:
	•	enter via ingestion
	•	obey traversal
	•	log traces
	•	pass through STAGE

## 13. What This Spec Guarantees

If implemented as written, Manny will:
	•	think locally
	•	learn continuously
	•	explain itself faithfully
	•	avoid mode collapse
	•	resist planner creep
	•	remain analyzable
	•	remain safe to explore

## 14. Final statement

Manny is not a model that answers questions. Manny is a space where thought moves, and leaves traces.

This spec describes how to run that space once the core behaviors exist.

## 15. Minimal reference implementation plan (MVP-ready)

This is a minimal, buildable plan that implements the spec while preserving canon invariants. It is intentionally small and favors determinism and observability over optimization.

### 15.1 Build order

1) **State + persistence skeleton**
   - SQLite (or Postgres) schema for Nodes, Edges, Traces, Deltas, Motifs, Artifacts
   - Snapshot export/import (`snapshot.json`) and deterministic seed handling

2) **Thread Runner (OBSERVE only first)**
   - Local k-hop frontier expansion
   - Energy/Cost Evaluator (κ, lens, mass proxy, valence, novelty) — deterministic given seed
   - Trace emission (`trace.json`) with per-step cost components

3) **Narration layer**
   - Deterministic trace-to-text renderer (template-based)
   - Optional LLM polishing step that cannot change content (post-render only)

4) **RunModes + write barriers**
   - Enforce LEARN/OBSERVE/STAGE plumbing
   - Prove PlasticityEngine is unreachable in OBSERVE by construction

5) **PlasticityEngine (LEARN)**
   - Apply bounded Δκ along trace
   - Local decay
   - Emit `delta_kappa.json` (LEARN only)

6) **Consolidation job**
   - Motif mining (frequency × benefit)
   - Prune low-κ noise
   - Recompute mass proxies

7) **Jazz Session harness**
   - OBSERVE-default loop using a conversation partner (LLM or scripted)
   - Replay support (same snapshot+seed)

This build order should be implemented as milestone-aligned slices; each step must be runnable and validated via `mm validate` before moving on.

### 15.2 Reference artifacts (must exist)

- `trace.json` — path, per-step cost breakdown, lens, runmode
- `delta_kappa.json` — Δκ updates (LEARN only)
- `snapshot.json` (optional) — canonical state snapshot for replay (SQLite DB + state hash is the durable truth)
- `stage_commit.json` — explicit STAGE commit gate (optional stub for v1)

### 15.3 Definition of “reference implementation passes”

The reference implementation is considered valid when:
- OBSERVE runs produce identical traces under the same snapshot+seed
- LEARN runs produce a delta report and a changed snapshot
- `/why` can be produced directly from `trace.json`
- A short jazz session can be replayed exactly
- No learning occurs in OBSERVE