# CAMIR — founder input for grill-me

Source material for `startup-forge` phase 0. Answers are written against
`references/grill-question-bank.md`. Where I don't know something, it says so —
tag those `(assumption)` rather than inventing a number.

**One-liner:** CAMIR routes each LLM request to the smallest model tier that can
answer it correctly, cutting inference cost without meaningful quality loss, on
self-hosted model pools.

**Context:** this is a master's capstone project (SJSU CMPE 295A), not a funded
company. There is no traction, no revenue, no pilot customer. Treat the business
layer as a positioning exercise grounded in real research, not as a claim of
existing commercial validation.

---

## 1. Problem & pain

**The exact moment.** A platform engineer opens the monthly inference bill for a
product feature that serves a mixed bag of user requests — some trivial lookups,
some genuine multi-step reasoning. Every one of those requests went to the same
frontier model, because that's the only way to guarantee the hard ones get
answered. They're paying frontier prices for "what's the refund policy."

**How it's solved today.** Pick one model. Usually the biggest one the budget
tolerates. Some teams hand-write routing rules — regex on prompt length, a
keyword list, route anything mentioning "code" to the big model. These break
constantly and nobody trusts them. Others split by endpoint rather than by
request difficulty, which misses the variance inside each endpoint.

**Vitamin, painkiller, or oxygen.** Painkiller, and it sharpens as volume grows.
At low volume nobody cares. At high volume, inference is a top-three line item
and every point of margin matters. If they don't solve it, nothing breaks — they
just overpay indefinitely, which is exactly why it persists.

**Frequency.** Every request. This isn't a monthly chore, it's a per-call tax.

**Existing duct tape.** Prompt-length heuristics, endpoint-level model
assignment, manual A/B tests to find the cheapest model that "seems fine," and
semantic caching to avoid inference entirely. The existence of all four is the
demand evidence.

---

## 2. Users & spectrum

**First user — one named archetype.** The engineer who owns the inference bill
at a mid-size company running an LLM feature at real volume. Not a research lab,
not a two-person startup. Someone with enough traffic that a 40% cost cut is a
number their director notices, and enough engineering capacity to self-host or
run models on their own infrastructure.

**Low-support edge.** A team that wants a drop-in proxy: point your client at a
different base URL, get a cheaper bill, change nothing else. They will not tune a
classifier.

**Elite edge.** A team that wants to define their own tiers, plug in their own
quality judge, set their own tolerance threshold, and read the cost-quality
frontier before deploying. They want the knobs.

Can one system serve both? Yes — the router is the same, the difference is
whether you accept defaults or configure. That's a config surface question, not
two products.

**User ≠ customer ≠ payer.** The engineer feels the pain and evaluates. The
engineering manager or head of platform signs. Finance notices the result but is
never in the conversation.

**Who loses if we win.** Frontier model vendors, directly — routing traffic away
from their top tier is revenue off their books. Any inference-serving vendor
whose pricing assumes uniform model usage.

---

## 3. Why now

**What changed.** Three things, all recent. Capable small and mid-size open-weight
models became genuinely usable, so there's now a real tier below frontier that
isn't embarrassing. Serving multiple models locally became practical on commodity
hardware. And LLM inference moved from experiment budget to production line item,
which is when anyone starts caring about unit cost.

`(assumption — needs research phase to date and cite these three shifts
specifically)`

**Why previous attempts fell short.** FrugalGPT and RouteLLM both demonstrated
the core idea works. Their evaluations assume a large hosted model catalog from a
cloud vendor, which means a team without that access can't reproduce the result
or deploy the approach. The idea is proven; the reproducible, self-hostable
version isn't.

**Why hasn't the biggest incumbent done it.** Model vendors have a structural
disincentive — routing traffic down-tier reduces their revenue per request. They
offer cheaper models, but they don't offer a system whose explicit purpose is to
use their expensive model less. That's not slowness, it's incentive alignment.

---

## 4. Wedge & vision

**Smallest 10x slice.** A router that beats fixed-model baselines on a
mixed-difficulty benchmark, with the whole cost-quality frontier published and
reproducible on a small self-hosted pool. Not a platform. One measured result
anyone can re-run.

**Ten years out, if everything works.** Routing is a default layer in the
inference stack — nobody sends every request to one model, the same way nobody
serves every static asset from origin.

**Deliberately not doing in year one.** No fine-tuning. No model hosting. No
caching layer. No multi-tenant SaaS. No agentic or multi-turn routing — single
request in, tier decision out.

---

## 5. Moat & compounding

Be honest here: the moat is weak and I'd rather say so than invent one.

