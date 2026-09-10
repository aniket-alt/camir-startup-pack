# CAMIR — Riskiest Assumptions Board

**What this is** — every load-bearing assumption in the CAMIR pack, ranked by decision value, each with its current evidence, the cheapest test that would decide it, that test's cost and elapsed time, and a status that is `untested` on all fourteen rows today.
**Why it exists** — CAMIR's pack reads confidently because its research layer is strong, and that confidence is dangerous: the strong evidence is *other people's* measurements about routing in general, while the three statements that decide whether CAMIR is a company — is the oracle ceiling high enough, does the self-hosted segment exist at scale, will anyone pay for routing when a frontier vendor gives it away [S20] — have no evidence at all. Without this board the team would spend six months building a router before learning the segment has no denominator [G1].
**How to read it** — read §The board's top four rows and stop. Ranks 1–4 are the only rows that can end the venture, and three of the four are decidable before any customer exists. A skeptic should attack rank 3 (A4): its kill threshold is the softest number here, because willingness to pay is being inferred from three conversations.
**Depends on / feeds** — depends on [../ASSUMPTIONS.md](../ASSUMPTIONS.md), [../BRIEF.md](../BRIEF.md) §Riskiest assumption, [../strategy/lean_canvas.md](../strategy/lean_canvas.md) ⚠ cells, [../strategy/business_model_canvas.md](../strategy/business_model_canvas.md) §Kill order; feeds [experiment_board.md](experiment_board.md), [stage_gate.md](stage_gate.md), [mvp_definition.md](mvp_definition.md) and [pivot_log.md](pivot_log.md).

**Status of every row today, 2026-09-09: `untested`.** No discovery interview has been conducted, no benchmark run has completed, no install exists, no price has been quoted. Nothing below is a finding.

---

## How the ranking was built

Rank is **not** kills-if-wrong severity alone. It is severity × decidability × sequence position — the Blank ordering: test the thing that is cheapest, soonest, and would waste the most subsequent work if left untested.

That produces one non-obvious result worth stating up front. `../BRIEF.md` nominates **A1 — difficulty is predictable pre-generation** — as the riskiest assumption. This board ranks it **fourth**, because [S4] already partly answered it in public: 21 routing methods across 5 benchmarks converge into a narrow band far below the oracle router, from a predictability bottleneck, and the best remedies bought up to **2.13 percentage points**. A1 is therefore less open than the published evidence suggests, but not a **known deployment ceiling**: the result is on selected benchmarks and does not settle CAMIR's pool, cascade route or artifact-controlled harness. The pack absorbs the evidence by re-scoping the technical claim away from "our router is more accurate" toward "your frontier is measurable on your pool" (locked decision 2, [../strategy/positioning.md](../strategy/positioning.md)). What A1 still decides is which *route* ships, not whether CAMIR exists.

The rows that decide whether CAMIR exists are **A2** (is there anything to route toward), **A3** (is there anyone to sell to) and **A4** (will they pay for it).

---

## The board

Status legend: `untested` · `testing` · `validated` · `invalidated`. Experiment IDs resolve in [experiment_board.md](experiment_board.md).

