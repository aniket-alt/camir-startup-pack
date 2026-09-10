# Journey — One ordinary day · Marcus, Ravi, Dana · the payer is not the user

**What this is** — a single Tuesday in month four of a live deployment, narrated across three people in two reporting lines: the engineer who owns the bill, the engineer who bears the quality risk and gains nothing, and the executive who signs and never evaluates. Timestamped, with the component firing and the record written at each beat.
**Why it exists** — every other journey in this pack follows one person adopting CAMIR. Adoption is not the failure mode. The failure mode is month four, on a day nobody planned, when a product metric moves for an unrelated reason and someone has to answer *"was it the router?"* within the hour. If that answer takes a day, Ravi pins to large permanently and the deployment is over without a meeting. This journey specifies the ordinary day the product must survive, and it is where CAMIR's metric M13 (question-to-attributed-answer, target under one hour) is either true or decorative.
**How to read it** — 09:47 to 10:31 is the journey; everything else is context for why that stretch is survivable. A skeptic should attack the claim that Ravi resolves his question **from his own telemetry, without CAMIR being involved**, since that is the difference between an off-switch he owns and an off-switch he requests.
**Depends on / feeds** — depends on [beachhead.md](beachhead.md), [../../strategy/personas.md](../../strategy/personas.md) P2/P4/P5, [../PRD.md](../PRD.md) §7–§8; feeds [../ux_spec.md](../ux_spec.md), [../../validation/decision_making_unit.md](../../validation/decision_making_unit.md), [../../validation/metrics_by_stage.md](../../validation/metrics_by_stage.md) and [../../narrative/one_pager.md](../../narrative/one_pager.md).

---

## The cast, and the structural fact

| | Marcus Bell | Ravi Menon | Dana Okonkwo |
|---|---|---|---|
| Role | Staff platform engineer | Senior product engineer | Head of Platform |
| Reports to | Dana's org | **A different VP** | — |
| Owns | The shared inference service, ~$50k/mo | His feature's quality metric | The infrastructure budget |
| Gains from routing | Meets a cost directive | **Nothing** | The saving |
| Can stop it | No | **Yes, unilaterally** | Only by not signing |
| Opens CAMIR | Twice a month | Never — reads his own traces | Once a quarter, one page |

**The structural fact this day is built on:** the saving accrues to Dana, the risk accrues to Ravi, and they have never met. Three of six endpoints are enforced; `account-reasoning` was disqualified in week one and still runs entirely on the 70B.

---

## 06:00 — Nobody is awake. The system works.

**06:00** · The **recalibration scheduler** wakes on its weekly trigger. No `pool_manifest` change, no drift trigger fired, so it does nothing and writes a line saying so. A scheduler that runs unnecessarily is how a router silently re-fits itself under a policy someone signed against a different curve.

**06:00–09:00** · ~180,000 requests. Per request: **tolerance policy engine** reads the endpoint's declared tolerance and owner; **pin registry** checks for a pin; **dispatcher** takes the **cascade route** — 8B first, **confidence gate** resolves or escalates; **cost meter** prices it on GPU-seconds at the declared utilisation and 4× all-in multiplier [S27]; **trace stamper** writes tier, route, confidence, escalation flag and policy version onto **the caller's own span**. **Written:** 180,000 `decision_record` rows, 180,000 stamped spans in the company's existing telemetry, and `savings_ledger` rows accumulating by endpoint and day.

Nobody looks at any of it. **This is the product working.** CAMIR's engagement metric M13 is about the hour after a question arrives — not daily active use, which for infrastructure is a symptom.

---

## 09:47 — Ravi's dashboard moves

Ravi's `structured-extraction` feature shows thumbs-down up **1.9 points** week over week. He did not get an alert from CAMIR, because CAMIR did not cause it — but he does not know that yet, and **the two weeks after a customer complains is exactly when a routed deployment dies** [S21].

