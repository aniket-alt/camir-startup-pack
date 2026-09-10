# CAMIR — Market Sizing

**What this is** — TAM, SAM and SOM for CAMIR, built bottom-up from one measured anchor, with every multiplier stated as a factor a reader can change and recompute.
**Why it exists** — the dominant risk of a niche re-segmented market is **segment size**, not adoption ([market_type.md](market_type.md)), and CAMIR's segment is defined by an architectural choice — running your own weights — for which no analyst has ever published a denominator [G1]. Without an explicit arithmetic, "large and growing LLM market" would paper over the finding this file actually produces: **the routing-fee market is small, in the tens of millions, and the venture case does not survive on routing fees alone.** That is the number the founder needs before writing a raise deck, and it is the number a top-down citation would have hidden.
**How to read it** — start at §The finding, which states the uncomfortable conclusion before the arithmetic that produces it. Then §TAM: the anchor is the only measured input; every other line is a factor with a flag. A skeptic should attack factor **f2** (self-hosted share) first — it is the widest and the least evidenced.
**Depends on / feeds** — depends on [../research/sources.md](../research/sources.md), [market_type.md](market_type.md), [positioning.md](positioning.md); feeds [../financials/pricing.md](../financials/pricing.md), [../financials/revenue_build.md](../financials/revenue_build.md), [../financials/use_of_funds.md](../financials/use_of_funds.md), [../narrative/one_pager.md](../narrative/one_pager.md) and [../narrative/vc_memo.md](../narrative/vc_memo.md).

---

## The finding, stated first

Routing fees on self-hosted inference are a **$40–90M/year global market today** on the arithmetic below — a good business, not a venture-scale one on its own. Two things change that verdict, and both are stated as claims to be tested rather than as assumptions to be enjoyed:

1. The denominator compounds at **~26% CAGR** [S24], so a 2026 sizing understates a 2030 opportunity by roughly 2.5×.
2. The routing fee is the **wedge**, not the ceiling. The natural expansion is the inference-efficiency control plane — the measurement, policy and attribution layer that routing is the first application of.

A pack that sized this at "1% of a $51B market" would have produced a bigger number and a worse decision. **The honest read is that CAMIR must plan for expansion revenue from day one, or plan to be a feature.**

---

## The anchor — the only measured input

Nearly every sizing in this space starts from an analyst headline. CAMIR's starts from a revealed transaction.

**OpenRouter reported $160M annualised revenue in August 2026, monetising at approximately 5% on top of customer inference spend** [S17].

$160M ÷ 0.05 = **~$3.2B of annual LLM inference spend routed through a single hosted aggregator.**

This is the strongest sizing input available anywhere in the research layer: it is a real price paid by real buyers for a routing/gateway layer, not a survey response or a forecast. Two caveats travel with it — OpenRouter is a gateway whose value includes catalog access and billing consolidation, not routing alone; and it is one aggregator among several, so $3.2B is a floor on aggregated hosted spend rather than a total.

---

## TAM — global annual routing-fee revenue available on self-hosted inference

Bottom-up. Each factor is labelled `[measured]`, `[sourced]` or `(assumption: basis)`.

| Step | Factor | Low | Base | High | Basis |
|---|---|---|---|---|---|
| **a** | Hosted inference spend flowing through aggregators | $3.2B | $3.2B | $3.2B | `[measured]` $160M ÷ 5% take rate [S17] |
| **f1** | Multiplier from one aggregator to total hosted-API inference spend | ×2 | ×3 | ×5 | `(assumption: OpenRouter is a leading but not sole aggregator, and much hosted spend goes direct to vendors, bypassing aggregators entirely — a direct-to-vendor share of 50–80% implies this range)` |
| **b** | **Total hosted-API inference spend** | $6.4B | $9.6B | $16B | a × f1 |
| **f2** | Self-hosted inference spend as a ratio of hosted spend | ×0.15 | ×0.30 | ×0.50 | `(assumption: the widest factor in this table and the one to attack.` Directional support only: Ollama grew from ~100K to **52M monthly downloads** between Q1 2023 and Q1 2026 [S30], but downloads are not production deployments [G1]. Countervailing: self-hosting **rarely wins on cost alone** against budget open-weight APIs [S28], so the population is bounded by non-price motives — data residency, latency, control`)` |
| **c** | **Total self-hosted inference spend (TAM denominator)** | $1.0B | $2.9B | $8.0B | b × f2 |
| **f3** | Share above the volume floor where routing is worth operating | ×0.60 | ×0.70 | ×0.80 | `(assumption: below roughly 16M tokens/day a team should not be self-hosting at all` [S27]`, so most sub-scale spend is out of scope by construction; the survivors are concentrated)` |
| **f4** | Share surviving upstream semantic caching | ×0.55 | ×0.70 | ×0.80 | `[sourced]` production cache hit rates **20–45%** [S36]. Base case takes the midpoint of the loss. **Note the adverse selection**: cache hits skew to the repetitive easy requests routing would have sent small, so post-cache traffic is harder than average and routing saves less on it [G4] |
| **d** | **Routable spend** | $0.33B | $1.4B | $5.1B | c × f3 × f4 |
| **f5** | Routing-layer take rate | ×3% | ×5% | ×7% | `[sourced]` OpenRouter charges **~5% of inference spend** [S17]; share-of-savings vendors in cloud FinOps price per-customer and publish no universal rate [S38][S39] |
| **TAM** | **Global annual routing-fee revenue on self-hosted inference** | **$10M** | **$70M** | **$360M** | d × f5 |

**Base-case TAM ≈ $70M/year, corridor $10–360M.** The corridor is wide because f2 is unevidenced, and it should stay wide until discovery narrows it. Anyone quoting the high end without f2's caveat is quoting a guess.