| # | Assumption | Kills? | Current evidence (with source) | Cheapest decisive test | Cost | Time | Status |
|---|---|---|---|---|---|---|---|
| **1** | **A2 — the oracle ceiling on realistic mixed traffic is high enough that a perfect router would save materially** | **YES** | None of CAMIR's own. Against: [S15] finds models fail on *overlapping* queries, bounding any combination strategy; [S4] shows every router sits far below oracle. For: [S3] found only 16.6% of queries reached GPT-4 in a cascade; [S5] argues much measured unsolvability is evaluation artefact — 65% MMLU / 57% MedQA truncation, 5–12% MMLU parse failures | **E1** — run every prompt of a public mixed-difficulty benchmark through a 3-tier self-hosted pool, label per-tier correctness under a fixed judging protocol, compute oracle-router cost at ≥99% of fixed-large quality | ~120 GPU-hours; dollar total is not yet reconstructible without token volume, throughput, judge cost and utilisation, so E1 must report those separately `(assumption: [S26][S27] anchor rates only)` | **2 weeks** | `untested` |
| **2** | **A3 — a real population of mid-size teams self-hosts an open-weight pool at production volume** | **YES** | **No denominator exists anywhere** [G1]. Directional only: Ollama 52M monthly downloads Q1 2026 vs ~100K Q1 2023 [S30], explicitly not production deployments. Countervailing: self-hosting rarely wins on cost alone against budget open-weight APIs [S28], so the population is bounded by non-price motives | **E4** — 20 discovery calls asking one question: *what share of your production LLM tokens runs on weights you operate, and what is the monthly spend?* Plus a count of companies visibly hiring for self-hosted inference roles | **$0**, ~60 hours of founder time | **3 weeks** | `untested` |
| **3** | **A4 — a team will pay a third-party fee for routing when a frontier vendor ships routing at no separate charge** [S20] | reshapes | None. Anchors are adjacent, not direct: OpenRouter's revealed ~5% take on inference spend [S17]; share-of-savings precedent in cloud FinOps with no published rate [S38][S39]. [G5] records that no pricing evidence for routing as a separate line item to a self-hosting team could be found | **E8** — after three shadow-mode installs produce a measured before-and-after, put a priced proposal in front of each | ~$0 beyond the installs | **gated on E7**, ~90 days out | `untested` |
| **4** | **A1 — prompt difficulty is predictable pre-generation well enough for a classifier route to beat a fixed-model baseline** | **YES as originally framed; demoted by the re-scope** | **Substantially answered against, in public.** [S4]: 21 methods, 5 benchmarks, narrow band far below oracle, predictability bottleneck, remedies worth ≤2.13pp. [S8]: simple calibrated confidence routes as well as trained routing models — which is the baseline the classifier must beat, not the fixed model | **E3** — classifier route versus calibrated-confidence cascade [S8] at matched cost on the E1 pool | GPU-hours only; reuses E1's generations | **1 week after E1** | `untested` |
| **5** | **A14 — semantic caching is orthogonal to routing rather than substitutive** | reshapes | Production cache hit rates **20–45%** [S36]; ~31% of queries semantically similar to a prior request. **The residual saving available to routing after aggressive caching has never been measured** [G4]. The specific worry is adverse selection: cache hits skew to repetitive easy requests, exactly the ones routing would have sent small | **E12** — compute E1's frontier twice on the same traffic, once raw and once on cache-miss traffic only, and compare achievable saving | GPU-hours, ~4 days | **1 week after E1** | `untested` |
| **6** | **Channel — the primary distribution partner accepts an upstream routing strategy, and does not ship a native equivalent first** | reshapes fatally | None. LiteLLM is the self-hosted proxy baseline [S22]; [S11] documents routing being absorbed into the serving engine itself, the same threat arriving from the other side | **E6** — open the conversation now: a proposal issue plus a working prototype | 1–2 engineer-weeks | **6 weeks** | `untested` |
| **7** | **A5 — savings are attributable enough to bill on** | reshapes | Counterfactual measurement is computable and disputable. FinOps precedent says the **definition of eligible savings is what actually gets negotiated** [S38] | **E9** — have two parties independently compute the same month's counterfactual from the same trace log, and compare | ~$0, days | **gated on a shadow install** | `untested` |
| **8** | **A6 — switching cost rises as per-deployment routing history accumulates** | no, but the moat rounds to zero | None. The founder flagged it untested; `../BRIEF.md` calls switching cost "low, and that is a genuine weakness" | **E10** — offline on one customer's logged traffic: train on the first N requests, measure realised saving at fixed tolerance as N grows | GPU-hours, 1 week | **gated on real traffic** | `untested` |
| **9** | **Keep — the frontier moves enough with each model upgrade that a stale policy loses material money** | reshapes the revenue *shape* | None. Directional: a 31B model within ~10 LMArena Elo of 600B–1000B+ open-weight models as of April 2026 [S29], so tier spreads are actively compressing | **E11** — re-run one fixed benchmark against the same pool before and after one model upgrade; measure how far the optimal tolerance point moves | GPU-hours, days | **one upgrade cycle**, ~Q1 2027 | `untested` |
| **10** | **A8 — the head of platform is the economic buyer and finance is never in the room** | no | Founder's stated view only. The organisation map in [../strategy/sales_roadmap.md](../strategy/sales_roadmap.md) is designed, not observed | Ask in every discovery call: *walk me through the last infrastructure tool your team paid for — who signed, and who else had to say yes?* | $0, inside E4 | **3 weeks** | `untested` |
| **11** | **Veto — the P0 set (per-endpoint tolerance, tier-stamped traces, shadow mode, unilateral pin) is sufficient to stop a consuming team escalating** | reshapes fatally at deployment | Public precedent that the *absence* of a dial produces immediate backlash [S20][S21]. No evidence that the presence of one prevents it | **E13** — at enforcement, watch whether any consuming team escalates to revert within 2 weeks | $0, observation | **gated on E7** | `untested` |
| **12** | **A7 — the control plane holds the data loop, so open-sourcing the router gives away nothing that compounds** | no | Reasoned from where the labelled routing data sits, not observed. The counter-example is dated: TensorZero archived its repository 12 June 2026 after $7.3M raised and 11,000 stars, citing the difficulty of finding fit for an OSS project and a commercial product simultaneously [S23] | The **one-artifact-two-audiences release check** — if a paid feature needs code the open harness lacks, the boundary has already broken | $0, per release | **continuous from first release** | `untested` |
| **13** | **Cost structure — gross margin stays above 70% after GPU-hours for training and judging** | no, but it changes the company's shape | None. The hazard is that judge inference is *recurring*, not one-off, because re-measurement is the retention mechanism | **E14** — meter GPU-hours per shadow install end to end; divide by the $30k ACV | metering only | **from the first install** | `untested` |
| **14** | **A13 — the team's test-automation background transfers to benchmark engineering for LLM routing** | no | Self-assessed. The transferable claim is narrow and specific: [S5]'s artefact taxonomy — truncation, parse failure, verbosity bias — is a test-harness problem, and [S33][S34] give a judging protocol with numbers attached (temperature 0, fixed position, ~76% inter-judge agreement) | **E2** — reproduce [S5]'s artefact controls on CAMIR's own pool and report the ceiling gap they open | inside E1 | **2 weeks** | `untested` |

