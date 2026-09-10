# CAMIR — Channel Plan and Channel Economics

**What this is** — the channel map by segment **plus the economics of each channel**: the stack from list price to net revenue, the cost to acquire through it, time to first revenue, and a viability verdict at CAMIR's price point.
**Why it exists** — [gtm.md](gtm.md) names channels and picks one. This computes whether they can actually work, and the computation changes an answer: at a **$30k ACV** against a reachable population of roughly **1,370 companies**, any channel taking a conventional infrastructure reseller margin destroys more value than it creates, and two channels that look sensible on a slide — marketplaces and system integrators — turn out to be net-negative or net-zero in year one. Naming channels without their margin stack is asserting distribution rather than planning it.
**How to read it** — §The stacks is the file; each stack shows list price falling to net revenue line by line. The **Verdict** column is the decision. §What is deliberately not a channel says which conventional options are rejected and why.
**Depends on / feeds** — depends on [gtm.md](gtm.md), [market_sizing.md](market_sizing.md), [../financials/pricing.md](../financials/pricing.md) (for list price), [personas.md](personas.md); feeds [../financials/unit_economics.md](../financials/unit_economics.md), [../financials/revenue_build.md](../financials/revenue_build.md) and [sales_roadmap.md](sales_roadmap.md).

**Reference price for every stack below:** ACV **$30,000/year**, derived in [market_sizing.md](market_sizing.md) as ~28% of a beachhead customer's measured annual saving. `(assumption A4, A5 — no pricing has been tested with any buyer.)`

---

## Channel map by segment

| Segment | Primary channel | Secondary | Never |
|---|---|---|---|
| **P2 Marcus** — beachhead, self-hosted at volume | Proxy-native distribution (routing strategy inside LiteLLM) [S22] | Published frontier + methodology; peer proof | Outbound sales; paid ads |
| **P1 Priya** — drop-in, data-residency motivated | Proxy-native, defaults-only path | Documentation and quickstart | Anything requiring a call |
| **P3 Wen** — knobs, own eval team | Published methodology and harness as a citable artifact | Conference talk; academic venue | Sales-led anything |
| **P4 Dana** — economic buyer | **No channel.** She is reached *through* Marcus, never directly | — | Direct outbound — it routes the deal around the evaluator and kills it |
| **P6 Sam** — OSS adopter below the volume floor | The repository itself | — | Any conversion attempt. He is the channel, not a lead |

---

## The stacks

### Stack A — Proxy-native distribution (primary)

The routing strategy ships inside the proxy the beachhead already runs. No reseller, no margin taken by anyone.

| Line | Value | Note |
|---|---|---|
| List price | $30,000 | ACV |
| Channel discount / partner share | **$0** | An upstream open-source contribution takes no revenue share |
| Payment processing (~3%) | −$900 | |
| **Net revenue per customer** | **$29,100** | **97.0% of list** |
| One-off channel build cost | ~$40,000 | `(assumption: 4–6 engineer-weeks to build and upstream the strategy plus its config surface)` |
| Marginal CAC per customer | ~$0 | The install is already there |
| Customers to repay the build | **~1.4** | |
| Time to first revenue | **4–7 months** | 1–2 months to build and upstream, then a shadow-mode period before enforcement |
| **Verdict** | **VIABLE — the only channel with no margin leakage and no per-customer acquisition cost.** Its risk is not economic but **dependency**: it requires a third party to accept the contribution ([business_model_canvas.md](business_model_canvas.md), block 8, first in the kill order) | |

### Stack B — Published frontier and methodology (primary, dual-purpose)

The self-hosted cost-quality frontier nobody has published [G2], with the judging protocol and inter-judge agreement statistics [S33][S34].

| Line | Value | Note |
|---|---|---|
| List price | $30,000 | |
| Channel discount | $0 | |
| Payment processing | −$900 | |
| **Net revenue** | **$29,100** | 97.0% |
| Cost per publication | ~$25,000 | `(assumption: GPU-hours plus engineering per major publication)` |
| Conversions per publication | ~3 evaluations over 12 months | `(assumption: no publication has been made)` |
| **CAC** | **~$8,300** | 28% of ACV |
| Payback | **~3.4 months** | |
| **Verdict** | **VIABLE, and structurally unusual: the spend is the research contribution.** The GPU-hours buy a result the field has asked for [S13] whether or not anyone buys anything. Charge it half to R&D | |

### Stack C — Peer proof / reference deployments

| Line | Value |
|---|---|
| Net revenue | $29,100 (97.0%) |
| Cost | ~$3,000 per acquisition `(assumption: support beyond normal onboarding for a reference deployment)` |
| **CAC** | **~$3,000** — 10% of ACV |
| Payback | **~1.2 months** |
| **Verdict** | **Best ratio in the plan, and not scalable on purpose.** References are earned, not bought. Treat as the output of Stacks A and B, not as an independent channel with a budget |

### Stack D — Conference talks and infrastructure meetups

| Line | Value |
|---|---|
| Net revenue | $29,100 (97.0%) |
| Cost per talk | ~$12,000 `(assumption: travel, preparation, opportunity cost for a three-person team)` |
| Conversions per talk | ~2 evaluations |
| **CAC** | **~$6,000** — 20% of ACV |
| Payback | **~2.4 months** |
| **Verdict** | **VIABLE and slow.** Compounds into Stack C. Cap at 3–4 talks a year; beyond that it competes with building |

### Stack E — Cloud marketplaces (AWS / GCP / Azure)

The conventional "obvious" channel for infrastructure software. The arithmetic rejects it for year one.

