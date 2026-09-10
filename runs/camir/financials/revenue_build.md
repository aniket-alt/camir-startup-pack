# CAMIR — Revenue Build

**What this is** — the bottom-up path from $0 to $1M, $10M and $50–100M ARR: customers × conversion × price per year, the expansion layers, and the named milestone that causes each bend.
**Why it exists** — [../strategy/market_sizing.md](../strategy/market_sizing.md) ends on a finding the rest of the pack has to absorb: **routing fees alone are not a venture-scale business at 2026 denominators**, and it explicitly asks this file to *name and size the second product* rather than leaving "control plane" as a word. If that request goes unanswered, the pack contains a $20M SAM and a $100M ambition with nothing connecting them — which is the exact shape of a deck that dies in the second meeting.
**How to read it** — §Product 2 first; it is the load-bearing section and everything past year four depends on it. Then §The build, which a spreadsheet can be reconstructed from line by line. A skeptic should attack the Y4→Y5 ACV lift from $45k to $50k and the population widening behind it.
**Depends on / feeds** — depends on [pricing.md](pricing.md), [../strategy/market_sizing.md](../strategy/market_sizing.md), [../strategy/channel_plan.md](../strategy/channel_plan.md), [../strategy/gtm.md](../strategy/gtm.md); feeds [unit_economics.md](unit_economics.md), [use_of_funds.md](use_of_funds.md), [comps_exits.md](comps_exits.md) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

**No revenue exists.** Year 1 begins after the first paid contract, which does not yet have a date. Every line is an assumption with its basis stated.

---

## The problem this file has to solve

| | Value | Source |
|---|---|---|
| Routing-fee TAM, base case, 2026 | **$70M/yr** (corridor $10–360M) | [../strategy/market_sizing.md](../strategy/market_sizing.md) |
| SAM after year-one product restrictions | **$20M/yr** | same |
| Reachable companies, 2026 | **~1,370** (corridor ~470–3,800) | same |
| Y3 SOM | **68 customers, $2.4M ARR** | same |

**$50M of ARR does not exist inside a $20M SAM.** No mix of conversion assumptions fixes that; the denominator has to change. Three things change it, and only the third is a decision rather than a hope:

1. The denominator compounds at **~26% CAGR** [S24]. That is a 3.2× lift over five years — real, but it arrives regardless of what CAMIR does.
2. **Hybrid pools** (A9): admitting hosted API models as an additional top tier widens the spend pool from self-hosters to some teams running mixed inference. Total hosted-API spend is ~$9.6B against ~$2.9B self-hosted [../strategy/market_sizing.md](../strategy/market_sizing.md), a **3.3× spend-pool ratio, not a company-count multiplier**; the reachable population must be re-derived separately.
3. **A second product priced against total inference spend rather than routing savings.** This is the one that has to be named, and §Product 2 names it.

---

## Product 2 — named and sized

**Product 1** is the routing control plane: per-deployment classifier training, tolerance policy, savings measurement and attribution for **tier selection**. Priced at 28% of measured savings. ACV ~$30k. Its ceiling is the $20M SAM.

**Product 2 — the inference-efficiency control plane.** The same measurement machinery — the benchmark harness, the judging protocol, the self-hosted cost axis, the frontier — applied to **every lever that trades inference cost against answer quality**, not just tier selection. Routing is the first of these levers; it is not the only one, and it is not the largest.

