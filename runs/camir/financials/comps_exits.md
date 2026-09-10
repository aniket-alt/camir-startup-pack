# CAMIR — Comparables and exit landscape

**What this is** — the comparable set with what is actually known about each and at what confidence, what the market rewarded and punished in this specific category during 2025–2026, realistic acquirer profiles with the reason each would pay, the conditions an IPO path would require, and — after the sober part, not instead of it — the standalone case.
**Why it exists** — the comparable set for LLM routing has an unusual property: **the two most instructive datapoints are a shutdown and a gateway that is not a router.** TensorZero archived its repository in June 2026 after $7.3M and 11,000+ stars [S23]; OpenRouter reached $160M annualised revenue by taking ~5% of a bill it sits on [S17], which is a *distribution* business wearing routing's clothes. A comps section listing only Martian's reported valuation would produce exactly the wrong lesson — that routing is valuable — when the evidence says **the gateway position is valuable and the routing algorithm is not.** This file exists so the pack's own valuation story is built on the right half of that distinction.
**How to read it** — §1's confidence column governs everything; two of the most quotable rows are `weak` and carry mandatory hedges. §2 is the actual lesson. A skeptic should attack §5, and the honest place to attack it is the $70M/yr routing-fee TAM.
**Depends on / feeds** — depends on [../research/sources.md](../research/sources.md), [../research/competitors.md](../research/competitors.md), [../strategy/market_sizing.md](../strategy/market_sizing.md), [revenue_build.md](revenue_build.md); feeds [use_of_funds.md](use_of_funds.md) §5, [../narrative/vc_memo.md](../narrative/vc_memo.md) and [../narrative/pitch_deck.md](../narrative/pitch_deck.md).

---

## 1. The comparable set

| # | Company | What it is | Known financials | Confidence | Multiple |
|---|---|---|---|---|---|
| 1 | **OpenRouter** | Gateway with routing: catalog access, billing consolidation, ~5% take on customer inference spend | **$160M annualised revenue, Aug 2026**, up from $50M at end-2025. Reported $1.3B round; Stripe reported to have agreed an acquisition **above $7B** [S17] | *secondary* | ~**44×** revenue at the reported $7B — if both numbers hold |
| 2 | **Martian** | Purpose-built commercial LLM router, per-request attribution, enterprise pricing | Reportedly nearing **~$1.3B valuation, April 2026** [S18] | **weak** — one secondary blog restating an unnamed report. **Hedge mandatory; never state as fact** | Unknown. Revenue is [G3] |
| 3 | **Not Diamond** | Recommender rather than proxy; ~$0.05/M tokens routed | No revenue disclosed [S19] | **weak** — competitor-authored | — |
| 4 | **TensorZero** | Open-source LLMOps: gateway, observability, evaluation | **Archived 12 June 2026** after raising **$7.3M**, passing **11,000 GitHub stars**. Founders returned unused capital [S23] | *secondary* | **Zero.** The most instructive row here |
| 5 | **Langfuse** | Open-source LLM observability | **Acquired by ClickHouse, January 2026**, inside a $400M Series D at a $15B valuation [S23] | *secondary* | Undisclosed |
| 6 | **LiteLLM** | Open-source proxy + enterprise tier | Undisclosed [S22] | *secondary* | — |
| 7 | **ProsperOps / nOps / Usage.ai** | Cloud FinOps, share-of-realised-savings | No universal published rate; percentage **and** the eligible-savings definition set per agreement [S38][S39] | *secondary* | — |

**What is missing and matters: Martian's and Not Diamond's actual revenue [G3].** The category's commercial size is inferred from OpenRouter alone — and OpenRouter is a gateway with routing, not a router. Any statement that "LLM routing is a large market" currently rests on a company whose revenue comes from being in the payment path.

---

## 2. What the market rewarded, and what it punished

**Rewarded — three patterns, and routing accuracy is in none of them:**

1. **Sitting in the payment path.** OpenRouter's ~5% take of $3.2B of routed spend [S17]. The router is not the asset; the position is.
2. **Open source acquired by infrastructure that needs the workload.** Langfuse into ClickHouse [S23] — the acquirer wanted the data volume and the developer surface, not the product's margin.
3. **Being a feature of something already deployed.** A frontier vendor shipped routing built-in with **no separate fee** [S20], and the serving engines are absorbing it [S11].

**Punished — one pattern, precisely:**

**TensorZero.** $7.3M raised, 11,000+ stars, archived June 2026, capital returned. The founders' own stated reason: the difficulty of finding product-market fit for an open-source project *and* a commercial product simultaneously. Context in the same source: frontier vendors and hyperscalers shipping native gateway, observability and evaluation features [S23].

**Three lessons CAMIR inherits directly.**

| Lesson | What CAMIR does about it | Where |
|---|---|---|
| **Stars are not conversion** | M15 tracked against the 1–5% band [S40]; stars explicitly named a vanity metric | [../validation/metrics_by_stage.md](../validation/metrics_by_stage.md) |
| **The open/paid line must be drawn before release, not under revenue pressure** | Published before first release; **no feature ever moves open → paid** | [../tech/architecture/D06.md](../tech/architecture/D06.md) |
| **A layer the platform can absorb will be absorbed** | Own the measurement half, which no serving engine will run | [../tech/architecture/D07.md](../tech/architecture/D07.md) |

**The uncomfortable read.** TensorZero is not a distant analogy — it is an open-source LLM-infrastructure company with an observability-and-evaluation product and an open-core plan. **It is the closest comparable in this table**, closer than Martian, and it returned its capital.

---

## 3. Acquirer profiles

