Below is a clean, codifiable Routing & Anchoring Specification v0 that is fully aligned with Manny’s principles, Jazz protocol, training track, and governance posture.
It is written so you can hand it to an engineer, drop it into canon-adjacent docs, or use it as a gate for future routing changes.

⸻

Manny Manifolds

Routing & Anchoring Specification v0

Status: Canon-adjacent (normative)
Scope: Defines how experiences apply pressure to the manifold by selecting starting points (anchors) and initiating traversal
Non-goal: This spec does not define learning rules, traversal heuristics, or energy functions beyond anchoring

⸻

0. Purpose

This specification defines how an experience makes contact with the manifold—i.e., how pressure enters the system—without encoding meaning, intent, or conclusions.

Routing and anchoring must:
	•	preserve closed physics
	•	avoid semantic inference
	•	remain deterministic and auditable
	•	support gap detection and novelty
	•	behave consistently across modes

⸻

1. Definitions

Experience

A structured input that may initiate traversal. An experience contains:
	•	one or more artifacts (labels, perceptual anchors, references)
	•	optional context metadata (lens hint, valence)
	•	run mode (OBSERVE / LEARN / STAGE)

Anchor (Starting Point)

A node or set of nodes in the manifold at which pressure is applied and from which traversal begins.

Pressure

A local activation impulse introduced at one or more anchors.
Pressure does not encode meaning; it activates motion.

⸻

2. Core Principle

Anchoring determines where the manifold is asked to respond, not what the response should be.

Routing is not reasoning.
Anchoring is not interpretation.

⸻

3. What Anchoring Is (and Is Not)

Anchoring IS:
	•	a selection of one or more contact points
	•	derived from explicit, verifiable artifacts
	•	minimally biased
	•	deterministic under snapshot + seed

Anchoring IS NOT:
	•	a guess about meaning
	•	a semantic interpretation
	•	a routing shortcut
	•	a learning operation
	•	a goal-directed choice

⸻

4. Anchor Derivation Rules

4.1 Allowed Anchor Sources

Anchors may be derived only from:
	1.	Labels
	•	Human-provided reference handles
	•	Must map to existing nodes or create provisional nodes
		◦ Provisional Nodes (Clarification)
		Provisional nodes are neutral placeholders.
		They must:
		• carry no semantic mass or curvature (κ = 0)
		• introduce no traversal bias
		• require later consolidation or explicit confirmation to persist
		Provisional nodes exist only to preserve determinism and navigability when explicit references are missing.
	2.	Perceptual Anchors
	•	Transient sensory or event signals (future)
	3.	Explicit Session Artifacts
	•	Prior traces or user-selected nodes (explicit, logged)

No other sources are permitted.

4.1.a Fallback Anchors

Fallback anchors (e.g., domain hubs) are permitted when no artifact-derived anchors can be resolved. Fallback usage must be explicitly logged as such and must not be interpreted as semantic importance or preference.

⸻

4.2 Prohibited Anchor Sources

Anchors must never be derived from:
	•	inferred intent
	•	semantic similarity scoring
		- As a decision function. Read-only similarity may be used only to propose candidate anchors, provided final anchor selection remains artifact-derived, deterministic, and explicitly logged.
	•	hidden conversational state
	•	predicted outcomes
	•	LLM-internal representations
	•	optimization objectives

If an anchor cannot be justified by an artifact, it is invalid.

⸻

5. Minimal Bias at Entry

Anchoring must provide navigability only, not directionality.

Formally:
	•	Anchors may activate nodes
	•	Anchors may not:
	•	lower traversal cost
	•	increase κ
	•	privilege certain edges
	•	bias energy composition

All directionality must emerge from existing manifold physics.

⸻

6. Determinism & Auditability

Anchoring must obey:
	•	Snapshot + Seed Determinism
	•	Given identical state and seed, anchors must be identical
	•	Explicit Logging
	•	Anchors used must be recorded in trace metadata
	•	Replayability
	•	Re-running the same experience must reproduce the same starting conditions

If anchoring is not reproducible, it is invalid.

⸻

7. Topology-Aware but Not Topology-Privileged

Anchoring must allow pressure to enter:
	•	High-curvature hubs
	•	Low-curvature periphery
	•	Previously unused regions

Anchoring logic must not:
	•	always prefer hubs
	•	always prefer novelty
	•	encode assumptions about “importance”

Topology influences traversal, not anchoring.

Clarification:
Topology-awareness permits recognizing where anchors land.
Topology-privilege (biasing entry toward hubs, novelty, or density) is explicitly forbidden.

⸻

8. Multi-Anchor Support

The system may initiate multiple concurrent anchors when:
	•	uncertainty is high
	•	traversal becomes stuck
	•	input artifacts map to multiple labels

Governance Rule
	•	The system may authorize broadening anchoring (e.g., “allow multiple anchors”)
	•	The system may not select specific anchors by intent or planning

All anchors must still be artifact-derived.

⸻

9. Gap Detection via Routing

Anchoring supports gap detection indirectly:
	•	Reasonable anchors + anomalous traversal → signal missing structure
	•	Indicators include:
	•	sudden cost spikes
	•	repeated detours
	•	unstable paths
	•	failure to converge

