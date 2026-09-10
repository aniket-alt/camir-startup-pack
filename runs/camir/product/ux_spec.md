# CAMIR — UX specification

**What this is** — the text specification of the eleven surfaces CAMIR ships: purpose, primary action, information hierarchy, empty/loading/error states, and the micro-interactions that carry the product's stance. Three are CLI, four are control-plane web, two are artifacts CAMIR emits into somebody else's tool, and two are documents.
**Why it exists** — CAMIR's three deciding people mostly never open it: Ravi answers his questions in his own tracing UI, Dana reads one page a quarter, and Marcus opens the control plane twice a month. A spec written the usual way — a dashboard as the product's centre — designs for a user who does not exist and quietly moves the veto-holder's attribution into a surface he will never visit, which is the exact failure in [journeys/day_in_life.md](journeys/day_in_life.md) at 09:47. This document fixes the opposite hierarchy: **the highest-priority surface is a span attribute in a tool CAMIR does not own.**
**How to read it** — S1 and S2 are the surfaces that decide adoption; S9 is the one that decides survival. Screens are ranked by consequence, not by navigation order. A skeptic should attack §Cross-cutting rule 3 — refusing to show a saving before the ceiling is measured is a conversion cost taken deliberately, and it is the most arguable decision here.
**Depends on / feeds** — depends on [PRD.md](PRD.md) §5, [features_flagship.md](features_flagship.md), [journeys/](journeys/) (all four), [../strategy/personas.md](../strategy/personas.md); feeds [../visuals/visual_manifest.md](../visuals/visual_manifest.md) (collages are phase 8, not here), [../tech/architecture/00_INDEX.md](../tech/architecture/00_INDEX.md) and [../narrative/pitch_deck.md](../narrative/pitch_deck.md).

---

## The surface inventory, ranked by consequence

| # | Surface | Form | Primary persona | If it is wrong |
|---|---|---|---|---|
| **S9** | **Tier decision on the caller's trace** | span attributes, in *their* tool | P5 Ravi | The deployment dies in month four |
| **S1** | Qualify — first run | CLI | P2 Marcus, P1 Priya | Nobody reaches a frontier; the tool is a router like every other |
| **S2** | The frontier report | web, single page | P2 Marcus | The saving is a claim, not a measurement |
| **S3** | Disqualification report | web, single page | P2 Marcus | CAMIR becomes a vendor doing the customer's math [S17] |
| **S4** | Endpoint tolerance panel | web | P5 Ravi, P2 Marcus | The risk-bearer does not own the risk; veto stays free |
| **S5** | Shadow comparison | web | P5 Ravi | Enforcement is a leap of faith |
| **S6** | Live routing view | web | P2 Marcus | Cascade economics drift invisibly (PR1) |
| **S7** | Frontier diff | web | P2 Marcus, P3 Wen | A pool upgrade silently invalidates a signed policy |
| **S8** | Savings report | one page, exportable | P4 Dana | The buyer has nothing to sign against |
| **S10** | Pin control | CLI flag + one web toggle | P5 Ravi | The off switch is a request, not a switch |
| **S11** | Component interfaces | code + docs | P3 Wen | Edge-high is unaddressable; no technical reference |

**Two surfaces are deliberately absent.** There is no prompt browser — prompt text is opt-in and off by default (O1), and a UI that displays prompts creates pressure to store them. There is no "savings so far" widget on any screen a frontier has not been produced for; see cross-cutting rule 3.

---

## S9 · Tier decision on the caller's trace — *the highest-priority surface*

**Purpose.** Let the person who can veto the deployment answer *"was it the router?"* in his own tooling, under time pressure, without CAMIR running.

**It is not a screen.** It is six OpenTelemetry span attributes written onto the caller's existing span: `camir.tier`, `camir.route` (cascade|classifier|pinned|shadow), `camir.confidence`, `camir.escalated`, `camir.policy_version`, `camir.pool_hash`.

**Information hierarchy.** `camir.tier` first — it is the group-by that resolves the question. `camir.escalated` second. `camir.policy_version` exists so "did the policy change?" is answerable without asking anyone.

