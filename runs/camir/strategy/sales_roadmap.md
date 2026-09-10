# CAMIR — Sales Roadmap

**What this is** — Blank's Customer Validation Phase 1 set: the **organisation map** (who sits where inside the buyer), the **influence map** (who actually sways the decision), the **access map** (how a stranger reaches them), and the sales process with the artifact required at each step.
**Why it exists** — "we'll sell to platform teams" hides the fact that nobody has mapped who signs. For CAMIR the map has a specific and unusual shape: **the person who benefits, the person who signs, and the person who can stop it are three different people, and only one of them is in the conversation.** The saving lands in the platform budget; the risk lands on a product engineer in a different reporting line who was never consulted. Without this file the plan optimises for the buyer and gets vetoed by someone the plan never named.
**How to read it** — the influence map is the load-bearing section, and the **saboteur row** is the one to read first. §Sales process is the sequence; its Artifact column is what has to exist before each step can happen.
**Depends on / feeds** — depends on [personas.md](personas.md), [gtm.md](gtm.md), [channel_plan.md](channel_plan.md), [petal_diagram.md](petal_diagram.md); feeds [../validation/decision_making_unit.md](../validation/decision_making_unit.md), [../validation/discovery_guide.md](../validation/discovery_guide.md) and [../financials/revenue_build.md](../financials/revenue_build.md).

**No sale has been made and no deal has been run.** Everything here is a designed process against a mapped organisation, not an observed one `(assumption)`.

---

## 1. Organisation map — who sits where

The target: a company of roughly 200–800 people running an LLM feature at production volume on self-hosted weights.

```
   VP Engineering / CTO
            │
            ├── Head of Platform Engineering ................. P4 DANA — signs the contract
            │        │
            │        ├── Platform / Infrastructure team
            │        │      └── Staff Platform Engineer ...... P2 MARCUS — owns the inference
            │        │                                          service, evaluates, decides
            │        └── SRE / on-call
            │
            ├── Product Engineering (separate reporting line)
            │        └── Senior Engineer ..................... P5 RAVI — consumes the shared
            │                                                   service, bears the quality risk,
            │                                                   can block unilaterally
            │
            ├── ML / Applied AI (present in ~half of targets)
            │        └── ML Infra Lead ...................... P3 WEN — technical validator;
            │                                                   an ally if won, a blocker if not
            │
            └── Security / Compliance ....................... gate for an in-path proxy,
                                                                not a decision-maker

   Finance ......... sees the result on the bill. Never in the conversation
                     (`../BRIEF.md` §Users is explicit about this)
```

**The structural fact that shapes everything:** Marcus and Ravi are in **different reporting lines** and meet only at the VP. The saving accrues to Dana's budget; the risk accrues to Ravi's metrics. No one below the VP is accountable for both, which is why a deployment that is technically successful can still be reverted.

---

## 2. Influence map — who actually sways it

| Role | Person | Formal power | Real influence | What moves them | What they fear |
|---|---|---|---|---|---|
| **Evaluator / champion** | **P2 Marcus** | none over budget | **highest** — Dana signs what he recommends | A frontier measured on his own traffic that he can re-derive | Being the cause of an unattributable regression [S21] |
| **Economic buyer** | **P4 Dana** | signs | medium — she is buying Marcus's judgment | A before-and-after on her own traffic, and a named person who checked | Approving something that breaks a product line; the "why pay for open source" question [S23] |
| **Technical validator** | **P3 Wen** | none | **high, and asymmetric** — one sentence of doubt ends the evaluation | Published judging protocol with inter-judge agreement [S33][S34]; a self-hosted cost axis [S7] | Endorsing a benchmark that does not reproduce |
| **Saboteur / veto** | **P5 Ravi** | none formally | **absolute in practice** | Per-endpoint tolerance he owns; tier stamped on his traces; a unilateral pin-to-large flag | Someone else's cost saving degrading his feature's metrics |
| **Gate** | Security / Compliance | blocks | low but binding | In-perimeter deployment, no data egress, self-hosted by construction | Data leaving the boundary — **CAMIR passes this by architecture, which is a genuine advantage over every hosted router** |
| **Absent** | Finance | — | none | — | — |

