# CAMIR — Founder Brief

**What this is** — the single source of truth for the CAMIR artifact pack: the idea's problem, users, mechanism, moat, business model and riskiest assumption, fixed in the founder's own words and vocabulary.
**Why it exists** — every downstream artifact is generated from this file, so if it says "developers" instead of "the engineer who owns the inference bill", fifty documents inherit the vagueness. It also fixes the two framing decisions the founder made explicitly — self-hosted-first model pool, open-core commercial form — which strategy, tech and financials would otherwise each re-decide differently and incompatibly.
**How to read it** — read §Riskiest assumption first; if that sentence is false the rest of the pack is a well-organised mistake. Then §Mechanism & moat, which is where a skeptic should attack hardest, because the moat is genuinely weak and this brief says so rather than dressing it up.
**Depends on / feeds** — depends on `CAMIR_FOUNDER_INPUT.md` and `Project_Abstract.md` (repo root); feeds [ASSUMPTIONS.md](ASSUMPTIONS.md) and every artifact in [research/](research/), [strategy/](strategy/), [product/](product/), [tech/](tech/), [narrative/](narrative/), [validation/](validation/) and [financials/](financials/).

---

one-line: **CAMIR is a cost-aware inference router for platform teams running LLM features at volume that sends each request to the smallest model tier predicted to answer it correctly — via a prompt-difficulty classifier and a confidence-triggered cascade over a self-hosted model pool — cutting inference spend without crossing a declared quality tolerance.**

domain: LLM infrastructure / inference serving and cost optimisation
stage: **idea + research prototype.** Origin is an SJSU CMPE 295A master's capstone (September 2026). No revenue, no pilot customer, no traction. Every forward-looking number in this pack is sourced to `research/sources.md` or tagged `(assumption)`.

---

## Problem

A platform engineer opens the monthly inference bill for one product feature that serves a mixed bag of requests — trivial lookups alongside genuine multi-step reasoning. Every request went to the same frontier model, because that is the only way to guarantee the hard ones get answered. They are paying frontier prices for "what's the refund policy."

The tax is **per request**, not monthly: there is no chore to skip, no deadline, nothing that breaks. That is exactly why it persists — nothing fails, they simply overpay indefinitely. It is a **painkiller that sharpens with volume**: invisible at prototype traffic, a top-three line item at production traffic, where every point of gross margin is contested.

Solved today by four workarounds, all of which are demand evidence:

1. Pick one model, usually the largest the budget tolerates.
2. Hand-written routing rules — prompt-length regex, keyword lists, "anything mentioning code goes large." Brittle; nobody trusts them.
3. Endpoint-level model assignment, which misses the difficulty variance *inside* each endpoint.
4. Semantic caching, which avoids inference entirely but only on repeats.

## Users & spectrum

**Beachhead — the named archetype.** The engineer who owns the inference bill at a mid-size company running an LLM feature at real volume. Not a research lab, not a two-person startup: enough traffic that a 40% cost cut is a number their director notices, and enough engineering capacity to self-host models on their own infrastructure.

**Edge-low — the drop-in team.** Wants a proxy: point the client at a different base URL, get a cheaper bill, change nothing else. Will never tune a classifier, will never read a frontier curve. Must succeed on defaults alone.

**Edge-high — the knobs team.** Defines its own tiers, plugs in its own quality judge, sets its own quality tolerance, and reads the cost-quality frontier before deploying a policy. Wants to see the frontier, not be told a number.

**One system, not two products.** The router is identical across all three; the difference is whether you accept defaults or configure. That is a config-surface question, not a second product.

**User ≠ payer.** The engineer feels the pain and evaluates. The **engineering manager / head of platform** owns the infrastructure budget and signs. Finance notices the result on the bill and is never in the conversation.

**Who loses if CAMIR wins.** Frontier model vendors, directly — routed-down traffic is revenue off their books. Any inference-serving vendor whose pricing assumes uniform top-tier usage.

## Why now

Three shifts, all recent, each to be dated and cited in `research/capability_table.md`:

1. Capable small and mid-size **open-weight models** became genuinely usable, so a tier below frontier now exists that is not embarrassing.
2. Serving **multiple models locally** on commodity hardware became practical.
3. LLM inference moved from experiment budget to **production line item** — the point at which anyone starts caring about unit cost.

**Why previous attempts fell short.** FrugalGPT and RouteLLM both demonstrated the core idea works. Both evaluate against a large *hosted* model catalog from a cloud vendor, so a team without that access can neither reproduce the result nor deploy the approach. The idea is proven; the reproducible, self-hostable version is not.

**Why the biggest incumbent hasn't.** Structural disincentive, not slowness. Model vendors offer cheaper models; they do not offer a system whose explicit purpose is to use their expensive model less. Routing traffic down-tier reduces revenue per request. Incentive alignment, not capability.

## Wedge & 10-year vision

**Smallest 10x slice.** A router that beats fixed-model baselines on a mixed-difficulty benchmark, with the **entire cost-quality frontier published and reproducible on a small self-hosted model pool**. Not a platform. One measured result anyone can re-run on their own hardware.

**Year one — deliberately not doing.** No fine-tuning. No model hosting. No caching layer. No multi-tenant SaaS. No agentic or multi-turn routing — single request in, tier decision out.

**Ten years.** Routing is a default layer in the inference stack. Nobody sends every request to one model, the same way nobody serves every static asset from origin.

## Mechanism & moat

**Mechanism.** Two routing strategies, evaluated against each other rather than assumed:

- **Classifier route** — predict difficulty from prompt features before generation, dispatch once to the predicted tier. Cheap decision, no wasted generation, but bounded by how predictable difficulty actually is.
- **Cascade route** — try the small tier first, escalate on low confidence. Robust to unpredictable difficulty, but pays for the failed small attempt on top of the large answer.

The output is not a point claim but a **cost-quality frontier**: the measured trade-off curve, against which a team sets its **quality tolerance** — the acceptable drop versus the always-large **fixed-model baseline**.

**Moat — stated honestly, it is weak.**

- *What compounds:* routing decisions plus observed outcomes are labelled training data for the classifier. A deployment that has seen a million of a customer's own requests routes that customer's traffic better than a cold start. A real loop, but **per-deployment, not network-wide**.
- *What a funded copycat still lacks after two years:* accumulated per-customer routing history and the calibration living in it. Not much else. **The algorithm is not the defensible part.**
- *Switching cost:* low, and that is a genuine weakness — CAMIR is a proxy; change the base URL and you are out. `(assumption: switching cost rises once quality-tolerance policies and per-customer routing history become expensive to recreate — untested)`
- *Partial fix, per founder decision:* **open-core**. Router and benchmark harness are open source and drive adoption; the paid **control plane** holds per-deployment classifier training, quality-tolerance policy, savings measurement and observability — which is where the data loop lives, and therefore the only place switching cost can accumulate.

## Competition & failed alternatives (as stated by founder)

| # | Alternative | Mechanism of failure |
|---|---|---|
| 1 | Do nothing — one fixed frontier model | Works and guarantees quality; overpays on every easy request. Fails on cost only. |
| 2 | Fixed cheap model | Cheapest available; fails the hard subset, and the quality loss is concentrated exactly where it is most visible. |
| 3 | Hand-written routing rules | Cheap to build, brittle. Surface features (length, keywords) correlate weakly with real difficulty; decays as traffic shifts. |
| 4 | Hosted commercial routers | Genuinely solve it — inside the vendor's own catalog. Adds vendor dependency, offers no path to self-hosting. |
| 5 | Semantic caching | Real savings, but only on repeated queries. **Orthogonal — composes with routing rather than replacing it.** |

**The two axes that matter:** cost reduction achieved × quality retained. Everything lives on that plane, and CAMIR's point is to *plot the frontier* rather than claim a point on it.

**Incumbent counter-move within 12 months.** A model vendor ships built-in routing across its own tiers. That handles the cost problem for teams already locked to that vendor and does nothing for teams running their own models — which is precisely the beachhead.

## Business model

