# Diagnostic Dashboard Specification (v0)

**Status:** Canonical-adjacent / Measurement discipline  
**Purpose:** Define what may be measured and visualized, and what must not be inferred or acted upon.

This is not a UI spec and not an implementation plan. This is a specification of observational boundaries that prevents dashboards from quietly becoming control panels.

⸻

## Allowed Inputs

Dashboards may consume:
- **Artifacts only** — `trace.json`, `delta_kappa.json`, `metrics.json`, `run_manifest.json`
- **Aggregated statistics** — computed from artifacts (counts, distributions, summaries)
- **Snapshot metadata** — state hashes, timestamps, run IDs

Dashboards may **not** consume:
- Live runtime state (beyond what's in artifacts)
- Internal engine state not captured in artifacts
- LLM hidden state or intermediate representations

⸻

## Allowed Aggregations

Dashboards may compute and display:
- **Counts** — interaction counts, trace lengths, motif reuse events
- **Distributions** — κ distributions, path length histograms, cost breakdowns
- **Heatmaps** — activation patterns, basin revisitation, frontier expansion
- **Time series** — evolution of metrics over runs/sessions
- **Comparisons** — before/after snapshots, A/B run comparisons

All aggregations must be:
- Computable from artifacts alone
- Reproducible given the same artifacts
- Clearly labeled with source artifact references

⸻

## Forbidden Uses

Dashboards must **not**:
- **Set thresholds** — no "healthy/unhealthy" classifications
- **Make decisions** — no automated actions based on dashboard signals
- **Generate alerts** — no "warning" or "error" states
- **Recommend changes** — no suggestions for parameter adjustments
- **Override RunModes** — no dashboard-initiated mode switches
- **Inject feedback** — no dashboard signals feeding back into learning

Dashboards are **read-only observation surfaces**.

⸻

## Relationship to Emergence Diagnostics

This dashboard spec operationalizes the observation layer referenced in `emergence_diagnostics_v0.md`.

- Emergence diagnostics define **what to look for**
- This dashboard spec defines **how to look** (and how not to act)

Together, they ensure emergence is detected, not declared.

⸻

## Explicit Non-Goals

Dashboards will **not** provide:
- **Health metrics** — no "system health" indicators
- **Performance scores** — no "good/bad" ratings
- **Optimization hints** — no suggestions for improvement
- **Anomaly detection** — no automated flagging of "unusual" behavior
- **Goal tracking** — no progress toward objectives

Dashboards show **what happened**, not **what should happen**.

⸻

## Output Format

Dashboard outputs must be:
- **Artifact-derived** — every visualization traceable to source artifacts
- **Reproducible** — same artifacts → same dashboard output
- **Auditable** — dashboard code/logic must be inspectable
- **Non-mutating** — dashboard generation must not alter artifacts or state

⸻

## Stability Requirement

This spec must be stable before learning begins. Dashboard boundaries must not shift during experiments, as that would invalidate observational consistency.

⸻

> **Principle:** A dashboard that cannot influence behavior cannot corrupt measurement.