### The saboteur row, at full strength

Ravi is the reason this file exists. He is not in the sales process, gains nothing from the purchase, and can end it by escalating a quality question nobody can answer quickly. **The public precedent is documented**: when routing shipped with a vendor-set tolerance, users immediately reported complex queries degraded by being sent to the smaller model, with no dial available [S20][S21].

**He is neutralised by product, not by selling.** Per-endpoint tolerance ownership, per-request tier attribution, shadow mode before enforcement, and a unilateral opt-out — the P0 set from [value_prop_canvas.md](value_prop_canvas.md). If those do not ship, no sales process saves the deployment in month three.

---

## 3. Access map — how a stranger reaches these people

| Target | Works | Does not work | Why |
|---|---|---|---|
| **P2 Marcus** | A routing strategy already present in the proxy he runs [S22]; a published frontier a peer shared; a conference talk on the measurement result | Cold email, LinkedIn, demo requests, webinars | He installs things his peers vouched for. `../BRIEF.md` and [channel_plan.md](channel_plan.md) both reject outbound: CAC of ~$35k against a $30k ACV, and it burns the open-source goodwill the primary channel depends on |
| **P4 Dana** | **Only through Marcus.** She is reached with his report, in his words | Direct outbound to her | Going over the evaluator's head routes the deal around the person whose judgment is actually being bought, and turns a champion into an opponent |
| **P3 Wen** | The methodology as a citable artifact; an academic or industry venue; the harness repository | Sales contact of any kind | She evaluates the protocol, not the product |
| **P5 Ravi** | **Through the product surface**, before he ever hears the company's name — a tolerance setting in his config and a tier field in his traces | Any conversation initiated after his metrics moved | By the time he is talking to anyone, he has already escalated |
| **Security** | A written deployment architecture showing no egress | A questionnaire response | The architecture answers it: CAMIR runs inside the perimeter by construction |

**The access map has exactly one front door: the software the customer already runs.** Every other route is either rejected on economics ([channel_plan.md](channel_plan.md)) or reaches the wrong person in the wrong order.

---

## 4. Sales process

| # | Step | Who | Artifact required | Exit criterion | Typical elapsed |
|---|---|---|---|---|---|
| 0 | **Passive discovery** — the strategy exists in his proxy, or a peer's frontier chart circulates | Marcus alone | The open harness; one published self-hosted frontier [G2] | He enables it in a non-production environment | — |
| 1 | **Self-serve frontier** — he runs the harness on his own logged traffic | Marcus alone | Quickstart producing a frontier chart in one afternoon on one GPU | **He has an oracle ceiling and a frontier for his own traffic** | 1 day |
| 2 | **Qualification** — is routing worth it here at all | Marcus, plus us if he asks | The ceiling number from step 1 | Oracle ceiling high enough that a real saving exists. **If not, we tell him so and the process ends here** — see §The disqualification step | 1 day |
| 3 | **Shadow mode** — decisions computed and logged, all traffic still to the fixed-model baseline | Marcus; Ravi **informed here, not later** | Tier-stamped traces; per-endpoint tolerance config; a shadow report | 2–4 weeks of real traffic with no enforcement, and Ravi has seen his own endpoint's numbers | 2–4 weeks |
| 4 | **Technical validation** | Wen, where present | Judging protocol with inter-judge agreement; self-hosted cost axis derivation [S33][S34][S7] | She has no objection she can state in one sentence | 1–2 weeks |
| 5 | **Enforcement at a declared tolerance** — Marcus picks the point on his curve | Marcus; Ravi holds a per-endpoint veto | Tolerance policy; alert on tolerance breach; unilateral pin-to-large | 2 weeks enforced with no escalation from any consuming team | 2 weeks |
| 6 | **The buyer conversation** | Marcus presents; Dana decides | **One slide: baseline cost, cost at declared tolerance, measured quality delta, who verified it** | Dana asks about price rather than about proof | 1 meeting |
| 7 | **Savings definition** — agree what counts as an eligible saving | Dana, Marcus | A written counterfactual definition | **Signed definition of eligible savings.** In FinOps share-of-savings contracts this is precisely what gets negotiated [S38] | 1–2 weeks |
| 8 | **Contract** | Dana; Security in parallel | Deployment architecture showing no egress | Signature | 2–4 weeks |