### Top-down sanity check

The enterprise LLM market is put at **$8.18B in 2026, growing to $51.66B by 2034 at 25.9% CAGR** [S24]. Against that denominator, base-case TAM is **~0.9%** of the 2026 enterprise LLM market. That is a plausible share for a thin infrastructure layer — gateway and observability layers typically take low single-digit percentages of the spend they sit on, which is what the 5% take rate [S17] independently says. The two methods agree on order of magnitude, which is the most that should be claimed for either.

**Growth:** at 25.9% CAGR [S24], base-case TAM reaches **~$220M by 2030** on an unchanged share.

---

## SAM — the segment CAMIR can serve with its declared year-one product

Restrictions from [positioning.md](positioning.md) and `../BRIEF.md` §Wedge (no fine-tuning, no hosting, no caching, no multi-turn or agentic routing, single request in / tier decision out):

| Restriction | Factor | Basis |
|---|---|---|
| English-language, US + EU commercial buyers reachable by a small team | ×0.55 | `(assumption: standard developer-infrastructure geographic concentration; no source found)` |
| Single-turn request traffic only — agentic and multi-turn excluded by declared non-goal | ×0.65 | `(assumption: agent traffic is a large and growing share of production LLM calls; excluding it removes roughly a third)` |
| Teams willing to place a proxy in the request path | ×0.80 | `(assumption: the drop-in proxy is the low-support path; some fraction will refuse an in-path dependency and need the recommender mode)` |

**SAM = $70M × 0.55 × 0.65 × 0.80 ≈ $20M/year** (corridor $3–103M).

---

## SOM — the beachhead, sized separately and in customers

The beachhead from `../BRIEF.md`: the engineer who owns the inference bill at a **mid-size company** running an LLM feature at real volume, with capacity to self-host.

### How many such companies exist

| Step | Value | Basis |
|---|---|---|
| Total self-hosted inference spend (base case, line **c**) | $2.9B | above |
| Average annual inference spend per company at beachhead scale | $600k/yr ($50k/month) | `(assumption: $50k/month is the threshold at which a 25% saving is ~$150k/yr — large enough for a director to notice, per` `../BRIEF.md` `§Users)` |
| **Implied population of companies self-hosting at beachhead scale or above** | **~4,800** | $2.9B ÷ $600k |
| Narrowed to SAM restrictions (×0.55 × 0.65 × 0.80 = 0.286) | **~1,370 reachable companies** | |

Corridor across f2's range: **~470 to ~3,800 reachable companies.** This is the single most decision-relevant number in this file, because it is what twenty discovery calls can actually check.

### SOM at three years

| Year | Share of reachable population | Customers | ACV | ARR |
|---|---|---|---|---|
| Y1 | 0.4% | ~5 | $30k | $0.15M |
| Y2 | 2% | ~27 | $32k | $0.9M |
| Y3 | 5% | ~68 | $35k | $2.4M |

**ACV derivation** `(assumption: no pricing tested with any buyer — A4, A5)`: a beachhead company spends $600k/yr on inference; routing saves 25% of routable spend `(assumption: conservative against RouteLLM's 1.41× on knowledge tasks` [S2]`, and no CAMIR measurement exists)` ≈ $105k/yr saved after the f3/f4 haircuts; CAMIR takes ~28% of measured savings ≈ **$30k/yr**, which also sits near 5% of the customer's spend — the two pricing anchors [S17][S38] converge, which is mild evidence the number is not arbitrary.

**Sanity check on Y3 share:** 5% of a reachable population is aggressive for a three-year-old open-core infrastructure company. Open-source conversion runs **1–5% of active users to hosted SaaS and 0.01–0.1% to enterprise licences** [S40], so 5% of *reachable companies* implies CAMIR's open half reaches a large majority of the segment. Treat Y3 as the optimistic edge, not the plan.

---

## What this arithmetic means for the venture case

Four consequences, stated plainly:

1. **Routing fees alone do not make a venture-scale company** at 2026 denominators. $2.4M ARR at year three against a $20M SAM is a healthy infrastructure business and a hard Series A story on its own.
2. **Factor f2 is the whole argument.** At the high end (×0.50) TAM is $360M and the case is straightforward; at the low end (×0.15) it is $10M and CAMIR is a feature. **Nothing else in this file moves the answer as much, and it is answerable by discovery rather than by building.**
3. **Expansion revenue is not optional.** The path from routing fee to inference-efficiency control plane — measurement, policy, attribution across more than tier selection — must be in the plan from the start. See [../financials/revenue_build.md](../financials/revenue_build.md).
4. **The cost spread is compressing.** Small models are converging on frontier quality — a 31B model within ~10 Elo of 600B–1000B+ models [S29] — which narrows the price gap routing arbitrages. The denominator grows at 26% [S24]; the savings *rate* per dollar of spend probably shrinks. **These pull in opposite directions and no source resolves them**, which is the honest state of the estimate.

---

## Recommended next 3

1. **Kill or confirm f2 with twenty discovery calls before any raise.** The question is not "would you use a router" but "what share of your production LLM tokens is served from weights you run, and what do you spend on it monthly". That single question collapses a 36× corridor.
2. **Re-anchor the ACV once the first frontier is measured.** Every number downstream of "routing saves 25%" is an assumption; the two-week oracle-ceiling experiment ([../validation/experiment_board.md](../validation/experiment_board.md)) replaces it with a measurement.
3. **Write the expansion thesis into `revenue_build.md` explicitly**, with the second product named and sized, rather than leaving "control plane" as a word. On this arithmetic the expansion *is* the venture case, and an unnamed expansion is not a plan.
