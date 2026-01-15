# manny_design — documentation-only repo

This repository is the documentation source of truth for Manny Manifolds. There is no runtime code here.

## Structure
- `canonical/` — canonical documents (source of truth)
  - `foundations_v1.1.md` — irreducible principles + provisional primitives addendum.
  - `design_document_v1.1.md` — contract, requirements, gates, primitive/compression/perception contracts.
  - `architecture_v1.1.md` — system shape, modules, run-modes, motion law, ingest contract.
  - `validation/` — validation framework (hypothesis-driven validation process and report requirements).
  - `high_level/` — north-star references (Unified Manifold PDFs + gap analysis) and `background_refs/` (supporting surveys/frictions/positioning).
  - `CANONICAL_INDEX.md` — authoritative list of canonical docs and precedence.
  - `README.md` — layout guide; archive pointers (v1.0 snapshots + candidate materials used for 1.1).
- `governance/` — canonical freeze and change rules
  - `canonical_freeze_v1.0.md` (historical)
  - `canonical_freeze_v1.1.md` (current)
- `future/` — non-canonical ideas compatible with Canonical v1.1
  - `deferred_future_work_register.md`
  - `human_behavior_and_agi.md`
  - `reference/ai_movement_psychology/` (reference pack)
  - `future_research_map.md` (research map / tracker)
- `governance/BUS_PACKET/` — handoff packet (canon index, non-negotiables, MVP proof plan, risks, safe expansions)
- `research/` — non-canonical research, legacy material, and surveys
  - `research/agi/research_tracks/` — safety-gated tracks (agency, goal formation, world model, self modification)
- `CHANGELOG.md`, `CONTRIBUTORS.md`, `LICENSE` — provenance and authorship stubs
- `archive/` — repo-wide archive (historical, non-normative).
- `ongoing_thoughts.md` — personal ideation notepad (lives at repo root; non-canonical).

Author: Guilherme Ribeiro. Unless explicitly licensed otherwise, all rights are reserved.

Use this repo to read, review, and evolve the design; code lives elsewhere.
