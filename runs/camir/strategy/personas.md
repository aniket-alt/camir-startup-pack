# CAMIR — Personas

**What this is** — six persona cards spanning the full user spectrum: the low-support drop-in team, the beachhead core, the elite knobs team, the economic buyer who signs, the engineer who can veto, and the open-source adopter who never pays but decides whether anyone hears about CAMIR.
**Why it exists** — CAMIR's user and payer are different people with opposite fears: the engineer's nightmare is a silent quality regression on their feature, the buyer's is a line item that keeps growing. A pack that writes to a single "developer" persona produces a pitch that reassures neither, and the deal stalls on the person nobody wrote a card for — the product engineer whose feature gets routed down and who can block the rollout without ever being in a sales conversation.
**How to read it** — read **P5 (Ravi)** first if you are evaluating risk; he is the one who kills deployments, and he is the persona most packs omit. Read P2 (Marcus) if you are evaluating the product. Each card's **Objection** line is the one that has to be answered in the product, not in the pitch.
**Depends on / feeds** — depends on [../BRIEF.md](../BRIEF.md) §Users & spectrum, [positioning.md](positioning.md), [../research/competitors.md](../research/competitors.md); feeds [../product/PRD.md](../product/PRD.md), [../product/journeys/](../product/journeys/), [value_prop_canvas.md](value_prop_canvas.md), [../validation/discovery_guide.md](../validation/discovery_guide.md) and [../validation/decision_making_unit.md](../validation/decision_making_unit.md).

All personas are composites constructed from the brief and the research layer. **None is a real interviewed person** — no discovery calls have been run. Language in the "would actually say" lines is plausible, not quoted `(assumption)`.

---

## P1 — Priya Raghunathan · the drop-in team · **edge-low**

**Role.** Senior backend engineer, 60-person B2B SaaS. Owns the LLM-backed feature end to end because nobody else does.
**Context.** Self-hosts two open-weight models on two rented GPUs — **not to save money**, but because their largest customer's contract forbids sending customer text to a third-party API. This is the common case: self-hosting rarely wins on cost alone against budget open-weight APIs [S28].
**Day-in-the-life pain moment.** Thursday afternoon. The GPU bill is fine; her problem is that everything goes to the 70B model because she picked it once, six months ago, and has no way to know whether the 8B would have been fine. She has never measured it and has no time to.
**Current workaround.** One model, chosen once, never revisited. A single hand-written rule: anything over 2,000 characters gets the big model.
**Trigger to switch.** Her director asks whether they can add the feature to the free tier. That requires the per-request cost to drop, and she has one afternoon to find out if it can.
**Must-have language she would actually say.** *"I don't want to train anything. I want to point the client at a different port and see a number that says whether it got worse."*
**Objection you must overcome.** *"If this silently makes answers worse, I find out from a support ticket, not from a dashboard."* — **Answered in the product, not the pitch**: the default configuration must ship with a conservative tolerance, a shadow-mode comparison against the fixed-model baseline before any traffic is routed, and an alert when measured quality crosses the declared tolerance.
**What she never does.** Tune a classifier. Read a frontier curve. Attend a demo.

---

## P2 — Marcus Bell · the engineer who owns the bill · **beachhead core**

**Role.** Staff platform engineer, ~400-person company. Owns the shared inference service that six product teams call.
**Context.** Self-hosted pool: a small model, a mid model, and a large model, on a modest GPU fleet. Roughly $50k/month of inference cost, of which he is asked about every month. Traffic is genuinely mixed — a support-answering endpoint that ranges from "what's the refund policy" to multi-step account reasoning, all through one API.
**Day-in-the-life pain moment.** The first Monday of the month. Finance's cost-allocation report lands, inference is the second-largest line in his budget [S25], and his director asks the same question every time: *"can we make this cheaper without breaking anything?"* He has no answer that is not a guess, because nobody has ever measured what the smaller models can actually handle on their traffic.
**Current workaround.** Endpoint-level model assignment plus one A/B test from March that concluded "the mid model seems fine for the FAQ path." Nobody has re-run it since two model upgrades ago.
**Trigger to switch.** Being told to cut infrastructure cost 20% without degrading anything — a directive with a number attached and no method attached.
**Must-have language he would actually say.** *"Show me, on my traffic, what it costs me at 99% of current quality and at 95%. Then I'll pick, and I'll be the one who picked."*
**Objection you must overcome.** *"Every vendor tells me they saved me money. They're also the ones doing the math."* — the incentive problem: OpenRouter's revenue is 5% of the bill it sits on [S17]; a frontier vendor is paid more when it routes up [S20]. **The answer is that the harness runs inside his perimeter and he can re-run it.**
**Why he is the beachhead.** He has the traffic volume for a saving to matter, the engineering capacity to self-host, and the ownership to act without a committee. `../BRIEF.md` names him; everything in this pack is written to him first.

