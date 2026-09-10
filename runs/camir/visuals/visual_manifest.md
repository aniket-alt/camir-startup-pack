# CAMIR — Visual manifest

**What this is** — the ranked map of every visual in the pack: 45 rows across four audiences, each with its source artifacts, its form, and a status reconciled to the files on disk.
**Why it exists** — CAMIR's claims live in tables and derivations, which are exactly what image models garble. A manifest that did not record form and source per row would let a visual invent a number, duplicate a Mermaid diagram as a poster, or leave a third of the pack's documents with no illustration at all — which is the state this file was in before its 2026-09-10 rebuild.
**How to read it** — find your audience section; every row names the documents it illustrates in its Source column, which is what `build_docimages.js` reads. A skeptic should check the Status column against `visuals/images/` and should note that **no row was produced by a diffusion model** — see §How the images were made.
**Depends on / feeds** — depends on [../narrative/pitch_deck.md](../narrative/pitch_deck.md), [../product/](../product/), [../tech/](../tech/), [../strategy/](../strategy/), [../validation/](../validation/) and [../financials/](../financials/); feeds [image_prompts.md](image_prompts.md), [docimages.json](docimages.json), [../audit/COVERAGE.md](../audit/COVERAGE.md) and [../README.md](../README.md).

## Visual language

Palette: ink `#14213D`, signal teal `#007C83` for sourced or measured values, warm amber `#F2A900` for assumptions and illustrative figures, coral `#D95D39` for vetoes, kill conditions and disqualification, mist `#F3F6F7` panels on white. Type: Georgia 31px headlines, Segoe UI 13–17px body. Canvas 1600×900 CSS px, rendered at 1.5× (2400×1350); Mermaid renders keep the diagram's own aspect ratio, so tall diagrams produce tall images.

## Targets and classification

**45 visuals: 34 HTML infographics and 11 architecture or technique diagrams rendered from their Mermaid source.** Every row has a PNG in `visuals/images/`.

- **HTML** where the words and numbers are the content — tables, derivations, journeys, thresholds — per the A50 contract. Each file is self-contained with inline CSS and a fit-to-canvas script.
- **Mermaid** where the source artifact already ships a diagram. No HTML poster is made for these (A50); the PNG is the diagram itself, rendered by the Mermaid library.
- **Coverage rows (V32–V45)** exist so that no substantive document over ~400 words is left unillustrated (A52b).

## How the images were made — read before citing any of them

**No text-to-image model was used.** Every PNG is a headless-browser screenshot of either the row's HTML infographic or its Mermaid source, so the text in each image is the text in the file: no garbling, no invented labels. `image_prompts.md` remains a separate set of diffusion prompts for stylised versions; **none of those has been rendered**, and the A52 rows remain `pending-image` in that sense. Statuses below were written from `ls visuals/images/`, not from memory.


## Investors

The deck set and the investment case.

