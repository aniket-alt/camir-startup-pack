# CAMIR — Experiment Board

**What this is** — the Blank-style learning log: fifteen experiments against the assumptions in [riskiest_assumptions.md](riskiest_assumptions.md), each carrying a falsifiable hypothesis with a number in it, a design, and a **pass/fail threshold written before the experiment runs**. Result, Learning and Decision are empty on every row, because every row is `planned`.
**Why it exists** — CAMIR's founders can run a benchmark well and enjoy running one, which is precisely the hazard: without thresholds declared in advance, a two-week oracle-ceiling run becomes a six-month tuning exercise that always finds one more configuration to try, and a "the segment is smaller than we hoped" call becomes an anecdote rather than a count. This board makes the negative result *cheap to accept* by writing it down before anyone is invested in the positive one.
**How to read it** — read the **Threshold** column only. Everything else is scaffolding. A skeptic should attack E4 and E7 on feasibility rather than on design: they require three people with no network and no customers to reach twenty platform engineers and persuade three of them to install unreleased software, and §Capacity check is where that objection is answered honestly rather than waved away.
**Depends on / feeds** — depends on [riskiest_assumptions.md](riskiest_assumptions.md), [../strategy/business_model_canvas.md](../strategy/business_model_canvas.md) §Kill order, [../strategy/gtm.md](../strategy/gtm.md) §The 90-day motion; feeds [stage_gate.md](stage_gate.md), [metrics_by_stage.md](metrics_by_stage.md), [pivot_log.md](pivot_log.md) and [mvp_definition.md](mvp_definition.md).

**Every row below is `planned`. Zero experiments have been run as of 2026-09-09.** The Result, Learning and Decision columns are structurally empty and will stay that way until an experiment finishes. A filled Result column in this file is the only evidence CAMIR will ever have; inventing one would make the rest of the pack worthless.

---

## The board — measurement track (no customer required)

These four can start on **2026-09-14** with three founders, one GPU allocation, and no outside party. They are the reason CAMIR can be falsified before it is built.

### E1 — Oracle ceiling on a self-hosted pool · assumption A2 · rank 1

| | |
|---|---|
| **Hypothesis** | On a public mixed-difficulty benchmark run against a 3-tier self-hosted open-weight pool, an oracle router achieves ≥99% of the fixed-large baseline's judged quality at **≤55%** of its cost. |
| **Design** | Assemble a mixed-difficulty set (~4,000 prompts) spanning conversation, knowledge, math and code, following [S7]'s dataset composition. Generate an answer from each of three tiers for every prompt. Judge every answer under a fixed protocol: temperature 0, fixed position, verbosity-controlled [S34]. Label per-prompt per-tier correctness. The oracle router is then the per-prompt argmin over tiers that answered correctly; its cost is computed on a **self-hosted cost axis** — amortised GPU-hours per token at a stated utilisation — not borrowed hosted list prices [S26][S27]. |
| **Threshold (declared 2026-09-09)** | **PASS**: oracle cost ≤55% of fixed-large at ≥99% quality. **AMBER**: 55–70%. **FAIL/KILL**: ≥70% — the saving is under 30% before any router imperfection, before the [S4] plateau and before the cache haircut [S36]. |
| **Cost / time** | ~120 GPU-hours; report token volume, throughput, model residency, judge cost and utilisation before converting to dollars `(assumption: [S26][S27] anchor rates only)` · 2 weeks · 2026-09-14 → 2026-09-28 |
| **Result** | — `planned` |
| **Learning** | — |
| **Decision** | On FAIL: publish the null result and stop. On AMBER: proceed only if E2 lifts the ceiling above 55%. |

### E2 — Artefact-controlled re-measurement · the harness thesis (rank 1b) and A13 (rank 14)

