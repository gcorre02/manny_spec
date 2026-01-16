# Scale Tier Plan (Validation Framework Extension)

**Purpose:** Introduce scale gradually as an **opt-in stressor**, not a prerequisite, while preserving deterministic proof at small scale.

Principle:
- **Correctness must be provable at small scale.**
- **Scale is used to reveal failure modes**, not to establish correctness.

---

## 1) Tier Definitions

### Tier 0 — Toy Proof (default)
Use to prove: **M0–M4**, and initial M5/M6 correctness.
- Nodes: 10–50
- Edges: 10–200
- Interactions: 3–20
- ANN: brute
- RunMode: OBSERVE/STAGE/LEARN as required by milestone
- Expected runtime: seconds

**Why:** maximizes determinism, debuggability, and falsifiability.

---

### Tier 1 — Small Stress (precision & false-positive control)
Use to validate: **M5 motif false positives**, **M6 consolidation safety**.
- Nodes: 200–1,000
- Edges: 2,000–10,000
- Interactions: 50–300
- ANN: brute or HNSW (A/B)
- Expected runtime: seconds to a few minutes

**Why:** enough diversity to test motif precision and consolidation stability.

---

### Tier 2 — Medium Stress (stability & drift)
Use to validate: **consolidation drift**, **κ distribution stability**, **ANN parity**.
- Nodes: 5k–50k
- Edges: 50k–500k
- Interactions: 1k+
- ANN: HNSW primary, brute spot-check
- Expected runtime: minutes

**Why:** reveals long-tail effects, plateau collapse, over-pruning, and performance regressions.

---

### Tier 3 — Performance/Operations (optional later)
Use to validate: **efficiency gates**, operational envelopes, and service mode.
- Nodes: 100k+
- Edges: 1M+
- Interactions: 10k+
- ANN: HNSW required
- Expected runtime: bounded by CI budget / nightly

**Why:** tests real deployment constraints (not required for functional proof).

---

## 2) Tier Entry Criteria (when to scale up)

A pack may advance to the next tier only if:
1. The pack passes all required assertions in its current tier.
2. Gate A and Gate B remain green (no regressions).
3. The negative control packs for the same phenomenon pass.
4. Determinism holds for the tier’s intended mode (Toy and Small Stress must be deterministic; Medium/Perf may accept bounded nondeterminism only if explicitly documented).

---

## 3) Tiered Packs (how to structure packs)

Each phenomenon should have:
- **positive control pack** (expected behavior)
- **negative control pack** (expected absence)
- optional **stress variant** (pathology inputs)

Suggested naming:
- `packs/<phenomenon>/tier0/…`
- `packs/<phenomenon>/tier1/…`

A pack must declare its tier in `pack.json`:
```json
{
  "tier": "tier0",
  "scale_profile": { "nodes": 50, "edges": 200, "interactions": 10 }
}
```

---

## 4) What changes with scale (and what must not)

### What must not change
- Artifact contracts
- RunMode write barriers
- Gate definitions
- Ownership boundaries (engine vs harness)

### What may change (tier-dependent)
- ANN strategy (brute vs HNSW)
- Runtime budgets and timeouts
- Dataset size and diversity
- Frequency of CI execution (tier2+ may be nightly instead of per-PR)

---

## 5) Execution policy (CI vs nightly)

- Tier 0: required on every PR (fast).
- Tier 1: required on PRs affecting motifs/consolidation, otherwise nightly.
- Tier 2: nightly or on demand (resource-aware).
- Tier 3: optional operations validation (separate pipeline).

---

## 6) Success definition at scale

Scale validation is successful when:
- gates remain green under larger, more diverse packs
- failure modes are observed and logged deterministically
- regressions are caught early by tiered execution policy

Scale does not redefine truth; it reveals brittleness.

---

> **Principle:** Prove correctness small. Stress-test large.