| # | Acquirer | Why they would pay | What they would pay for | Likelihood |
|---|---|---|---|---|
| 1 | **Serving-stack commercial entities** (vLLM/SGLang-adjacent, inference platforms) | They are absorbing routing already [S11] and have no measurement layer: no ceiling probe, no judging protocol, no cost axis | **The measurement layer and the methodology**, not the router | **Highest.** Also the most likely to build it instead |
| 2 | **Observability platforms** (the ClickHouse-Langfuse shape [S23]) | Routing decisions are a new telemetry stream; the trace-stamp integration is a natural extension | The attribution layer and the customer base | High |
| 3 | **Cloud FinOps vendors** [S38][S39] | Inference is now the second-largest line in enterprise AI budgets [S25]; they have the buyer and no LLM-native measurement | The counterfactual engine and the savings ledger | Medium-high — **the most natural strategic fit and the least glamorous** |
| 4 | **Hyperscalers** | Own the GPU fleet and the utilisation problem; a customer-facing efficiency control plane is a retention feature | The whole control plane | Medium. They tend to build |
| 5 | **Gateway incumbents** (OpenRouter shape) | Hold the position and lack the measurement; adding a defensible quality axis is a moat upgrade | The methodology and the credibility | Medium |
| 6 | **Enterprise infra platforms** (Datadog/Grafana class) | Cost-and-quality attribution for AI workloads sits inside their existing FinOps story | Attribution + integrations | Medium |

**The pattern across all six: every plausible acquirer wants the measurement half.** Nobody would buy CAMIR for its router — the field is plateaued [S4] and the mechanism is being commoditised [S11]. That is a consistent read across the strategy, tech and financial layers, and it is the strongest single argument for [../HANDOFF.md](../HANDOFF.md) §2's decision that the measurement layer is the asset.

---

## 4. The IPO path, and its honest conditions

A standalone public company requires roughly $100M ARR with durable growth. [revenue_build.md](revenue_build.md) reaches **$95.1M in Y8** — and only on assumptions that must each hold:

| Condition | Required | Confidence |
|---|---|---|
| **The expansion is real** | Product 2 becomes the primary contract by Y7. Routing fees alone do not carry it: base-case TAM is **$70M/yr** [../strategy/market_sizing.md](../strategy/market_sizing.md) | **The load-bearing one.** Currently a thesis with zero measurements |
| **Hybrid pools widen the population ×3.3 at Y5** | A9 flips from sequencing choice to product reality | Medium |
| **ACV rises from $30k to $95k** | Via lever attach, not price rises | Medium — depends entirely on the above |
| **Churn stays ≤ 15%, then 12%** | Switching cost rises as tolerance policies accumulate (A6) | **Low confidence. A6 is untested** and CAMIR is a proxy: change one base URL and you are out |
| **Tier compression does not outrun volume growth** | [S29] vs [S24][S25] | Medium |

**Stated plainly: the IPO path requires the Product 2 thesis to be true, and it currently has no evidence.** The routing business alone is an acquisition, not a listing, and [../strategy/market_sizing.md](../strategy/market_sizing.md) says so as its first finding rather than its last caveat.

---

## 5. Why this could nonetheless be a standalone generational company

The sober case is above. Here is the case that survives it.

**The asset is a standard, and standards are winner-take-most.** Router evaluations are not comparable across papers [S13]; RouterBench prices against hosted API list prices [S7], so no published cost axis exists for a self-hosted pool [G2]; judges agree with each other only ~76% of the time [S33] and almost nobody publishes the statistic. **A team that derives the self-hosted cost axis, fixes a judging protocol against the 2026 reliability results, and publishes reproducible frontiers is not building a router — it is building the unit of account for inference efficiency.** RouterBench and MLPerf demonstrate the position exists; neither is a company that captured it.

**Underneath the standard is a control plane with five levers, four of which nobody measures at all.** Cache policy thresholds set by feel. Quantisation tiers chosen without a measured quality cost. Batch and utilisation policy by intuition, against an idle-GPU penalty of 10× per token [S27]. Model-upgrade regression on the customer's own traffic — the thing that killed every internal router built once and maintained by nobody. **Routing is lever one of five**, which is why the Product 2 TAM reaches $460–760M by 2031 where the routing fee does not.

**And the position is structurally hard to attack from either side.** A frontier vendor cannot credibly measure a customer's self-hosted pool. A serving engine has no reason to tell a customer their ceiling is too low to bother. A gateway earning a share of the bill cannot afford to render a disqualification report [S17][S20]. **The one thing CAMIR does that none of them can do is tell a customer not to buy** — and M16 requires that rate to be non-zero.

**What has to be true for this paragraph to survive contact:** the ceiling exists (R1), and the serving engines stay in dispatch (R2). Both are open. **Neither is rhetoric — one is a measurement available for $280k, and the other is a quarterly note somebody has to write.**

---

## Recommended next 3

1. **Treat TensorZero as the base case, not Martian.** It is the closest structural comparable in the table — open-source LLM infrastructure, evaluation-shaped, open-core, well-regarded — and it returned its capital. A pack that models the reported $1.3B and ignores the archived repository is reading the row it prefers.
2. **Never quote Martian's valuation without the hedge, and never quote it in a deck.** [S18] is a single secondary blog restating an unnamed report; the pack's credibility rests on the register's confidence column being honoured, and this is the row most likely to be quoted loosely by someone who did not read it.
3. **Publish the cost-axis derivation and the judging protocol as public standards work in year one.** It is the only route to the acquirer profile every column of §3 converges on, it costs one document plus a reference `frontier_run`, and it is the difference between being bought for a customer list and being bought for a position.