| Line | Value | Note |
|---|---|---|
| List price | $30,000 | |
| Marketplace fee (~3–5%) | −$1,200 | Take 4% `(assumption: published marketplace rates vary by program and commitment)` |
| Co-sell / partner incentive | −$1,500 | 5% `(assumption)` |
| Listing, security review, compliance amortised | −$2,000/customer at 10 customers/yr | `(assumption: ~$20k one-off for listing, security questionnaire and compliance artifacts)` |
| **Net revenue** | **$25,300** | **84.3% of list — 12.7 points of leakage** |
| Time to first revenue | **9–15 months** | Listing plus procurement cycles |
| **Verdict** | **REJECTED for year one, revisit at ~25 customers.** It costs 16% of gross and 9+ months to reach a population that is *already reachable for free through the proxy they run*. Marketplaces earn their fee by solving procurement — and CAMIR's beachhead buyer is an engineering leader with a budget, not a procurement office. **Revisit trigger:** the first customer who says they can only buy through committed marketplace spend |

### Stack F — System integrators and MLOps consultancies

| Line | Value | Note |
|---|---|---|
| List price | $30,000 | |
| SI margin (25–35%) | −$9,000 | Take 30% `(assumption: standard infrastructure reseller margin)` |
| Partner enablement, amortised | −$2,500 | `(assumption: training and support per partner, amortised across their deals)` |
| Payment processing | −$570 | |
| **Net revenue** | **$17,930** | **59.8% of list — 40 points of leakage** |
| **Verdict** | **REJECTED, and not marginally.** A 30% margin on a $30k ACV leaves $18k against a CAC-equivalent that must still cover partner management. **Worse, it is structurally wrong:** an SI's revenue comes from implementation hours, and CAMIR's whole promise is that measurement is automated. The partner's incentive is to make the deployment bespoke. **Revisit only if ACV exceeds ~$100k**, where the margin can carry a genuine services layer |

### Stack G — Direct outbound sales

| Line | Value |
|---|---|
| Net revenue | $29,100 |
| Fully-loaded SDR + AE cost allocated per closed deal | ~$35,000 `(assumption: infrastructure-ACV outbound benchmarks)` |
| **CAC** | **~$35,000 — 117% of ACV** |
| Payback | **Never in year one** |
| **Verdict** | **REJECTED on arithmetic and on strategy.** CAC exceeds ACV, and outbound to engineers antagonises the open-source base that Stack A depends on. The population is ~1,370 companies — small enough that outbound would burn the entire market's goodwill in two quarters |

---

## Summary table

| Stack | Channel | Net % of list | CAC | Payback | Verdict |
|---|---|---|---|---|---|
| **A** | Proxy-native (LiteLLM) | **97.0%** | ~$0 marginal | 1.4 customers repay the build | **PRIMARY** |
| **B** | Published frontier | **97.0%** | ~$8,300 | 3.4 mo | **PRIMARY** |
| **C** | Peer proof | 97.0% | ~$3,000 | 1.2 mo | Consequence of A+B |
| **D** | Conference talks | 97.0% | ~$6,000 | 2.4 mo | Secondary, capped |
| **E** | Cloud marketplaces | 84.3% | high + 9–15 mo | — | Rejected, revisit at ~25 customers |
| **F** | System integrators | **59.8%** | — | — | Rejected; revisit above ~$100k ACV |
| **G** | Outbound sales | 97.0% | ~$35,000 | never | Rejected |

**Blended Y1–Y3 CAC across the live channels (A, B, C, D): ~$4,200**, or **14% of ACV**, with payback around 2.2 months at the steady-state 75% gross margin `(assumption: mix of 40% A, 30% B, 20% C, 10% D)`. The arithmetic: B, C and D contribute 0.3 × $8,300 + 0.2 × $3,000 + 0.1 × $6,000 = **$3,690** of marginal CAC; Stack A adds its ~$40,000 one-off build amortised over the ~29 customers it brings in across Y1–Y3 (40% of 73 new logos), ≈ $1,380 each, weighted 0.4 → **~$4,240**. **Year one alone is ~$11,700**, because the whole build lands on the ~2 customers Stack A brings in that year. *(An earlier version of this file stated $5,400, which does not follow from these inputs under any amortisation.)* That is a healthy figure, and it holds only because the two primary channels have no intermediary. **The moment an intermediary is added, this plan stops working** — which is the real content of this file.

---

## What is deliberately not a channel

- **Product-led self-serve with a credit card.** The buyer is an engineering leader signing a five-figure contract against a measured saving, not a developer swiping a card. Self-serve applies to the *open* half only.
- **A hosted CAMIR SaaS.** It would contradict the positioning: the harness runs inside the customer's perimeter precisely so the savings number is theirs to audit ([positioning.md](positioning.md)).
- **Model-vendor partnerships.** Structurally impossible. A vendor whose revenue rises when traffic routes up will not distribute a system whose purpose is to route it down [S20]; `../BRIEF.md` §Why now says exactly this.

---

## Recommended next 3

1. **Treat Stack A's partner acceptance as a gating dependency, not a task.** If the contribution is rejected, the blended CAC rises ~45–65% — from ~$4,200 to ~$6,000–7,000, depending on how Stack A's share redistributes — and the primary channel disappears. It belongs in [../financials/risk_matrix.md](../financials/risk_matrix.md) with a leading indicator.
2. **Budget Stack B's GPU-hours as half R&D.** The publication is the research contribution and the acquisition channel simultaneously; costing it entirely to sales overstates CAC by roughly 2×.
3. **Set the marketplace revisit trigger explicitly at ~25 customers or the first procurement-blocked deal**, so Stack E gets reconsidered on evidence rather than re-litigated every quarter.