| Lever | What is decided | What CAMIR measures | Who does this today |
|---|---|---|---|
| **Tier selection** (Product 1) | Which tier answers this request | Cost-quality frontier across the model pool | CAMIR, or nobody |
| **Cache policy** | Similarity threshold for a semantic-cache hit | The quality cost of a stale hit — currently **unmeasured by anyone** [G4]; hit rates run 20–45% in production [S36] | Nobody. Thresholds are set by feel |
| **Quantisation tier** | FP16 vs INT8 vs INT4 per tier | Elo/correctness loss per unit of memory and throughput gained | Nobody, per deployment |
| **Batch and utilisation policy** | Batch size, concurrency, when to consolidate onto fewer GPUs | Latency/quality cost of higher batching — and **an idle GPU at 10% utilisation costs 10× per token** [S27] | Serving engineers, by intuition |
| **Model-upgrade regression** | Is the new pool member actually better on *our* traffic | Frontier movement across a pool change | Nobody. This is why internal routers go stale ([../strategy/petal_diagram.md](../strategy/petal_diagram.md), Petal 5) |

**The unifying claim, stated as a hypothesis:** all five decisions share an auditable measurement pattern — *what does this change cost me in quality, on my traffic, judged by a protocol I can audit* — while latency, throughput, memory and operational constraints remain distinct per lever [S12]. A team that buys CAMIR for tier selection may already have the apparatus for the other four, but discovery must test that before code is written. `(assumption: no customer has asked for levers 2–5; see` [../validation/discovery_guide.md](../validation/discovery_guide.md) `.)`

### Why Product 2 must be priced differently

Product 1's metric — share of *routing* savings — cannot price levers 2–5, because their savings are not separable. If a cache-threshold change, a quantisation change and a routing change all land in the same month, no attribution scheme divides the bill honestly. Product 2 is therefore priced as **2–3% of inference spend under management**, which is the repricing trigger already declared in [pricing.md](pricing.md).

That change of metric is what breaks the $20M ceiling, and the reason is arithmetic, not ambition: Product 1 charges a percentage of a *saving*, Product 2 charges a percentage of the *bill*. The bill is roughly 5.7× the saving ($600k vs $105k).

### Product 2, sized

| Step | Value | Basis |
|---|---|---|
| Total self-hosted inference spend, 2026 | $2.9B | [../strategy/market_sizing.md](../strategy/market_sizing.md) line c, base case |
| Grown at 26% CAGR to 2031 [S24] | **$9.2B** | ×3.18 |
| Widened to hybrid pools (self-hosted + hosted, A9) | **$30.5B spend pool scenario** | ×3.3 spend-pool ratio, not a company-count estimate ([../strategy/market_sizing.md](../strategy/market_sizing.md)) |
| Control-plane take rate | ×1.5–2.5% | `(assumption: below OpenRouter's 5% gateway take` [S17] `because CAMIR is not in the request path for levers 2–5, and observability/FinOps layers typically price at low single digits of the spend they govern)` |
| **Product 2 TAM, 2031** | **$460M–$760M** | self-hosted-only floor: **$140–230M** |

**Product 2's TAM is roughly 6–10× Product 1's**, and that ratio — not the absolute number — is the claim worth defending. Both descend from the same anchor [S17] and inherit the same f1/f2 uncertainty, so **the corridor on Product 2 is at least as wide as the 36× corridor on Product 1.** Anyone quoting $760M without f2's caveat is quoting a guess twice over.

**Honest counterweight:** every lever in Product 2's table is adjacent to something a serving engine could absorb [S11], the way routing is being absorbed. The defence is the same as Product 1's and no stronger: **the measurement layer is the asset; the mechanism commoditises.** A serving engine can ship a cache threshold; it cannot ship a judged, auditable, per-customer frontier that a Head of Platform defends in a budget review.

---

## The build

Units × conversion × price, per year. Year 1 = first year with a paid contract.

### Assumption block (change these and everything below recomputes)

