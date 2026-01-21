# Manny Physics Suite v1.1 — Spec Index

**Status:** Canon-adjacent index  
**Purpose:** Provide a single authoritative map of Manny's core physics specifications and their responsibilities.

This index defines the Manny Physics Suite v1.1: the closed set of specifications that govern how cognition, learning, and emergence are allowed to occur in Manny.

⸻

## Core Principle

All cognition in Manny must be explainable as the interaction of these physics layers.  
No behavior may bypass or contradict them.

⸻

## Physics Suite Components

### 1. Routing & Anchoring Specification (v0)

**Responsibility:**  
Defines how experiences apply pressure to the manifold by selecting starting points.

**Governs:**
- anchor derivation
- artifact-only contact
- determinism (snapshot + seed)
- multi-anchor rules
- gap detection via distortion

**Explicitly forbids:**
- semantic inference at entry
- planner-driven routing
- intent-based anchor selection

⸻

### 2. Traversal Physics Specification (v0)

**Responsibility:**  
Defines how thought moves once pressure is applied.

**Governs:**
- local free-fall traversal
- frontier semantics
- energy/cost composition
- termination rules
- mode invariance

**Explicitly forbids:**
- global search
- planners or lookahead
- semantic shortcuts
- LLM scoring for decisions

⸻

### 3. Lens Specification (v0)

**Responsibility:**  
Defines temporary, explicit, reversible bias over traversal.

**Governs:**
- novelty bias
- directional field bias
- cost reweighting only

**Explicitly forbids:**
- persistent attraction
- goal definition
- anchor influence
- physics masking

⸻

### 4. Plasticity & Learning Physics Specification (v0)

**Responsibility:**  
Defines how the manifold learns (κ mutation).

**Governs:**
- trace-bounded learning
- mode-gated plasticity
- bounded Δκ
- reversibility
- decay and consolidation

**Explicitly forbids:**
- learning in OBSERVE
- semantic authority
- unlogged κ changes

⸻

### 5. Promotion & Motif Emergence Specification (v0)

**Responsibility:**  
Defines how higher-order reusable structure stabilizes.

**Governs:**
- motif discovery from traces
- promotion gates (recurrence, benefit, stability)
- motif reuse rules
- lifecycle (active, deprecated, pruned)

**Explicitly forbids:**
- manual motif injection
- motifs as planners or goals
- hidden compression

⸻

### 6. Gravity & Emergence Placeholder Specification (v0)

**Responsibility:**  
Defines what gravity and emergence may be used to describe — and what they may never become.

**Governs:**
- descriptive-only gravity
- prevention of attractor targets
- protection against "soft planners"

**Explicitly forbids:**
- gravitational goals
- emergent intent
- long-range attraction fields

⸻

## Suite-Level Invariants

- Physics is closed.
- Reasoning is motion.
- Learning is curvature.
- Structure is earned.
- Emergence is detected, not declared.
- Artifacts are the ground truth.

⸻

## Versioning

- Manny Physics Suite v1.1 consists of the specs listed above.
- Any addition or modification requires explicit review against all suite invariants.
- No single spec may be modified in isolation if it affects cross-spec guarantees.

⸻

---

# Manny Physics Suite — Unified Conformance Harness Checklist

**Status:** Implementation-facing checklist  
**Purpose:** Provide a single, cross-physics validation surface ensuring Manny's behavior remains compliant with the Physics Suite.

This checklist must pass for any release claiming Manny Physics Suite compliance.

⸻

## A. Determinism & Replay

- Given identical snapshot + seed, routing anchors are identical
- Traversal traces are identical up to termination
- Lens effects reproduce identically
- Plasticity deltas reproduce identically
- Replay reconstructs full behavior from artifacts

⸻

## B. Routing & Anchoring

- Anchors derive only from explicit artifacts
- No semantic inference used at entry
- Multi-anchor ambiguity emits parallel anchors
- No anchor favoritism
- Anchors identical across OBSERVE / LEARN / STAGE

⸻

## C. Traversal Physics

- Traversal is bounded (k-hop frontier)
- No global scans or lookahead
- No planner heuristics detected
- Step selection logged with full cost breakdown
- Termination reason explicitly logged
- Traversal invariant across modes

⸻

## D. Lens Behavior

- Lenses applied only post-anchoring
- Lenses reweight cost additively only
- Lens weights bounded and logged
- Lenses do not influence anchoring
- Lenses do not persist across sessions
- No lens masks κ, mass, or valence

⸻

## E. Plasticity & Learning

- No κ changes in OBSERVE
- Every Δκ references a trace id
- Δκ bounded by clamps
- Local-only updates (trace + neighborhood)
- Rollback restores prior snapshot exactly
- STAGE requires explicit commit artifact

⸻

## F. Motif Promotion & Reuse

- No motif from single trace
- Motifs require recurrence across contexts
- Motifs demonstrate cost/step reduction
- Motifs expandable for /why
- Motif reuse logged explicitly
- Motifs do not override traversal physics
- Motif lifecycle reversible

⸻

## G. Gravity & Emergence Constraints

- No "gravitational goals" detected
- No long-range attraction fields
- No planner-like behavior framed as emergence
- Gravity used descriptively only (post-hoc)

⸻

## H. Artifact Integrity

- `trace.json` complete and consistent
- `delta_report.json` emitted for all learning
- `snapshot.json` versioned and restorable
- `stage_commit.json` required for staged learning
- All artifacts sufficient for independent audit

⸻

## Final Gate

If any single checkbox fails, the system does not conform to Manny Physics Suite v1.1.

⸻