---

## P3 — Wen Xu · the knobs team · **edge-high**

**Role.** ML infrastructure lead, high-volume consumer AI product. Has an evaluation team and an internal quality bar with numbers attached.
**Context.** Five tiers, not three, including a fine-tuned specialist. Runs an internal judge. Already tried RouteLLM and abandoned it — two tiers were not enough, and the published numbers are MT-Bench numbers that did not reproduce on their traffic [S1][S2].
**Day-in-the-life pain moment.** A quarterly planning meeting where she has to justify GPU capacity. She knows there is money on the floor but cannot prove *how much* without a month of engineering she cannot spare, and she does not trust anyone else's benchmark because she has read the comparability critique [S13].
**Current workaround.** A bespoke internal router, built once, maintained by nobody, calibrated against a benchmark from last year.
**Trigger to switch.** A published methodology she can audit — specifically, a judging protocol that survives the 2026 reliability findings [S33][S34] and a cost axis derived for self-hosted pools rather than borrowed from hosted list prices [S7].
**Must-have language she would actually say.** *"I'll plug in my own judge and my own tiers. What I want from you is the harness and the inter-judge agreement numbers. If you don't publish judge agreement, your frontier is noise."*
**Objection you must overcome.** *"I could build this in three weeks."* — **true, and the answer is not to deny it.** She could. The answer is that she would then own it, and the thing she is short of is not capability but maintenance capacity, which is exactly what killed her last internal router.
**Why she matters more than her segment size.** She is the technical reference. Wen adopting CAMIR is the credibility event that makes Marcus's evaluation short.

---

## P4 — Dana Okonkwo · Head of Platform · **economic buyer, payer ≠ user**

**Role.** Head of Platform Engineering. Owns the infrastructure budget. Marcus reports two levels below her.
**Context.** Signs the contract. Never runs the proxy, never reads the frontier curve, never opens the repository.
**Day-in-the-life pain moment.** Quarterly budget review. She is asked why inference grew 40% while headcount was flat, and "it's a usage-based cost" is not an answer that survives a second quarter.
**Current workaround.** Pressure downward. She asks Marcus the cheaper-without-breaking question and accepts whatever he says.
**Trigger to buy.** A before-and-after on **her own traffic**, produced by her own engineer, that she can put in a slide. `../BRIEF.md` is explicit: what she needs to see is not a benchmark on someone else's traffic.
**Must-have language she would actually say.** *"What did it cost us before, what does it cost now, and who checked that nothing got worse? I need the third one in writing."*
**Objection you must overcome.** *"If it's open source, why am I paying?"* — the honest open-core answer: the router is free and always will be; the paid layer is per-deployment classifier training, tolerance policy management, savings attribution she can defend in a budget review, and someone to call when the frontier moves. **If that answer is not crisp, the open-core model has the TensorZero failure shape** [S23].
**What she never does.** Evaluate. Her role is to approve, and to ask who checked.

---

## P5 — Ravi Menon · the product engineer whose feature gets routed down · **the veto**

