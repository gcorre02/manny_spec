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

## Required Provenance

Every chart/table must display (or embed in metadata):
- `spec_pin` (from `SPEC_VERSION.md` where applicable)
- `run_id` (and `pack` if present)
- `state_hash_before` / `state_hash_after` (when available)
- source artifact paths (e.g., `runs/<run_id>/interactions/*/trace.json`)
- dashboard logic version/hash (so outputs are reproducible)

If any provenance element is missing, the dashboard may still render, but must show `provenance_incomplete=true` in its metadata.

⸻

## Allowed Aggregations

Dashboards may compute and display:
- **Counts** — interaction counts, trace lengths, motif reuse events
- **Distributions** — κ distributions, path length histograms, cost breakdowns
- **Heatmaps** — activation patterns, basin revisitation, frontier expansion
- **Time series** — evolution of metrics over runs/sessions
- **Comparisons** — before/after snapshots, A/B run comparisons
- **Derivations** — deterministic computed metrics from artifacts (e.g., `goal_reached_pct`, `termination_reason_counts`, `distinct_region_visits`) provided each derivation is documented, traceable, and represents monotonic transformations of artifact data, not composite judgments.

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
- **Rank or score runs** — no ordering, rating, or scalar "quality" measures comparing runs, sessions, or configurations
- **Sort or rank by performance** — dashboards must not sort or rank runs by performance, quality, or desirability
- **Apply threshold coloring** — dashboards must not apply color-coding or visual thresholds that imply good/bad states

Dashboards are **read-only observation surfaces**.

## Allowed Outputs

Dashboards may output:
- static images (PNG/SVG)
- HTML reports
- JSON bundles (data + provenance) used to render charts
- Markdown summaries

All outputs must be written to a **separate output directory** (never into `runs/`), and must never modify or delete run artifacts.

⸻

## Relationship to Emergence Diagnostics

This dashboard spec operationalizes the observation layer referenced in `emergence_diagnostics_v0.md`.

- Emergence diagnostics define **what to look for**
- This dashboard spec defines **how to look** (and how not to act)

Together, they ensure emergence is detected, not declared.

This dashboard spec also supports the Experiential Learning Curriculum (candidate). Dashboards may be used to visualize baseline and post-learning diagnostics, but must not be used to decide whether learning should occur or whether learning was successful. All such decisions are governed by the curriculum and promotion checklists, not dashboards.

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

## Change Control (Stability)

Because dashboards define the observation surface, changes must be slow and explicit:
- Any change to aggregation definitions or chart semantics requires a version bump (e.g., `dashboard_spec_version=v0.1 → v0.2`).
- Experiments must record the dashboard spec version used.
- Do not backfill new aggregations into historical dashboards unless clearly marked as “computed later”.
- **Dashboard immutability during experiments** — dashboard definitions must remain fixed during a learning or gravity experiment. No dashboard changes may occur between baseline and post-experiment measurements.

⸻

## Stability Requirement

This spec must be stable before learning begins. Dashboard boundaries must not shift during experiments, as that would invalidate observational consistency.

⸻

> **Principle:** A dashboard that cannot influence behavior cannot corrupt measurement.

⸻

## Appendix — Example Observational Panels (Non-Normative)

These examples illustrate permitted panels; they must remain read-only.

- **Run overview:** interactions count, mode, state hashes, pack name, spec pin.
- **Trace distribution:** histogram of step counts; termination reason counts.
- **Cost composition:** stacked distribution of cost components (distance, kappa_term, mass_term, field_term, novelty_term, friction_term, penalty_term).
- **Reuse distribution:** motif reuse events by motif_id; reuse_event_pct over time.
- **Region exploration:** distinct `meta.region` visits per session; region transition counts.
- **Rollback proof view:** checkpoint hash vs rollback hash equality and row-set equality result flags.

Panels must not include pass/fail labels; interpretation belongs to packs and gates, not the dashboard.
