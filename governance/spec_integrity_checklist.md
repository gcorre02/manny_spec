# Spec Integrity Checklist

Use before freezing a new version (e.g., v1.2) or merging major doc changes.

## Required structure
- `canonical/` exists with: `foundations_vX.md`, `design_document_vX.md`, `architecture_vX.md`, `validation/validation_framework.md`, `CANONICAL_INDEX.md`.
- `canonical/high_level/` exists (advisory only); archive intact.
- `governance/` has freezes, doc_classification, BUS_PACKET.
- `implementation/` has current milestone plan and specs.

## Naming and references
- All internal references point to current versioned filenames (vX) or canonical index.
- Redirect stubs (if any) point to current vX files.

## Validation contract
- Artifacts listed (manifest, validation_report, interaction/trace/delta_kappa/metrics) match validation spec.
- Gate definitions align with `design_document_vX` and `validation_framework`.

## Run-modes/invariants
- Observation ≠ Learning stated; LLM/ANN propose-only; no opaque blobs as knowledge.

## Provenance
- CHANGELOG, CONTRIBUTORS, LICENSE present and current.
- BUS_PACKET present and up to date.

## Optional checks
- Packs described in validation README.
- High-level references do not contradict canon.