| | |
|---|---|
| **Hypothesis** | Controlling for the three evaluation artefacts [S5] documents — truncation under fixed generation budgets, output-format parse failure, judge verbosity bias — raises the measured oracle ceiling by **≥5 percentage points** of small-tier correctness relative to a naive harness on the same prompts and the same models. |
| **Design** | Reuse the same raw generations where possible, then use staged or factorial comparisons so generation budget, parsing, and judging effects are isolated. **Naive arm**: fixed 512-token generation budget, strict single-format parse, single judge at default temperature. **Controlled arm**: budget raised until truncation is measured, tolerant parser with parse-failure counts, two judges at temperature 0 with position fixed or permuted and length normalised, with inter-judge agreement published [S33][S34]. Report the measured-label delta separately from any change in routing or savings. |
| **Threshold** | **PASS**: controlled arm's small-tier correctness ≥5pp above the naive arm, with truncation and parse-failure rates reported per benchmark. **FAIL**: <2pp — in which case "much of the apparent ceiling is your harness" is not supported on this pool and the wedge narrative in [../strategy/positioning.md](../strategy/positioning.md) loses its mechanism, even if E1 passes. |
| **Why the number** | [S5] reports truncation affecting 65% of MMLU and 57% of MedQA cases and 5–12% parse failures on MMLU. If artefacts at that rate move the ceiling by less than 2pp on CAMIR's pool, either the pool is unusually clean or the harness is not measuring what [S5] measured. Both are findings; neither supports the pitch. |
| **Cost / time** | Inside E1's GPU budget · reported 2026-09-28 |
| **Result / Learning / Decision** | — `planned` |

### E3 — Classifier route versus calibrated confidence · assumption A1 · rank 4

| | |
|---|---|
| **Hypothesis** | A trained prompt-difficulty classifier captures **≥50%** of the gap between the cascade route and the oracle ceiling, and beats a calibrated-confidence cascade [S8] by **≥2pp** of judged quality at matched cost. |
| **Design** | Reuse E1's generations, so no new inference is required. Three arms at matched cost: (a) cascade with a calibrated-confidence escalation threshold [S8]; (b) classifier route, dispatch-once, trained on prompt features with a held-out split; (c) oracle. Report all three as curves on the cost-quality plane, not as points [S7]. |
| **Threshold** | **PASS**: both conditions met. **FAIL**: either missed. |
| **Why the number** | [S4] tested 21 routing methods across 5 benchmarks and found the best available remedies bought **up to 2.13 percentage points**. A 2pp threshold is deliberately set at the edge of the published state of the art: passing it is a publishable result, failing it is the expected outcome and costs CAMIR nothing, because locked decision 3 already makes cascade the primary route. |
| **Cost / time** | GPU-hours only · 1 week · 2026-09-29 → 2026-10-05 |
| **Decision on FAIL** | Classifier route is permanently demoted to a published ablation. Cascade with calibrated confidence ships. No pack artifact changes, which is the point of having decided this in advance. |

### E12 — Residual saving after semantic caching · assumption A14 · gap [G4] · rank 5

| | |
|---|---|
| **Hypothesis** | Routing on cache-miss traffic retains **≥60%** of the saving it achieves on uncached traffic — i.e. caching and routing are substantially additive rather than substitutive. |
| **Design** | Use a time-ordered production-like replay corpus, or label the public corpus synthetic and non-decisive. Apply an embedding-similarity semantic cache [S37] and report hit quality, false/stale hits and residual difficulty; do not tune only to a target hit rate. Recompute the oracle ceiling and cascade frontier on cache-miss traffic and compare per-remaining-request results with the uncached run. |
| **Threshold** | **PASS**: ≥60% retained. **FAIL**: <60% — caching has already harvested most of the easy traffic, CAMIR's addressable saving shrinks materially, and factor f4 in [../strategy/market_sizing.md](../strategy/market_sizing.md) plus the ACV derived from it are both wrong. |
| **Why it matters more than it looks** | The adverse-selection worry is specific: cache hits skew to repetitive easy requests, exactly the ones routing would have sent to the small tier. If that skew is strong, post-cache traffic is harder than average and routing saves less on it. [G4] records that nobody has published this measurement, which makes it both a risk and a publishable contribution. |
| **Cost / time** | GPU-hours, ~4 days · 2026-10-05 → 2026-10-09 |
| **Result / Learning / Decision** | — `planned` |

---

## The board — discovery track (people required)