| ID | Title | Audience | Source artifact | Form | Status |
|---|---|---|---|---|---|
| V01 | Every request pays for the hardest request's capacity | Investors | narrative/pitch_deck.md, BRIEF.md | html | rendered — HTML + PNG snapshot |
| V02 | Borrowed routing headlines do not transfer to a self-hosted pool | Investors | narrative/pitch_deck.md, research/landscape.md | html | rendered — HTML + PNG snapshot |
| V03 | Routing accuracy has plateaued — so measurement is the wedge | Investors | narrative/pitch_deck.md, research/survey.md | html | rendered — HTML + PNG snapshot |
| V04 | A meaningful part of the apparent ceiling is the harness | Investors | narrative/pitch_deck.md, tech/whitepaper.md, research/capability_table.md | html | rendered — HTML + PNG snapshot |
| V05 | The first output can be "do not deploy" | Investors | narrative/pitch_deck.md, product/journeys/beachhead.md, validation/mvp_definition.md | html | rendered — HTML + PNG snapshot |
| V09 | $30,000 ACV, priced out of the inference bill — as a share of a saving the customer computes | Investors | narrative/pitch_deck.md, financials/pricing.md, narrative/one_pager.md | html | rendered — HTML + PNG snapshot |
| V10 | Routing fees alone are not venture-scale — the control plane is the option | Investors | narrative/pitch_deck.md, financials/revenue_build.md, strategy/market_sizing.md | html | rendered — HTML + PNG snapshot |
| V11 | One channel's economics survive — and it depends on a maintainer nobody has asked | Investors | narrative/pitch_deck.md, strategy/channel_plan.md, strategy/gtm.md | html | rendered — HTML + PNG snapshot |
| V12 | Every competitor is paid by the volume it routes — so none can tell a customer not to buy | Investors | narrative/pitch_deck.md, financials/comps_exits.md, strategy/market_type.md | html | rendered — HTML + PNG snapshot |
| V13 | The evidence ladder starts with an experiment, not traction | Investors | narrative/pitch_deck.md, validation/stage_gate.md, narrative/founder_story.md | html | rendered — HTML + PNG snapshot |
| V14 | $2.2M in three gated blocks — 87% unspent if the ceiling is not there | Investors | narrative/pitch_deck.md, financials/use_of_funds.md, validation/pivot_log.md | html | rendered — HTML + PNG snapshot |
| V32 | The market divides on who owns the weights and who sets the tolerance — not on cost | Investors | strategy/positioning.md, research/competitors.md | html | rendered — HTML + PNG snapshot |
| V33 | Every number traces to one of 40 sources — graded, and two of them are weak | Investors | research/sources.md | html | rendered — HTML + PNG snapshot |
| V34 | Two canvases, and where each is weakest | Investors | strategy/lean_canvas.md, strategy/business_model_canvas.md | html | rendered — HTML + PNG snapshot |
| V36 | Only one of five adjacent markets holds real, approved budget | Investors | strategy/petal_diagram.md | html | rendered — HTML + PNG snapshot |
| V41 | At every stage, the flattering number is the wrong one | Investors | validation/metrics_by_stage.md | html | rendered — HTML + PNG snapshot |
| V42 | Year-one margin is 42%, not 75% — and the bend is onboarding automation, not scale | Investors | financials/unit_economics.md | html | rendered — HTML + PNG snapshot |
| V43 | The 2031 press release is only true if five milestones and four commitments all hold | Investors | narrative/future_press.md | html | rendered — HTML + PNG snapshot |
| V44 | Five values, each a trade-off the company will actually make | Investors | narrative/mission_vision.md | html | rendered — HTML + PNG snapshot |

## Operators

How the system is built and run, including every architecture diagram rendered from its Mermaid source.