---

## The four rows that matter, at full strength

### Rank 1 — A2, the oracle ceiling

The only assumption whose failure makes every other row moot, and also the cheapest to test and the one this team is most capable of testing. If the oracle-router cost at ≥99% of fixed-large quality is not materially below the fixed-large cost, **no router helps, including a perfect one**, and CAMIR's correct output is a published null result rather than a company.

Two published results pull in opposite directions and neither settles it. [S15] finds models fail on overlapping queries — a co-failure ceiling bounding routing, voting and mixture-of-agents alike. [S5] finds that a large share of "the small model cannot do this" is an artefact of how it was measured. **Both can be true**: the real ceiling is higher than naive measurement says, and still lower than the routing literature's headline savings imply. E1 and E2 exist to separate those two, and separating them is the most defensible contribution this team can make.

**Kill threshold, declared now:** oracle-router cost ≥ **70%** of the fixed-large baseline at ≥99% quality retention, under artefact-controlled measurement (E2). That is a saving under 30% *before* router imperfection, *before* the [S4] plateau and *before* the cache haircut [S36] — which leaves nothing to sell.

### Rank 2 — A3, does the segment exist

Gap [G1] is the honest state: no survey anywhere gives the share of production LLM workloads served from self-hosted open-weight pools with a denominator. [../strategy/market_sizing.md](../strategy/market_sizing.md) builds bottom-up instead and lands on ~1,370 reachable companies, corridor ~470 to ~3,800 — a **36× corridor**, driven almost entirely by factor **f2** (self-hosted spend as a ratio of hosted spend, ×0.15 / ×0.30 / ×0.50).