**What compounds.** Routing decisions plus observed outcomes are training data
for the classifier. A deployment that has seen a million of a customer's own
requests routes that customer's traffic better than a cold start. That's a real
loop but it's per-deployment, not network-wide.

**What a copycat would still lack after two years.** Accumulated per-customer
routing data and whatever calibration lives in it. Not much else. The algorithm
isn't the defensible part.

**Switching cost.** Low, and that's a genuine weakness. It's a proxy — you point
your base URL somewhere else and you're out. `(assumption: switching cost rises
if quality-tolerance policies and per-customer routing history are hard to
recreate — untested)`

---

## 6. Competition

Five alternatives, with the mechanism of failure for each:

1. **Do nothing — one fixed frontier model.** Works, guarantees quality, and
   overpays on every easy request. Fails on cost only.
2. **Fixed cheap model.** Cheapest option available, and it fails the hard subset
   of traffic. Quality loss is concentrated exactly where it's most visible.
3. **Hand-written routing rules.** Cheap to build, brittle, and rules keyed on
   surface features (length, keywords) don't correlate well with actual
   difficulty. Decays as traffic shifts.
4. **Hosted commercial routers.** Solve it, but assume the vendor's model catalog
   and add a vendor dependency. No path to self-hosting.
5. **Semantic caching.** Genuinely reduces cost, but only on repeated queries.
   Orthogonal — it composes with routing rather than replacing it.

**The two axes that matter.** Cost reduction achieved, against quality retained.
Everything lives on that plane, and the point of this project is to plot the
frontier rather than claim a point on it.

**Incumbent counter-move within 12 months.** A model vendor ships built-in
routing across its own tiers — which handles the cost problem for teams already
locked to that vendor, and does nothing for teams running their own models.

---

## 7. Business

**Who pays, how much, how often.** The team running inference at volume. The
comparable line item is their existing inference spend, which is what makes the
pricing conversation easy — it's a percentage of a bill they already see.

**Price anchored to what.** Cost saved. Charging a share of measured savings
aligns the incentive and makes the ROI conversation self-evident. Per-request
pricing is the simpler alternative if savings measurement proves contentious.
`(assumption: no pricing has been tested with any buyer)`

**Sales motion.** Self-serve for the drop-in proxy case, since the value is
measurable within a day of pointing traffic at it. The economic buyer is the
engineering leader who owns the infrastructure budget, and what they need to see
is a before-and-after on their own traffic, not a benchmark on someone else's.

---

## 8. Founder edge

This is the weakest section and it should read that way.

**What we know that a smart generalist doesn't.** Not much about the LLM routing
domain specifically — the team came to this area recently. What we do have is
directly relevant to the measurement half of the problem: two years of
professional test automation and validation work, building data-driven test
harnesses at scale (1,000+ test cases), CI pipelines, and systematic evaluation
infrastructure. The hard part of this project isn't the router, it's the
evaluation — building a mixed-difficulty benchmark, judging correctness
consistently, and producing a cost-quality frontier that survives scrutiny.
That's benchmark engineering, and it's the part we've actually done before.

**What we've built in this space.** Nothing commercial. This is the first
project in the domain.

**Talent and distribution.** No claim here. `(assumption)`

---

## 9. Riskiest assumption

**The one sentence that kills it if false:** prompt difficulty is predictable
from the prompt alone, accurately enough that routing beats a fixed model on the
cost-quality frontier.

If difficulty can't be predicted before generation, the classifier route
collapses and only the cascade route survives — and cascade pays for the small
model's failed attempt on top of the large model's answer, which erodes the
savings. This is precisely why the project evaluates both strategies rather than
committing to one.

**How to test it in two weeks for under $1k.** Take a mixed-difficulty benchmark.
Run every prompt through the small and large models. Label which prompts the
small model got right — that's the ground-truth ceiling for any router. Then
train a classifier on prompt features and measure how close it gets to that
ceiling. If the ceiling itself is low, the whole premise is wrong and no router
helps. Small open-weight models on local hardware, so the cost is time.

**What would make me walk away.** A ceiling so low that even a perfect oracle
router saves little — meaning the difficulty distribution is bimodal in a way
that sends almost everything to the large tier anyway.

---

## Vocabulary

Use these nouns, not placeholders:

- **router** — the component that makes the tier decision
- **tier** — small / medium / large model class
- **model pool** — the set of self-hosted models available to route across
- **classifier route** — predict difficulty before generation, dispatch once
- **cascade route** — try small first, escalate on low confidence
- **cost-quality frontier** — the measured trade-off curve
- **quality tolerance** — the acceptable drop versus the always-large baseline
- **fixed-model baseline** — sending everything to one model, the thing being beaten

Avoid: "the product", "the platform", "the solution", "AI-powered", "seamless".
