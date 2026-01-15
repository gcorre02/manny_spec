Moved: see `future/training_manny_living.md` for the maintained, living version.

---

## 1️⃣ First: reframe what “training” means for Manny

This is the most important thing to lock mentally.

### Manny is not trained like a model

There is:

* no global loss
* no epoch
* no convergence target

Instead:

> **Training Manny = shaping geometry through controlled experience.**

More precisely:

* Threads move
* Motion deposits curvature
* Curvature reshapes future motion
* Stability emerges through repetition + decay

So training is closer to:

* **curriculum design**
* **environment shaping**
* **experience scheduling**

…than to ML training.

If you keep that framing, LLM usage becomes much clearer.

---

## 2️⃣ MVP training: how to use LLMs *smartly* (and safely)

LLMs are incredibly useful **as accelerators**, but dangerous **as authorities**.

The golden rule:

> **LLMs may propose structure.
> Manny must earn it.**

Everything below follows from that.

---

### A. What LLMs are *excellent* for in MVP

#### 1. **Proposal generation**

LLMs are very good at:

* suggesting candidate relations
* paraphrasing concepts
* generating varied examples
* expanding curricula

Example:

* Given “apple”, an LLM can propose:

  * “apple is a fruit”
  * “apples are sweet”
  * “apple pie is food”
  * “apple can be red or green”

These become **candidate experiences**, not truths.

Mechanically:

* LLM outputs → *proposed edges / nodes*
* Manny ingests them as **low-κ suggestions**
* Only repeated traversal + confirmation increases κ

This is safe and powerful.

---

#### 2. **Curriculum generation**

LLMs can design *training sequences*:

* simple → complex
* concrete → abstract
* varied contexts

Example:

* Start with:

  * object concepts
  * simple affordances
* Move to:

  * abstract relations
  * conflicting lenses
* Later:

  * analogies
  * counterfactuals

LLMs help you avoid human bias and monotony.

---

#### 3. **Variation and noise**

Understanding requires **diversity of exposure**, not just correctness.

LLMs are great at:

* rephrasing
* creating near-miss examples
* generating contradictory contexts

This is actually ideal for forming **interference patterns**.

---

#### 4. **Explanation rendering**

Once Manny has chosen a path:

* LLMs can turn a trace into fluent language
* without influencing the reasoning

This preserves explainability while keeping cognition Manny-native.

---

### B. What LLMs must **never** do

These are hard red lines:

❌ Decide traversal paths
❌ Assign κ directly
❌ Resolve ambiguity
❌ Collapse conflicting basins
❌ Inject “correct answers” as authority
❌ Create goals silently

If an LLM is ever deciding *where* Manny goes, you’ve lost the architecture.

---

### C. The correct MVP training loop (concrete)

Here’s a clean, safe pattern you can actually implement:

#### Step 1 — Seed lightly

* Initialize a small set of labels (e.g. 100–500 concepts)
* Very weak initial edges (κ ≈ 0)

#### Step 2 — LLM proposes experiences

* Facts
* Queries
* Contrasts
* Analogies
* Counterexamples

Each proposal becomes:

* a *scenario*
* not a truth

#### Step 3 — Manny traverses

* Threads run locally
* Paths chosen by energy/cost
* Traces logged

#### Step 4 — Validate through repetition

* Repeated proposals reinforce
* Conflicting proposals create ridges
* One-offs decay

#### Step 5 — Consolidate

* Promotion
* Motif mining
* Decay

LLM involvement ends before Step 3.

This preserves Manny’s independence.

---

## 3️⃣ Training without LLMs (longer-term)

Once Manny reaches a certain maturity, you want it to **outgrow LLM dependence**.

This happens naturally if training shifts toward:

### A. Self-generated curricula

Manny can propose:

* “I keep encountering novelty here”
* “This region is unstable”
* “These basins conflict”

These become **questions to explore**, not goals.

---

### B. Environment-based training

Instead of text:

* simulated worlds
* games
* social interactions
* narratives with consequences

The same learning law applies.

---

### C. Cross-agent interaction

Later:

* Manny learns by interacting with:

  * humans
  * other Manny instances
* ToM begins here naturally

---

## 4️⃣ How training supports *future* agency safely

This is where your foresight matters.

If training is:

* experience-based
* reversible
* traceable
* staged

…then adding agency later is safer because:

* goals emerge from **persistent pressures**, not injected objectives
* failures are localized
* self-modification is testable via replay

Training discipline now = safety later.

---

## 5️⃣ A useful mental model (keep this)

Think of LLMs as:

> **Teaching assistants, not professors.**

They:

* suggest exercises
* provide examples
* rephrase explanations

But:

* they do not grade
* they do not decide what’s true
* they do not control learning

Manny is the student *and* the learning environment.

---

