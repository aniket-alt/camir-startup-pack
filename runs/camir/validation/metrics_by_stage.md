# CAMIR — Metrics by stage, and the vanity metrics to ignore at each one

**What this is** — the three to five metrics that decide whether CAMIR is working at each of Blank's four stages, paired at every stage with the metrics that will look most impressive and mean least, and why each of those is misleading *here specifically*.
**Why it exists** — CAMIR has an unusually rich supply of flattering numbers. GitHub stars are the field's default proxy and TensorZero passed 11,000 of them before archiving [S23]. Ollama's 52 million monthly downloads [S30] describe laptops, not production, which is why [G1] exists. Measured savings can be inflated 3–5× by quoting against raw GPU cost [S27] and further by picking MT-Bench as the benchmark [S2]. **Four of the most quotable numbers available to this company are each independently capable of concealing that it is not working**, and a metrics page that lists only what to watch leaves them all in play.
**How to read it** — read the "ignore" column of your current stage first; it is the one that changes behaviour. A skeptic should check that no metric here rewards attention or engagement, since CAMIR succeeds by being opened twice a month.
**Depends on / feeds** — depends on [stage_gate.md](stage_gate.md), [../product/PRD.md](../product/PRD.md) §8, [get_keep_grow.md](get_keep_grow.md), [experiment_board.md](experiment_board.md); feeds [../financials/revenue_build.md](../financials/revenue_build.md), [../financials/unit_economics.md](../financials/unit_economics.md) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

**Status: none of these has a value.** Every target is `(assumption)`; CAMIR has no measurements of its own.

---

## Stage 1 — Customer Discovery *(current)*

**The question:** is the ceiling real, does the segment exist, and does the veto exist?

| # | Metric | Definition | Threshold | Source |
|---|---|---|---|---|
| 1 | **Oracle ceiling** (M1) | Share of requests a perfect router resolves at a lower tier within tolerance | Above the disqualification threshold on ≥ 3 corpora. **Reported, never targeted** | E1, PR7 |
| 2 | **Artifact share** (M3) | Ceiling with guard minus ceiling without, as a fraction of the gap to oracle | ≥ 5pp on ≥ 2 of 3 corpora. **This is the contribution** | E2, [S5] |
| 3 | **f2** — share of a team's production tokens on self-hosted weights | From screening question S2 | Recorded for all 20 calls; ≥ 6 of 20 qualify on E4's definition | E4, [G1] |
| 4 | **Never-measured rate** | Qualified teams who have never measured their small tier's ceiling | ≥ 12 of 15 | Q6 |
| 5 | **Veto incidence** | Qualified teams reporting a consuming team blocking a shared-service change | ≥ 7 of 15 | E13 |

### Ignore at this stage

| Vanity metric | Why it misleads **here** |
|---|---|
| **GitHub stars** | TensorZero archived at 11,000+ stars after raising $7.3M [S23]. Stars measure a README, and CAMIR's README is unusually good — which makes this metric actively dangerous for this company |
| **Ollama-style download counts** | 52M monthly downloads [S30] is the growth of laptops. [G1] exists precisely because downloads have no production denominator; it is the number that would make the TAM look large and settle nothing |
| **Any savings percentage** | No CAMIR measurement exists [G2]. A percentage quoted now would be borrowed from RouteLLM's MT-Bench 3.66× — which collapses to 1.41× on MMLU [S2] |
| **Interview enthusiasm** | *"They loved it"* from an unqualified respondent is worse than no data, because it is quotable |
| **Pack completeness** | Sixty artifacts is an output of reasoning. [stage_gate.md](stage_gate.md) §1 says so plainly |

---

## Stage 2 — Customer Validation

**The question:** will a risk-bearer accept it, and will someone pay?

| # | Metric | Definition | Threshold |
|---|---|---|---|
| 1 | **Owner-initiated enforcement** (M10) | Endpoints moved shadow → enforced **by the consuming engineer**, not the platform team | ≥ 2 of 5 partners within 6 weeks |
| 2 | **Pin-to-large rate** (M11) | Share of endpoints pinned back to the large tier | < 2% and flat. **Rising is a product failure and is on the front page as one** |
| 3 | **Escalation rate vs break-even** (M6) | Cascade requests reaching a higher tier, against the drawn inversion line | Below break-even on ≥ 60% of enforced endpoints |
| 4 | **Time to first frontier** (M7) | Install → `frontier_run` | Median < 5 days (G1) |
| 5 | **Paying pilots** | At or above $30,000/yr ACV | ≥ 3 |
| 6 | **Disqualification rate** (M16) | Evaluations CAMIR itself ends | **Non-zero.** Zero means the ceiling probe is not being believed |

### Ignore at this stage