| # | Assumption | Value | Basis |
|---|---|---|---|
| R1 | Gross logo churn, Y1–Y4 | **15%/yr** | `(assumption: deliberately high. Switching cost is low — CAMIR is a proxy, change the base URL and you are out; A6 says so in the founder's own words)` |
| R2 | Gross logo churn, Y5+ | **15%/yr in the table; 12%/yr is an unbanked upside** | `(assumption: churn may improve once a signed savings definition and an attribution history exist, but A6 is untested, so the table below holds 15% in every year — Y5 churn 19 of 128, Y8 97 of 648 — and does not bank the improvement. At 12% from Y5, Y8 ending customers would be ~1,040 rather than 1,001)` |
| R3 | Net revenue retention | **112–118% scenario range** | `(assumption: customer inference spend grows ~26%` [S24]`, savings rate per dollar compresses` [S29]`; cohort expansion, churn and Product 2 attach are not yet observed)` |
| R4 | ACV Y1 | **$30,000** | [pricing.md](pricing.md) §The ACV arithmetic |
| R5 | Reachable population, 2026 | **1,370**, growing 26%/yr | [../strategy/market_sizing.md](../strategy/market_sizing.md) |
| R6 | Spend-pool widening on hybrid GA (Y5) | **×3.3 scenario** | hosted/self-hosted spend ratio, not a company-count estimate; [../strategy/market_sizing.md](../strategy/market_sizing.md) |
| R7 | Blended CAC, Y1–Y3 | **~$4,200** (14% of ACV); year one alone ~$11,700 | [../strategy/channel_plan.md](../strategy/channel_plan.md) blended across stacks A/B/C/D, Stack A's ~$40k build amortised over Y1–Y3 |

### The table

| Year | New logos | Churned | **End customers** | Blended ACV | **Ending ARR** | Reachable pop. | Penetration | GTM motion carrying the stage |
|---|---|---|---|---|---|---|---|---|
| **Y1** | 5 | 0 | **5** | $30,000 | **$0.15M** | 1,730 | 0.3% | Stack B (published frontier) → Stack C (peer proof) |
| **Y2** | 23 | 1 | **27** | $32,000 | **$0.86M** | 2,180 | 1.2% | **Stack A live** — routing strategy inside LiteLLM [S22] |
| **Y3** | 45 | 4 | **68** | $35,000 | **$2.38M** | 2,740 | 2.5% | A + C compounding; Stack D capped at 3–4 talks |
| **Y4** | 70 | 10 | **128** | $45,000 | **$5.76M** | 3,450 | 3.7% | A + C + **Product 2 attach on existing base** |
| **Y5** | 110 | 19 | **219** | $50,000 | **$10.95M** | **11,400** | 1.9% | **Hybrid GA** widens population ×3.3; Stack E (marketplaces) revisited |
| **Y6** | 200 | 33 | **386** | $62,000 | **$23.9M** | 14,360 | 2.7% | Marketplace + first inside-sales hires; land-and-expand |
| **Y7** | 320 | 58 | **648** | $78,000 | **$50.5M** | 18,090 | 3.6% | Multi-pool enterprise motion; Product 2 is the primary contract |
| **Y8** | 450 | 97 | **1,001** | $95,000 | **$95.1M** | 22,790 | 4.4% | Enterprise + partner-led |

**Penetration never exceeds 4.4% of the reachable population in any year.** That is the discipline check on this table: a build that requires 15% share of a segment defined by an architectural choice is a build that has assumed away the competition. Note the reconciliation with [../strategy/market_sizing.md](../strategy/market_sizing.md), which puts Y3 at "5% of reachable" — the same 68 customers, against a population held static at 1,370 rather than grown at 26%. **The customer count is identical; this file simply grows the denominator, which is the less aggressive read.**

### Where the ACV lift comes from

The ACV column is the part a skeptic should attack, because it does more work than the logo count. It is not price inflation:

| Bend | ACV | Composition |
|---|---|---|
| Y1–Y3 | $30k → $35k | Product 1 only. Growth is the customer's own inference spend compounding at ~26% [S24], partly offset by a compressing savings rate [S29]. **Net +5–8%/yr, not +26%** |
| Y4 | $45k | Product 2 attaches to ~35% of the base at ~$23k incremental `(assumption: attach rate and price untested)`. $35k × 1.06 + 0.35 × $23k ≈ $45k |
| Y5–Y6 | $50k → $62k | Product 2 attach rises to ~55%; hybrid customers carry larger pools and larger bills |
| Y7–Y8 | $78k → $95k | Product 2 becomes the **primary** contract, priced at 2–3% of inference spend. A customer spending $3M/yr on inference at 2.5% is $75k, and hybrid customers cross that threshold routinely |

