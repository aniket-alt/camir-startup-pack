# CAMIR — Image prompts

**What this is** — production-ready schematic prompts corresponding one-to-one with the 45-row visual manifest, for stylised raster versions if a text-to-image tool becomes available. **None has been rendered**; the PNGs in `images/` are browser snapshots of the HTML and Mermaid sources, not renders of these prompts.
**Why it exists** — image generation can garble exact numbers and labels; these prompts constrain each raster to structure while the HTML and source artifacts remain the exact record.
**How to read it** — each prompt names its visual ID, labels to render and source; never mark a raster rendered until the file exists and has been opened.
**Depends on / feeds** — depends on [visual_manifest.md](visual_manifest.md), [narrative/pitch_deck.md](../narrative/pitch_deck.md) and source artifacts throughout [../](../); feeds optional `images/*.png` and [docimages.json](docimages.json).

## Shared style block

Extremely information-dense professional infographic, white background, elite systems-architect quality, clean modern typography, one bold headline, 3–5 labeled information zones, palette ink #14213D, signal teal #007C83, warm amber #F2A900, coral #D95D39, mist #F3F6F7, print-grade, no watermark, no lorem ipsum, render ONLY the quoted labels.

## Prompts

### P01 — V01
TITLE: Mixed-difficulty requests cross a fixed-model baseline. Draw exactly three request cards labeled “easy”, “mixed”, “hard” flowing into one fixed-model block, with a cost meter beneath. Use no extra text. Source: narrative/pitch_deck.md slide 1.

### P02 — V02
TITLE: Hosted claims versus self-hosted cost axis. Draw exactly two vertical bars labeled “hosted $0.60/M” and “raw H100 $0.10/M”, plus one warning marker labeled “idle 10x”. Bind labels inside bars or markers. Source: [S26][S27].

### P03 — V03
TITLE: Plateau evidence and the measurable-frontier thesis. Draw exactly 21 small method dots converging into one narrow band, an oracle ceiling line above, and labels “21 methods”, “narrow band”, “oracle”, “measure the harness”. Source: [S4][S5].

### P04 — V04
TITLE: Artifact-controlled oracle ceiling decomposition. Draw exactly four labeled blocks: “truncation”, “parse failure”, “judge bias”, “held-out labels”, leading to “frontier run”. Do not render statistics; the HTML carries them. Source: [S5].

### P05 — V05
TITLE: Qualify, measure, disqualify. Draw exactly three connected panels labeled “qualify”, “measure”, “do not deploy”. Source: product/PRD.md.

### P06 — V06
TITLE: CAMIR core loop. Draw exactly five nodes in a closed circuit labeled “Classify”, “Dispatch”, “Judge”, “Attribute”, “Recalibrate”. Source: product/PRD.md; reader should use Mermaid source.

### P07 — V07
TITLE: Ravi control path. Draw exactly four nodes labeled “shadow mode”, “trace”, “pin large”, “record”, with coral on “pin large”. Source: product/features_flagship.md.

### P08 — V08
TITLE: Self-hosted cost-quality frontier. Draw axes labeled “cost” and “quality”, exactly three model points and one curved route labeled “frontier”. Source: tech/whitepaper.md; reader should use Mermaid/source artifact.

### P09 — V09
TITLE: ACV derivation and open-core boundary. Draw exactly four connected blocks labeled “$600k spend”, “70% cache-miss”, “25% scenario”, “28% hypothesis”, ending in “$30k ACV”; draw a separate open/paid split labeled “open harness” and “paid control plane”. Source: financials/pricing.md.

### P10 — V10
TITLE: Product 1 ceiling and conditional Product 2 expansion. Draw two containers labeled “routing control plane” and “inference-efficiency hypothesis”, with a dotted conditional arrow. Do not render TAM figures. Source: financials/revenue_build.md.

### P11 — V11
TITLE: Channel dependency and fallback path. Draw exactly three blocks labeled “LiteLLM proposal”, “maintainer decision”, “fallback channel”, with a clock marker. Source: strategy/channel_plan.md.

### P12 — V12
TITLE: Commodity mechanism versus customer-owned records. Draw a split scene labeled “serving engine” on the left and “frontier run / tolerance / savings ledger” on the right, with an ownership boundary. Source: tech/whitepaper.md.

### P13 — V13
TITLE: Evidence ladder from capstone to customer proof. Draw exactly four rungs labeled “capstone”, “artifact-controlled run”, “shadow mode”, “buyer proof”. Source: narrative/founder_story.md.

### P14 — V14
TITLE: Three-gate validation sequence. Draw exactly three gates labeled “ceiling”, “segment”, “price”, with a stop symbol after each failed gate. Source: validation/experiment_board.md.

### P15 — V15
TITLE: Feature priority and dependency boundary. Draw exactly three horizontal lanes labeled “qualify”, “control”, “route”, with “classifier ablation” outside the first lane. Source: product/features_prioritized.md.

