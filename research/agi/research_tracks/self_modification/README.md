# Research Track: Self-Modification (Non-Canonical)

Contract: self-modification = staged configuration proposals; replay against validation suite; explicit approval + rollback before commit.

Threat model: uncontrolled self-rewrite, hidden semantics. Mitigation: STAGE mode; deterministic replay; approval gate.

Promotion rule: no writes to canonical curvature except via LEARN mode and passing validation.