His first instinct is the one that ends deployments: pin everything to the 70B and figure it out later. He can. It is one flag, effective on the next request, no ticket, no Marcus. **That the off-switch is genuinely his is what makes the next four minutes possible** — a person who cannot exit does not investigate, they escalate.

**09:48 · He does not open CAMIR.** He opens his own tracing UI and groups his feature's spans by `camir.tier` — an attribute the **trace stamper** wrote onto his spans, in his system, four months ago.

```
camir.tier=small   68%   thumbs-down 4.1%   (last week: 4.0%)
camir.tier=large   32%   thumbs-down 4.3%   (last week: 2.4%)   ←
camir.policy_version=7   unchanged since 2026-06-14
camir.escalated=true → thumbs-down 4.3%
```

**09:51 · The finding is the opposite of the suspicion.** The regression is concentrated in requests that went to the **large** tier — the ones CAMIR did *not* route down. Whatever moved, it moved on the 70B path. Routing is exonerated in **four minutes**, from his own telemetry, using data that would survive CAMIR being uninstalled (O2).

**09:58 · He finds it.** His team shipped a prompt-template change on Friday that pushed long inputs past the context budget on the 70B path. Not the router.

> **The counterfactual, stated plainly.** Without the tier attribute on his spans, Ravi's Tuesday is: pin to large at 09:48 (which does not fix it, because the problem *is* the large path), spend two days convinced routing is the cause, escalate to Marcus, and leave the pin in place forever out of caution. The deployment would be dead by Thursday and the actual bug would still be shipping. **A per-request attribute nobody looks at 364 days a year is the reason the product survives the 365th.** It is why per-request tier attribution is P0 in [../PRD.md](../PRD.md) G2, ranked above classifier accuracy.

---

## 10:31 — Marcus finds out, from Ravi

Marcus learns about all of this in a message from Ravi that reads *"false alarm, it was our template, tier attribution made it quick."* He did not investigate anything. He was not paged.

**10:35** · He checks two numbers he actually watches, in about ninety seconds:

- **Escalation rate**, 24% on `structured-extraction`. Steady. This is the number that decides whether the cascade is still cheaper than going straight to the large tier (PR1) — if it drifts toward 60% the cascade becomes a more expensive way to serve identical traffic, and the failed-small-attempt cost is real money that no dashboard showing "savings" would reveal.
- **Pin-to-large rate**, 0.6%, up from 0.4%. **Rising is a product failure and it is on the front page as one** (M11). Two pins this month, both from a team onboarding a new endpoint, both released after their shadow period. Fine — but it is visible, and a product that hid this number would be hiding its own adoption failing.

**10:40** · The **drift monitor** shows classifier calibration error creeping on one endpoint whose traffic mix shifted. Not a breach. It schedules a recalibration for Sunday rather than acting now, and tells him. He closes the tab. **Total CAMIR time today: about six minutes.**

---

## 14:00 — The threat that is not a bug

The **drift monitor**'s escalation-rate anomaly detector flags `support-answering`: escalation up from 22% to 41% over ninety minutes, concentrated in requests from one API key.

This is O5, and it is a security control wearing an economics costume. Semantics-preserving input perturbations can suppress small-tier confidence and force escalation — a **cascade deferral attack** that inflates the victim's inference bill without degrading any output [S9]. Nothing looks broken. Quality is fine. The bill is being attacked.

The alert goes to Marcus with the key, the request signature and the cost delta. It turns out to be a customer's misconfigured batch retry, not an attacker — which is the common case and the reason the detector has to distinguish them rather than page on either. **The point is that escalation rate is monitored as an adversarial surface at all**, which follows from PR1: if cascade cost is set by first-stage resolution, then first-stage resolution is the thing an adversary attacks.

---

## 16:20 — Dana, one page, once a quarter

Dana is preparing for a budget review. She opens the **savings report** — the only CAMIR surface she has ever seen — and it is one page:

