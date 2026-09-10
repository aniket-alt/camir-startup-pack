# CAMIR — Get / Keep / Grow

**What this is** — the funnel per segment: how a team first encounters CAMIR and reaches the activation moment, what makes them keep it once the novelty of the first chart has passed, and the two expansion motions that turn one endpoint into a control-plane contract. Each stage carries a metric, a target and the lever that moves it.
**Why it exists** — CAMIR's Keep problem is unusual and easy to get wrong in the direction that kills it. **The product succeeds by being opened twice a month**, so every conventional retention signal — daily actives, session length, dashboard visits — is inverted here: rising engagement means something is going wrong. A funnel copied from a SaaS template would optimise for the exact behaviour that indicates the router is causing quality questions. Separately, Get runs through a population that will never pay: open-core converts 1–5% of active users [S40], and a funnel that treats the other 95–99% as leads destroys the channel it depends on.
**How to read it** — the Keep section is the one to attack; its central claim is that a monthly-opened product retains, and the evidence offered is structural rather than observed. A skeptic should also check that no metric here rewards attention.
**Depends on / feeds** — depends on [../strategy/gtm.md](../strategy/gtm.md), [../strategy/channel_plan.md](../strategy/channel_plan.md), [../strategy/personas.md](../strategy/personas.md), [mvp_definition.md](mvp_definition.md); feeds [metrics_by_stage.md](metrics_by_stage.md), [../financials/revenue_build.md](../financials/revenue_build.md) and [../financials/unit_economics.md](../financials/unit_economics.md).

**Status: no funnel data exists.** Every target below is `(assumption)` with its basis named.

---

## The shape, in one line

**Get** is free and open and mostly reaches people who will never pay. **Keep** is measured in months, not days, and is held by a re-measurement obligation rather than a habit. **Grow** is endpoint-by-endpoint inside one account, not seat-by-seat.

---

## 1. GET

### The channel, and the four that were rejected

[../strategy/channel_plan.md](../strategy/channel_plan.md) computes the margin stack and finds one channel survives: **ship as a routing strategy inside LiteLLM** [S22], plus the open repository itself. Cloud marketplaces take 84.3% net, system integrators 59.8%, and outbound CAC runs ~117% of ACV against a $30,000 ACV. **The LiteLLM dependency is unagreed** and is first in the business-model kill order.

### The path, per segment

| Segment | First contact | Activation moment | Metric | Target `(assumption)` |
|---|---|---|---|---|
| **P6 Sam** — OSS, below the volume floor [S27] | A post, a repo, a conference talk | **A frontier curve from his own logs in one afternoon** | Installs → first `frontier_run` | ≥ 40% of installs reach a frontier |
| **P2 Marcus** — beachhead | Sam's post or Wen's talk, then the repo | **A ceiling with an artifact decomposition on his own traffic** — or a disqualification | Time to first frontier (M7) | Median < 5 days; G1 says under a week |
| **P1 Priya** — drop-in | Search, or the LiteLLM strategy list | Same, at smaller scale | Same | An afternoon ([../product/journeys/edge_low.md](../product/journeys/edge_low.md)) |
| **P3 Wen** — edge-high | The published methodology and judge-agreement numbers | **Her own judge plugged in, agreement computed by our code** | Interface replacements | Any is a strong signal |

**The activation moment is identical for everyone and contains no routing.** It is a number about their own traffic that they could not previously obtain. That is why the low-fidelity MVP is the wedge and not the trial ([mvp_definition.md](mvp_definition.md)).

### The lever, and the one that is banned

**Lever:** reduce the distance from `pip install` to a frontier. The likely real drop-off is not interest — it is **log parsing**, the mundane blocker named in [../product/journeys/edge_low.md](../product/journeys/edge_low.md) §Timing. Every format CAMIR reads natively is worth more than any marketing.

**Banned lever: converting Sam.** He is below the volume floor and will never buy. Open-core conversion runs 1–5% to hosted SaaS and 0.01–0.1% for enterprise licences [S40]; **treating the 95–99% as a pipeline is how open-core projects lose the community that is their distribution.** No email sequence, no usage-based upsell nag, no feature gate that appears mid-run.

---

## 2. KEEP

### The habit that is not a habit

CAMIR is opened **twice a month** by the one person who opens it at all ([../product/journeys/day_in_life.md](../product/journeys/day_in_life.md): total human time across three people on an ordinary day, about twenty minutes). That is the target, not a shortfall. What retains is not a habit loop — it is a **standing obligation**: the frontier is a dated measurement that decays every time the pool changes, a model is upgraded, or the traffic mix shifts [S29]. Wen's last internal router died of exactly this — correct when written, wrong six months later, owned by nobody ([../product/journeys/edge_high.md](../product/journeys/edge_high.md) §Act IV).

### The metric that predicts retention

**Not usage. Endpoints under an owned tolerance policy (M8), and specifically M9 — the share of those policies whose owner is the *consuming* team rather than the platform team.**

An endpoint whose tolerance is owned by the platform team is a configuration. An endpoint whose tolerance is owned by the consuming engineer is a **decision a named person made and would have to un-make**, and that person is the one who can otherwise kill the deployment. M9 rising is the deployment becoming organisationally load-bearing; M9 flat means one engineer is running a tool.