### E4 — Does the segment exist, and what is f2 · assumption A3 · rank 2

| | |
|---|---|
| **Hypothesis** | Of 20 screened platform engineers at companies of 200–800 people running an LLM feature in production, **≥6** run a self-hosted open-weight pool of two or more model sizes at ≥$50k/month with mixed-difficulty traffic through a shared service. |
| **Design** | 20 problem interviews under Mom-Test discipline, script in [discovery_guide.md](discovery_guide.md). The f2-collapsing question is asked in every one: *what share of your production LLM tokens runs on weights you operate, and what is the monthly spend?* Answers are recorded as two numbers per company, never as impressions. Run alongside a desk count of companies visibly hiring for self-hosted inference roles, as an independent check on the same population. |
| **Threshold** | **PASS**: ≥6 of 20 qualify. **KILL**: <6 — the beachhead is thinner than [../strategy/market_sizing.md](../strategy/market_sizing.md)'s base case and the corridor collapses toward the $10M end, where CAMIR is a feature rather than a company. |
| **Secondary output** | The recorded numbers give a direct read on **f2**, the widest and least evidenced factor in the sizing (×0.15 / ×0.30 / ×0.50 across a 36× corridor). Collapsing f2 is the highest-value single outcome available anywhere in this plan. |
| **Cost / time** | $0, ~60 founder-hours · 3 weeks · 2026-09-14 → 2026-10-05 |
| **Result / Learning / Decision** | — `planned` |

### E5 — Tolerance-first versus savings-first framing · positioning

| | |
|---|---|
| **Hypothesis** | Presented with two one-paragraph descriptions of the same system, **more than 6 of 20** interviewees choose the tolerance-first framing as the one they would forward to their manager. |
| **Design** | At the end of each E4 call, after all past-behaviour questions are complete so the framing cannot contaminate them: read both paragraphs in an order alternated call by call, ask which they would forward, then ask *why* and record the reason verbatim. The reason matters more than the count. |
| **Threshold** | **PASS**: tolerance-first chosen by ≥7 of 20. **FAIL**: savings-first preferred by **more than 14 of 20** — positioning is re-derived from scratch, and [../strategy/positioning.md](../strategy/positioning.md)'s central claim that the cost-quality plane is an output rather than a position is wrong. |
| **Known weakness of this design** | It is the one question on the board that asks about a stated preference rather than a past action, which is exactly what the Mom Test warns against. It is included because there is no past behaviour to ask about — nobody has ever been offered a customer-set tolerance — and it is scored as *weak evidence, deliberately*: a pass moves nothing on its own, a strong fail is what would move the pack. |
| **Cost / time** | $0, inside E4 · 2026-10-05 |
| **Result / Learning / Decision** | — `planned` |

### E6 — Channel acceptance · rank 6

| | |
|---|---|
| **Hypothesis** | The maintainers of the self-hosted proxy that the beachhead already runs [S22] will accept an upstream routing strategy contributed by an outside team, and will not have shipped a native equivalent first. |
| **Design** | A proposal issue plus a working prototype opened publicly on **2026-09-15**. Track three signals: maintainer response within 30 days; a merged or accepted-in-principle decision; and any native routing/measurement feature landing in the same window from either the proxy or the serving engine [S11]. |
| **Threshold** | **PASS**: substantive maintainer engagement by **2026-10-15** and acceptance-in-principle by **2026-12-15**. **FAIL**: silence, rejection, or a native equivalent landing first — [../strategy/gtm.md](../strategy/gtm.md) then has no primary channel, and the whole GTM is re-planned rather than patched, because every other channel was already rejected on economics in [../strategy/channel_plan.md](../strategy/channel_plan.md). |
| **Cost / time** | 1–2 engineer-weeks · opened 2026-09-15, decided 2026-12-15 |
| **Result / Learning / Decision** | — `planned` |

### E7 — Will anyone install it for free · leading indicator on A4

