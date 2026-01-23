# Agent guide (Codex/Cursor)

This repository (`manny_design`) is the **documentation** source of truth for Manny Manifolds. There is **no runtime code** here.

## Where to start
- `README.md`
- `canonical/CANONICAL_INDEX.md`
- `governance/canonical_freeze_v1.1.md`
- `governance/BUS_PACKET/00_README_START_HERE.md`

## Document precedence
- **Canonical (frozen):** `canonical/` (v1.1). In conflicts, follow `canonical/CANONICAL_INDEX.md` precedence.
- **Governance:** `governance/` controls freezes, decisions, and integrity constraints.
- **Non-canonical:** `implementation/`, `future/`, `research/`, `archive/`, `ongoing_thoughts.md` (supporting material only).

## Editing rules
- Prefer small, surgical edits; preserve document tone/formatting (including `⸻` separators).
- Don’t change Canonical v1.1 semantics; for semantic changes, create a new version (v1.2+) and update indices/freezes accordingly.
- Keep file paths and cross-references accurate; if you add/move docs, update `PROJECT_TREE.md` and any relevant indices.
- This repo can reference `mm ...` commands and `packs/` conceptually, but they are **not runnable here**.