**Primary action (in Grafana/Honeycomb/Datadog, not here).** Group this feature's quality metric by `camir.tier`.

**States.** *Shadow*: attributes present with `route=shadow` and the tier CAMIR *would* have chosen — so the group-by works before any traffic is routed. *Pinned*: `route=pinned`, so a pin never masquerades as a routing decision. *CAMIR down*: the ingress proxy fails open to the baseline tier and stamps `route=fallback`. *CAMIR uninstalled*: **historical attributes remain in the customer's telemetry forever** (O2).

**Micro-interaction that carries the stance.** Attribute names are stable and documented as a compatibility surface, versioned like an API. Renaming one breaks four months of a customer's saved queries — which is the cost of this being genuinely theirs.

---

## S1 · Qualify — the first run

**Purpose.** Take a stranger from `pip install` to a measured oracle ceiling without a router existing yet, and without an account.

**Primary action.** `camir qualify --logs ./requests.jsonl`

**Information hierarchy.** (1) the pre-flight estimate; (2) live progress with the current step named; (3) the ceiling, guarded and unguarded; (4) everything else.

**The pre-flight estimate is the load-bearing element** and the thing most CLIs skip:

```
Corpus     1,200 requests (stratified: 6 endpoints × 4 length bands)
Tiers      3      →  3,600 generations
Judging    2 judges, temperature 0, position-fixed, verbosity-controlled
Estimated  47 min on your declared throughput.   Proceed? [y/N]
```

CAMIR **refuses to start a run it estimates will not finish inside a stated budget**, and offers a smaller stratified corpus instead. Priya's whole journey fits in an afternoon or does not happen ([journeys/edge_low.md](journeys/edge_low.md) §Timing), so an unbounded probe is a broken feature, not a slow one.

**States.** *Empty* — no logs found: CAMIR names the three formats it reads and does not offer to generate synthetic traffic, because a ceiling measured on invented requests is worse than no ceiling. *Loading* — progress by step, never a spinner; `[2/5] oracle ceiling probe · 1,412/3,600 generations · 22 min remaining`. *Error* — a tier that OOMs reports which tier, at what context length, and continues with the remaining tiers, marking the `frontier_run` **partial** rather than discarding an hour of work. *Degraded* — one judge unavailable: the run continues, single-judge, and every downstream artifact is stamped **inter-judge agreement unavailable — not reproducible** [S13].

**Micro-interaction.** The utilisation prompt asks *"what fraction of the hour are these cards actually generating?"* with the consequence attached inline — *"a card idle at 10% costs 10× per token [S27]"* — because a defaulted utilisation figure moves the answer more than any routing improvement will.

---

## S2 · The frontier report

**Purpose.** One page carrying the entire technical claim: your frontier is measurable on your pool, and part of the apparent ceiling is your harness.

**Information hierarchy — the order is the argument.**

1. **Oracle ceiling, guarded and unguarded, with the delta labelled** — *"19pp of your apparent ceiling was your harness."* [S5]
2. **Inter-judge agreement**, with the ~0.76 field baseline printed beside it [S33]. A frontier without it is watermarked *not reproducible*.
3. **The curve** — models as points, routes as curves, cost on the horizontal axis (PR9, [S7]).
4. **Cost at three tolerance points** — 100%, 99%, 95% of the fixed-model baseline.
5. **Non-nested tier count** — requests where a smaller tier was right and a larger wrong (PR10).
6. **Provenance footer** — corpus hash, pool manifest, cost-axis parameters, judge set, date.

**The saving is not in the first five.** It is derived on S8 from a tolerance somebody chose. Putting it at the top would make CAMIR the party asserting the number, which is the objection the whole product answers.

**States.** *Empty*: no `frontier_run` yet — this page shows the S1 command and nothing else. *Stale*: the pool manifest has changed since this run; the page is banded amber and links to S7. *Partial*: a tier failed; the affected curve segment is dashed and labelled, never interpolated.

**Micro-interaction.** The cost-axis parameters — utilisation, all-in multiplier — are **editable in place**, and the curve re-renders live. Marcus can set the multiplier to 5× and watch the saving shrink. A vendor whose number survives the customer pushing it in the pessimistic direction has a different conversation from one whose number is defended.

