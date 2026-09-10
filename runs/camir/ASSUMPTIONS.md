# CAMIR — Assumptions Register

**What this is** — every choice in this pack that was made without evidence, listed with its basis and whether it kills the pack if wrong.
**Why it exists** — CAMIR has no customers, no revenue and no deployed traffic, so a reader cannot tell by inspection which statements are measured and which are reasoned. Without this register the pack's confident tone would be indistinguishable from confident invention, and the one assumption that actually matters — that prompt difficulty is predictable pre-generation — would sit in a paragraph looking like a fact.
**How to read it** — sort by the `Kills pack?` column and read those rows only. A1 and A2 are the two that decide whether CAMIR is a company or a null result; attack those. Everything below A10 is a framing choice, cheap to change.
**Depends on / feeds** — depends on [BRIEF.md](BRIEF.md); feeds [validation/riskiest_assumptions.md](validation/riskiest_assumptions.md), which turns the kill-rows into dated experiments with pass/fail thresholds declared in advance.

---

## Register

Format: `ID: assumption — basis: why it was chosen — kills-pack-if-wrong: yes/no`

### Tier 1 — kills the pack if wrong

**A1: Prompt difficulty is predictable from the prompt alone, accurately enough that a classifier route beats a fixed-model baseline on the cost-quality frontier.**
— basis: founder's own stated riskiest assumption; FrugalGPT and RouteLLM report positive results, but on hosted catalogs, not self-hosted open-weight pools.
— kills-pack-if-wrong: **yes.** If false, the classifier route collapses to the cascade route alone, and cascade's wasted small-tier generation erodes the savings the whole pitch rests on.

**A2: The oracle ceiling on realistic mixed traffic is high enough to matter — i.e. a meaningful share of real production prompts are answered correctly by a small tier.**
— basis: founder's stated walk-away condition. Untested on this project's own benchmark.
— kills-pack-if-wrong: **yes.** If the difficulty distribution is bimodal such that almost everything needs the large tier, no router — not even a perfect one — saves enough to sell.

**A3: A meaningful population of mid-size teams self-hosts open-weight model pools at production volume today, and will keep doing so.**
— basis: founder's beachhead definition. To be dated and sized in `research/landscape.md` and `strategy/market_sizing.md`.
— kills-pack-if-wrong: **yes.** The self-hosted-first wedge exists only if this population exists. If everyone stays on hosted APIs, CAMIR competes head-on with vendor-native routing on the vendor's home turf.

### Tier 2 — reshapes the business layer, does not kill the technology

**A4: Buyers will pay for routing as a distinct line item rather than treating it as a feature they expect free from their serving stack.**
— basis: reasoning from the size of the inference line item, not from any buyer conversation. No pricing has been tested with anyone.
— kills-pack-if-wrong: no — but it turns the open-core plan into an open-source project with no revenue layer.

**A5: Savings are measurable and attributable well enough to support share-of-savings pricing.**
— basis: founder's stated preference, with per-request pricing named as the fallback. Counterfactual measurement ("what would the fixed-model baseline have cost") is computable but disputable.
— kills-pack-if-wrong: no — fallback to per-request pricing is already declared in `BRIEF.md` §Business model.

**A6: Switching cost rises once quality-tolerance policies and per-customer routing history become expensive to recreate.**
— basis: the founder flagged this as untested and named low switching cost as a genuine weakness. This is the only mechanism by which the moat improves.
— kills-pack-if-wrong: no — but the honest moat then rounds to zero, and the pack's own §Mechanism & moat already says so.

**A7: The control plane is where the data loop lives, so open-sourcing the router does not give away the compounding asset.**
— basis: founder decision this session (open-core + commercial control plane). Reasoned from the placement of the labelled routing data, not observed.
— kills-pack-if-wrong: no — a closed-source router is a live fallback, at the cost of the reproducibility wedge.

**A8: The engineering manager / head of platform is the economic buyer, and finance is never in the room.**
— basis: founder's stated view of the buying unit. To be tested in customer discovery; see `validation/decision_making_unit.md`.
— kills-pack-if-wrong: no — but a finance-signed deal has a different cycle length, and the self-serve motion in `strategy/gtm.md` would be wrong.

### Tier 3 — framing and scope choices made this session

**A9: "Self-hosted first, hybrid later" — the beachhead is self-hosted open-weight pools, with hosted API models admitted later as an additional top tier.**
— basis: founder decision this session, choosing against both "self-hosted purist" and "mixed pool from day one".
— kills-pack-if-wrong: no — it is a sequencing decision. It does set the competitive frame in `research/competitors.md` and the TAM boundary in `strategy/market_sizing.md`.

**A10: The pack is written as a venture pack with the capstone origin stated plainly, rather than an academic report with a commercial annex.**
— basis: founder decision this session.
— kills-pack-if-wrong: no — it is a presentation choice, but it obliges every forward number in the pack to carry a source tag or an `(assumption)` tag, since none of them can be traction.

**A11: CAMIR is the venture name as well as the project name.**
— basis: default; no brand exercise was requested and no name conflict was checked.
— kills-pack-if-wrong: no.

**A12: The three "why now" shifts (usable open-weight small models, practical local multi-model serving, inference as a production line item) are real and datable within the last ~18–24 months.**
— basis: founder asserted them and explicitly flagged them as needing research to date and cite.
— kills-pack-if-wrong: no on its own — but an undated "why now" is the first thing an investor falsifies, so `research/capability_table.md` must close this or the claim gets dropped.

**A13: The team's test-automation and evaluation-infrastructure background transfers to benchmark engineering for LLM routing.**
— basis: founder's own framing of the edge, stated as the strongest available claim while conceding no domain history.
— kills-pack-if-wrong: no — it weakens the founder-market-fit narrative in `narrative/founder_story.md`, not the technology.

**A14: Semantic caching is orthogonal and composes with routing rather than substituting for it.**
— basis: founder's stated read of the competitive set. Mechanically plausible — caching removes repeats, routing sizes the non-repeats — but the *residual* savings available to routing after aggressive caching has not been measured here.
— kills-pack-if-wrong: no — but if caching already harvests most of the savings on real traffic, CAMIR's addressable saving shrinks, and `financials/unit_economics.md` inherits the error.

---

## What is *not* assumed

Stated explicitly, because a register that only lists assumptions can imply everything else is measured. **Nothing in this pack is measured yet.** There is no traction, no revenue, no pilot deployment and no benchmark run completed at the time of writing. Claims sourced to `research/sources.md` are other people's measurements, not CAMIR's. The first CAMIR number that will be its own evidence is the oracle ceiling from the A1/A2 experiment in [validation/experiment_board.md](validation/experiment_board.md).
