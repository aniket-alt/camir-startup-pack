# CAMIR — Unit economics

**What this is** — the per-customer P&L: CAC by channel with its basis, the fully-loaded cost to serve one deployment including **CAMIR's own compute cost per unit**, gross margin, payback, LTV under the stated churn, contribution-margin trajectory, and what happens to all of it as model prices fall.
**Why it exists** — CAMIR's revenue is a share of a saving, so its price falls when its customer's inference gets cheaper — and inference is getting cheaper every quarter [S26][S29]. That makes the margin question here structurally different from a normal SaaS: **the cost curve moves the top line, not just the bottom.** Worse, the paid tier's core deliverables — per-deployment classifier training, gate fitting, policy management — are *services* wearing a subscription's clothes, and a company that never computes them per customer discovers a 45% gross margin in year three, when it is priced into every contract. This file computes both before either is signed.
**How to read it** — §2 (cost to serve) then §5 (the cost-curve argument). A skeptic should attack the 25% savings rate in the ACV derivation, which is `(assumption)` and which moves everything; and §2's onboarding-cost line, which is the number most likely to be underestimated.
**Depends on / feeds** — depends on [pricing.md](pricing.md), [revenue_build.md](revenue_build.md), [../strategy/channel_plan.md](../strategy/channel_plan.md), [../tech/not_vaporware.md](../tech/not_vaporware.md) §3; feeds [use_of_funds.md](use_of_funds.md), [risk_matrix.md](risk_matrix.md), [../validation/metrics_by_stage.md](../validation/metrics_by_stage.md) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

**No figure below is measured.** Every line derives from a stated assumption or a cited source; change an assumption and recompute.

---

## 1. The unit, and the revenue on it

**The unit is one customer deployment**, not a seat, not a request. Nobody sits in front of CAMIR and the buyer is the bill.

| Line | Y1 | Y3 | Basis |
|---|---|---|---|
| Customer inference spend | $600,000 | $600,000 | [../strategy/personas.md](../strategy/personas.md) P2, $50k/month |
| × routable share after upstream cache (f4) | ×0.70 | ×0.70 | Production cache hit rates 20–45% [S36]; midpoint of the loss [G4] |
| × realised saving rate on routable spend | ×25% | ×25% | `(assumption: below the whitepaper's own base case — 28% reduction on post-cache token-proportional GPU cost without harness repair, 33% with it (`../tech/whitepaper.md` §2.6); RouteLLM's hosted multiples [S2] are context only; no CAMIR measurement exists [G2])` |
| = measured annual saving | $105,000 | $105,000 | |
| × CAMIR share | ×28% | ×28% | `(assumption: no share-of-savings vendor publishes a rate [S38][S39])` |
| **= ACV** | **$30,000** | **$35,000** | Y3 lift is Product 2 attach, not a price rise ([revenue_build.md](revenue_build.md)) |

---

## 2. Cost to serve one deployment — including CAMIR's own compute

This is the line most routing pitches omit, and it is not small: **CAMIR's product is a measurement, and measurement costs GPU-hours.**

### 2.1 The compute cost per unit

CAMIR's compute is the customer's, not CAMIR's — the harness runs inside their perimeter (N4, [../tech/architecture/D06.md](../tech/architecture/D06.md)). **But it is a real cost the customer bears, it is netted against the saving, and if it is large the disqualification threshold moves.** It is computed here because a saving quoted gross of it is the same error as quoting against raw GPU cost.

| Item | Per event | Frequency | Annual GPU cost to the customer |
|---|---|---|---|
| Ceiling probe: 8,000 requests × 3 tiers = 24,000 generations | **~11–240 GPU-hours.** The low end assumes [S26]'s high-concurrency batching; the high end scales [../validation/experiment_board.md](../validation/experiment_board.md) E1's unbatched budget (~120 GPU-hours for 12,000 generations, both harness arms) | 4–6×/yr (pool changes [S29]) | **~$110–5,400** at $2.50–3.75/GPU-hr [S26] |
| Judging: 24,000 answers × 2 judges | ~7 GPU-hours; collapses to near zero where programmatic verification applies | same | ~$100–140 |
| **Shadow mode**: doubles generation on shadowed endpoints | — | continuous while shadowing | **~$1,500–4,000/yr** — the dominant term, and the one nobody expects |
| Gate scoring on the critical path | ~1–5 ms, no extra generation on the default estimator | every cascade request | negligible |
| Control-plane VM in the customer's VPC | one small instance | continuous | ~$600–1,200/yr |
| **Total customer-borne compute** | | | **~$2,300–10,800/yr** — the width is almost entirely the probe's batching assumption |