```
Pool baseline cost, month 4        $50,100        (fixed-model counterfactual)
Actual cost                        $41,800
Measured saving                    16.6%
Endpoints enforced                 3 of 6   (1 disqualified by CAMIR in week 1)
Measured quality delta             -0.2%    against declared tolerances
Inter-judge agreement              0.79     (field baseline ~0.76 [S33])
Tolerance owners who signed        R. Menon · S. Idris · M. Bell
Counterfactual computed by         open-source code, on your hardware, from your records
```

`(assumption: illustrative; no CAMIR measurement exists. [G2] records that no routing savings have been published for a self-hosted open-weight pool, and RouteLLM's headline 3.66× is MT-Bench, collapsing to 1.41× on MMLU [S2])`

Her question has never changed: *"what did it cost us before, what does it cost now, and who checked that nothing got worse — I need the third one in writing."* The third line is a **schema constraint**, not a report section: `tolerance_policy.owner` is required and non-null (O3), so the report cannot be produced without a named human, and the named humans are not Marcus and are not CAMIR.

The line she rereads is **"1 disqualified by CAMIR in week 1."** A vendor that removed her most expensive endpoint from its own scope is a vendor whose remaining number she believes. That single row does more for renewal than the 16.6% does.

**16:26** · She closes it. Next time she opens CAMIR is in three months.

---

## The day in one table

| Time | Person | Component | Record written | What would have happened without it |
|---|---|---|---|---|
| 06:00 | — | recalibration scheduler | no-op line | A silent re-fit under a policy signed against a different curve |
| 06:00–24:00 | — | dispatcher · confidence gate · cost meter · trace stamper | 180k `decision_record`, stamped spans, `savings_ledger` | — |
| 09:47 | Ravi | *(none — his own telemetry)* | — | Two days of suspicion, a permanent pin, the real bug still shipping |
| 09:51 | Ravi | trace stamper's output, read in **his** tool | — | The off-switch becomes an escalation instead of an investigation |
| 10:35 | Marcus | escalation-rate accounting · pin registry | — | Cascade drifts to a more expensive way to serve the same traffic, invisibly (PR1) |
| 10:40 | Marcus | drift monitor | recalibration scheduled | Calibration error compounds until it shows up as a quality complaint |
| 14:00 | Marcus | drift monitor (anomaly) | alert | A bill inflated by forced escalation, indistinguishable from growth [S9] |
| 16:20 | Dana | savings report | — | An unsigned saving, which is a claim, not a record |

---

## What this day says about the product

1. **Total human time across three people: about twenty minutes.** For infrastructure, engagement is a cost. The metric that matters is M13 — time from a quality question to an attributed answer — and today it was four minutes.
2. **The most valuable component fired 180,000 times and was read once.** Per-request attribution has no daily value and decisive annual value. It is the first thing a roadmap under pressure deprioritises and the reason deployments die.
3. **Two of the three people never opened CAMIR.** Ravi used his own telemetry; Dana read one page. Only Marcus touched the product, for six minutes. **Any UX decision that assumes Ravi will open a CAMIR dashboard is a decision that CAMIR gets uninstalled in month four**, and [../ux_spec.md](../ux_spec.md) is written against that constraint.

---

## Recommended next 3

1. **Treat `camir.tier` on the caller's span as the highest-priority integration in the product, above the control-plane UI.** It is the component that resolved 09:47, it is the only CAMIR output that survives CAMIR's removal, and it is the mechanism by which the person who can veto the deployment answers his own question without entering a sales conversation.
2. **Ship the escalation-rate anomaly detector with the confidence gate, not as a later security feature.** [S9] makes forced deferral a cost-inflation attack that looks exactly like organic growth, and PR1 makes escalation rate the variable that governs whether the cascade saves anything at all. One detector serves both, and shipping the gate without it means the first real incident is discovered on an invoice.
3. **Rehearse this Tuesday as the first design partner's success criterion**, ahead of the saving. Ask the partner's Ravi to answer "was it the router?" from his own tooling, under time pressure, with CAMIR's control plane deliberately switched off. If he cannot, the four Ravi features are not sufficient and [../../validation/riskiest_assumptions.md](../../validation/riskiest_assumptions.md) has a new top row.
