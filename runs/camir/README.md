# CAMIR — Cost-Aware Multi-Model Inference Router

CAMIR routes each LLM request to the smallest model tier predicted to answer it correctly, cutting inference spend without crossing a declared quality tolerance — on a self-hosted model pool a team can actually run. Two routing strategies are evaluated against each other rather than assumed: a **classifier route** that predicts difficulty before generation, and a **cascade route** that tries the small tier first and escalates on low confidence. The deliverable is not a point claim but a published, reproducible **cost-quality frontier**. Origin: SJSU CMPE 295A master's capstone, September 2026. No revenue, no pilot customer, no traction.

**Status:** `PARTIAL` · generated 2026-09-09 · run slug `camir` · **2/61 required artifacts · 0 visuals rendered**

> This is a live run. The front door exists from phase 0 so an interrupted run is never mistaken for a failed one. Row-by-row status lands in [audit/COVERAGE.md](audit/COVERAGE.md) at phase 9.

## Start here

The 60-second path does not exist yet — the narrative layer is generated in phase 5. Until then:

1. [BRIEF.md](BRIEF.md) — the founder brief: problem, users, mechanism, moat, business model, riskiest assumption, and the vocabulary every other artifact must use.
2. [ASSUMPTIONS.md](ASSUMPTIONS.md) — the fourteen assumptions holding the pack up, with the three that kill it if wrong marked as such.

## Reading paths by audience

Populated as layers land. Today, every audience reads the same two files above; the skeptic's entry point is `ASSUMPTIONS.md` §Tier 1.

## Full artifact map

| Path | What it holds | Files | Owning skill |
|---|---|---|---|
| `./` | Brief, assumptions, this front door | 3 | grill-me / startup-audit |
| `research/` | Landscape, competitor teardown, capability table, survey, sources | 0 | startup-research |
| `strategy/` | Market type, positioning, sizing, personas, canvases, GTM, channels, sales roadmap | 0 | startup-strategy |
| `product/` | PRD, feature sets, four journeys, UX spec | 0 | startup-product |
| `tech/` | Whitepaper, deep dives, ten architecture diagrams, technique waves, not-vaporware | 0 | startup-tech |
| `narrative/` | One-pager, VC memo, pitch deck, future press, founder story, mission | 0 | startup-narrative |
| `validation/` | Riskiest assumptions, experiment board, discovery guide, MVP definition, DMU | 0 | startup-validation |
| `financials/` | Pricing, revenue build, unit economics, use of funds, risk matrix, comps | 0 | startup-financials |
| `visuals/` | Visual manifest, HTML infographics, image prompts, rendered images | 0 | startup-visuals |
| `audit/` | Coverage table and gap draw order | 0 | startup-audit |

## Visual index

Empty — phase 8 has not run.

## Top 5 sharpest claims

1. **Model vendors will not build this, and it is incentive alignment rather than slowness** — a system whose explicit purpose is to use their expensive model less reduces their revenue per request. `BRIEF.md` §Why now.
2. **The idea is already proven; the reproducible version is not** — FrugalGPT and RouteLLM both work, and both evaluate on a hosted vendor catalog no self-hosting team can reproduce or deploy against. `(to be sourced in research/sources.md)`
3. **The moat is weak and the brief says so** — the routing data loop is per-deployment, not network-wide, and switching cost is a base-URL change. `BRIEF.md` §Mechanism & moat.
4. **The hard part is the evaluation, not the router** — building a mixed-difficulty benchmark, judging correctness consistently, and producing a frontier that survives scrutiny is benchmark engineering, which is the one thing this team has professional history in. `BRIEF.md` §Founder edge.
5. **The whole idea has a measurable ceiling, and the first experiment measures it** — the oracle ceiling bounds what any router could achieve; if it is low, no router helps and the project reports a null result. `ASSUMPTIONS.md` A2.

## Completeness

`PARTIAL`. Phase 0 (brief) is complete; research, strategy, product, tech, narrative, validation, financials, visuals and audit have not yet run. Row-by-row status will be tracked in [audit/COVERAGE.md](audit/COVERAGE.md) once phase 9 runs. This file is refreshed at every phase checkpoint and finalised by `startup-audit`.
