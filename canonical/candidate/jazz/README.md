# Jazz Candidate Specifications

**Status:** Candidate / Non-canonical

This directory contains candidate specifications related to Jazz Sessions — observational conversational protocols that allow Manny to engage in open-ended interaction while preserving trace fidelity and architectural invariants.

## Current Specifications

- `jazz_narration_spec_v1_1.md` — Constraints and invariants for Jazz narration layer

## What Jazz is NOT

Jazz is explicitly:
- Not evaluation
- Not reward learning
- Not prompt-engineering
- Not supervised training

Jazz is observation and exploration, not optimization or instruction.

## Relationship to Canon

Jazz Sessions are an ingestion pattern, not a new execution mode. They obey all RunMode constraints and never bypass traversal, trace logging, or write barriers.

For the canonical Jazz protocol, see `canonical/high_level/runtime_protocols/observability/jazz_protocol.md`.