| | |
|---|---|
| **Hypothesis** | **≥3** of the 20 teams from E4 will run shadow mode — routing decisions computed and logged, all traffic still served by the fixed-model baseline — on real production traffic for 2–4 weeks, at no charge. |
| **Design** | Offer only to teams that qualified in E4. Shadow mode ships with tier-stamped traces and per-endpoint tolerance config from the first version, because those cannot be retrofitted after a deployment has already alarmed a consuming team. The consuming team is **informed at shadow start, not at enforcement**. |
| **Threshold** | **PASS**: ≥3 installs running on real traffic by **2027-01-31**. **FAIL**: <3 — A4 is answered *no* before any price is ever quoted. A team that will not accept a free, non-enforcing, in-perimeter measurement of its own inference bill is not a team that will pay a fee for the enforcing version. |
| **Cost / time** | Founder time plus support · 2026-10-06 → 2027-01-31 |
| **Result / Learning / Decision** | — `planned` |

### E8 — Will anyone pay · assumption A4 · rank 3

| | |
|---|---|
| **Hypothesis** | **≥1 of 3** shadow-install teams, having seen a measured before-and-after on their own traffic, will name a specific number they would pay. |
| **Design** | Present two pricing shapes side by side rather than one: share of measured savings (~28%) and per-request / ~5% of inference spend [S17][S38][S39]. Ask which shape and what number. Record the objection verbatim when the answer is no. |
| **Threshold** | **PASS**: ≥1 of 3 names a figure. **FAIL/KILL for the commercial layer**: **zero of three** will discuss a number. "Maybe next quarter" counts as a fail — the threshold is written that way on purpose, because the ambiguous answer is the one that can absorb a year. |
| **Cost / time** | ~$0 · 2027-02-15 → 2027-03-15 |
| **Result / Learning / Decision** | — `planned` |

### E9 — Is the saving billable · assumption A5 · rank 7

| | |
|---|---|
| **Hypothesis** | Two parties independently computing the same month's counterfactual from the same trace log arrive at figures within **15%** of each other. |
| **Design** | During a shadow install, the customer's engineer and CAMIR each compute "what the fixed-model baseline would have cost" from the identical logged traffic, without conferring, using a written definition of eligible savings drafted in advance. Compare. |
| **Threshold** | **PASS**: within 15%. **FAIL**: >15% divergence — share-of-savings pricing is unbillable and the declared fallback (per-request, or a share of inference spend) becomes the plan. FinOps precedent says the definition of eligible savings is precisely what gets negotiated [S38], so a wide divergence is the expected outcome of *not* drafting the definition early. |
| **Cost / time** | ~$0, days · during the first shadow install, ~2027-01 |
| **Result / Learning / Decision** | — `planned` |

### E13 — Does the veto hold · rank 11

| | |
|---|---|
| **Hypothesis** | Across three enforced deployments, **every** escalation by a consuming team in the first two weeks of enforcement is answered with per-request attribution within one hour and closed with a recorded decision — resume, loosen, or stay pinned. |
| **Design** | Enforcement follows 2–4 weeks of shadow mode in which the consuming team has already seen its own endpoint's numbers. The P0 set is live: per-endpoint tolerance the consuming team owns, tier stamped on every trace, unilateral pin-to-large requiring no ticket. Log every escalation, every pin event, and the elapsed time from a quality question being raised to it being answered with data. |
| **Threshold** | **PASS**: every pin/revert is safe, acknowledged, traceable within **1 hour**, and the team resumes or rejects routing with a recorded reason. **FAIL**: an unacknowledged breach, missing attribution, or inability to recover; a veto itself is a promised safety control, not automatic evidence that P0 failed. |
| **Why this is on the board at all** | The public precedent is one-sided: when routing shipped with a vendor-set tolerance and no dial, users reported degradation on complex queries immediately [S20][S21]. That the *absence* of a dial causes escalation is evidenced. That the *presence* of one prevents it is assumed. |
| **Cost / time** | $0, observation · ~2027-02 |
| **Result / Learning / Decision** | — `planned` |

### E10 — Is there a moat · assumption A6 · rank 8

