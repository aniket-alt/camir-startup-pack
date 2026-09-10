# CAMIR — Stage gate: where this company actually is, and what the next gate costs

**What this is** — CAMIR's placement in Blank's four steps (Customer Discovery → Validation → Creation → Company Building), the evidence supporting that placement, and the numbered exit criteria for each gate with thresholds declared before the results exist.
**Why it exists** — a pack this thorough reads like a company much further along than this one is. Sixty artifacts, a fourteen-row assumptions register, a 139-technique arsenal and eleven architecture diagrams are all outputs of *reasoning*, and none of them is evidence about a customer. **Documentation is the most convincing thing a pre-discovery company can produce and the least informative**, and without this page a reader — or a founder — could mistake the pack's completeness for progress. The specific failure it prevents: skipping the discovery gate because the research layer feels like it already answered the questions, when [G1] records that the one number the whole sizing rests on could not be sourced at all.
**How to read it** — §1's placement is the claim; §2's gate table is the operational content. A skeptic should attack the Gate 1 thresholds as too lenient, and should check that none of the "evidence" cited for the current stage is a document this pack produced.
**Depends on / feeds** — depends on [riskiest_assumptions.md](riskiest_assumptions.md), [experiment_board.md](experiment_board.md), [discovery_guide.md](discovery_guide.md), [mvp_definition.md](mvp_definition.md), [../ASSUMPTIONS.md](../ASSUMPTIONS.md); feeds [metrics_by_stage.md](metrics_by_stage.md), [pivot_log.md](pivot_log.md), [../financials/use_of_funds.md](../financials/use_of_funds.md) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

---

## 1. Placement: **Customer Discovery, phase 1 — hypotheses stated, none tested**

```
   [ DISCOVERY ]------ VALIDATION ------ CREATION ------ COMPANY BUILDING
     ^
     |
   CAMIR is here.
   Hypotheses written and falsifiable. Zero interviews. Zero code. Zero measurements.
```

**The evidence for this placement, and note what is absent from it.**

| Fact | Source |
|---|---|
| Zero customer interviews conducted | [discovery_guide.md](discovery_guide.md) — the instrument exists, no results |
| Zero lines of CAMIR code written | [../tech/not_vaporware.md](../tech/not_vaporware.md) — a buildability argument, not a build report |
| Zero measurements of CAMIR's own | [../ASSUMPTIONS.md](../ASSUMPTIONS.md) §"What is not assumed": nothing in this pack is measured |
| The beachhead's size cannot be sourced | [G1] — no survey gives self-hosted production share with a denominator |
| No published routing savings exist for a self-hosted pool | [G2] — the gap CAMIR claims to fill, and the reason no prior number transfers |
| Pre-traction, CMPE 295A capstone origin | [../BRIEF.md](../BRIEF.md), A10 |

**What this pack *is* evidence of:** that the hypotheses are stated precisely enough to be falsified, that the riskiest one has been re-scoped away from a claim [S4] would demolish, and that the kill conditions are written down in advance. That is the correct output of pre-discovery work and it is not progress toward a customer.

**The one thing that could be mistaken for traction and is not:** the research layer's forty live-searched sources. They are other people's measurements. RouteLLM's 3.66× is MT-Bench and collapses to 1.41× on MMLU [S2]; none of it transfers to a self-hosted pool [G2].

---

## 2. The gates

### Gate 1 — Discovery → Validation

**The question:** is there a problem, does the segment exist, and is the ceiling real?

Two tracks run in parallel because they need different resources: the measurement track needs GPUs and no people, the discovery track needs people and no GPUs.

| # | Exit criterion | Threshold, declared now | Track | Experiment |
|---|---|---|---|---|
| 1.1 | **The oracle ceiling exists** | Median ceiling across ≥ 3 realistic mixed-traffic corpora **above the disqualification threshold** | measurement | E1 |
| 1.2 | **The artifact share is real** | Guarded-minus-unguarded ≥ **5 percentage points** of small-tier correctness on at least 2 of 3 corpora; **below 2pp fails**, 2–5pp is amber | measurement | E2 |
| 1.3 | **The segment exists** | **≥ 6 of 20 screened qualify on [experiment_board.md](experiment_board.md) E4's definition — a self-hosted pool of two or more sizes, ≥ $50k/month, mixed-difficulty traffic through a shared service — with f2 recorded for every call**; fewer than 6 fails | discovery | E4 |
| 1.4 | **Nobody has measured it** | **≥ 12 of 15** qualified teams have never measured their small tier's ceiling | discovery | E4/Q6 |
| 1.5 | **The veto is real** | **≥ 7 of 15** report a consuming team blocking or constraining a shared-service change | discovery | E13 |
| 1.6 | **Free adoption happens** | ≥ 10 third parties reach a `frontier_run` on their own logs | both | E7 |