Collapsing f2 is the highest-value discovery outcome in the plan and it costs nothing but conversation. At the low end TAM is $10M and CAMIR is a feature; at the high end it is $360M and the case is straightforward.

**Kill threshold:** fewer than **6 of 20** qualified teams meet the profile — self-hosted open-weight pool, ≥$50k/month, mixed-difficulty traffic through a shared service.

### Rank 3 — A4, will anyone pay

The softest number on this board and the one to attack. Three priced proposals is a thin basis for a revenue model, and the real failure mode is not "no" but "maybe, after next quarter" — which reads as neither validation nor invalidation and can absorb a year.

So the threshold is written to make the ambiguous answer count as failure: **zero of three willing to discuss a specific number** — not a signature, not a contract, a number — after seeing their own measured saving. A team that has seen a real saving on its own traffic and still will not name a figure has answered A4.

The context is fixed and public: routing ships free inside a frontier vendor's own catalog with no separate fee [S20]. CAMIR's price is defended against zero for the adjacent segment, and the argument that it is a *different* segment is exactly A3. **Ranks 2 and 3 are therefore not independent** — if A3 comes back weak, A4's threshold is being applied to a population that barely exists, and the honest reading is that both failed together.

### Rank 4 — A1, and why it moved down

The honest reading of [S4] is that the field has already run CAMIR's headline experiment and found a plateau. That is not a reason to skip E3 — it is a reason to change what E3 decides. E3 no longer decides whether CAMIR exists. It decides whether the classifier route ships at all, against the baseline that actually matters: **calibrated confidence** [S8], not a fixed model. Beating a fixed model is table stakes; beating a simple confidence threshold is the only result that would justify a trained classifier's existence and maintenance cost.

**Threshold:** the classifier route must capture ≥50% of the oracle gap **and** beat calibrated-confidence cascade by ≥2pp at matched cost. Note that 2pp sits at the edge of what [S4]'s best remedies achieved (2.13pp), so a positive result would be publishable and a negative one is the expected outcome. On failure the classifier route is permanently demoted to an ablation and cascade plus calibrated confidence is what ships — which is already locked decision 3, so nothing downstream moves.

---

## What is *not* on this board, and why

- **"Will people use it"** — an outcome, not an assumption. Free self-hosted routing already has users [S11][S22]; usage is not in doubt, monetisation is.
- **Router accuracy as a competitive claim** — killed by [S4], removed from positioning, no longer load-bearing anywhere. See [pivot_log.md](pivot_log.md) §Already killed.
- **Team, hiring and fundraising assumptions** — real, but they decide whether *this team* attempts the venture, not whether the venture is worth attempting.

---

## Recommended next 3

1. **Start E1 and E4 in the same week.** They share no resources — one is GPU time, the other is founder calendar time — and between them they decide ranks 1 and 2, which is the whole venture. Neither needs a customer, a product, or a line of shipped code.
2. **Open the channel conversation (rank 6) in that same week**, because it is the only row whose clock is controlled by someone else and whose lead time runs to months. [../strategy/business_model_canvas.md](../strategy/business_model_canvas.md) already puts it first in the kill order.
3. **Draft the E1 null-result publication before running E1.** Writing the paper that says "the ceiling is too low" in advance is the cheapest guard available against motivated measurement, and [../strategy/gtm.md](../strategy/gtm.md) already commits to publishing it. A team holding a pre-written negative result cannot quietly re-run the benchmark until it passes.
