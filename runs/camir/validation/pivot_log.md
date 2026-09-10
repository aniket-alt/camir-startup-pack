# CAMIR — Pivot log: what has already been killed, and the standing pivot-or-persevere criteria

**What this is** — the decision journal: nine directions that were considered and rejected before any code was written, with the reason and the evidence for each; and the standing criteria in the form *we pivot to X if Y by date Z*, written before any result exists.
**Why it exists** — the decisions in this pack that most constrain the company were made in reasoning, not in the market: routing was demoted from the claim to the object under measurement, the classifier was demoted to an ablation, share-of-savings pricing was kept only on the condition that CAMIR never computes its own invoice, and multi-tenant SaaS was refused despite being the only shape with operating leverage. **A future session, a new hire or a discouraged founder will re-open every one of these**, and without the reason recorded the re-opening will look like fresh thinking. The second job is harder: a pivot criterion written *after* a bad result is indistinguishable from a rationalisation, so all of them are dated and thresholded now.
**How to read it** — §1 is the record of what is closed; §2 is the only part that governs future behaviour. A skeptic should attack §2's dates: a criterion with no date is a wish, and the dates here are the ones most likely to slip.
**Depends on / feeds** — depends on [../BRIEF.md](../BRIEF.md), [../ASSUMPTIONS.md](../ASSUMPTIONS.md), [../HANDOFF.md](../HANDOFF.md) §2, [riskiest_assumptions.md](riskiest_assumptions.md), [stage_gate.md](stage_gate.md); feeds [../strategy/market_type.md](../strategy/market_type.md), [../financials/risk_matrix.md](../financials/risk_matrix.md) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

**Status: zero pivots have occurred**, because zero results exist. Everything in §1 is a decision taken in reasoning, dated 2026-09-09/10.

---

## 1. Already considered and killed

| # | The direction | Why it was killed | Evidence | Reversible? |
|---|---|---|---|---|
| **K1** | **"Our router is more accurate"** as the central claim | [S4] benchmarks **21 routing methods across 5 benchmarks**, finds them converged in a narrow band far below the oracle from a *predictability bottleneck*, with the best remedies worth **up to 2.13pp**. A claim an investor can falsify with one search is worse than no claim | [S4], [S8] | **No.** Reversing it means betting the company on the field's least movable variable |
| **K2** | Positioning on the **cost-quality plane** | That plane is what CAMIR *plots*, not what it competes on. The real axes are who owns the weights and who sets the quality tolerance | [../strategy/positioning.md](../strategy/positioning.md) | No — it would collapse CAMIR into the category [S1][S18] already occupy |
| **K3** | **Classifier route as primary** | Simple calibrated confidence routes as well as trained routers [S8]. The cascade is primary and the classifier is the ablation, reported against calibrated confidence rather than a fixed model | [S8], [S4] | Yes — if the ablation shows real lift, which is not expected |
| **K4** | **Share-of-savings computed by the vendor** — the FinOps default, where the party billing on the saving is the party measuring it | It is the exact incentive shape this pack accuses incumbents of [S17][S20], and Marcus's first objection. **Share-of-savings itself is kept** — it is the value metric in [../financials/pricing.md](../financials/pricing.md) and the only pricing that reaches an already-approved budget ([../strategy/petal_diagram.md](../strategy/petal_diagram.md)). What is killed is CAMIR computing it: the counterfactual runs as open code inside the customer's perimeter, and the eligible-savings definition is signed before measurement [S38][S39] | A5, [../tech/deep_dives.md](../tech/deep_dives.md) DD6 | **No.** A vendor-computed saving cannot be made credible after the fact. The declared fallback if the saving proves unbillable is ~5% of spend under management [S17] |
| **K5** | **Multi-tenant SaaS control plane** | The beachhead self-hosts for **data residency, not price** [S28]. Multi-tenancy moves prompts out of the perimeter and disqualifies P1 outright. Cost: no cross-customer learning, no pooled labels, poor field visibility | N4, [../tech/architecture/D06.md](../tech/architecture/D06.md) | No, for the beachhead. A separate hosted product for a different segment is a different company |
| **K6** | **Building a caching layer** | Caching composes upstream and takes 20–45% of traffic first [S36]. Building it would let CAMIR bank cache savings as routing savings | N3, [G4] | No — it is the incentive problem in miniature |
| **K7** | **Hosting models** | Puts CAMIR on the customer's uptime critical path and collapses G3: the vendor becomes the party computing the bill again | N2 | No |
| **K8** | **Latency SLO management** | A genuine third axis [S12], and it triples the frontier's dimensionality before the two-dimensional one has been measured once | N6 | Yes, after year one. [S12] is 2026 work |
| **K9** | **Outbound sales, cloud marketplaces, SI channel** | Computed, not assumed: marketplaces 84.3% net, SIs 59.8% net, outbound CAC ~117% of a $30,000 ACV | [../strategy/channel_plan.md](../strategy/channel_plan.md) | Only if ACV rises materially, which would be a different business |

**The four in K1–K4 are the ones a future session will re-open**, because each removes something that would make the pitch easier. K1 removes the demo every competitor gives; K4 removes the ability to present a saving CAMIR alone has calculated, which is the version of the number that is fastest to put in a deck.