**Fail conditions, and each has a different consequence.** 1.1 fails → **the company does not exist**; publish the negative result, which is genuinely publishable [G2]. 1.2 fails → the technical thesis is true only of sloppy harnesses; CAMIR is a router like the others and [S4] applies to it. 1.3 fails → the beachhead is smaller than the sizing corridor; hybrid pools move up the roadmap (A9). 1.5 fails → the four P0 features are over-weighted and [../product/features_prioritized.md](../product/features_prioritized.md) is wrong at the top.

**Cost:** GPU time for the measurement track plus roughly two months of interviewing. **This is the cheapest gate and the only one that can end the company for a quarter's spend** — which is why [mvp_definition.md](mvp_definition.md) puts the ceiling probe first.

### Gate 2 — Validation → Creation

**The question:** will someone accept the risk, and will someone pay?

| # | Exit criterion | Threshold | Experiment |
|---|---|---|---|
| 2.1 | **A risk-bearer enables enforcement on his own endpoint** | **≥ 2 of 5** design partners, within 6 weeks of a shadow report, enabled by the *consuming* engineer | E13 |
| 2.2 | **Someone pays** | **≥ 3 paid pilots** at or above $30,000/yr ACV | E8 |
| 2.3 | **The saving is billable** | The counterfactual survives a customer's own re-derivation in ≥ 2 accounts, no dispute on method | E9 |
| 2.4 | **Cascade economics hold** | Escalation rate below the break-even line on ≥ 60% of enforced endpoints | E1 follow-on |
| 2.5 | **The channel works** | The LiteLLM strategy is accepted upstream, or standalone reaches ≥ 10 production deployments | E6 |
| 2.6 | **Disqualification is real** | Disqualification rate **non-zero** across the first 10 evaluations (M16) | — |

**2.1 is the gate.** It is the one this pack's whole organisational thesis rests on, and the one no amount of engineering substitutes for. **2.6 is the gate most likely to be quietly skipped** under revenue pressure — a zero disqualification rate means the ceiling probe is not being believed, by anyone, including CAMIR.

### Gate 3 — Creation → Company Building

| # | Exit criterion | Threshold |
|---|---|---|
| 3.1 | Repeatable sale without founder involvement | ≥ 5 closes where a founder was not in the room |
| 3.2 | Retention through a pool change | ≥ 80% of accounts recalibrate and stay after a model upgrade — the moment [S29] guarantees will come |
| 3.3 | Expansion inside accounts | Median ≥ 3 enforced endpoints per account (M8) |
| 3.4 | Deployment spread past the champion | **M9 ≥ 50%** of tolerance policies owned by consuming teams |
| 3.5 | Open-core conversion in band | ≥ 1% of active OSS deployments convert, against the 1–5% band [S40] |
| 3.6 | ARR | ~$500k, consistent with the Y2 build in [../financials/revenue_build.md](../financials/revenue_build.md) |

**3.2 is the one that decides whether this is a subscription or a consulting engagement**, and it is the empirical form of the argument in [../product/journeys/edge_high.md](../product/journeys/edge_high.md) §Act IV: the scarce resource is the standing obligation to re-measure.

---

## 3. What would move CAMIR *backwards*

Stage gates are usually written as one-way. These are not.

| Trigger | Back to | Why |
|---|---|---|
| Median ceiling below threshold across new corpora | **Discovery** | The premise, not the product, is wrong |
| The serving layer ships the measurement half [S11] | **Discovery** | The wedge is gone; the remaining question is whether a different one exists |
| Design partners pin and never unpin (M11 rising) | **Validation** | Organisational fit failed, and no feature list fixes it from Creation |
| The LiteLLM channel closes | **Validation** | The only channel whose economics survive [../strategy/channel_plan.md](../strategy/channel_plan.md) |

---

## 4. The honest summary

**CAMIR is a pre-discovery company with an unusually well-specified hypothesis set and no evidence.** The pack's value is that it makes the company cheap to falsify: Gate 1.1 can end it in a quarter, and the negative result would itself be publishable [G2]. Everything downstream of Gate 1 is written in advance so that a passing result cannot be manufactured by reinterpreting it afterwards.

**What a reader should not conclude from this pack's completeness:** that anything in it is known.

---

## Recommended next 3

1. **Run Gate 1's measurement track before anything else.** It needs GPUs and no customers, it produces the pack's contribution, and 1.1 is the only criterion in this document whose failure means there is nothing here — which makes it the cheapest information available anywhere in the plan.
2. **Do not begin Gate 2 work while Gate 1.3 and 1.5 are open.** Building the high-fidelity MVP against an unconfirmed segment and an unconfirmed veto is how a quarter becomes a year, and both are interview questions rather than engineering.
3. **Put the 2.6 disqualification threshold in front of whoever funds this.** A company whose plan requires it to turn away qualified prospects is making an unusual commitment, and it is far easier to hold when it was declared before there was revenue to lose by holding it.
