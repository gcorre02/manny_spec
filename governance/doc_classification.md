# Doc Classification (to prevent canon creep)

- **Canonical** (frozen per version): `canonical/` (foundations_vX, design_document_vX, architecture_vX, validation/validation_framework), `canonical/CANONICAL_INDEX.md`, governance freezes. Changes require a new version and freeze.
- **High-level references**: `canonical/high_level/` (north-star PDFs, runtime protocol, training track) and `background_refs/` — advisory only; do not override canon.
- **Governance**: freezes, BUS_PACKET, integrity checklists; controls process.
- **Implementation**: milestone plans, runtime specs; must conform to canon; not themselves canon.
- **Future**: non-canonical ideas, research maps; cannot override canon.
- **Research**: reference/legacy/ideation; cannot override canon.
- **Research tracks**: safety-gated explorations; non-canonical.

Rule: only `canonical/` defines the authoritative contract; everything else is supporting or advisory.