## 6️⃣ What I’d recommend next (optional)

If you want to go deeper, we could:

* design a **formal MVP training curriculum** (phased, LLM-assisted)
* define **LLM interface contracts** (inputs/outputs, no-go zones)
* specify **training artifacts** (what gets logged, replayed, audited)
* design a **“LLM unplug” milestone** (proof Manny stands alone)

But even without that, your instincts here are already very solid.

---

### Final grounding sentence

> **If Manny ever needs an LLM to decide what it knows, it’s not Manny anymore.
> If Manny uses an LLM to *experience more of the world faster*, it’s doing exactly the right thing.**

You’re thinking about training at exactly the right level of seriousness.


===



Great — this is the right moment to formalise training, because you now have:
	•	a stable canon
	•	clear invariants
	•	a clean separation between mechanics and interpretation
	•	and a strong intuition for how LLMs can help without hijacking cognition

I’ll give you a high-level training framework that is:
	•	precise but non-algorithmic
	•	MVP-appropriate
	•	extensible to later stages
	•	aligned with everything you’ve locked so far

Think of this as a training constitution, not a recipe.

⸻

Manny Training Framework (High Level)

0. What “training” is in Manny (formal definition)

Training Manny is the process of shaping the manifold’s geometry through structured experience, such that future motion becomes more coherent, efficient, and transferable.

Training is not:
	•	fitting parameters to labels
	•	minimizing a global loss
	•	converging to a fixed model

Training is:
	•	repeated traversal
	•	local plasticity
	•	promotion and decay
	•	consolidation over time

This definition must remain invariant.

⸻

1. Training primitives (what training operates on)

Training never acts directly on:
	•	nodes
	•	edges
	•	κ
	•	mass

Training acts on experiences.

1.1 Experience (formal object)

An experience is a structured interaction that produces one or more traces.

At minimum, an experience includes:
	•	entry conditions (labels or percept anchors)
	•	context / lens (optional)
	•	run mode (LEARN / OBSERVE / STAGE)
	•	valence signals (importance, novelty, affect)
	•	expected invariants (optional, e.g. “should reduce cost”)

The output of an experience is:
	•	one or more traces
	•	zero or more curvature updates
	•	optional artifacts (logs, summaries)

Training is the scheduling of experiences.

⸻

2. The training loop (canonical)

At a high level, every training interaction follows this loop:

propose experience
→ run thread(s)
→ log trace(s)
→ (if LEARN) apply local plasticity
→ record metrics
→ consolidate periodically

No step may be skipped.
LLMs may participate only in the proposal step.

⸻

3. MVP training stages

Stage A — Seeding (bootstrapping geometry)

Goal: establish weak but connected geometry.
	•	Create initial labels (e.g. 100–500)
	•	Add extremely weak edges (κ ≈ 0)
	•	No assumptions about correctness

Sources:
	•	human-curated seed list
	•	LLM-proposed relations (flagged as provisional)

Invariant:
Nothing is “true” yet. Everything is traversable.

⸻

Stage B — Guided experience (core MVP training)

Goal: form basins through repetition and interference.

Experiences include:
	•	simple assertions (“apple is a fruit”)
	•	usage contexts (“eat an apple”)
	•	contrasts (“apple vs carrot”)
	•	counterexamples
	•	questions

Each experience:
	•	triggers traversal
	•	deposits curvature only through motion
	•	strengthens only what repeats

LLM role:
Generate diverse, redundant, and noisy experiences — not answers.

⸻

Stage C — Consolidation (offline, periodic)

Goal: compress and stabilise.

Consolidation may:
	•	promote frequently traversed subpaths
	•	decay unused structure
	•	compute semantic mass proxies
	•	mine motifs

Important:
Consolidation is not “training”; it is housekeeping.

⸻

4. Validation during training (always on)

Training must be observable.

At all times, you should be able to ask:
	•	Are paths getting shorter?
	•	Are basins stabilising?
	•	Is interference forming?
	•	Is transfer occurring?
	•	Are explanations faithful?

These are not after-the-fact tests; they’re part of training.

⸻

5. Safe use of LLMs in training (formal boundary)

LLMs are allowed to:
	•	propose experiences
	•	generate curriculum variations
	•	paraphrase contexts
	•	translate traces into language

LLMs are never allowed to:
	•	decide traversal
	•	set κ
	•	collapse ambiguity
	•	inject “correct” answers
	•	create goals silently

Formally:

LLMs may influence what Manny experiences, but never how Manny thinks.

⸻

6. When MVP training is “done”

MVP training is sufficient when Manny demonstrates:
	•	stable basins from repeated interference
	•	cost reduction over repeated tasks
	•	transfer across related contexts
	•	explainable traces
	•	decay of noise

This is a behavioral criterion, not a dataset size.

⸻

