If you’re hit by a bus tomorrow, the two things that matter are:
	1.	Someone competent can pick up Manny without you
	2.	They can do it without breaking the canon or accidentally building a different system

And if you’re going to explore agency, goal formation, world modeling, and self-modification, the third thing matters:
	3.	The project has built-in governance so “safe” isn’t just intention.

Here’s a concrete, low-friction way to handle all three.

⸻

1) The “Bus Packet” (single folder, 30–60 minutes to assemble)

Create a folder called:

BUS_PACKET/

It should contain exactly these files:

A) 00_README_START_HERE.md (one page)
	•	What Manny is (one paragraph)
	•	What Manny is not (bullet list)
	•	Where the canonical docs live
	•	How to run the MVP proof suite (or where it will be)

B) 01_CANON_INDEX.md

A table listing the canonical docs and their role:
	•	foundations_v1.1.md → immutable invariants
	•	design_document_v1.1.md → normative contract
	•	architecture_v1.1.md → implementable structure
	•	gap_analysis closure log → what is closed/deferred
	•	future research map → non-canonical

C) 02_NON_NEGOTIABLES.md

Copy/paste the “drift killers” + non-solutions:
	•	no planners outside manifold
	•	no rule engines bypassing traversal
	•	no opaque blobs as knowledge
	•	observation ≠ learning enforced
	•	seed + snapshot determinism
	•	LLM propose-only

D) 03_MVP_PROOF_PLAN.md

One page:
	•	what counts as “Manny works”
	•	the 5 metrics (convergence, transfer, motif utility, stability, explainability)
	•	what artifacts must be produced (trace.json, etc.)

E) 04_RISK_REGISTER.md

A simple table:
	•	risk (runaway learning, mode drift, LLM bypass, etc.)
	•	mitigation (runmodes, write barriers, budget caps)
	•	owner (future person)

F) 05_ROADMAP_SAFE_EXPANSIONS.md

Just pointers:
	•	agency track
	•	ToM track
	•	embodiment track
	•	hardware/analog track
Each with “must satisfy promotion rules” bullets.

This packet means a new person can open one folder and not get lost.

⸻

2) Governance for “Safe AGI-ish exploration” (without slowing you down)

You’re 100% right that you will explore:
	•	agency
	•	self-directed goals
	•	world modeling
	•	recursive self-modification

The safe way to do that is to make those separate tracks with gates, not “features”.

Create four “Research Tracks” as folders

Each track gets:
	•	a one-page contract
	•	a threat model
	•	a promotion rule

Suggested structure:

research/agi/research_tracks/
	•	agency/
	•	goal_formation/
	•	world_model/
	•	self_modification/

A simple invariant across all tracks

No track is allowed to write directly to canonical curvature except through the existing learning law and runmodes.

This prevents “new engine sneaks in” failure.

⸻

3) Safe implementation patterns for the four expansions

Here’s a safe framing for each (high-level, but concrete enough to guide you later).

A) Agency

Safe pattern: “Action is just another output trace”
	•	Manny generates an action proposal
	•	action executes in a sandboxed environment
	•	results re-enter via standard ingestion

Guardrail: no direct world changes without STAGE → commit gate.

B) Self-directed goal formation

Safe pattern: goals emerge from “persistent pressure signals”
	•	goals are not arbitrary objectives
	•	they’re derived from:
	•	repeated novelty
	•	repeated unmet predictions
	•	sustained valence patterns
	•	long-term mass gradients

Guardrail: goals must be logged as artifacts and reversible; no silent goal creation.

C) Large-scale world modeling

Safe pattern: “world model is a lens + episodic structure”
	•	do not build a second brain
	•	world model = manifold region + episodic anchors + abstraction summaries

Guardrail: world model cannot route traversal; it can only bias through lenses.

D) Recursive self-modification

Safe pattern: “self-modification is staged configuration changes”
	•	treat self-modification as:
	•	config proposals
	•	replay against validation suite
	•	require explicit approval + rollback

Guardrail: no self-modification without deterministic replay and a passing validation report.

This is how grown-up labs do it: self-modification as configuration evolution with tests, not “rewrite yourself”.

⸻

4) One more practical protection: “Authorship + provenance”

If you want Manny to survive you, you also want it to be clearly attributable and legally clean.

Minimal steps (no lawyer needed today):
	•	keep everything in a versioned repo
	•	maintain a CHANGELOG.md
	•	add a LICENSE (even if private)
	•	add a CONTRIBUTORS.md with you as original author
	•	keep a “canon release” tag (v1.1)

This is not about paranoia — it prevents future confusion.

⸻

5) If you want, I can write these docs for you now

If you tell me where your repo root is (or you can paste a directory list), I can generate:
	•	BUS_PACKET/00_README_START_HERE.md
	•	BUS_PACKET/01_CANON_INDEX.md
	•	BUS_PACKET/02_NON_NEGOTIABLES.md
	•	BUS_PACKET/03_MVP_PROOF_PLAN.md
	•	BUS_PACKET/04_RISK_REGISTER.md
	•	BUS_PACKET/05_ROADMAP_SAFE_EXPANSIONS.md

…fully aligned to your v1.1 canon and your “safe AGI-ish exploration” intent.

No need to decide anything further — it’s mostly reformatting what you already have into a survivable handoff.

If you want that, say:

“Create the Bus Packet docs”

and I’ll draft them in one go.