| | |
|---|---|
| **Hypothesis** | A router trained on 1,000,000 of a customer's own requests delivers **≥3%** more realised saving at fixed tolerance than a cold-start router on the same traffic. |
| **Design** | Offline, on one customer's logged traffic. Train on the first N requests for N across four orders of magnitude; measure realised saving at fixed tolerance as N grows; plot the curve. |
| **Threshold** | **PASS**: ≥3% gap and a curve still rising at N = 1M. **FAIL**: cold start within 3% of the warmed router, or a curve that flattens by N = 10,000 — the moat is zero, and the pack must say so plainly rather than quietly retaining the sentence about accumulated routing history. |
| **Honest note** | This is the only experiment on the board that can produce a *positive* moat finding. `../BRIEF.md` and [../strategy/lean_canvas.md](../strategy/lean_canvas.md) both already default to "the moat is weak", so a fail changes no artifact and a pass improves several. |
| **Cost / time** | GPU-hours, 1 week · gated on real traffic, ~2027-02 |
| **Result / Learning / Decision** | — `planned` |

### E11 — Is re-measurement recurring · rank 9 · the retention mechanism

| | |
|---|---|
| **Hypothesis** | Across one model-upgrade cycle on an unchanged pool, the cost-optimal point at a fixed quality tolerance moves by **≥5%** of the cost axis. |
| **Design** | Freeze one benchmark and one tolerance. Measure the frontier before an upgrade to any tier in the pool, and again after. Report the displacement of the optimal operating point and the realised cost of leaving the old policy in place. |
| **Threshold** | **PASS**: ≥5% movement, i.e. a stale policy demonstrably loses money and re-measurement is a subscription. **FAIL**: <5% — routing is a one-time consulting engagement, the control plane has no recurring value, and the revenue model in [../strategy/lean_canvas.md](../strategy/lean_canvas.md) changes shape entirely. |
| **Countervailing evidence to watch** | Tier spreads are compressing — a 31B model within ~10 LMArena Elo of 600B–1000B+ open-weight models as of April 2026 [S29]. Compression could move the optimal point a great deal (helping this hypothesis) while simultaneously shrinking the saving available at that point (hurting E1). The two effects must be reported together or the result is misleading. |
| **Cost / time** | GPU-hours, days · one upgrade cycle, ~Q1 2027 |
| **Result / Learning / Decision** | — `planned` |

### E14 — Is the margin software-shaped · rank 13

| | |
|---|---|
| **Hypothesis** | GPU-hours for per-deployment classifier training plus recurring judge inference cost **<30%** of the $30k ACV per customer per year. |
| **Design** | Meter every GPU-hour attributable to a single shadow install end to end — benchmark generation, classifier training, and the recurring judge inference that continuous quality measurement requires. Annualise; divide by ACV. |
| **Threshold** | **PASS**: <30% of ACV, gross margin above 70%. **FAIL**: ≥30% — the business is services-shaped, not software-shaped, and should be described that way rather than modelled as SaaS. |
| **Why it is measurable this early** | It is the one unit-economics number that does not need a paying customer: metering a free shadow install gives the numerator, and the denominator is already assumed. |
| **Cost / time** | Metering only · from the first install |
| **Result / Learning / Decision** | — `planned` |

### E15 — Does the open half convert · open-core rate

| | |
|---|---|
| **Hypothesis** | **≥0.5%** of installs that enable the routing strategy cross the volume floor (~16M tokens/day [S27]) at which the paid control plane is worth anything. |
| **Design** | Instrument enable-rate and a coarse volume bucket from the first public release, opt-in and privacy-preserving, published openly so the instrumentation itself does not damage the open-core relationship. No conversion attempt is made against this data. |
| **Threshold** | **PASS**: ≥0.5% above the floor. **FAIL**: <0.5% — proxy-native distribution does not reach a payable population, and CAMIR is a widely-used free tool with no revenue: the exact shape TensorZero had at 11,000 stars when it archived the repository and returned $7.3M [S23]. |
| **Cost / time** | Product-native, 0 marginal · continuous from first release |
| **Result / Learning / Decision** | — `planned` |

---

## Sequence and dependency