**Against a $105,000 measured saving that is 2–10%.** The first measured probe collapses this range, and E14 is the experiment that meters it. It is not a threat to the deal, and it *is* a threat to a small deployment: a customer saving $20,000 who runs three endpoints in permanent shadow keeps only ~$16,000, which is why the pricing floor exists at $12,000/yr and why shadow cost is surfaced per endpoint ([../tech/architecture/D09.md](../tech/architecture/D09.md)).

**The line that must never be quoted without correction:** raw GPU rental understates true cost **3–5×** once engineering time is counted [S27], and a card idle at 10% utilisation costs **10× per token** [S27]. Both apply to CAMIR's own probe cost as much as to the customer's serving cost.

### 2.2 CAMIR's own cost to serve

| Line | Y1 (5 customers) | Y3 (68 customers) | Basis |
|---|---|---|---|
| **Onboarding** — log-format work, corpus design, first frontier, cost-axis parameterisation | **$9,000** | **$3,200** | ~5 engineer-days at Y1, falling to ~2 as formats are pre-supported. `(assumption: the largest single cost and the one most likely underestimated — [../product/journeys/edge_low.md](../product/journeys/edge_low.md) names log parsing as the real blocker)` |
| **Per-deployment classifier training and gate fitting** | $2,400 | $900 | Recurring on each recalibration. **This is a service.** Productising it is the margin question |
| **Support** — the frontier moved, the alert fired, the ceiling changed | $3,000 | $1,900 | Twice-a-month product, but each contact is an engineer's |
| **CAMIR-side infrastructure** — licence service, release, reference frontier runs | $1,100 | $600 | Small; CAMIR hosts almost nothing (N4) |
| **Success / renewal** | $2,000 | $2,200 | Rises: renewal is a re-measurement conversation, not an invoice |
| **Total cost to serve** | **$17,500** | **$8,800** | Y3 figure matches the kill threshold in [../strategy/business_model_canvas.md](../strategy/business_model_canvas.md) block 7 |
| **Gross margin** | **42%** | **75%** | |

**Year one gross margin is 42%, and that is the honest number.** It is not a software margin. It becomes one only if onboarding falls from five engineer-days to two, which is an engineering problem (log-format coverage, pre-flight estimation, corpus defaults) rather than a scale effect. **[../strategy/business_model_canvas.md](../strategy/business_model_canvas.md) sets the kill threshold at measurement cost above 30% of ACV; Y1 sits at 58% and Y3 at 25%.** The whole margin thesis is the trajectory between those two, and E14 is the experiment.

---

## 3. CAC by channel

Blended **~$4,200 (14% of ACV)** in Y1–Y3, from [../strategy/channel_plan.md](../strategy/channel_plan.md). **Year one alone is ~$11,700**, because Stack A's one-off build lands on the two customers it brings in that year.

| Channel | CAC | Basis | Share of Y1–Y3 logos |
|---|---|---|---|
| **Stack A — LiteLLM strategy** [S22] | **~$0 marginal**; ~$40,000 one-off build ≈ $1,380 per customer over Y1–Y3 | 4–6 engineer-weeks to build and upstream the strategy. **Unagreed dependency**, first in the kill order | 40% |
| **Stack B — published frontier / methodology** | **~$8,300** | ~$25,000 of GPU-hours and engineering per publication, ~3 evaluations each. Half of it is fairly chargeable to R&D | 30% |
| **Stack C — peer proof** (Wen's talk, Sam's post) | **~$3,000** | Supporting a reference deployment beyond normal onboarding. **Lowest CAC of the live channels, least controllable** | 20% |
| **Stack D — conference talks** | **~$6,000** | ~$12,000 per talk, ~2 evaluations each; capped at 3–4 talks/yr | 10% |
| **Stack E — cloud marketplaces** | rejected Y1–Y3 | **84.3% net** after marketplace fees; revisited at ~25 customers | 0% |
| **Outbound** | rejected | **CAC ~117% of a $30,000 ACV** | 0% |

**Blend:** 0.4 × $1,380 + 0.3 × $8,300 + 0.2 × $3,000 + 0.1 × $6,000 = **~$4,240**.

**Payback: ~2.2 months** at the steady-state 75% gross margin ($4,200 ÷ ($30,000 × 0.75) × 12). **At the honest Y1 margin of 42% it is 4.0 months**; and a year-one customer carrying the ~$11,700 build-loaded CAC pays back in **11 months** at 42%. All three are true at different points in the company's life, which is why all three appear here.

**Where the dependency bites:** if the LiteLLM contribution is refused, Stack A's 40% redistributes to B, C and D and the blend rises to **~$6,150** (0.5 × $8,300 + 0.33 × $3,000 + 0.17 × $6,000) — still viable, and 46% worse.

---

## 4. LTV, and why it is quoted at the low end