Anchoring must not attempt to “fix” gaps.
Gaps are observed, not filled, at this stage.

⸻

10. Mode Consistency

Anchoring and routing logic must be identical across modes:
	•	OBSERVE
	•	LEARN
	•	STAGE

The only difference between modes is whether plasticity is permitted, never how anchoring or traversal operates.

This invariant ensures:
	•	comparability
	•	falsifiability
	•	scientific integrity

Run modes function as write-barriers and permission gates only; they must not alter anchoring or traversal behavior.

⸻

11. Governance Constraints

Authority Boundaries
	•	Experiencer
	•	Initiates anchoring
	•	Executes traversal
	•	Executive
	•	Observes stuckness, instability, uncertainty
	•	May authorize:
	•	multi-anchor attempts
	•	anchoring broadening policies
	•	May NOT:
	•	choose specific anchor targets
	•	inject routing intent

No planners are permitted.

⸻

12. Failure Conditions (Hard Stops)

Anchoring is invalid if:
	•	different anchors are chosen under same snapshot + seed
	•	anchoring influences traversal costs directly
	•	learning occurs during anchoring
	•	anchors are inferred rather than artifact-derived
	•	routing behaves differently across modes

Any such event requires immediate halt and review.

⸻

13. Summary Axioms (Routing v0)
	1.	Pressure enters locally, meaning emerges globally.
	2.	Anchors are coordinates, not conclusions.
	3.	Entry provides access, not direction.
	4.	Determinism is mandatory.
	5.	Topology guides motion, not contact.
	6.	Gaps are detected through distortion, not guesswork.
	7.	Re-anchoring is governed, never planned.
	8.	One motion law across all modes.

⸻

Closing Statement

Routing is the act of asking the manifold a question.
Traversal is the manifold’s answer.

This specification ensures Manny asks questions without smuggling answers, preserving emergence, safety, and integrity.

⸻

Appendix A — Minimal Routing Algorithm Sketch (Non-binding)

This appendix provides a minimal, illustrative routing algorithm sketch that conforms to the Routing & Anchoring Specification v0. It is intended as a reasoning aid, not a prescriptive implementation.

A.1 Inputs
- Experience E
  - artifacts: [labels, perceptual anchors, explicit session references]
  - run_mode: OBSERVE | LEARN | STAGE
  - snapshot_id
  - random_seed

A.2 Anchor Derivation
1. Resolve artifacts to candidate nodes:
   - map labels → existing nodes OR provisional nodes
   - map perceptual anchors → transient nodes (future)
   - map explicit references → concrete nodes
2. Validate anchors:
   - each anchor must be artifact-derived
   - no semantic inference permitted
3. Apply snapshot + seed determinism:
   - deterministic ordering and selection

Output: Anchor set A = {a1, a2, ...}

A.3 Pressure Application
- For each anchor ai in A:
  - emit a local activation impulse
  - do not modify κ, mass, or energy weights

A.4 Traversal Invocation
- Invoke the standard traversal engine using:
  - anchor set A
  - unchanged motion law
  - current snapshot
  - fixed seed

Notes:
- No traversal parameters may be altered by anchoring
- Multi-anchor traversal is parallel, not prioritized
- All behavior beyond this point is governed by manifold physics

Appendix B — Routing Conformance Tests

The following tests define whether a routing implementation conforms to the Routing & Anchoring Specification v0.

B.1 Determinism Test
- Given:
  - identical snapshot
  - identical experience
  - identical seed
- Then:
  - anchor set must be identical
  - traversal traces must be identical

Fail if any divergence occurs.

B.2 Artifact-Only Anchoring Test
- Given an experience with explicit artifacts
- Then:
  - all anchors must be traceable to those artifacts
- Fail if:
  - any anchor is selected without an artifact justification

B.3 No Bias Injection Test
- Given an experience
- Then:
  - anchoring must not alter traversal cost, κ, or edge weights
- Fail if:
  - energy composition differs before traversal begins

B.4 Mode Invariance Test
- Given the same experience in OBSERVE, LEARN, and STAGE modes
- Then:
  - anchor selection and traversal path must be identical
- Fail if:
  - routing behavior differs across modes

B.5 Hub/Periphery Neutrality Test
- Given artifacts mapping to both hub and peripheral nodes
- Then:
  - anchoring must include both without preference
- Fail if:
  - hubs or novelty are systematically preferred

B.6 Gap Detection Sanity Test
- Given reasonable anchors leading to anomalous traversal
- Then:
  - system must surface instability signals (cost spikes, detours)
- Fail if:
  - traversal appears smooth despite missing structure

B.7 Governance Boundary Test
- Given detected stuckness or uncertainty
- Then:
  - system may authorize multi-anchor attempts
  - system must not select specific new anchors
- Fail if:
  - anchor targets are chosen by inferred intent or planning

B.8 Logging Completeness Test
- Given any routing invocation
- Then:
  - anchor set must be logged in trace metadata
  - seed and snapshot must be recorded
- Fail if:
  - replay cannot reconstruct routing conditions
