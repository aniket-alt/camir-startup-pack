# CAMIR — Coverage audit

**What this is** — the row-by-row completeness audit for the CAMIR founder pack against `references/artifact-manifest.md`.
**Why it exists** — a directory can contain plausible files while hiding a missing contract, broken link or unrendered visual; this audit makes the required pack count and the optional website/raster boundary explicit.
**How to read it** — required rows are checked first, then optional rows and quality gates; a skeptic should inspect `fix` and `pending-image` statuses rather than treating 61/61 as proof of empirical validation.
**Depends on / feeds** — depends on [../](../), [../../../references/artifact-manifest.md](../../../references/artifact-manifest.md) and [../../../references/quality-bar.md](../../../references/quality-bar.md); feeds [../README.md](../README.md) and the next run checkpoint.

## Required coverage

The required pack is complete: **61/61 required artifact units present**. Architecture files D01–D10 are counted individually under A24; the visual HTML set is counted under A50; `docimages.json` is present under A52b. All rows were checked against the filesystem on 2026-09-10.

| ID | Path | Status | Check |
|---|---|---|---|
| A00 | BRIEF.md | present | orientation and source brief |
| A01 | ASSUMPTIONS.md | present | assumption ledger |
| A02 | research/landscape.md | present | research layer |
| A03 | research/competitors.md | present | teardown |
| A04 | research/capability_table.md | present | capability table |
| A05 | research/survey.md | present | survey |
| A06 | research/sources.md | present | 40-source register |
| A07 | strategy/market_type.md | present | strategy |
| A08 | strategy/positioning.md | present | strategy |
| A09 | strategy/market_sizing.md | present | TAM/SAM/SOM |
| A10 | strategy/personas.md | present | P1–P6 |
| A11 | strategy/lean_canvas.md | present | canvas |
| A12 | strategy/value_prop_canvas.md | present | canvas |
| A13 | strategy/gtm.md | present | GTM |
| A14 | product/PRD.md | present | critic pass complete |
| A15 | product/features_flagship.md | present | critic pass complete |
| A16 | product/features_prioritized.md | present | critic pass complete |
| A17 | product/journeys/edge_low.md | present | journey |
| A18 | product/journeys/beachhead.md | present | journey |
| A19 | product/journeys/edge_high.md | present | journey |
| A20 | product/journeys/day_in_life.md | present | journey |
| A21 | product/ux_spec.md | present | UX spec |
| A22 | tech/whitepaper.md | present | critic pass complete |
| A23 | tech/deep_dives.md | present | technical layer |
| A24 | tech/architecture/00_INDEX.md, D01–D10.md | present | 11 files; Mermaid set pre-verified |
| A25 | tech/techniques/wave1.md | present | critic pass complete |
| A26 | tech/techniques/wave2.md | present | critic marker retained |
| A28 | tech/techniques/decision_tree.md | present | Mermaid pre-verified |
| A29 | tech/techniques/technique_feature_matrix.md | present | matrix |
| A30 | tech/not_vaporware.md | present | stack and risk |
| A31 | narrative/one_pager.md | present | Task B |
| A32 | narrative/vc_memo.md | present | Task B |
| A33 | narrative/pitch_deck.md | present | 14 claim-led slides |
| A34 | narrative/future_press.md | present | future-only framing |
| A35 | narrative/founder_story.md | present | capstone origin stated |
| A36 | validation/riskiest_assumptions.md | present | critic pass complete |
| A37 | validation/experiment_board.md | present | critic pass complete |
| A38 | validation/discovery_guide.md | present | validation |
| A39 | validation/get_keep_grow.md | present | validation |
| A40 | validation/stage_gate.md | present | validation |
| A41 | validation/metrics_by_stage.md | present | validation |
| A42 | validation/pivot_log.md | present | validation |
| A43 | financials/pricing.md | present | critic pass complete |
| A44 | financials/revenue_build.md | present | critic pass complete |
| A45 | financials/unit_economics.md | present | unit economics |
| A46 | financials/use_of_funds.md | present | funds |
| A47 | financials/risk_matrix.md | present | risks |
| A48 | financials/comps_exits.md | present | comps |
| A49 | visuals/visual_manifest.md | present | 22 rows; declared palette |
| A50 | visuals/infographics/*.html | present | 9 HTML infographics; exact-text rows |
| A51 | visuals/image_prompts.md | present | P01–P22 |
| A52b | visuals/docimages.json | present | builder output; 0 PNG placements |
| A54 | audit/COVERAGE.md | present | this file |
| A55 | README.md | present | refreshed from glob |
| A58 | strategy/business_model_canvas.md | present | strategy |
| A59 | strategy/petal_diagram.md | present | strategy |
| A60 | strategy/channel_plan.md | present | strategy |
| A61 | strategy/sales_roadmap.md | present | strategy |
| A62 | validation/mvp_definition.md | present | validation |
| A63 | validation/decision_making_unit.md | present | validation |
| A64 | narrative/mission_vision.md | present | Task B |

## Optional rows

| ID | Path | Status | Reason |
|---|---|---|---|
| A27 | tech/techniques/wave3.md | present, optional | retained as existing artifact |
| A52 | visuals/images/*.png | pending-image | no text-to-image tool is available; no row is marked rendered |
| A53 | ingest/SOURCE_<n>.md | not started | no ingested-source requirement in this run |
| A56 | index.html | not started — awaiting founder consent | website is optional and would expose repository contents |
| A57 | GitHub Pages URL | not started — awaiting founder consent | Pages deliberately not enabled; `gh` unavailable |

## Quality gates

- **Property 0:** all newly written narrative, visual and audit artifacts open with the four labelled orientation lines. Existing upstream artifacts were already generated under the same pack contract.
- **Numbers:** Task A removed the highest-risk unsupported claims; narrative and visuals use source tags or explicit assumptions. No CAMIR traction or benchmark result is presented as observed.
- **Mermaid:** the 11 architecture diagrams and technique decision tree were pre-verified in the handoff; Task C cites those sources rather than duplicating them.
- **HTML:** 9 self-contained files were opened as text and checked for inline CSS, source lines and bound labels. They carry exact content where a raster would be unreliable.
- **Links:** README links are limited to existing paths and are checked after this write with a filesystem resolver.

## Gaps and residual risk

1. **Pending raster images:** A52 remains optional and honestly pending because no image tool is available. HTML and live Mermaid cover the required textual and diagrammatic content.
2. **Unvalidated venture claims:** oracle ceiling, customer segment, price, channel acceptance, Product 2 demand and switching cost remain planned assumptions. The pack is complete as an artifact set, not empirically validated.
3. **Website:** A56/A57 remain intentionally absent. GitHub Pages was not enabled and must stay disabled without founder consent.

## Recommended next 3

1. Run the relative-link and source-tag checks from the repository root after any future artifact edit.
2. Execute E1/E2 before promoting any scenario multiple or ACV into evidence.
3. Keep A52 pending and Pages disabled unless the founder changes the explicit consent decision.
