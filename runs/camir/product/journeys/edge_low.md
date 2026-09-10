# Journey — Edge-low · Priya Raghunathan · "one afternoon, one port change"

**What this is** — the end-to-end narration of the least-supported CAMIR user: one engineer, two GPUs, no evaluation team, no budget authority, and four hours before she has to answer her director. Every beat names the component that fires and the durable record it writes.
**Why it exists** — CAMIR's defaults are the whole product for this persona, and a router whose safe configuration takes a week to reach has already lost her: she will conclude "everything goes to the 70B" is the cheap decision because it is the *safe* one. This journey is the specification for what has to be true out of the box, and it is where the pack proves the disqualification behaviour is real — Priya's run is the one that could legitimately end in *do not deploy*.
**How to read it** — read Act III first if you are a skeptic; it is where the number arrives and where the temptation to overclaim lives. Attack the wall-clock budget in §Timing: if the oracle ceiling probe cannot finish on two GPUs in an afternoon, G1 in [../PRD.md](../PRD.md) is false and this journey is fiction.
**Depends on / feeds** — depends on [../../strategy/personas.md](../../strategy/personas.md) P1, [../PRD.md](../PRD.md) §5.0–5.4, [../features_flagship.md](../features_flagship.md); feeds [../ux_spec.md](../ux_spec.md), [day_in_life.md](day_in_life.md), [../../validation/mvp_definition.md](../../validation/mvp_definition.md) and [../../narrative/one_pager.md](../../narrative/one_pager.md).

---

## The profile

**Priya Raghunathan.** Senior backend engineer, 60-person B2B SaaS, ~$6k/month of GPU rental. Two open-weight models resident: an 8B and a 70B, served by vLLM behind an internal HTTP wrapper. She self-hosts because her largest customer's contract forbids sending customer text to a third-party API — **not to save money** [S28].

**Session goal.** Her director asked on Tuesday whether the document-summarisation feature can go into the free tier. That needs the per-request cost to fall by roughly half. She has Thursday afternoon.

**What she will not do.** Train a classifier. Read a paper. Book a demo. Configure a judge. If any step requires one of those, she closes the tab and reports "we can't."

**Her starting state.** Every request goes to the 70B, because she picked it once, six months ago, and one hand-written rule sends anything over 2,000 characters to it as well — which is a no-op, since everything already goes there. She has never measured the 8B on her traffic. Nobody has.

---

## Act I — Qualify · 13:40 to 14:25 · *before any routing exists*

**Beat 1 · 13:40 — she does not install a router.** She installs the harness. `pip install camir && camir init` writes a `pool_manifest` skeleton and prints one question: *where are your logged requests?* No account, no key, no network call. This ordering is deliberate and is the product's first argument: **CAMIR measures before it routes**, so the first thing it can tell her is whether routing is worth doing at all (PR7).

**Beat 2 · 13:44 — the replay corpus builder** reads 30 days of her request logs from a JSONL path, strips nothing, and draws a stratified sample by endpoint and prompt-length band. It stops at 1,200 requests, tells her why (*"1,200 gives ±3pp on the ceiling estimate at your traffic's variance"*), hashes the sample, and writes `corpus_2026-09-10_a41f9c.jsonl`. **Written:** the corpus hash that will pin every later comparison.

> She never uploads it. The file stays on the box her logs already sit on. The `pool_manifest` records two tiers — `small: llama-3.1-8b`, `large: llama-3.1-70b` — with the hardware and the utilisation figure CAMIR asks her for rather than assumes: *"what fraction of the hour are these cards actually generating?"* She guesses 25%. The guess is stored as a **declared parameter**, not a hidden constant, because it moves the answer more than any routing decision will [S27].

**Beat 3 · 13:52 — the oracle ceiling probe** runs every one of the 1,200 requests through **both** tiers. Not the router — both tiers, exhaustively. This is the expensive step and the only one that produces a defensible ceiling: with perfect foreknowledge of which tier answers correctly, what would the cost be? On her two cards it takes 31 minutes.