---

## The milestone behind each bend

**No bend in the table above is allowed without a named, dated, falsifiable milestone.** This is the section the file exists for.

| Bend | ARR moves | **Milestone that causes it** | How you know it happened | What happens if it does not |
|---|---|---|---|---|
| **0 → Y1** | $0 → $0.15M | **M1: a published, reproducible cost-quality frontier on a self-hosted model pool**, with judge-agreement statistics [S33][S34]. Nobody has published this [G2] | The artifact exists, is re-runnable by a third party, and Wen-class evaluators cite it | No evidence, no evaluations, no year one. This is the gating milestone for the entire build |
| **Y1 → Y2** | $0.15M → $0.86M | **M2: the routing strategy is merged and enabled inside LiteLLM** [S22] | Merged upstream; install→enable telemetry exists; installs above the volume floor are counted | Blended CAC rises from ~$4,200 to ~$6,000–7,000 as Stack A's 40% shifts to B, C and D, and the 23-new-logo step becomes ~12. **Y2 ARR halves.** This is a tracked third-party dependency, not a task — see [risk_matrix.md](risk_matrix.md) R7 |
| **Y2 → Y3** | $0.86M → $2.38M | **M3: three enforced deployments with signed eligible-savings definitions and published before-and-afters** | Three signed one-page definitions; three customer-authored posts | Share-of-savings is unbillable without an agreed definition [S38][S39]; fall back to 5% of spend and lose the alignment story |
| **Y3 → Y4** | $2.38M → $5.76M | **M4: Product 2 GA** — cache policy, quantisation tier and model-upgrade regression measured on the same frontier | ≥1 customer paying for a non-routing lever, and the repricing trigger in [pricing.md](pricing.md) exercised at least once | ARR tracks Product 1 only: ~$3.6M in Y4, ~$5M in Y5, and the curve asymptotes inside the $20M SAM. **The venture case ends here and the healthy-infrastructure-business case begins** |
| **Y4 → Y5** | $5.76M → $10.95M | **M5: hybrid pool GA (A9)** — hosted API models admitted as a top tier, widening the reachable population ×3.3 | Reachable population re-derived from *total* inference spend, not self-hosted spend | Population stays ~3,450; 219 customers is 6.3% penetration rather than 1.9%, which is not credible. **$10M ARR is unreachable without M5** |
| **Y5 → Y7** | $10.95M → $50.5M | **M6: Product 2 is the primary contract**, priced on spend under management, with multi-pool attribution | Majority of ARR is spend-percentage, not savings-share | ARR peaks in the mid-$20Ms |
| **Y7 → Y8** | $50.5M → $95.1M | **M7: >1,000 customers with a partner-led motion** — the marketplace channel rejected in year one [../strategy/channel_plan.md](../strategy/channel_plan.md) reopens once ACV clears ~$100k and the SI margin stack stops being fatal | Marketplace and SI channels net-positive at the higher ACV | The direct motion caps somewhere around $50–60M ARR |

**Read the "what happens if it does not" column as the real forecast.** Three of the seven milestones (M2, M4, M5) each individually cut the terminal number by more than half, and none of the three is under CAMIR's sole control: M2 depends on a third-party maintainer, M4 depends on a customer wanting a lever nobody has asked for, M5 depends on hybrid routing being commercially distinct from a frontier vendor's free router [S20].

---

## Expansion revenue layers

Ranked by evidence, not by size.