---

## S3 · Disqualification report

**Purpose.** Tell a prospect on day two that routing cannot help them, with the evidence.

**Primary action.** Export. This page exists to be forwarded.

**Hierarchy.** The verdict as a sentence — *"Do not deploy CAMIR on `account-reasoning`."* Then the gap: ceiling 22%, baseline, operating cost, and the arithmetic showing the gap is smaller than the cost of running the router. Then the per-request histogram. Then which endpoints, if any, are still worth routing.

**Why it is a first-class screen rather than an error state.** Only a vendor whose harness runs inside the customer's perimeter can afford to render this page, and M16 says the disqualification rate should be **non-zero** — a zero rate means the ceiling probe is not being believed. It is the surface [../strategy/positioning.md](../strategy/positioning.md) is built on.

**Micro-interaction.** No "contact us to discuss options" call to action. The page ends. The next thing it says is what to measure again after a pool change.

---

## S4 · Endpoint tolerance panel

**Purpose.** Make the person who bears the quality risk the person who sets the tolerance.

**Primary action.** Set tolerance, with your own name attached.

**Hierarchy.** Endpoint · declared tolerance · **owner (required, non-null)** · the `frontier_run` it was set against · effective from · version history, append-only.

**States.** *Unowned*: an endpoint with no named owner **cannot be enforced** — it can only run in shadow. The constraint is in the schema (O3), so the panel is reporting a database truth rather than enforcing a convention. *Breached*: banded red, with the auto-revert status and who was notified. *Stale*: the tolerance was set against a superseded `frontier_run`; the row links to S7 so the owner sees whether their point moved.

**Micro-interaction.** Changing a tolerance shows the projected escalation-rate and cost change **before** confirming, from the current frontier. Tightening from 1% to 0.5% is a cost decision as much as a quality decision, and the panel says so rather than letting it read as free caution.

---

## S5 · Shadow comparison

**Purpose.** Give an endpoint owner two weeks of evidence on their own traffic before a single user request is routed.

**Hierarchy.** Measured quality delta versus the declared tolerance, as one number with a sign. Then escalation rate. Then the **cost decomposition** — failed small attempt · gate · large answer (PR1). Then per-request disagreements, sampled, so the owner can look at the actual answers where the tiers differed.

**States.** *Insufficient data*: below a stated request count the page refuses to show a delta and says how many more are needed. *Ready*: a single enforcement action, available only to the tolerance owner.

**Micro-interaction.** Shadow is **on by default for every new endpoint** and must be actively disabled. Nobody in any journey chose it; it is the shape of the defaults, and it is why Ravi can agree to shadow — a much smaller thing to agree to than enforcement — which is what unblocks the beachhead deal.

---

## S6 · Live routing view

**Purpose.** The twice-a-month check. Two numbers, ninety seconds.

**Hierarchy.** **Escalation rate per endpoint**, trended, with the break-even line drawn — the point above which the cascade costs more than dispatching straight to the large tier (PR1). Then **pin-to-large rate**, trended, **labelled as a failure metric on the face of the chart** (M11): a product that hides rising pins is hiding its own adoption failing. Then the drift monitor's state and any scheduled recalibration.

**States.** *Anomaly*: escalation-rate spike attributed by API key and request signature — the forced-deferral detector, which is a cost-inflation attack that looks exactly like organic growth [S9] (O5). *Drifting*: calibration error rising, with the scheduled recalibration date, and the explicit statement that CAMIR **will not re-fit under a signed policy without saying so**.

---

## S7 · Frontier diff

**Purpose.** Answer "did my point move?" after a pool upgrade or a traffic shift.

**Hierarchy.** Old curve and new curve overlaid, with **every tolerance point projected onto both** and named by owner. Then per-endpoint ceiling change. Then what triggered the re-run.

**Why it exists as its own surface.** The frontier is a dated measurement that decays on every pool change [S29]. This screen is the visible form of the argument in [journeys/edge_high.md](journeys/edge_high.md) §Act IV: the scarce resource is the standing obligation to re-measure, not the ability to write a router. It is the subscription, drawn.

**Micro-interaction.** Owners whose point moved materially are notified individually. Nobody has to notice.