**Framing decision (founder, this session):** venture pack, capstone origin stated plainly. A positioning exercise grounded in real research, never a claim of commercial validation.

- **Model pool scope:** **self-hosted first, hybrid later.** The beachhead is open-weight pools on the customer's own hardware; the tier abstraction admits hosted API models as an additional top tier for teams keeping a frontier escape hatch. The cascade route's "escalate to frontier" then reads as a feature rather than a compromise.
- **Commercial form:** **open-core plus a commercial control plane.** Open: router, tier abstraction, benchmark harness, published frontier. Paid: per-deployment classifier training, quality-tolerance policy management, savings measurement and attribution, routing observability.
- **Who pays:** the team running inference at volume. The comparable budget line is their **existing inference spend** — which is what makes the pricing conversation easy, because it is a bill they already stare at.
- **Price anchored to:** cost saved. A share of measured savings aligns incentives and makes the ROI conversation self-evident; per-request pricing is the fallback if savings measurement proves contentious. `(assumption: no pricing has been tested with any buyer)`
- **Sales motion:** self-serve for the drop-in proxy case — value is measurable within a day of pointing traffic at it. The economic buyer is the engineering leader who owns the infrastructure budget, and what they need to see is a **before-and-after on their own traffic**, not a benchmark on someone else's.

## Founder edge

Stated weakly on purpose, because it is weak on the domain axis and real on the measurement axis.

- **Domain:** the team came to LLM routing recently. No commercial work in this space. Nothing built here before this project.
- **The real edge — benchmark engineering.** Two years of professional test automation and validation work: data-driven test harnesses at scale (1,000+ test cases), CI pipelines, systematic evaluation infrastructure. **The hard part of CAMIR is not the router, it is the evaluation** — building a mixed-difficulty benchmark, judging correctness consistently, and producing a cost-quality frontier that survives scrutiny. That is benchmark engineering, and it is the part this team has actually done before.
- **Team:** Abhishek Darji, Aniket Anil Naik, Tamizh Selvan Manivannan. Advisor: Prof. Vijay Eranti, SJSU CMPE 295A.
- **Talent and distribution:** no claim. `(assumption)`

## Riskiest assumption

> **Prompt difficulty is predictable from the prompt alone, accurately enough that routing beats a fixed model on the cost-quality frontier.**

If difficulty cannot be predicted before generation, the classifier route collapses and only the cascade route survives — and the cascade route pays for the small tier's failed attempt on top of the large tier's answer, eroding the savings. This is exactly why CAMIR evaluates both strategies instead of committing to one.

**Two-week, sub-$1k test.** Take a mixed-difficulty benchmark. Run every prompt through the small and the large tier. Label which prompts the small tier got right — that is the **oracle ceiling**, the best any router could possibly do. Then train a classifier on prompt features and measure how close it gets to that ceiling. If the ceiling itself is low, the premise is wrong and no router helps. Small open-weight models on local hardware, so the cost is time rather than money.

**Walk-away evidence.** An oracle ceiling so low that even a perfect router saves little — meaning the difficulty distribution is bimodal in a way that sends almost everything to the large tier regardless.

## Vocabulary

Use these nouns. Never placeholders.

| Noun | Means |
|---|---|
| **router** | the component that makes the tier decision |
| **tier** | small / medium / large model class |
| **model pool** | the set of self-hosted models available to route across |
| **classifier route** | predict difficulty before generation, dispatch once |
| **cascade route** | try small first, escalate on low confidence |
| **cost-quality frontier** | the measured trade-off curve — CAMIR's unit of value |
| **quality tolerance** | the acceptable drop versus the always-large baseline |
| **fixed-model baseline** | sending everything to one model — the thing being beaten |
| **oracle ceiling** | the score a perfect router would achieve; the upper bound on the whole idea |
| **escalation rate** | share of cascade-route requests that fall through to a higher tier |
| **control plane** | the paid layer: classifier training, policy, savings measurement, observability |
| **request** | the unit CAMIR acts on — one prompt in, one tier decision out |

**Banned:** "the product", "the platform", "the solution", "AI-powered", "seamless", "revolutionary", "cutting-edge".
