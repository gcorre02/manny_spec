# Manny Manifolds — Canonical Freeze v1.1

Status: FROZEN  
Date: 2026-01-15  
Applies to: `canonical/foundations_v1.1.md`, `canonical/design_document_v1.1.md`, `canonical/architecture_v1.1.md`


## Scope of this freeze
The following documents together define the canonical contract of Manny Manifolds:
- Foundations — immutable physics and invariants (with provisional primitives addendum)
- Design — behavioral contract, gates, promotion/ingestion/run-mode rules
- Architecture — system shape, authority boundaries, motion law, and run-mode enforcement

From this point onward:
- These documents must not drift silently.
- Any change requires a new canonical version (e.g., v1.2, v2.0) or an explicit experimental deviation documented outside the canon.

## Manny MVP Freeze (M6)

Manny MVP is frozen at the completion of **M6 (Consolidation & Rollback)**. At this point, **Gates A–C** are fully evaluated and passing, and **Gate D.a** (offline consolidation with atomic swap and exact rollback) is proven. Reasoning is path-based and replayable (Gate A), learning is explicit, persistent, and bounded (Gate B), transfer via runtime reuse is real, selective, and falsifiable (Gate C), and maintenance operations are auditable and exactly reversible (Gate D.a). **Gate D.b** (maintenance behavior quality over repeated cycles) is intentionally deferred as post-MVP evaluation and does not block MVP completeness. All future work extends capability, scale, or optimization without redefining the core guarantees frozen here.

## What “canonical” means
The canonical documents define:
- What Manny Manifolds is and is not
- How learning occurs (edge-backed traversal + Δκ)
- How learning is measured (gates, metrics, reports)
- What constitutes valid generalization (convergence/transfer/motif utility)
- What operations are learning-neutral vs learning-active (RunModes, Virtual Stage)

They intentionally do not define:
- UI/UX details
- Specific storage engines/providers
- Specific model vendors
- Performance tuning/optimizations

## Non-negotiable invariants (v1.1)
- Learning occurs only via manifold-native traversal and curvature updates; no opaque artifacts as knowledge.
- Primitives are provisionally atomic and decomposable; promotion/decay must remain manifold-native.
- Observation ≠ Learning: RunMode `OBSERVE/STAGE` cannot mutate κ/motifs/lenses; only `LEARN` can.
- Reuse is an explicit event, not implicit overlap.
- Convergence is local/directional, not global ASPL reduction.
- Single engine of truth; derived indices (ANN/LLM) are advisory only.
- LLM usage is bounded and scoped to non-reasoning paths; external encoders may propose, never decide traversal.
- Executive regulates parameters only (τ, η, ζ); never routes paths.

## Canonical authority
If any implementation, experiment, or future proposal contradicts the canonical documents, the documents are authoritative.

⸻

## Phase Clearance History

**2026-01-15** — Observational Emergence Phase (v0) cleared.  
Evidence: `canonical/validation/observational_emergence_clearance_record_v0.md`

The Observational Emergence Phase has been executed, evidenced, and formally cleared. Manny now has a validated observational baseline. No learning or gravity has been authorized.