| | Y1–Y4 | Y5+ (upside, not in the revenue table) |
|---|---|---|
| Gross logo churn | **15%/yr** | 12%/yr |
| Implied life | 6.7 yrs | 8.3 yrs |
| NRR | 112–118% | same |
| **LTV at 75% GM, no NRR** | **$150,000** | $187,000 |
| **LTV at 75% GM, 112% NRR** | ~$280,000 | ~$350,000 |
| **LTV:CAC** | **~36:1** at the no-NRR figure and $4,200 CAC; **~13:1** against the year-one build-loaded $11,700 | — |

**The 15% churn assumption is deliberately pessimistic and should stay that way.** CAMIR is a proxy: change one base URL and you are out. A6 records that rising switching cost from accumulated tolerance policies and routing history is **untested**, and this pack does not improve on it. A 36:1 LTV:CAC computed on an unvalidated retention assumption is a ratio, not evidence — **the number to watch is survival through a pool change** ([../validation/metrics_by_stage.md](../validation/metrics_by_stage.md) stage 3), which is the event [S29] guarantees will come.

---

## 5. The cost-curve argument — where this differs from normal SaaS

**Falling model prices move CAMIR's revenue, not only its costs.** Three forces, and they do not point the same way.

| Force | Direction | Magnitude |
|---|---|---|
| **1. Inference gets cheaper per token** [S26] | **Revenue down.** A 28% share of a saving on a smaller bill is a smaller fee | Structural, continuous |
| **2. Tier spread compresses** [S29] | **Revenue down, and this is the real one.** A 31B-class model within ~10 Elo points of far larger open-weight models means the *gap* CAMIR monetises narrows. Routing between two nearly equal tiers saves nearly nothing | Structural, and the sharpest slope |
| **3. Inference volume grows** [S24][S25] | **Revenue up.** Enterprise LLM spend growing 25.9% CAGR; inference is now the second-largest line in enterprise AI budgets, 55–80% of AI GPU spend | Strong, and currently the largest of the three |

**Net, on stated assumptions:** force 3 outruns forces 1 and 2 through the Y1–Y5 window, which is what [revenue_build.md](revenue_build.md)'s R3 encodes as 112–118% NRR — volume growth partly cancelled by savings-rate compression. **Force 2 is the one that could invert it**, and it is not modelled beyond a haircut because nobody can date it.

**The strategic answer is not a pricing tweak.** It is Product 2 in [revenue_build.md](revenue_build.md): the inference-efficiency control plane, whose levers — cache policy, quantisation tier, batch and utilisation policy, model-upgrade regression — are **not** affected by tier compression, because they are about measuring decisions nobody currently measures at all. That is why [../strategy/market_sizing.md](../strategy/market_sizing.md) states first that routing fees alone are not a venture-scale business at 2026 denominators, and the expansion is.

**The margin does improve** on the cost side: CAMIR's own probe and judging costs fall with GPU prices, and they are a small term. The compute-cost line is not where this company's margin risk lives. **The margin risk is that per-deployment fitting is a service** — §2.2, and E14.

---

## 6. Contribution margin trajectory

| | Y1 | Y2 | Y3 | Y5 |
|---|---|---|---|---|
| ACV | $30,000 | $32,000 | $35,000 | $50,000 |
| Cost to serve | $17,500 | $11,800 | $8,800 | $9,500 |
| Gross margin | **42%** | **63%** | **75%** | **81%** |
| CAC | $4,200 | $4,200 | $4,200 | $6,800 |
| **First-year contribution after CAC**, per customer landed that year | **$8,300** | $16,000 | $22,000 | $33,700 |

*Y5 CAC is higher because marketplaces are revisited once the proxy channel saturates (assumption). A customer landed in year one against the build-loaded ~$11,700 CAC contributes **$800** in its first year — the build is paid for by the customers who follow it.*

**The bend from 42% to 75% is entirely onboarding automation**, not scale. It is the single most important engineering investment in [use_of_funds.md](use_of_funds.md) that is not a product feature, and it is invisible on any roadmap organised by customer-facing capability.

---

## Recommended next 3

1. **Measure onboarding cost in engineer-days on the first three design partners and publish it internally.** It is the largest cost line, the least examined, and the entire 42%→75% margin story is the claim that it falls from five days to two. If it does not, the paid tier is a consultancy and [pricing.md](pricing.md)'s floor is set too low.
2. **Surface customer-borne shadow-mode compute per endpoint in the product.** At $1,500–4,000/yr it is the dominant customer-side cost of running CAMIR and the one that accumulates silently on endpoints nobody ever enforces — a deployment paying a routing premium to run no routing should be able to see it.
3. **Model force 2 explicitly before the next round.** Tier-spread compression [S29] shrinks the gap CAMIR monetises and is the only force here that could invert the unit economics rather than dent them. A dated sensitivity — savings rate at 25%, 15% and 8% — belongs in the deck, because a reader who has read [S29] will construct it themselves and reach a worse conclusion than the honest one.
