# CAMIR — Image prompts

**What this is** — production-ready schematic prompts corresponding one-to-one with the visual manifest.
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
TITLE: LiteLLM channel dependency and fallback. Draw exactly three paths labeled “upstream proposal”, “accepted”, “re-plan”, with “third-party dependency” as a small warning label. Source: strategy/channel_plan.md.

## Recommended next 3

1. Render one prompt at a time and inspect identity, count and label binding before updating status.
2. Keep all exact numeric content in the HTML or source artifact; do not ask a raster to carry dense tables.
3. Leave every unavailable PNG as `pending-image` and report that honestly in the audit.