**Total: 60–120 days from first install to signature** `(assumption: no deal has been run)`. Consistent with the niche-re-segmentation posture in [market_type.md](market_type.md) — short technical evaluation, long budget decision.

### The disqualification step

Step 2 can end the process, and CAMIR should let it. If a customer's oracle ceiling is low — their traffic is uniformly hard — **no router helps them and we say so on day two.** This is uncomfortable commercially and correct in three ways: it is the same walk-away logic `../BRIEF.md` applies to the whole venture; it is the fastest possible qualification, freeing a small team from unwinnable evaluations; and telling a skeptical platform engineer not to buy is the single most credible thing a vendor can do. **It is also only possible because the harness is open and runs in his perimeter** — a hosted competitor whose revenue is a share of the bill has no incentive to ever reach this conclusion.

---

## 5. The objection register

| Objection | Who raises it | Answer | Artifact |
|---|---|---|---|
| *"I could build this in three weeks."* | Marcus, Wen | True. The build is not the problem; the **frontier moves with every model release** and nobody maintains an internal router. Wen already built one and abandoned it. **This is the primary competitor** — Petal 5, not Martian [petal_diagram.md](petal_diagram.md) | Continuous re-measurement; the drift evidence from one model-upgrade cycle |
| *"Routing is free, my vendor does it."* | Dana | Only inside one catalog, with a tolerance you cannot set, on weights you do not run [S20]. If your models are your own, that router does not exist for you | Positioning one-liner |
| *"If it's open source, why am I paying?"* | Dana | Router and harness free forever. Paid: per-deployment training, tolerance policy, savings attribution you can defend in a budget review. **No feature ever moves from open to paid** | The published open-core boundary |
| *"How do I know the saving is real?"* | Dana, Marcus | You computed it. The harness ran in your perimeter on your traffic and you can re-run it. Every hosted alternative has the biller computing the counterfactual [S17] | Step 6 slide plus the re-runnable harness |
| *"This will degrade my feature."* | **Ravi** | You set your endpoint's tolerance, you see the tier on every trace, and you can pin to large without asking anyone | Product, not argument — and it must exist before step 3 |
| *"vLLM will do this natively."* | Wen | Probably, for the mechanism [S11]. The measurement layer is not on that path today, and it is what you are actually short of | Honest acknowledgement — she respects it more than a denial |
| *"You have no customers."* | Dana | True. Here is a published, reproducible frontier and a methodology you can audit. Re-run it yourself | The publication |

---

## 6. What has to exist before selling starts

1. **The open harness**, running on one GPU to a frontier chart in an afternoon (step 1 and the entire access map).
2. **One published self-hosted frontier with methodology** [G2] — the only credibility artifact CAMIR has pre-traction.
3. **Per-endpoint tolerance, tier-stamped traces, shadow mode, unilateral pin** — without these, step 3 creates the Ravi escalation the process exists to prevent.
4. **A written eligible-savings definition template** — step 7 is unbillable without one, and it is the step FinOps precedent says gets negotiated hardest [S38].

## Recommended next 3

1. **Build the step-3 shadow-mode report for Ravi before building anything for Dana.** The veto is unmanaged in every version of this plan that does not, and it costs nothing to sequence correctly.
2. **Draft the eligible-savings definition now, not at step 7.** It is the contractual crux of share-of-savings pricing [S38][S39] and drafting it early surfaces whether A5 is even answerable before three months of deployment ride on it.
3. **Write the disqualification step into the public documentation.** "We will tell you on day two if routing cannot help your traffic" is the most credible sentence available to a pre-traction vendor selling to skeptical engineers, and no competitor with a share-of-bill revenue model can say it.
