# CAMIR — Value Proposition Canvas

**What this is** — Osterwalder's value proposition canvas for CAMIR's three decision-relevant personas: customer jobs, pains and gains on one side; pain relievers, gain creators and products on the other; with the top fit ranked per persona.
**Why it exists** — CAMIR's most tempting pitch — "cut your inference bill" — maps to a *gain* for a persona who does not sign (Marcus) and to nothing at all for the persona who can block the rollout (Ravi). Writing the canvas persona by persona is what exposes that the strongest pain reliever in the whole set is not a saving but an **attribution**: the ability to say which tier answered which request. Without this file, that feature reads as observability polish and gets deprioritised, and then the deployment dies the way the GPT-5 rollout was publicly criticised [S21].
**How to read it** — the **Rank** column is the output; everything above it is the derivation. Compare the #1 fit across the three personas — they are three different things, and that is the finding.
**Depends on / feeds** — depends on [personas.md](personas.md), [positioning.md](positioning.md), [../research/competitors.md](../research/competitors.md); feeds [../product/PRD.md](../product/PRD.md), [../product/features_prioritized.md](../product/features_prioritized.md), [../narrative/one_pager.md](../narrative/one_pager.md) and [../validation/discovery_guide.md](../validation/discovery_guide.md).

---

## Canvas 1 — P2 Marcus Bell, the engineer who owns the bill (beachhead)

### Customer profile

**Jobs to be done**

| # | Job | Type |
|---|---|---|
| J1 | Answer "can we make inference cheaper without breaking anything" with evidence rather than a guess | functional, recurring monthly |
| J2 | Keep six product teams' quality bars intact while changing shared infrastructure underneath them | functional |
| J3 | Not be the person who caused a silent quality regression | emotional — **the strongest of the three** |
| J4 | Spend as little of his own time as possible on a cost question that is not his roadmap | functional |
| J5 | Be able to defend the decision upward and downward with the same artifact | social |

**Pains**

| # | Pain | Severity | Evidence |
|---|---|---|---|
| Pn1 | He has never measured what the smaller tiers can handle on his traffic, and has no method to | high | The problem `../BRIEF.md` opens on; no published frontier exists for self-hosted pools [G2] |
| Pn2 | Any saving he claims is contestable because nobody agrees how it was computed | high | Router evaluations are not comparable across papers [S13]; vendors compute their own counterfactual [S17] |
| Pn3 | Static policies decay silently — his March A/B test is two model upgrades stale | high | Surface heuristics correlate weakly with difficulty and decay as traffic shifts [S4] |
| Pn4 | A quality regression would surface as a support ticket, weeks later, unattributable | **highest** | The documented failure mode when routing shipped vendor-side [S21] |
| Pn5 | Borrowed benchmark numbers do not transfer — 3.66× on MT-Bench is 1.41× on MMLU [S2] | medium | |

**Gains**

| # | Gain | Type |
|---|---|---|
| G1 | A number he chose, on his own traffic, that he can re-derive next quarter | required |
| G2 | 20–40% lower inference cost `(assumption: no CAMIR measurement exists)` | expected |
| G3 | Being the person who brought the method, not just the saving | desired |
| G4 | Finding out the answer is "no, your traffic is all hard" — cheaply, in a week | unexpected, and genuinely valuable |

### Value map

| Pain relievers | Addresses |
|---|---|
| **PR1 — Frontier measured on his own traffic**, not a public benchmark: run his logged requests through every tier, judge, plot | Pn1, Pn5 |
| **PR2 — Auditable savings**: the harness is open and runs in his perimeter, so the counterfactual is his to re-compute, not a vendor's to assert | Pn2 |
| **PR3 — Per-request tier attribution stamped on every trace**, so a quality question has an answer in minutes | **Pn4** |
| **PR4 — Shadow mode before enforcement**: route decisions computed and logged while all traffic still goes to the baseline | Pn4 |
| **PR5 — Continuous re-measurement**, because the frontier moves on every model upgrade | Pn3 |
| **PR6 — Declared quality tolerance with an alert when measured quality crosses it** | Pn4, J3 |

| Gain creators | Creates |
|---|---|
| **GC1 — He picks the point on the curve.** The product's job is to show the exchange rate, not to choose | G1, G3 |
| **GC2 — Oracle ceiling reported first**, so he learns in week one whether routing can help at all | **G4** |
| **GC3 — One artifact serves both audiences**: the same report defends the saving to Dana and the quality to Ravi | J5, G3 |

**Products:** open router (cascade + classifier routes) · frontier harness (mixed-difficulty benchmark + judging protocol + self-hosted cost axis) · control plane (per-deployment training, tolerance policy, savings attribution).

### Ranked fit for Marcus

| Rank | Fit | Why it ranks here |
|---|---|---|
| **1** | **PR3 + PR4 (attribution and shadow mode) → Pn4** | His highest pain is emotional and career-shaped: being the cause of an unattributable regression. Nothing else he can buy addresses it, and it is the reason a deployment survives month three |
| **2** | **GC2 (oracle ceiling first) → G4** | Cheap disconfirmation is worth more to a skeptical engineer than an optimistic projection, and it is CAMIR's most unusual offer: a product that tells you in week one not to buy it |
| **3** | **PR2 (auditable savings) → Pn2** | The incentive argument against every incumbent [S17][S20], and the one no hosted competitor can match |
| 4 | PR1 → Pn1 | The obvious value, and correctly *not* first: measurement without attribution gets deployed and then reverted |
| 5 | GC1 → G1 | The positioning claim; real, but it only matters once 1–3 hold |