### P16 — V16
TITLE: Technique decision tree. Draw exactly three decision nodes labeled “structured task?”, “logprobs?”, “judge available?”, and leaf labels “verification”, “confidence”, “cascade”. Source: tech/techniques/decision_tree.md; reader should use Mermaid source.

### P17 — V17
TITLE: Technique feature matrix. Draw a small heat-table with exactly five row labels “cascade”, “confidence”, “preference router”, “grammar”, “cache” and three column labels “baseline”, “primary”, “boundary”. Source: tech/techniques/technique_feature_matrix.md.

### P18 — V18
TITLE: Experiment board gates. Draw exactly four cards labeled “ceiling”, “segment”, “install → price”, “safety → margin”. Source: validation/experiment_board.md.

### P19 — V19
TITLE: Edge-low to beachhead to edge-high journey spectrum. Draw exactly three persona lanes labeled “Priya”, “Marcus”, “Wen”, connected by one shared router line. Source: product/journeys/day_in_life.md and strategy/personas.md.

### P20 — V20
TITLE: One request, five records. Draw one request token flowing to exactly five record cards labeled “decision_record”, “judgment_record”, “savings_ledger”, “tolerance_policy”, “frontier_run”. Source: product/PRD.md.

### P21 — V21
TITLE: Buyer decision rights and eligible-savings definition. Draw exactly four roles labeled “Marcus validates”, “Ravi vetoes”, “Dana signs”, “finance checks”, surrounding one document labeled “eligible savings”. Source: financials/pricing.md.

### P22 — V22
TITLE: Two risks stay High. Draw exactly TWO tall coral blocks labeled “R1 ceiling” and “R2 engine” and SIX short grey blocks labeled “R3” “R4” “R5” “R6” “R7” “R8”, in one row, left to right. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P23–P31 — V23–V31 (and V06, V16)
No diffusion prompt. These rows are rendered deterministically from the Mermaid source in their architecture or technique file; a text-to-image render could only degrade a diagram whose exact labels are the content (A50). The PNG in `images/` is the diagram itself.

### P32 — V32
TITLE: The market divides on two axes. Draw exactly FOUR quadrant tiles in a 2×2 grid; the top-right tile highlighted teal and labeled “CAMIR”; the other three labeled “vendor-set”, “hosted”, “absent”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P33 — V33
TITLE: 40 sources, graded. Draw exactly THREE horizontal bars of lengths 21, 17 and 2 units, labeled inside “21 primary”, “17 secondary”, “2 weak”; the shortest coral. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P34 — V34
TITLE: Two canvases. Draw exactly TWO grids side by side, one of 9 cells and one of 9 cells, with exactly two cells of the first marked coral and labeled “moat” and “revenue”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P35 — V35
TITLE: Three top fits. Draw exactly THREE cards labeled “attribution”, “tolerance”, “signed report”, and one struck-through card labeled “cheaper bill”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P36 — V36
TITLE: Five petals. Draw exactly FIVE petals around a centre labeled “CAMIR”; one teal petal labeled “inference bill”, one coral petal labeled “own engineers”, three grey petals unlabeled. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P37 — V37
TITLE: Surfaces by consequence. Draw exactly FOUR stacked bars, longest at top, labeled inside “S9 trace”, “S1 qualify”, “S2 frontier”, “S3 disqualify”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P38 — V38
TITLE: Buildable vs research. Draw exactly TWO columns: a tall teal column labeled “buildable now” and a short amber column labeled “research bets”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P39 — V39
TITLE: Discovery kit. Draw exactly FIVE numbered steps left to right labeled “screen”, “world”, “blame”, “money”, “solution”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P40 — V40
TITLE: Get, keep, grow. Draw exactly THREE funnel sections top to bottom labeled “get”, “keep”, “grow”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P41 — V41
TITLE: Metric vs vanity. Draw exactly FOUR rows, each a teal tile and a crossed-out grey tile; teal tiles labeled “ceiling”, “enforcement”, “M9”, “NRR”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P42 — V42
TITLE: Unit economics. Draw exactly FOUR rising bars labeled inside “42%”, “63%”, “75%”, “81%”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P43 — V43
TITLE: Road to 2031. Draw exactly FIVE milestones on a timeline labeled “2026”, “2027”, “2028”, “2030”, “2031”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P44 — V44
TITLE: Five values. Draw exactly FIVE balance scales in a row, each tilted, with no labels except a single headline “trade-offs”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

### P45 — V45
TITLE: Locked decisions. Draw exactly THREE padlocks labeled “self-hosted first”, “open-core”, “capstone stated”. Render ONLY the labels quoted above, each inside the shape it describes; no other text.

## Recommended next 3

1. Render one prompt at a time and inspect identity, count and label binding before updating status.
2. Keep all exact numeric content in the HTML or source artifact; do not ask a raster to carry dense tables.
3. Leave every unavailable PNG as `pending-image` and report that honestly in the audit.