| ID | Title | Audience | Source artifact | Form | Status |
|---|---|---|---|---|---|
| V06 | D02 — The closed loop: Classify → Dispatch → Judge → Attribute → Recalibrate | Operators | tech/architecture/D02.md, tech/architecture/00_INDEX.md, narrative/pitch_deck.md | mermaid | rendered — PNG from Mermaid source |
| V07 | The engineer who can veto gets the controls — and answers "was it the router?" in four minutes | Operators | narrative/pitch_deck.md, product/journeys/day_in_life.md, validation/decision_making_unit.md, strategy/personas.md | html | rendered — HTML + PNG snapshot |
| V08 | Base case 1.4–1.5× on GPU cost — and the conservative case is a null result | Operators | narrative/pitch_deck.md, tech/whitepaper.md, narrative/vc_memo.md | html | rendered — HTML + PNG snapshot |
| V15 | Build first the four features that contain no algorithm | Operators | product/features_prioritized.md, product/features_flagship.md | html | rendered — HTML + PNG snapshot |
| V16 | Technique decision tree — five priority bands | Operators | tech/techniques/decision_tree.md | mermaid | rendered — PNG from Mermaid source |
| V17 | The features that decide adoption rest on no technique at all | Operators | tech/techniques/technique_feature_matrix.md, tech/techniques/wave1.md, tech/techniques/wave2.md, tech/techniques/wave3.md | html | rendered — HTML + PNG snapshot |
| V18 | Every experiment's pass/fail line is written before it runs | Operators | validation/experiment_board.md, validation/riskiest_assumptions.md, ASSUMPTIONS.md | html | rendered — HTML + PNG snapshot |
| V23 | D01 — The qualify pipeline: logs to first frontier | Operators | tech/architecture/D01.md | mermaid | rendered — PNG from Mermaid source |
| V24 | D03 — Request path orchestration | Operators | tech/architecture/D03.md | mermaid | rendered — PNG from Mermaid source |
| V25 | D04 — Durable records schema | Operators | tech/architecture/D04.md | mermaid | rendered — PNG from Mermaid source |
| V26 | D05 — Model pool, tier registry and cost metering | Operators | tech/architecture/D05.md | mermaid | rendered — PNG from Mermaid source |
| V28 | D07 — Integrations and ecosystem | Operators | tech/architecture/D07.md | mermaid | rendered — PNG from Mermaid source |
| V29 | D08 — Observability, drift and the breach path | Operators | tech/architecture/D08.md | mermaid | rendered — PNG from Mermaid source |
| V30 | D09 — Multi-endpoint scale and isolation | Operators | tech/architecture/D09.md | mermaid | rendered — PNG from Mermaid source |
| V31 | D10 — Human-in-the-loop and escalation | Operators | tech/architecture/D10.md | mermaid | rendered — PNG from Mermaid source |
| V38 | Everything the product needs is buildable this quarter — the research bets are optional | Operators | tech/not_vaporware.md | html | rendered — HTML + PNG snapshot |
| V39 | Discovery asks only about past behaviour, and its thresholds are fixed first | Operators | validation/discovery_guide.md | html | rendered — HTML + PNG snapshot |
| V40 | CAMIR succeeds by being opened twice a month | Operators | validation/get_keep_grow.md | html | rendered — HTML + PNG snapshot |
| V45 | What is locked — decided with the founder or derived from the research — and not to be re-opened | Operators | HANDOFF.md, GEMINI_TASK.md | html | rendered — HTML + PNG snapshot |

## Practitioners

The people who use or configure CAMIR.

| ID | Title | Audience | Source artifact | Form | Status |
|---|---|---|---|---|---|
| V19 | One system across the whole spectrum — only the configuration surface changes | Practitioners | product/journeys/edge_low.md, product/journeys/beachhead.md, product/journeys/edge_high.md, product/journeys/day_in_life.md | html | rendered — HTML + PNG snapshot |
| V20 | One request, five phases, five durable records | Practitioners | product/PRD.md, tech/deep_dives.md | html | rendered — HTML + PNG snapshot |
| V37 | The highest-priority surface is a span attribute in a tool CAMIR does not own | Practitioners | product/ux_spec.md | html | rendered — HTML + PNG snapshot |

## Buyers

The people who sign, gate, or can say no.

| ID | Title | Audience | Source artifact | Form | Status |
|---|---|---|---|---|---|
| V21 | The buyer negotiates the definition, not the price | Buyers | financials/pricing.md, strategy/sales_roadmap.md, validation/decision_making_unit.md | html | rendered — HTML + PNG snapshot |
| V22 | Two risks stay High after mitigation, and they are different in kind | Buyers | financials/risk_matrix.md | html | rendered — HTML + PNG snapshot |
| V27 | D06 — Perimeter, privacy and the open/paid boundary | Buyers | tech/architecture/D06.md | mermaid | rendered — PNG from Mermaid source |
| V35 | Three deciding personas, three different top fits — and none is a cheaper bill | Buyers | strategy/value_prop_canvas.md | html | rendered — HTML + PNG snapshot |

## Recommended next 3

1. **Re-run both builders after any edit to a source document's numbers.** Visuals are generated from the same figures as the documents; a corrected number in `financials/` must be regenerated here, as the 2026-09-10 CAC correction was.
2. **Render `image_prompts.md` only if a text-to-image tool becomes available, and never replace an HTML row with a raster.** The HTML carries exact figures; a diffusion render is decoration on top of it, and must pass both verification stages before its row changes.
3. **Keep the Mermaid renders tied to their sources.** Each is regenerated from the diagram in its architecture file, so fixing a diagram and re-rendering keeps both in step — editing a PNG by hand breaks that.