**Beat 4 · 13:53 — the artifact guard is on by default**, and this is the beat most reviewers skim. It sets a generous generation budget and logs every truncation, parses outputs strictly and counts every parse failure, and normalises for length before any judgment. It runs the ceiling **twice** — with the guard and without — because [S5] found truncation in 65% of MMLU cases and 5–12% parse failures on MMLU alone, and Priya's 8B is exactly the tier that gets truncated and then blamed for being incapable (PR6).

**Beat 5 · 14:24 — the judge harness** scores the 2,400 answers under the forced protocol: temperature 0 (same-verdict >95%, versus ~70% at temperature 1 [S34]), fixed answer position, verbosity-controlled, and **two judges**, because ~76% inter-judge agreement means one judge is a coin with a bias, not a measurement [S33]. Her summarisation task admits no exact-match check, so both judges are model judges and CAMIR says so on the report. **Written:** 2,400 `judgment_record` rows, each carrying both verdicts, the agreement flag, a truncation flag, and parse status.

**Beat 6 · 14:25 — the frontier builder** writes `frontier_run/2026-09-10T14-25`, pinning corpus hash, pool manifest, cost-axis parameters, judge set and inter-judge agreement. Nothing has been routed. No request from a real user has been touched.

---

## Act II — The number, and the fork · 14:25 to 14:32

She opens one page. It has four lines above the curve, and the first two are the ones that matter.

```
Oracle ceiling            68% of requests resolvable at the small tier within tolerance
  ├─ with artifact guard  68%
  └─ without guard        49%      ← 19pp of your apparent ceiling was your harness
Inter-judge agreement     0.81 (field baseline ~0.76 [S33])
Cost at 99% quality       $0.0031/req   (baseline $0.0074)
```

**The artifact line is the contribution.** Nineteen points of what looked like "the 8B can't do this" was truncation and parse failure, not capability [S5]. Nobody has published this decomposition on a self-hosted pool [G2], and Priya got it in forty-five minutes without knowing it was a research contribution — which is the correct way for it to reach her.

**The fork.** If the ceiling had come back at, say, 11%, the **disqualification report** fires instead of the frontier: *"the gap between your ceiling and your current baseline is smaller than the cost of operating this. Do not deploy CAMIR. Here is the histogram."* This is only sayable because the harness is open and ran inside her perimeter — a vendor billing a share of her bill cannot afford to say it, and that asymmetry is the positioning wedge, not a courtesy.

CAMIR also shows her the **non-nested tier report**: 34 requests where the 8B was right and the 70B wrong. Small, but it is the evidence that routing is an assignment problem, not downgrading (PR10) — and it is the line she quotes to her director on Friday.

---

## Act III — Shadow, then the port change · 14:32 to 15:10

**Beat 7 · 14:32 — she does not turn routing on.** The default is **shadow mode**, and she does not choose it; she would have to disable it. She changes one base URL in her service config to point at the **ingress proxy**, an OpenAI-compatible endpoint. Every request still goes to the 70B. The router computes what it *would* have done and logs it.

**Beat 8 · 14:33 onward — per request, in shadow:** the **tolerance policy engine** reads the endpoint's tolerance (the shipped default: 1% measured quality drop versus the fixed-model baseline, conservative on purpose); the **pin registry** finds no pin; the **dispatcher** takes the **cascade route** — small tier first, **confidence gate** resolves or escalates — because the cascade is primary and the classifier route is the ablation (PR3). The gate's threshold came from her own 2,400 `judgment_record` rows twenty minutes ago. **Written:** a `decision_record` per request with the shadow tier, tokens, GPU-seconds per tier, escalation flag and the tolerance policy version.

**Beat 9 · the trace stamper** writes the tier decision, route type, confidence and escalation flag as span attributes on **her own** OpenTelemetry spans — not into a CAMIR dashboard she would have to open. If she rips CAMIR out next week, the historical attribution stays in her telemetry. That is O2 in the PRD and it is the reason her future self can answer a quality question without CAMIR's cooperation.

**Beat 10 · 15:05 — the cost meter and savings attributor** have 4,100 shadow requests. The **savings ledger** carries, per row, what the fixed-model baseline actually cost and what the cascade would have cost, priced on amortised GPU-hours per token at her declared 25% utilisation with the 3–5× all-in multiplier shown as a slider, not buried [S26][S27]. Escalation rate: 29%. The cascade's cost is decomposed in front of her — failed small attempt + gate cost + large answer — because a first stage that rarely resolves makes a cascade *more* expensive than going straight to the large tier (PR1), and she is entitled to see that before she believes the saving.

