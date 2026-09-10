# CAMIR — Cost-Aware Multi-Model Inference Router

CAMIR routes each LLM request to the smallest model tier predicted to answer it correctly, cutting inference spend without crossing a declared quality tolerance — on a self-hosted model pool a team can actually run. Two routing strategies are evaluated against each other rather than assumed: a **classifier route** that predicts difficulty before generation, and a **cascade route** that tries the small tier first and escalates on low confidence. The deliverable is not a point claim but a published, reproducible **cost-quality frontier**, against which a team sets its own quality tolerance. Origin: SJSU CMPE 295A master's capstone, September 2026. No revenue, no pilot customer, no traction, no completed benchmark run.

**Status:** `PARTIAL` · generated 2026-09-09 → 2026-09-10 · run slug `camir` · **29/61 required artifacts · 0 visuals rendered**

> This run is mid-pipeline and was handed between agent sessions. **[HANDOFF.md](HANDOFF.md) carries the resume instructions and the locked decisions** that constrain every remaining artifact. Row-by-row status lands in `audit/COVERAGE.md` when phase 9 runs.

## Start here

The narrative layer (one-pager, deck, VC memo) has not been generated yet, so the 60-second path runs through the source documents:

1. **[BRIEF.md](BRIEF.md)** — problem, users, mechanism, moat, business model, riskiest assumption, and the vocabulary every artifact uses.
2. **[tech/whitepaper.md](tech/whitepaper.md)** — the mechanism arithmetic: where the money is lost today and what removes each friction.
3. **[research/survey.md](research/survey.md)** — the science, including §6.2, the evidence *against* CAMIR's core mechanism, stated at full strength.

## Reading paths by audience

**Investor** → [strategy/market_type.md](strategy/market_type.md) (why this is a re-segmented market, and the post-mortem it must survive) → [strategy/positioning.md](strategy/positioning.md) (the two axes, and why the obvious ones are wrong) → [strategy/market_sizing.md](strategy/market_sizing.md) (the uncomfortable number, stated first) → [financials/pricing.md](financials/pricing.md) → [financials/revenue_build.md](financials/revenue_build.md).

**Engineer** → [tech/whitepaper.md](tech/whitepaper.md) → [product/PRD.md](product/PRD.md) (the Classify → Dispatch → Judge → Attribute → Recalibrate loop and its named components) → [tech/techniques/wave1.md](tech/techniques/wave1.md) → [research/capability_table.md](research/capability_table.md) (what is and is not possible today).

**Operator** → [strategy/personas.md](strategy/personas.md) (six personas; read P5 Ravi first — he can veto and is in no sales conversation) → [strategy/gtm.md](strategy/gtm.md) → [strategy/channel_plan.md](strategy/channel_plan.md) (channel economics that reject three conventional channels) → [strategy/sales_roadmap.md](strategy/sales_roadmap.md).

**Skeptic** → [ASSUMPTIONS.md](ASSUMPTIONS.md) §Tier 1 → [research/survey.md](research/survey.md) §6.2 → [validation/riskiest_assumptions.md](validation/riskiest_assumptions.md) → [research/sources.md](research/sources.md) §Gaps.

## Full artifact map

| Path | What it holds | Files | Owning skill |
|---|---|---|---|
| `./` | Brief, assumptions, handoff, this front door | 4 | grill-me / startup-audit |
| [`research/`](research/) | Landscape, competitor teardown, capability table, survey, 40 sources | 5 | startup-research |
| [`strategy/`](strategy/) | Market type, positioning, sizing, personas, both canvases, GTM, petal, channel economics, sales roadmap | 11 | startup-strategy |
| [`product/`](product/) | PRD, 20 flagship features, 50 prioritised features | 3 | startup-product |
| [`tech/`](tech/) | Whitepaper, three technique waves | 4 | startup-tech |
| [`validation/`](validation/) | Riskiest assumptions, experiment board | 2 | startup-validation |
| [`financials/`](financials/) | Pricing, revenue build | 2 | startup-financials |
| `narrative/` | *not yet generated* | 0 | startup-narrative |
| `visuals/` | *not yet generated* | 0 | startup-visuals |
| `audit/` | *not yet generated* | 0 | startup-audit |

## Visual index

Empty — phase 8 has not run. No text-to-image capability was available in the generating sessions; the HTML infographics that carry this pack's data are still to be built.

## Top 5 sharpest claims

1. **CAMIR's riskiest assumption has already been tested by the field, and answered weakly.** 21 routing methods across 5 benchmarks converge into a narrow band far below the oracle router, from a predictability bottleneck; the best remedies bought ~2.13 percentage points [S4]. The pack therefore does not claim router superiority. — `research/landscape.md` §4.2
2. **A large part of the apparent ceiling is instrumentation, not capability** — judge verbosity bias, truncation under fixed generation budgets in 65% of MMLU and 57% of MedQA cases, 5–12% parse failures [S5]. That relocates the open problem from algorithm to measurement, which is where this team's professional background sits. — `research/survey.md` §6.3
3. **Model vendors will not build this, and it is incentive alignment rather than slowness** — routing traffic down-tier reduces revenue per request, and vendor-native routing already ships free with a tolerance the customer cannot set [S20][S21]. — `BRIEF.md` §Why now
4. **Routing fees alone are not a venture-scale business at 2026 denominators.** Bottom-up from a measured anchor — OpenRouter's $160M ARR at a ~5% take rate [S17] — the base-case routing-fee TAM is ~$70M/yr. Expansion to the control plane is the venture case, not upside. — `strategy/market_sizing.md`
5. **The moat is weak and the pack says so.** The routing data loop is per-deployment rather than network-wide, and switching cost is a base-URL change. The nearest business-model analogue, TensorZero, archived its repository on 12 June 2026 after $7.3M and 11,000 stars [S23]. — `BRIEF.md` §Mechanism & moat

## Completeness

`PARTIAL`. Phases 0–2 are complete (brief, research, strategy). Phases 3, 4, 6 and 7 are partially generated — a parallel agent swarm was interrupted by an API session limit, and the salvaged artifacts are listed in the map above. Phases 5, 8, 9 and 10 have not started. Nine of the phase 3–7 artifacts are unreviewed drafts awaiting a `startup-critic` pass.

Full resume instructions, the binding locked decisions, and the exact remaining draw order are in **[HANDOFF.md](HANDOFF.md)**. Row-by-row manifest status will live in `audit/COVERAGE.md` once phase 9 runs.