---

## Canvas 2 — P4 Dana Okonkwo, Head of Platform (economic buyer)

**Jobs.** J1 Defend the infrastructure budget with a causal story. J2 Not approve something that breaks a product line. J3 Show a cost trend that bends without a headcount change.

**Pains.** Pn1 Inference grew 40% on flat headcount and "usage-based" is not an answer twice. Pn2 Vendor savings claims are unverifiable by anyone she trusts — the biller computes the counterfactual [S17]. Pn3 She cannot personally evaluate anything technical, so she is buying her engineer's judgment. Pn4 **"If it is open source, why am I paying?"** — the open-core question, and the one with the recent, dated cautionary tale [S23].

**Gains.** G1 A before-and-after on her own traffic that fits on one slide. G2 A named person who checked that nothing got worse. G3 A defensible unit — cost per request — that trends down.

| Pain relievers / gain creators | Addresses |
|---|---|
| **PR-D1 — A savings report produced by her own engineer from an open harness**, so the verifier and the vendor are different parties | Pn2, G2 |
| **PR-D2 — Cost per request as the reported unit**, not total spend, so growth and efficiency are separable | Pn1, G3 |
| **PR-D3 — A crisp open-core line**: the router is free forever; you pay for per-deployment training, tolerance policy and attribution | **Pn4** |
| **GC-D1 — One slide** — baseline cost, cost at declared tolerance, measured quality delta, who signed off | G1 |

**Ranked fit:** **1.** PR-D1 → Pn2/G2 (she is buying verification, not routing). **2.** GC-D1 → G1. **3.** PR-D3 → Pn4 — *and if this answer is not crisp, no other fit matters*, because the open-core question is asked in the first meeting.

---

## Canvas 3 — P5 Ravi Menon, the product engineer who can veto

The canvas that decides whether CAMIR survives contact with production.

**Jobs.** J1 Keep his feature's quality metrics flat or better. J2 Diagnose any regression fast enough to matter. J3 Retain control of his own dependencies.

**Pains.** Pn1 **He bears the risk and someone else banks the saving** — the structural asymmetry. Pn2 A quality change he cannot attribute to a cause. Pn3 Being told a shared-infrastructure change is not his decision. Pn4 The public precedent: complex queries degraded by a smaller model, with no dial [S21].

**Gains.** G1 Confidence that if quality moves he will know why within the hour. G2 A tolerance *he* sets for *his* endpoint. G3 Being consulted rather than informed.

| Pain relievers / gain creators | Addresses |
|---|---|
| **PR-R1 — Per-endpoint tolerance ownership**: the tolerance is his setting, not a platform-wide constant | **Pn1, Pn3, G2** |
| **PR-R2 — Tier decision on every trace**, queryable alongside his own metrics | Pn2, G1 |
| **PR-R3 — Unilateral pin-to-large, one flag, no ticket** | Pn3, G3 |
| **PR-R4 — Shadow mode with a per-endpoint report before any enforcement** | Pn4 |

**Ranked fit:** **1.** PR-R1 → Pn1 (it converts the asymmetry into a choice he owns; nothing else neutralises the veto). **2.** PR-R3 → Pn3. **3.** PR-R2 → Pn2.

---

## The finding: three personas, three different #1 fits

| Persona | Their #1 fit | What they are actually buying |
|---|---|---|
| **Marcus** (uses, decides) | Attribution + shadow mode | Not being blamed |
| **Dana** (signs) | An auditable report from a party that is not the vendor | Verification |
| **Ravi** (can veto) | A tolerance he owns for his own endpoint | Control |

**None of the three is "a cheaper bill."** The saving is the *occasion* for the purchase and the justification afterwards, but it is the top-ranked fit for nobody in the buying unit. Two consequences:

1. **Product.** Per-endpoint tolerance ownership, per-request tier attribution and shadow mode are **P0**, ranking above classifier accuracy. They are the top fit for all three personas and they are the features that make routing survivable in a shared-infrastructure organisation.
2. **Narrative.** The one-pager and deck open on **measured tolerance and auditable attribution**, with savings as the consequence. Opening on savings pitches a benefit that is free elsewhere [S20] and ranks first for no one.

## Recommended next 3

1. **Promote attribution, per-endpoint tolerance and shadow mode to P0 in the PRD** — this canvas is the justification, and without it they read as observability features and slip to "Later".
2. **Validate the ranking in discovery before building.** The Mom-Test form is past-behaviour: *"tell me about the last time a change to shared infrastructure affected your feature's quality — how did you find out, and how long did attribution take?"* See [../validation/discovery_guide.md](../validation/discovery_guide.md).
3. **Rewrite the one-pager's opening line from savings to tolerance** and check it against [positioning.md](positioning.md)'s anti-positioning list, which already bans the savings-first sentence.