**Beat 11 · 15:10 — she flips enforcement on for one endpoint.** Not the account-reasoning one. The summarisation one. The **tolerance breach alert** is armed on the same tolerance the shadow ran under, with auto-revert enabled by default: if measured quality on that endpoint crosses 1% below baseline, traffic returns to the 70B and she gets a message. She did not configure this. It was on.

---

## Act IV — Friday, and the thing that does not happen

Friday 09:20. The **drift monitor** has 19,000 production requests. Escalation rate steady at 30%. No breach. The **label recycler** has promoted 19,000 production `judgment_record` rows into the gate's calibration set — the loop closing on her own traffic, per-deployment and never network-wide, because N4 forbids the prompts leaving (and forbidding it is what let her use this at all).

She sends her director four lines from the **savings report**: baseline cost, cost at the declared tolerance, measured quality delta, judge agreement, and her own name in the *who checked* field. The free-tier answer is yes.

**What does not happen** is the point of the journey. She never opened the tier registry, never saw a classifier, never tuned a threshold, never read a curve for longer than eight seconds. **Every step she skipped had a safe default, and every default was the conservative one.** The version of this product where the powerful configuration is the default is a product Priya deploys once, degrades something, and removes.

---

## Timing — the falsifiable part

| Step | Component | Wall clock | What breaks it |
|---|---|---|---|
| Install + corpus | replay corpus builder | 12 min | Logs not in a parseable form — the most common real blocker `(assumption)` |
| Ceiling probe, 1,200 × 2 tiers | oracle ceiling probe, artifact guard | 31 min | Corpus size scales this linearly; 5,000 requests on two cards is ~2 hours, past her budget |
| Judging, 2,400 answers, 2 judges | judge harness | 32 min (overlapped) | A third judge adds ~50%; exact-match tasks collapse it to seconds |
| Read the frontier | frontier builder | 7 min | — |
| Shadow to first enforcement | ingress proxy, dispatcher, confidence gate | 38 min | — |
| **Total** | | **≈ 1h 50m** | G1 says "under a week." This journey claims an afternoon at 1,200 requests on 2 GPUs `(assumption: no CAMIR measurement exists; derived from [S26] throughput at batch)` |

---

## Components that fired, in order

`replay corpus builder` → `pool manifest / tier registry` → `oracle ceiling probe` → `artifact guard` → `judge harness` → `frontier builder` → (`disqualification report` on the negative branch) → `non-nested tier report` → `ingress proxy` → `tolerance policy engine` → `pin registry` → `dispatcher` → `confidence gate` → `model pool` → `cost meter` → `savings attributor` → `trace stamper` → `tolerance breach alert` → `drift monitor` → `label recycler` → `savings report`.

**Never fired:** `difficulty classifier`, `classifier route`, `judge interface`, `recalibration scheduler`, `frontier diff`. Priya's whole journey runs on the cascade and the defaults. That is the design holding.

---

## Recommended next 3

1. **Make the artifact-guard delta a first-class line on the first screen she ever sees**, above the saving. It is the number no competitor can show her, it is the pack's technical thesis in one row, and it arrives before any routing risk is taken — so it is also the only credibility CAMIR has on the first afternoon.
2. **Budget the ceiling probe against her hardware, not ours.** The probe is O(corpus × tiers) full generations and it is the only step that can blow the afternoon. Ship a pre-flight estimate — *"1,200 requests × 2 tiers on your declared throughput ≈ 31 minutes"* — and refuse to start a run that will not finish inside a stated budget, offering a smaller stratified corpus instead.
3. **Test the disqualification branch first in discovery.** Priya's run ending in *do not deploy* is the highest-value thing to observe, because it is the claim in [../../strategy/positioning.md](../../strategy/positioning.md) that no incumbent can copy, and it is worthless if the ceiling probe is not believed. See [../../validation/experiment_board.md](../../validation/experiment_board.md).
