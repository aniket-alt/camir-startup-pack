# CAMIR — Cost-Aware Multi-Model Inference Router

CAMIR routes each LLM request to the smallest model tier predicted to answer it correctly, cutting inference spend without crossing a declared quality tolerance — on a self-hosted model pool a team can actually run. Two routing strategies are evaluated against each other rather than assumed: a **classifier route** that predicts difficulty before generation, and a **cascade route** that tries the small tier first and escalates on low confidence. The deliverable is not a point claim but a published, reproducible **cost-quality frontier**, against which a team sets its own quality tolerance. Origin: SJSU CMPE 295A master's capstone, September 2026. No revenue, no pilot customer, no traction, no completed benchmark run.

**Status:** `COMPLETE` · generated 2026-09-10 · run slug `camir` · **61/61 required artifacts · 9 HTML infographics · 0 PNGs rendered**

> This is a pre-traction SJSU CMPE 295A capstone pack. **[HANDOFF.md](HANDOFF.md) carries the locked decisions** that constrain every artifact; [audit/COVERAGE.md](audit/COVERAGE.md) records row-by-row coverage.

## Start here

The 60-second path moves from claim to mechanism to evidence:

1. **[narrative/one_pager.md](narrative/one_pager.md)** — the claim, mechanism and commercial boundary in one page.
2. **[narrative/pitch_deck.md](narrative/pitch_deck.md)** — the investor arc, with every slide title as a claim.
3. **[tech/whitepaper.md](tech/whitepaper.md)** — the cost arithmetic and the evidence against borrowed routing headlines.

## Reading paths by audience

**Investor** → [narrative/vc_memo.md](narrative/vc_memo.md) (teardown and risks) → [strategy/market_sizing.md](strategy/market_sizing.md) (routing ceiling) → [financials/pricing.md](financials/pricing.md) (price hypothesis) → [financials/revenue_build.md](financials/revenue_build.md) (conditional expansion).

**Engineer** → [product/PRD.md](product/PRD.md) (the Classify → Dispatch → Judge → Attribute → Recalibrate loop) → [tech/whitepaper.md](tech/whitepaper.md) → [tech/architecture/00_INDEX.md](tech/architecture/00_INDEX.md) → [tech/techniques/wave1.md](tech/techniques/wave1.md).

**Operator** → [product/journeys/day_in_life.md](product/journeys/day_in_life.md) (Marcus, Ravi and Dana in one day) → [strategy/personas.md](strategy/personas.md) → [strategy/channel_plan.md](strategy/channel_plan.md) → [strategy/sales_roadmap.md](strategy/sales_roadmap.md).

**Skeptic** → [validation/riskiest_assumptions.md](validation/riskiest_assumptions.md) → [validation/experiment_board.md](validation/experiment_board.md) → [research/sources.md](research/sources.md) §Gaps → [audit/COVERAGE.md](audit/COVERAGE.md).

## Full artifact map

| Path | What it holds | Files | Owning skill |
|---|---|---|---|
| `./` | Brief, assumptions, work order, handoff and this front door | 5 | grill-me / startup-audit |
| [`research/`](research/) | Landscape, competitor teardown, capability table, survey, 40 sources | 5 | startup-research |
| [`strategy/`](strategy/) | Market type, positioning, sizing, personas, both canvases, GTM, petal, channel economics, sales roadmap | 11 | startup-strategy |
| [`product/`](product/) | PRD, features, journeys and UX spec | 8 | startup-product |
| [`tech/`](tech/) | Whitepaper, deep dives, architecture and techniques | 19 | startup-tech |
| [`validation/`](validation/) | Assumptions, experiments, discovery, metrics and gates | 9 | startup-validation |
| [`financials/`](financials/) | Pricing, revenue, unit economics, funds, risks and comps | 6 | startup-financials |
| [`narrative/`](narrative/) | One-pager, memo, deck, future press, founder story, mission | 6 | startup-narrative |
| [`visuals/`](visuals/) | Manifest, prompts, builders, indexes and 9 HTML infographics | 16 | startup-visuals |
| [`audit/`](audit/) | Coverage audit | 1 | startup-audit |

## Visual index

HTML infographics: [V04 artifact ceiling](visuals/infographics/V04_artifact-controlled-oracle.html), [V05 qualification](visuals/infographics/V05_qualify-measure-disqualify.html), [V07 Ravi control](visuals/infographics/V07_ravi-control-path.html), [V09 ACV](visuals/infographics/V09_acv-open-core.html), [V10 Product 1/Product 2](visuals/infographics/V10_product1-product2.html), [V14 validation gates](visuals/infographics/V14_three-gates.html), [V15 feature priority](visuals/infographics/V15_feature-priority.html), [V17 technique matrix](visuals/infographics/V17_technique-feature-matrix.html), [V18 experiment gates](visuals/infographics/V18_experiment-gates.html). Mermaid visuals remain live in their source artifacts. No PNGs are embedded because no text-to-image tool is available; A52 remains pending-image.

## Top 5 sharpest claims

1. **CAMIR's riskiest assumption has already been tested by the field, and answered weakly.** 21 routing methods across 5 benchmarks converge into a narrow band far below the oracle router, from a predictability bottleneck; the best remedies bought ~2.13 percentage points [S4]. The pack therefore does not claim router superiority. — `research/landscape.md` §4.2
2. **A large part of the apparent ceiling is instrumentation, not capability** — judge verbosity bias, truncation under fixed generation budgets in 65% of MMLU and 57% of MedQA cases, 5–12% parse failures [S5]. That relocates the open problem from algorithm to measurement, which is where this team's professional background sits. — `research/survey.md` §6.3
3. **Model vendors will not build this, and it is incentive alignment rather than slowness** — routing traffic down-tier reduces revenue per request, and vendor-native routing already ships free with a tolerance the customer cannot set [S20][S21]. — `BRIEF.md` §Why now
4. **Routing fees alone are not a venture-scale business at 2026 denominators.** Bottom-up from a measured anchor — OpenRouter's $160M ARR at a ~5% take rate [S17] — the base-case routing-fee TAM is ~$70M/yr. Expansion to the control plane is the venture case, not upside. — `strategy/market_sizing.md`
5. **The moat is weak and the pack says so.** The routing data loop is per-deployment rather than network-wide, and switching cost is a base-URL change. The nearest business-model analogue, TensorZero, archived its repository on 12 June 2026 after $7.3M and 11,000 stars [S23]. — `BRIEF.md` §Mechanism & moat

## Completeness

`COMPLETE` for the 61 required artifact units. The pack is still pre-traction: oracle ceiling, customer segment, price, channel acceptance and Product 2 demand remain planned assumptions, not observed results. Nine HTML infographics and live Mermaid sources cover the textual visual layer; PNG rows are honestly pending because no image tool is available. Website rows A56/A57 remain optional and intentionally not started because GitHub Pages would expose repository contents and founder consent was not given. See [audit/COVERAGE.md](audit/COVERAGE.md) for row-by-row status.

The binding locked decisions remain in **[HANDOFF.md](HANDOFF.md)**. The website is deliberately disabled.