---

## S8 · Savings report

**Purpose.** The one page the economic buyer ever sees, once a quarter.

**Hierarchy — seven lines, in this order.** Baseline (fixed-model counterfactual) · actual · measured saving · endpoints enforced, **including those CAMIR disqualified** · measured quality delta versus declared tolerances · inter-judge agreement with the field baseline · **the named humans who signed the tolerances**.

**Why "disqualified" is on the buyer's page.** In [journeys/day_in_life.md](journeys/day_in_life.md) it is the line Dana rereads. A vendor that removed her most expensive endpoint from its own scope is a vendor whose remaining number she believes, and that row does more for renewal than the percentage does.

**States.** *Unsigned*: any enforced endpoint without a named tolerance owner blocks the report from rendering — it cannot exist without the "who checked" field, because that is the thing she needs in writing (O3).

**Micro-interaction.** A footer naming the open-source module and commit that computed the counterfactual, so an auditor who is not CAMIR can re-run it (G3, O6).

---

## S10 · Pin control

**Purpose.** An off switch the risk-bearer genuinely owns.

**Primary action.** `camir pin <endpoint> --tier large` — or one toggle on S4. Effective on the **next request**, no ticket, no approval, no Marcus.

**States.** Pinning writes an audit row, notifies the platform owner, and increments the pin-to-large rate on S6. **Nothing about pinning is discouraged in the interface.** A person who cannot exit does not investigate, they escalate — which is the difference between Ravi's four-minute Tuesday and a dead deployment.

---

## S11 · Component interfaces

**Purpose.** Let the edge-high user replace judge, tier registry, cost model and classifier while keeping the harness.

**Documentation order is the credibility signal**, and it is not the obvious one: **judge interface first, cost model second**, router last. Wen audits temperature pinning and cost derivation in her first ten minutes ([journeys/edge_high.md](journeys/edge_high.md) §Act I); a repository that leads with the router answers a question she is not asking.

**Micro-interaction.** A replaced judge gets its inter-judge agreement computed by CAMIR's code, not the plug-in's. The whole value of bringing your own judge is that somebody else's harness produced the agreement number.

---

## Cross-cutting rules

1. **Every number on every surface carries its provenance inline** — corpus hash, judge set, cost-axis parameters — or it is not shown. A frontier without inter-judge agreement is watermarked *not reproducible* [S13].
2. **No surface displays prompt text.** Prompt storage is opt-in and off by default (O1); a UI that shows prompts creates the pressure to store them, and P1's contract forbids it [S28].
3. **No screen shows a saving before a ceiling has been measured.** The sequence is ceiling → tolerance → saving, and it never runs backwards. This costs conversions — a "you could save 40%" landing state converts better than "measure first" — and it is taken deliberately: the saving is only meaningful against a tolerance somebody chose, and CAMIR asserting it first re-creates the incentive problem it exists to fix [S17][S20].
4. **Failure states name the component.** "The confidence gate could not fit a threshold: 340 labelled examples, minimum 1,000" — not "something went wrong."
5. **CAMIR never claims router superiority in copy.** [S4] falsifies it in one citation (N7). The claim in every empty state, header and export is the measurable one: *your frontier is measurable on your pool, and part of the ceiling is your harness.*

---

## Recommended next 3

1. **Build S9 and S1 first, and nothing else until both work.** S1 is how anyone reaches a frontier and S9 is how the deployment survives month four. The control-plane screens S2–S8 are readable as CLI output and static exports for the first three design partners; the span attributes are not, and they are the surface CAMIR does not own.
2. **Prototype S2's editable cost axis before the first customer demo.** Letting Marcus push the all-in multiplier to 5× and watching the saving shrink is the single strongest trust interaction in the spec [S27], and it is the one that most needs to be seen working rather than described.
3. **Usability-test S3 (disqualification) on a real prospect who does not qualify.** It is the surface no competitor can render, M16 requires its rate to be non-zero, and it has never been shown to anyone. If a disqualified prospect reads it as a sales tactic rather than a finding, the positioning in [../strategy/positioning.md](../strategy/positioning.md) needs rewriting before the product does.