| Keep metric | What it means | Target `(assumption)` | Lever |
|---|---|---|---|
| **M9** — policies owned by consuming teams | The deployment has spread past its champion | ≥ 50% by month 6 | Shadow reports addressed to the endpoint's own owner |
| **M11** — pin-to-large rate | **Rising is a product failure**, and it is on the front page as one | < 2%, trend flat | Fast attribution ([../tech/architecture/D08.md](../tech/architecture/D08.md)) |
| **M13** — quality question → attributed answer | The survival condition | < 1 hour; 4 minutes in the worked case | The trace stamp, which is the feature that does this |
| **M12** — recalibrations per quarter | The obligation is being met | ≥ 1 | Unconditional re-run on `pool_manifest` change |
| **M16** — disqualification rate | CAMIR's honesty is being believed | **non-zero** | The report as a product surface |

### The churn shapes, ranked

| # | How it churns | Signal | Countermeasure |
|---|---|---|---|
| 1 | **Ravi pins and never unpins** | M11 rising, month 3–4 | The four P0 features; the fast attribution path |
| 2 | **The ceiling was never there** | Disqualification in week 1 | Not churn — a correct, cheap negative that should be non-zero |
| 3 | Escalation rate drifts past break-even | M6 above the drawn line | Escalation-rate accounting shown *before* enforcement (PR1) |
| 4 | Champion leaves | M9 low — one owner, one person | M9 is the leading indicator; raising it is the mitigation |
| 5 | Serving layer ships it natively | — | The measurement half, which no engine will run [S11] |
| 6 | Alert fatigue | Breach alerts muted | Run rules; publish the false-positive rate |

**Churn shape 4 is the one a low-engagement product is most exposed to.** A tool opened twice a month by one person disappears when that person changes teams — which is the strongest practical argument for driving M9 rather than usage.

---

## 3. GROW

Two motions, and neither is seats.

### 3.1 Endpoint expansion — inside the account

The unit is the **endpoint**, not the seat, because that is the isolation unit ([../tech/architecture/D09.md](../tech/architecture/D09.md)). A landed account has six endpoints and typically starts with one enforced, one disqualified and four in shadow.

| Stage | Metric | Target `(assumption)` | Lever |
|---|---|---|---|
| Land | 1 endpoint enforced | month 1 | The endpoint whose ceiling was highest and whose owner was most willing |
| Spread | endpoints enforced | 3 of 6 by month 4 ([../product/journeys/day_in_life.md](../product/journeys/day_in_life.md)) | **Each owner enables their own**, after their own shadow report |
| Deepen | pool tiers measured | +1 tier per pool change | Recalibration as the recurring event |

**The referral mechanism inside the account is Ravi telling another product engineer that the attribution worked.** That is the domain's actual word of mouth and it is generated by [../tech/architecture/D08.md](../tech/architecture/D08.md)'s fast path, not by marketing.

### 3.2 The publication loop — outside the account

CAMIR's only real viral mechanism, and it is a research one: **a team measures their own oracle ceiling, finds part of it was their harness, and says so publicly.** Wen's conference sentence — *"we measured our own oracle ceiling and 6 points of it was our harness"* — shortens Marcus's evaluation by two weeks. Sam's chart is the same loop at smaller scale.

This works because it is a finding rather than an endorsement, it is checkable, and nobody has published the decomposition on a self-hosted pool [G2]. **It stops working the moment the artifact carries a product claim**, because a finding is repeatable and an endorsement is spent.

| Metric | Target `(assumption)` | Lever |
|---|---|---|
| Public `frontier_run`s published by third parties | ≥ 3 in year 1 | Make export one command; never watermark it |
| Conference talks citing the methodology | ≥ 1 in year 1 | Publish the cost-axis derivation as a standalone document |

### What is not a growth motion — and one tension that is

**Seats.** Nobody sits in front of CAMIR; the buyer is the bill.

**The tension, stated rather than hidden.** Year one prices on **share of measured savings** ([../financials/pricing.md](../financials/pricing.md)), which grows only when the customer's saving grows — as more endpoints are enforced. Product 2 and the declared repricing trigger move to a **percentage of spend under management** ([../financials/revenue_build.md](../financials/revenue_build.md) M6), which grows with the customer's bill whether or not CAMIR saved anything. The second is the OpenRouter shape [S17] that this pack argues against, and it is where the revenue build reaches venture scale. The guard is the same one that makes share-of-savings credible: the counterfactual is computed by open code inside the customer's perimeter, so a customer paying on spend can always see what the spend bought. **Whether that guard survives a spend-percentage contract is untested**, and it is the incentive question an investor should ask about the Y5–Y8 rows.

---

## Recommended next 3

1. **Instrument install → first frontier and publish the drop-off honestly.** It is the single Get metric that matters, the activation moment is identical across every segment, and the likely blocker is log parsing rather than interest — a mundane problem worth more engineering than any channel work.
2. **Make M9 the retention KPI from the first deployment, not usage.** The share of tolerance policies owned by consuming teams is the only metric that measures whether the deployment survived the organisation, and it is the leading indicator for both churn shape 1 and churn shape 4.
3. **Write and enforce a no-conversion policy toward OSS users before the first release.** Open-core credibility is spent once [S23], Sam is 95–99% of users and 0% of revenue by design [S40], and the first growth-pressure quarter is when the upsell nag gets added by someone acting reasonably.