| Vanity metric | Why it misleads |
|---|---|
| **Requests routed** | Volume through the proxy says nothing about whether anyone accepted a tolerance. A high-volume deployment with every endpoint pinned is a failure that looks like adoption |
| **Total measured savings** | Inflatable three ways: quoting against raw GPU cost (3–5× [S27]), against an unrepresentative benchmark [S2], and by counting cache savings as routing savings — which is why N3 exists |
| **Dashboard sessions / DAU** | Inverted for this product. Marcus opens CAMIR twice a month and rising engagement means quality questions are being asked |
| **Endpoints onboarded** | Shadow-mode endpoints are not adoption. Only owner-initiated enforcement is |
| **Logo count** | Three pilots at $30k are a different company from three logos at $0 |

---

## Stage 3 — Customer Creation

**The question:** does it repeat, and does it survive a pool change?

| # | Metric | Definition | Threshold |
|---|---|---|---|
| 1 | **M9 — policies owned by consuming teams** | The retention KPI ([get_keep_grow.md](get_keep_grow.md)) | ≥ 50% by month 6 of an account |
| 2 | **Survival through a pool change** | Accounts that recalibrate and stay after a model upgrade | ≥ 80%. **This is the metric that distinguishes a subscription from a consulting engagement** |
| 3 | **Enforced endpoints per account** (M8) | Expansion unit is the endpoint, not the seat | Median ≥ 3 |
| 4 | **Founder-free closes** | Sales without a founder in the room | ≥ 5 |
| 5 | **Open-core conversion** (M15) | Active OSS deployments → control plane | ≥ 1%, against the 1–5% band [S40] |
| 6 | **M13** — question to attributed answer | The survival condition | < 1 hour |

### Ignore at this stage

| Vanity metric | Why it misleads |
|---|---|
| **Community size** | Sam is 95–99% of users and 0% of revenue **by design** [S40]. Growing him grows distribution, not the business, and conflating the two produces an upsell nag that destroys the channel |
| **Classifier accuracy** | [S4]: 21 methods converged in a narrow band, remedies worth up to 2.13pp. Improving it is the most legible engineering progress available and the least commercially relevant |
| **Feature count** | Four of the twenty flagship features have no technique behind them and are the ones that matter ([../tech/techniques/technique_feature_matrix.md](../tech/techniques/technique_feature_matrix.md)) |
| **Cost per token, absolute** | Falls industry-wide regardless of CAMIR [S26][S29]. Only the counterfactual delta on the same traffic means anything |

---

## Stage 4 — Company Building

| # | Metric | Threshold |
|---|---|---|
| 1 | **Net revenue retention** (M17) | > 110%, from endpoint expansion — not from traffic growth, which CAMIR deliberately does not price on |
| 2 | **CAC payback** | ~2.2 months at a blended ~$4,200 Y1–Y3 CAC (14% of a $30,000 ACV) and 75% gross margin |
| 3 | **Gross margin** | Software-shaped, ≥ 75%. **The real test**, because per-deployment classifier training and policy work are services wearing a subscription's clothes (E14) |
| 4 | **Churn at pool change** | < 20% annually |
| 5 | **Frontier-runs published by third parties** | ≥ 3/yr — the publication loop is the only viral mechanism |

### Ignore at this stage

**ARR growth rate in isolation** — ACV rises from $30,000 to ~$95,000 in [../financials/revenue_build.md](../financials/revenue_build.md), but through Product 2 attach and the customer's own spend compounding, so a growth rate says nothing about whether accounts are surviving pool changes. **Spend under management reported as value delivered** — Product 2 prices on it (revenue build M6), which makes it a legitimate billing basis and a misleading success metric: it rises when the customer's bill rises, including when CAMIR saved nothing. Report it beside the measured saving, never instead of it.

---

## The three metrics that are never vanity, at any stage

1. **M16, the disqualification rate.** The only metric that gets *worse* when CAMIR is dishonest. Non-zero at every stage.
2. **M11, the pin-to-large rate.** A failure metric on the front page. A product that hid it would be hiding its own adoption failing.
3. **M3, the artifact share.** The contribution, and it is expected to *shrink* as a customer's harness quality improves — 19pp, 14pp, 6pp across the three journeys. **A metric that is designed to decline cannot be used to flatter anyone.**

---

## Recommended next 3

1. **Publish M16 and M11 externally from the first customer.** Both get worse when the company is failing and neither can be gamed upward, which is what makes publishing them a credibility asset rather than a disclosure cost — and it is the same move as publishing inter-judge agreement.
2. **Ban "requests routed" and "total savings" from every internal review deck now, before either has a value.** They are the two numbers that will grow fastest, look best and conceal a deployment where every endpoint is pinned. Naming them before they exist is far cheaper than retiring them afterwards.
3. **Instrument survival-through-a-pool-change as a tracked metric from the first design partner.** [S29] guarantees the event will come, it is the empirical form of the maintenance-capacity argument the whole subscription rests on, and it cannot be measured retroactively.