---

## 2. Standing pivot-or-persevere criteria

Written 2026-09-10, before any result exists. Each names the trigger, the date it is evaluated, and **what specifically we do** — a criterion whose consequence is "reconsider" is not a criterion.

| # | We pivot if... | Evaluated by | We pivot **to** | Basis |
|---|---|---|---|---|
| **P1** | Median oracle ceiling across ≥ 3 realistic mixed-traffic corpora is **below the disqualification threshold** | End of the measurement track, Gate 1 | **Nothing — we stop, and publish the negative result.** It is genuinely publishable [G2] and it is the founder's own declared walk-away condition | A2, E1 |
| **P2** | The **artifact share is under 5pp** on well-instrumented pools | Gate 1 | Drop the harness thesis; CAMIR becomes an ordinary router in a plateaued field [S4] and there is no reason to prefer it. Treat as close to P1 | A13, E2, [S5] |
| **P3** | **≤ 3 of 20 screened teams qualify** on self-hosted production volume | After 20 screening calls | **Hybrid pools move to the primary roadmap** (A9 flips): admit hosted API models as a top tier and re-scope the frontier to mixed pools | A3, [G1] |
| **P4** | **≤ 2 of 5 design partners' consuming engineers** enable enforcement on their own endpoint within 6 weeks | Gate 2 | Sell the **measurement layer alone** — harness, ceiling, artifact decomposition, disqualification — as a paid product, with no request-path component at all | E13 |
| **P5** | Escalation rate is **above break-even on the majority of enforced endpoints** | 3 months of enforcement | The cascade does not pay at compressed self-hosted tier spreads [S29][S26]. Fall back to the classifier route as primary and re-run K3 | PR1, [S3] |
| **P6** | **Zero paid conversions** from ≥ 20 active OSS deployments | 12 months from first release | Open-core has the TensorZero shape [S23]. Move the control plane's paid boundary, or take the company to services — **and if neither, stop** | A4, [S40] |
| **P7** | **vLLM Semantic Router or an equivalent ships the measurement half**, not just dispatch | Quarterly review | The wedge is gone. The remaining question is whether the cost-axis derivation and judging protocol stand alone as a standard | [S11] |
| **P8** | The **LiteLLM strategy surface is refused or removed** | On notice | Standalone proxy becomes primary; re-price for a longer sales cycle. This is first in the business-model kill order and the dependency is **unagreed by anyone** | [../strategy/channel_plan.md](../strategy/channel_plan.md) |
| **P9** | Gross margin **below 60%** across ≥ 5 accounts | 12 months of revenue | Per-deployment classifier training is a service, not software. Either productise the fitting or reprice it as services and stop calling the margin software-shaped | E14 |

**P1 is the only one whose consequence is "stop", and it is deliberately first.** A pivot log where every branch leads to another attempt is a document about persistence rather than evidence.

---

## 3. What is explicitly *not* a pivot trigger

Each of these will feel like one at the time, and each is either expected or irrelevant.

| Not a trigger | Why |
|---|---|
| **The classifier fails to beat calibrated confidence** | This is the **expected** result [S4][S8]. It is published as an ablation, not treated as a setback. Pivoting on it would mean pivoting on a prediction the pack already made |
| **A disqualification loses a deal** | M16 requires the rate to be non-zero. A disqualification is the product working |
| **A competitor announces routing** | Already happened: a frontier vendor shipped routing as a built-in feature with no separate fee [S20]. It is the premise, not news |
| **GitHub stars are low** | TensorZero archived at 11,000+ [S23]. Stars are not the signal in either direction |
| **A design partner asks for fine-tuning, caching or latency SLOs** | N1, N3, N6. Being asked is confirmation the non-goals were the right ones to declare, not evidence they were wrong |
| **The tier cost spread compresses** | [S29] says it is already compressing and it compresses *against* CAMIR. It is on the risk matrix as a slope, not a cliff |

---

## 4. The re-opening protocol

When one of K1–K9 is proposed again — and K1 and K4 will be:

1. **Name the new evidence.** Not a new argument; new evidence. Every one of these was killed on reasoning from a cited source, and the same reasoning does not become wrong by being re-stated more forcefully.
2. **State what changed since 2026-09-10.** A source that superseded the one it was killed on, a measurement CAMIR now has, or a market fact.
3. **Write the reversal cost.** K1 and K5 are marked irreversible for a reason — a public claim of router superiority, once made, is falsifiable by one citation forever.
4. **Append here with a date.** Do not edit §1; add a row. **A pivot log that is rewritten is a story.**

---

## Recommended next 3

1. **Date P1 and P3 to specific calendar weeks now.** They are the two cheapest criteria to evaluate and the two most likely to slip indefinitely, because slipping them feels like continuing to work rather than avoiding an answer.
2. **Put P1 in writing in front of whoever funds this, before funding.** A declared willingness to stop on a measurement — rather than pivot toward whatever the measurement permits — is unusual, and it is far easier to hold when it was stated before there was anything to lose by holding it.
3. **Review P7 quarterly against the actual vLLM Semantic Router roadmap [S11], and write the one-line answer each time.** It is the only trigger whose timing is set by someone else entirely, and the only one that can arrive without any CAMIR result at all.
