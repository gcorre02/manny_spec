# Manny Manifolds — Canonical Freeze v1.0

Status: FROZEN  
Date: 2026-01-14  
Applies to: `foundations.md`, `design_document.md`, `architecture.md` (superseded by v1.1; retained for history)

## Scope of this freeze
The following documents together define the canonical contract of Manny Manifolds:
- Foundations — immutable physics and invariants
- Design — behavioral contract, gates, metrics, and lifecycle rules
- Architecture — system shape, authority boundaries, and evolution constraints

From this point onward:
- These documents must not drift silently.
- Any change requires either a new canonical version (v1.1, v2.0) or an explicit experimental deviation documented outside the canon.

## What “canonical” means
The canonical documents define:
- What Manny Manifolds is and is not
- How learning occurs
- How learning is measured
- What constitutes valid generalization
- What operations are learning-neutral vs learning-active

They intentionally do not define:
- UI/UX details
- Specific storage engines
- Specific model vendors
- Performance optimizations

## Non-negotiable invariants (v1.0)
- Learning occurs only via geometric traversal and curvature updates.
- Observation, interpretation, diagnostics, and visualization never alter learning physics.
- Reuse is an explicit event, not implicit overlap.
- Convergence is local or directional, not global ASPL reduction.
- There is a single engine of truth.
- ANN / search indices are advisory only.
- LLM usage is bounded and scoped to non-reasoning paths.

## Canonical authority
If any implementation, experiment, or future proposal contradicts the canonical documents, the documents are authoritative.