| # | Layer | Mechanism | Contribution to NRR | Evidence |
|---|---|---|---|---|
| 1 | **The customer's own spend compounding** | The fee is a percentage; the base grows | +8–12 pts | Strongest: enterprise LLM spend grows ~26% CAGR [S24], inference is the #2 line item [S25] |
| 2 | **Product 2 attach** | Levers 2–5 on the same apparatus | +5–15 pts from Y4 | **Weakest: nobody has asked for it.** Test in discovery before building |
| 3 | **More pools per customer** | A 400-person company has more than one inference service; Marcus owns one of several | +3–6 pts | Reasoned from [../strategy/personas.md](../strategy/personas.md) P2 (six product teams call his service) |
| 4 | **Hybrid top tier** | Hosted models admitted, larger bill under management | +4–8 pts from Y5 | Depends on A9 sequencing |
| 5 | **Enterprise upgrade** | Judge certification, air-gapped, multi-tenant attribution | +2–4 pts | Enterprise OSS licences convert at 0.01–0.1% of users at much higher value [S40] |

**Against these, one contraction layer that most builds omit:** as the tier cost spread compresses [S29], the measured saving per dollar of spend falls, and a share-of-savings contract **shrinks automatically without anyone renegotiating**. Layer 1 and this contraction are in direct opposition, and no source resolves which wins. R3's 112–118% NRR is the residual after netting them, and it is a guess with the components shown rather than a benchmark borrowed from another category.

---

## Sensitivity — the three numbers that move the answer

| Scenario | Change | Y5 ARR | Y8 ARR |
|---|---|---|---|
| **Base** | as tabled | $10.9M | $95.1M |
| **M2 fails** (LiteLLM contribution rejected) | New logos −45% in Y2–Y4, CAC ~$6,000–7,000 | **$6.2M** | **$54M** |
| **M4 fails** (no Product 2) | ACV frozen at ~$38k by Y5, flat thereafter | **$8.3M** | **$31M** — and the terminal number is capped by the SAM, not by execution |
| **M5 fails** (no hybrid) | Population stays 3,450; growth caps at ~6% penetration | **$9.8M** | **$36M** |
| **Savings rate comes in at 15%, not 25%** | ACV → $17.6k until the repricing trigger fires | **$6.4M** | $88M — **the repricing to spend-percentage largely absorbs it, which is the point of declaring it in advance** |
| **f2 at the low end (×0.15)** | Reachable population ~470, SAM ~$3M | **$3.5M** | **$12M** — CAMIR is a feature, and no execution fixes it |

**The last row is the one to act on.** f2 is answerable by twenty discovery calls before a line of code is written ([../strategy/market_sizing.md](../strategy/market_sizing.md) says so, [use_of_funds.md](use_of_funds.md) funds it), and it moves the terminal number by 8×. Nothing else in this file has that ratio of information value to cost.

---

## Recommended next 3

1. **Test Product 2 in the same twenty discovery calls that test f2**, with one question: *when you last changed a cache threshold, a quantisation setting or a model version, how did you know it did not make answers worse?* If the answer is consistently "we didn't", Product 2 has demand and the venture case is live. If it is "we ran an eval", Product 2 is a feature of something they already have.
2. **Instrument the LiteLLM funnel from day one of M2** — installs, enable rate, share above the volume floor. The Y2 bend is entirely this funnel, and a bend with no telemetry behind it is a hockey stick with a story attached.
3. **Re-run this whole table after the first measured frontier.** R4 descends from the 25% savings assumption [G2]; if that number is 15%, the Y1–Y3 rows are wrong by 40% and the repricing trigger fires three years earlier than modelled.

<!-- critic: unresolved — Product 2's demand is reasoned from the mechanism, not observed: no customer, prospect or public source has asked for measured cache-threshold or quantisation decisions. It carries the majority of terminal ARR in this build, and it is the single least-evidenced load-bearing claim in the financial layer. It is flagged as an assumption in three places above rather than resolved, because resolving it requires discovery this run has not done. -->