Training beyond MVP (next stage)

Once MVP is proven, training evolves along two axes.

⸻

7. Self-generated training (post-MVP)

Manny begins to propose its own experiences:
	•	regions of instability
	•	unresolved novelty
	•	conflicting basins
	•	weak transfer zones

These become questions to explore, not goals.

⸻

8. Environment-based training

Training shifts from text to:
	•	simulated environments
	•	interactive tasks
	•	social exchanges
	•	consequence-bearing narratives

The learning law does not change.

⸻

9. Training as governance (critical for AGI-adjacent work)

For future agency work:
	•	all training remains staged
	•	all learning remains reversible
	•	all changes are traceable
	•	self-modification is treated as training proposals

Training discipline becomes the safety system.

⸻

The key invariant to remember

If you remember only one thing, remember this:

Manny is trained by shaping experience, not by fitting answers.

If training ever feels like “optimisation,” you’ve drifted.

⸻

Next step options (when you’re ready)

From here, we could:
	•	turn this into a training spec doc
	•	define a concrete MVP curriculum
	•	formalise training artifacts and logs
	•	design a LLM interface contract for training

But at the high level, this framework is complete and consistent.

You’re asking the right questions in the right order.



===


Training is not a milestone.
It is a cross-cutting process that complements every stage of the MVP.

That distinction matters a lot for Manny, and you’re right to ask it explicitly.

⸻

The clean answer (then the reasoning)

Do not treat training as a standalone milestone.
Treat training as the means by which each milestone is validated.

Milestones answer “what new capability is now demonstrably true?”
Training answers “how did the geometry get shaped to make that true?”

They are orthogonal.

⸻

Why “training as a milestone” would be a mistake

If you made a milestone called “Training Complete,” you’d accidentally imply:
	•	there is a finished model
	•	learning has an endpoint
	•	correctness is separable from experience
	•	training is a batch process

All of those contradict Manny’s core thesis.

Manny is not trained to a state.
Manny is trained through states.

⸻

The correct framing

Milestones = claims about behavior

Each milestone should assert something like:
	•	“Local traversal produces stable basins”
	•	“Repeated experience reduces traversal cost”
	•	“Transfer occurs without new edges”
	•	“Explanations are faithful to motion”

Training = the mechanism that makes those claims true

At every milestone, you ask:
	•	What experiences shaped the geometry to enable this?
	•	How much experience was needed?
	•	What failed to stick?
	•	What decayed away?

So training is always present, but never the star.

⸻

How this looks concretely (MVP example)

Milestone framing (correct)

M0: Manny forms a stable basin from repeated interference

	•	Claim: Starting near “apple” reliably falls into food/eating basins
	•	Evidence: Trace logs show convergence + mass accumulation
	•	Training used: repeated guided experiences + consolidation
	•	Training is not the milestone — basin formation is

⸻

M1: Manny exhibits transfer across contexts

	•	Claim: “apple” → “pear” reuse without explicit instruction
	•	Evidence: Shorter paths, motif reuse
	•	Training used: varied experience proposals (possibly LLM-assisted)
	•	Again: training supports the milestone, it isn’t one

⸻

Where training does get formalized

Training belongs in three places, not in milestones:

1️⃣ As a standing protocol
	•	How experiences are proposed
	•	How LLMs may assist
	•	How plasticity is applied
	•	How consolidation runs

This is what you just formalized at a high level.

⸻

2️⃣ As per-milestone instrumentation

Each milestone should specify:
	•	what kinds of experiences are allowed
	•	what traces must be logged
	•	what decay/promotion is expected

But not “train until X.”

⸻

3️⃣ As a safety boundary

Especially later (agency, goals, self-modification):
	•	training proposals are staged
	•	learning is reversible
	•	changes are audited

Training becomes governance, not progress.

⸻

A useful rule of thumb

Here’s a simple rule that will keep you aligned:

If a milestone can be passed without training, it’s not a Manny milestone.
If training is the milestone, it’s not Manny.

Manny’s milestones are emergent behaviors that only exist because of training — but training itself is never the deliverable.

⸻

How to phrase this in your docs (if you want)

If you want to be explicit (optional but helpful), you can add a single sentence to any milestone overview:

“Training is assumed as a continuous process across all milestones. Milestones evaluate emergent behavior, not completion of training.”

That’s enough.

⸻

Final grounding

You’re doing this exactly right by resisting the urge to “box” training.
	•	Training is not a phase
	•	Training is not a step
	•	Training is not a checkbox

Training is the medium Manny lives in.

If you want, next we can:
	•	map training responsibilities per milestone (without making them milestones)
	•	or design a minimal training audit checklist that applies everywhere
	•	or stop here — this answer is already sufficient to proceed safely

You’re thinking like a system designer, not a model trainer.