**Role.** Senior engineer on a product team that consumes Marcus's shared inference service. Not a customer, not a user, not in any sales conversation.
**Context.** His feature's quality is measured by his own team's metrics. He did not ask for routing and gains nothing from a cheaper bill that lands in someone else's budget.
**Day-in-the-life pain moment.** Two weeks after rollout, his feature's thumbs-down rate is up and he cannot tell whether it is the router, a prompt change, or noise. He escalates. **This is the single most likely way a CAMIR deployment dies**, and it is the exact failure the public GPT-5 routing backlash documented: complex queries degraded by being sent to a smaller model, with no dial and no attribution [S21].
**Current workaround.** Insisting his endpoint stays pinned to the large model. Which is endpoint-level assignment — one of the four workarounds CAMIR exists to replace.
**Trigger to *not* block.** Per-request tier attribution in his own traces, a per-endpoint tolerance he controls, and the ability to pin a route back to the large tier unilaterally without filing a ticket.
**Must-have language he would actually say.** *"If my p95 answer quality moves, I need to know within the hour whether it was you. And I need to be able to turn it off myself."*
**Objection you must overcome.** *"This optimises someone else's budget using my quality."* — the structural asymmetry: **the saving accrues to Dana, the risk accrues to Ravi.** Product answers: per-endpoint tolerance ownership, tier decision stamped on every trace, one-flag opt-out, and shadow mode before enforcement.
**Why this card exists.** Personas usually cover who benefits. Ravi benefits from nothing here and can stop everything. See [../validation/decision_making_unit.md](../validation/decision_making_unit.md).

---

## P6 — Sam Ortega · the open-source adopter who never pays · **distribution**

**Role.** Infrastructure engineer at a 12-person startup; also the person who writes the "we cut our inference bill" post that reaches Marcus.
**Context.** Runs one model on one GPU. **Below the volume floor where any of this matters** — roughly 16M tokens/day is where self-hosting even begins to pay [S27]. He will never be a customer.
**Day-in-the-life pain moment.** Curiosity plus a small bill. He wants to know whether the 8B can do what the 70B does, because it would be a good post.
**Current workaround.** Ollama and vibes. He is one of the 52 million monthly downloads [S30], and the reason downloads are a bad proxy for the beachhead [G1].
**Trigger to adopt.** A repository that runs on one GPU in an afternoon and produces a chart.
**Must-have language he would actually say.** *"Cloned it, pointed it at my logs, got a frontier curve in twenty minutes. Here's my graph."*
**Objection you must overcome.** *"Is this actually open, or open until you need revenue?"* — open-core credibility is fragile and one relicensing kills it. The commitment in [market_type.md](market_type.md) — router and harness open, control plane paid, no feature ever moved from open to paid — has to be stated publicly and kept.
**Why he matters.** Open-core conversion runs **1–5% of active users to hosted SaaS** [S40]. Sam is the 95–99%. He is not a failed customer; he is the channel, and treating him as a lead to convert is how open-core projects lose their community.

---

## The spectrum, on one line

| | Volume | Configures? | Pays? | Can block? | What they need |
|---|---|---|---|---|---|
| **P1 Priya** — drop-in | low-mid | no | maybe | no | Defaults that cannot silently hurt her |
| **P2 Marcus** — beachhead | high | some | evaluates | no | A frontier on *his* traffic, and to be the one who picks the point |
| **P3 Wen** — knobs | very high | everything | yes | no | The harness, her own judge, published judge-agreement statistics |
| **P4 Dana** — buyer | n/a | never | **signs** | yes, by not signing | Before-and-after she can defend, and a name attached to "who checked" |
| **P5 Ravi** — consumer | n/a | per-endpoint | never | **yes, unilaterally** | Attribution, a tolerance he owns, and an off switch |
| **P6 Sam** — OSS | below floor | plays | **never** | no | An afternoon to a chart, and a licence that stays put |

**One system serves all six.** The router is identical; what varies is how much of the config surface is exposed and who owns the tolerance. Priya takes defaults, Wen replaces every component, Ravi gets a per-endpoint override, Dana gets a report. That is a configuration question, not five products — the position `../BRIEF.md` §Users takes, and the one the PRD must hold.

---

## Recommended next 3

1. **Design for Ravi first, sell to Dana second, build for Marcus.** The deal is won on Dana's report and lost on Ravi's escalation, and only one of those is in a sales conversation. Per-endpoint tolerance ownership and per-request tier attribution are therefore **P0 in the PRD**, not observability nice-to-haves.
2. **Run the first ten discovery calls against P2's profile only**, using the sizing question from [market_sizing.md](market_sizing.md) — what share of your production tokens runs on weights you own, and what do you spend monthly. Ten conversations collapse factor f2 and the persona set at the same time.
3. **Write P6's licence commitment publicly before the first release.** Open-core credibility is spent once; the TensorZero post-mortem [S23] is what happens when the two halves are not clearly separated from the start.
