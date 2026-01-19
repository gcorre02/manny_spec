# Lens Specification (v0 — Candidate)

## Purpose
This document defines **lenses** as explicit, reversible, non-semantic influences on traversal that *reweight existing manifold physics* without modifying anchoring, routing, or durable state.

Lenses are not dimensions, concepts, or knowledge. They are **session-scoped fields** that modulate how traversal energy flows through the manifold.

This specification is intentionally minimal and defers gravity and other higher-order constructs.

---

## Core Principles

1. **Physics-Preserving**  
   Lenses must not introduce new traversal dimensions. They may only reweight existing cost components.

2. **Post-Anchoring Only**  
   Lenses act *after* anchoring. They must not affect anchor resolution or goal selection.

3. **Explicit and Logged**  
   All active lenses and parameters must be recorded per interaction and per session.

4. **Session-Scoped and Reversible**  
   Lenses are ephemeral by default. They must not write to durable state in OBSERVE or STAGE.

5. **Mode-Invariant Semantics**  
   The mathematical effect of a lens must be identical across OBSERVE, STAGE, and LEARN. Modes differ only in whether learning is permitted.

---

## Risks & Mitigations (v0)

This section documents known risks in early lens usage and the explicit constraints that mitigate them.

### Risk 1 — Lenses Become Goals
**Description:** Directional or novelty lenses are misused as goal definitions rather than traversal biases.
**Mitigation:**
- Lenses may only reweight cost; they must not define success conditions.
- Lenses must not trigger re-anchoring, path selection, or termination.

### Risk 2 — Physics Masking
**Description:** Lens weights overpower base cost components, effectively masking κ, mass, or valence.
**Mitigation:**
- Lens terms are additive only and must not neutralize or cancel underlying cost components.
- Lens weights must be conservatively bounded and logged.
- Implementations SHOULD enforce lens weights such that individual lens terms remain within ~10–20% of the baseline distance cost, unless explicitly justified and documented.

### Risk 3 — Anchor Favoritism
**Description:** Lenses implicitly privilege one anchor over another in multi-anchor traversal.
**Mitigation:**
- Lenses must apply uniformly across all anchors in a traversal.
- Anchoring decisions remain exclusively governed by the Routing & Anchoring Specification.

### Risk 4 — Silent Persistence
**Description:** Lens effects leak across sessions or influence durable state.
**Mitigation:**
- Lenses are session-scoped by default.
- No lens may write to κ, edges, motifs, or mass proxies.
---

## Lens Taxonomy (v0)

### 1. Novelty Lens

**Intent:** Encourage lateral exploration by penalizing repeated traversal within a session.

**Effect:** Adds a `novelty_term` to traversal cost.

**Definition (v0):**
- Maintain a session-local set of visited nodes (or edges).
- For a candidate step `u → v`:
  - `novelty_term = novelty_weight * 1.0` if `v` has been visited in this session
  - `novelty_term = 0.0` otherwise

**Constraints:**
- Deterministic
- Session-local only
- Must not affect κ, edges, or motifs

**Parameters to log:**
- `novelty_enabled`
- `novelty_weight`
- `novelty_scope` (`nodes` | `edges`)

---

### 2. Directional Field Lens

**Intent:** Bias traversal toward or away from a specified target set without selecting paths or encoding meaning.

**Conceptual Model:** A temporary potential field over the graph.

**Effect:** Adds a directional term to cost that rewards movement reducing distance to the target set.

**Definition (v0):**
- Let `T` be a set of target nodes.
- Let `d(u, T)` be the shortest hop-distance from node `u` to any node in `T` (precomputed deterministically).
- For a candidate step `u → v`:
  - `directional_term = field_weight * (d(v, T) - d(u, T))`

Movement toward the target set lowers cost; movement away increases cost.

**Constraints:**
- Deterministic
- Session-scoped
- Must not modify anchoring or goal selection
- Must not write durable state

**Parameters to log:**
- `field_enabled`
- `field_weight`
- `field_targets` (node ids or region ids)

---

## Lens Interaction Rules

- Multiple lenses may be active simultaneously.
- Lens terms are additive components of the existing cost function.
- No lens may override or cancel another lens implicitly.
- All lens contributions must appear explicitly in `trace.steps[*].cost_breakdown`.

Lenses may bias traversal but must not mask, neutralize, or override underlying physical cost components.

---

## Relationship to Anchoring & Routing

- Anchoring determines **where pressure enters** the manifold.
- Lenses determine **how traversal energy flows** after anchoring.
- Lenses must not influence anchor selection or fallback behavior.

- Lenses must apply uniformly across all anchors in a multi-anchor traversal.
- Lenses must not trigger re-anchoring, anchor expansion, or fallback behavior.

See: `anchoring_routing_spec_v1.md` for anchoring rules.

---

## Deferred Concepts (Out of Scope v0)

The following are intentionally deferred:
- Gravity as an emergent property of stable κ and motifs
- Signed or negative mass terms
- Long-lived or cross-session lenses
- Learning-driven lens formation
- Semantic or intent-based lenses
- LLM-driven dynamic lens generation or adjustment.

These will be addressed in a future **Gravity & Emergence Specification**.

---

## Status

This document is **CANDIDATE**.

It is safe to implement v0 novelty and directional field lenses under this specification, provided all constraints above are enforced and logged.

No claims about emergent meaning or gravity are made here.

---

End of document.
