# Manny Manifolds Conversational Layer Delegation and Escalation Protocol

This is the runtime protocol reference. The conversational layer is an interface to the core engine; it must not become a separate planner or memory.

## MVP Goal
A power-user CLI/API that provides leverage while preserving core constraints: path-based reasoning only, edge-backed learning, no hidden planning or learning outside the manifold.

## Non-negotiables / grounding
- All cognition occurs via threads in the manifold; no chain-of-thought outside.
- No opaque state: everything loggable and reconstructable.
- No external orchestration: LLM cannot take over traversal or learning.
- All learning resides in the manifold; artifacts are logs, not state.
- Determinism: given same context/inputs/settings, runs are reproducible.

## Why build a conversational layer
- Enable “agencies” using Manny as a substrate.
- Provide observability/debuggability at path/node level.
- Support multi-role agent setups (e.g., Agents in Residence).
- Maintain multi-surface consistency (CLI/TUI/API) without drift.
- The conversational layer is only a better interface to the same core; no other runtimes.

## Shape (MVP)
1. **Agents** — role, permissions, POV.
2. **Tools** — single-purpose, reversible, auditable.
3. **Policies** — no hidden state; no persistent per-tool instructions; complete logs.

## MVP Protocol
1. Input → `Query`.
2. Agent system prompt defines role/boundaries, allowed tools/budget, seed.
3. Agent may call allowed tools or continue, but cannot exceed budget, use disallowed tools, create state outside working set; must log all actions.
4. Each agent turn emits a `Trace` or conversation transcript + tool logs consistent with graph traversal.
5. Constraints enforced: no external policy, no hidden state, no unbounded authority.

## Tools (MVP set)
- `Search` — deterministic search over corpus; returns rank + citation.
- `Fetch` — fetches/caches content with source citation.
- `ListDocs` — lists available docs/policies/tools.
- `ExplainPath` — renders a stored `Trace` (no narrative invention).

## Logging and artifacts
- Per agent turn: timestamp, role, input, tools (name/params/output), result text.
- All prompts/system messages recorded; seeds logged for determinism.

## Safety/abuse
- Hard limits on tool calls per turn.
- Define unsafe content handling (block/fallback).
- Sandbox content fetching.

## Post-MVP
Specialized tools or multi-agent setups can be added only if they honor the constraints above.
