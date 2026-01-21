# Jazz Narration Specification (v1.1 — Candidate)

## Status
**CANDIDATE — NON-BINDING**

This document defines constraints and invariants for a future Jazz narration layer that may involve LLMs. It is intentionally restrictive and descriptive. No implementation is authorized by this document.

---

## Purpose
Jazz narration exists to provide a *human-readable rendering* of what Manny did during a Jazz session.

Narration is **not** a reasoning surface, learning surface, planning aid, or insight generator. It must never influence traversal, routing, learning, or consolidation.

Narration is downstream of truth. All system truth remains in traces and artifacts.

---

## Core Invariants (Non-Negotiable)

1. **Trace-Grounded Only**  
   Narration may reference only data present in authoritative artifacts (e.g. traces, cost breakdowns, anchor metadata).

2. **Non-Authoritative**  
   Narration output is discardable. Deleting narration must not remove or alter any system truth.

3. **No Feedback Paths**  
   Narration must not influence routing, traversal, lenses, learning, κ, motifs, or mass.

4. **No Semantic Injection**  
   Narration must not introduce new entities, relations, concepts, or interpretations not present in traces.

5. **No Intent Attribution**  
   Narration must not ascribe goals, beliefs, desires, or intent to Manny.

6. **Replay Safety**  
   The same session replayed with narration disabled must produce identical traversal artifacts.

---

## Allowed Narration Inputs

Narration may consume only the following inputs:

- `trace.json` (paths, step order, termination reason)
- `interaction.json` (user input, goal mode, anchor metadata)
- `session_summary.json` (turn ordering)
- `narration_map.json` (explicit trace-to-text mappings)
- Logged lens parameters (e.g. novelty enabled, field targets)

No other data sources are permitted.

---

## Prohibited Narration Behaviors

Narration must not:

- Summarize across multiple traces
- Infer user intent or Manny’s intent
- Suggest what Manny should do next
- Explain why something “matters” or is “important”
- Perform abstraction, synthesis, or teaching
- Create new terminology
- Smooth over uncertainty or gaps

Narration must remain literal and grounded.

---

## LLM Usage Constraints (If Applicable)

If an LLM is used to render narration:

- The LLM acts as a **pure renderer**, not a reasoner
- Inputs to the LLM must be explicitly enumerated and logged
- Outputs must be:
  - Non-persistent
  - Non-referential (no memory of prior sessions)
  - Clearly marked as non-authoritative

LLMs must not be used to:
- Choose what to narrate
- Decide emphasis
- Generate explanations or insights

---

## Output Artifacts

Narration may be emitted as:

- `session_transcript.txt` (human-readable)
- Optional structured variants (e.g. JSON)

All narration artifacts must:
- Reference trace IDs explicitly or implicitly
- Include a `/why` hint for authoritative inspection
- Be reproducible from underlying artifacts

---

## Validation Requirements (Future)

Future narration implementations must demonstrate:

- **Trace Fidelity**: every narration segment maps to trace steps
- **Disable Safety**: turning narration off does not alter system behavior
- **Replay Equivalence**: export → replay produces identical traces with or without narration

---

## Explicit Non-Goals

Jazz narration does **not** aim to:

- Explain Manny’s reasoning
- Interpret meaning
- Teach concepts
- Provide advice or guidance
- Demonstrate intelligence
- Replace `/why`

---

## Relationship to Other Specifications

- Routing & Anchoring Specification: narration must not influence anchoring
- Lens Specification: narration may describe active lenses but must not justify them
- Gravity & Emergence Specification: narration must not claim gravity or meaning has emerged

---

## Summary Rule

> Narration may describe what Manny did, but must never describe what Manny *knows*, *intends*, or *should do next*.

---


---

## Appendix A — Minimal LLM Input Contract (Defensive, Spec-Only)

This appendix defines a **defensive input contract** for any future use of LLMs in Jazz narration. It is *spec-only* and does not authorize implementation.

### Purpose
Ensure that any LLM used for narration cannot access, infer, or introduce semantics beyond what is already present in authoritative artifacts.

### Allowed LLM Inputs
If an LLM is used, its input must be a strictly structured payload composed only of:

- Trace identifiers
- Ordered node/edge paths
- Step-level cost breakdown fields (verbatim values)
- Explicit anchor metadata (start/goal, sources)
- Explicit lens flags and parameters
- Termination reason

No free-form text, inferred summaries, or external context may be included.

### Prohibited LLM Inputs
An LLM must not receive:

- Natural-language user intent interpretations
- Historical session context beyond the current trace
- Prior narration outputs
- Any learning state, κ values, motifs, or derived analytics
- Any system prompts that encourage explanation, interpretation, or abstraction

### Output Constraints
LLM outputs must:

- Be renderings of provided inputs only
- Preserve ordering and structure of the underlying trace
- Avoid evaluative or interpretive language
- Include no speculative or inferential statements

### Auditability
All LLM inputs and outputs must be:

- Logged verbatim for audit
- Discardable without loss of system truth
- Reproducible in structure from the same inputs (wording may vary)

### Non-Goals
This contract does not permit:

- LLM-driven summarization
- LLM-driven insight generation
- LLM-driven goal suggestion or learning triggers
- Any form of semantic enrichment

---

End of document.