```
  2026-09    E1 ─── E2 ────┐                  E4 ────────┐        E6 (opened, decides 12-15)
  measurement track        │                  discovery  │
  2026-10          E3 ─ E12┤                        E5 ──┘
                           │                             │
                           ▼                             ▼
                     KILL GATE 1                   KILL GATE 2
                  (A2: is there a ceiling)     (A3: is there a segment)
                           └──────────┬──────────────────┘
                                      ▼
  2026-10 → 2027-01              E7 (3 shadow installs)  ── E14, E9 ──┐
                                      │                               │
  2027-02                        E13 (enforcement) ── E10 ── E11 ─────┤
                                      ▼                               │
  2027-03                          E8 (price) ◄───────────────────────┘
                                      ▼
                               KILL GATE 3 (A4: will anyone pay)
```

**Nothing downstream of Kill Gate 1 or Kill Gate 2 is worth starting until both are passed.** Both are resolved by mid-October 2026 for ~120 metered GPU-hours (≈ $300–450 at [S26] rates, on the node budgeted in [../financials/use_of_funds.md](../financials/use_of_funds.md) Block 1) and sixty hours of conversation.

---

## Capacity check — can three people actually run this

The board is only honest if the team can execute it. Three founders, part-time, on a capstone timeline, with no customers and no network.

| Track | Real constraint | Honest assessment |
|---|---|---|
| **E1, E2, E3, E12** | ~120 GPU-hours and a judging harness | **Comfortably feasible.** This is exactly the benchmark-engineering work the team has done before, it needs no outside party, and the four experiments share one set of generations. |
| **E4, E5 — 20 interviews in 3 weeks** | **The binding constraint on the whole plan.** Three students with no network reaching twenty staff platform engineers is the hardest thing on this board, harder than any GPU run | At a realistic 8–12% response rate, twenty completed calls needs **roughly 200 qualified outreach attempts**. Sources, in descending expected yield: the capstone advisor's industry network; issue and discussion participants in the self-hosted proxy and serving-engine repositories, who are pre-qualified by behaviour; local infrastructure meetups; practitioner forums. `(assumption: response rates are inferred from developer-tool discovery norms, not measured)` **If twenty calls prove unreachable, report the achieved N and apply the threshold proportionally — 6-of-20 becomes 3-of-10 — and say so.** Silently shrinking the denominator to reach a pass is the single most likely way this board gets corrupted. |
| **E6** | Another project's maintainers and their review queue | Out of the team's control; 1–2 engineer-weeks of the team's own effort, then waiting. This is why it opens in week one. |
| **E7, E9, E13, E14** | Three teams willing to install unreleased software | **The second binding constraint.** Gated entirely on E4 producing qualified teams and on the harness being genuinely afternoon-easy. If E4 yields six qualified teams, three shadow installs is a 50% conversion from a warm conversation, which is optimistic. |
| **E10, E11** | Real customer traffic and one model-upgrade cycle | **Cannot be run in the first 90 days at all**, and the board says so rather than scheduling them optimistically. E10 has a weaker substitute if no customer traffic materialises: run it on a public multi-turn trace corpus and label the result as a proxy, not a measurement. |

**What this table means:** the four experiments that can kill CAMIR cheapest are also the four the team is best equipped to run, and the experiments most likely to slip are the ones requiring other people. That is the correct shape, and it is the argument for starting the discovery track on the same day as the measurement track rather than after it.

---

## Recommended next 3

1. **Start E1 and E4 on 2026-09-14, in parallel.** Kill Gate 1 and Kill Gate 2 both resolve by mid-October for ~120 GPU-hours and sixty hours of conversation. Nothing else on this board deserves attention until they do.
2. **Open E6 on 2026-09-15.** It is the only experiment whose clock belongs to someone else, and a rejection in December costs far less than a rejection in June.
3. **Pre-register E1 and E2 publicly before running them** — the prompt set, the judging protocol, the cost axis, and both thresholds. The field's own critique is that router evaluations are not comparable across papers [S13]; a pre-registered protocol is a cheap, credible answer to that and it removes the team's ability to move its own goalposts.

<!-- critic: round 1 recorded 2026-09-10 in ../audit/CRITIC_LOG.md — 2 major, 1 minor fixed. Round 0 (commit 712241c) edits were retained but left no verdict record. -->
