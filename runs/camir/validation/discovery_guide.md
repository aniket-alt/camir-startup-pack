# CAMIR — Customer discovery guide

**What this is** — the interview kit: who counts as a qualified conversation for each persona, the screening questions that establish it before time is spent, fourteen problem-interview questions that ask only about past behaviour, a separate solution-interview script that is never run in the same call, and the synthesis template the answers are coded into.
**Why it exists** — CAMIR's three kill-the-pack assumptions are A1, A2 and A3, and **only A3 is answerable by talking to people**: does a population of teams running self-hosted open-weight pools at production volume actually exist, and how large is it [G1]. There is no survey that answers it, so the number in [../strategy/market_sizing.md](../strategy/market_sizing.md) rests on Ollama download proxies [S30] — which count laptops, not production. A discovery process that drifts into pitching produces enthusiasm instead of the one number the sizing needs, and enthusiasm from an unqualified respondent is worse than no data because it is quotable.
**How to read it** — §2's screening questions decide whether the rest of the call happens; run them first, always. A skeptic should attack §3's question list for leading questions — the discipline is that a question mentioning routing, savings or CAMIR before the respondent does has failed.
**Depends on / feeds** — depends on [../strategy/personas.md](../strategy/personas.md), [riskiest_assumptions.md](riskiest_assumptions.md), [experiment_board.md](experiment_board.md) E4/E5/E13, [../BRIEF.md](../BRIEF.md); feeds [decision_making_unit.md](decision_making_unit.md), [mvp_definition.md](mvp_definition.md), [pivot_log.md](pivot_log.md) and [../strategy/market_sizing.md](../strategy/market_sizing.md).

**Status: zero interviews conducted.** Every persona in [../strategy/personas.md](../strategy/personas.md) is a composite constructed from the research layer. This guide is the instrument, not a result.

---

## 1. The three things these interviews must produce

Ranked. A round of conversations that produces only the third has failed.

| # | Output | Why it cannot come from anywhere else | Feeds |
|---|---|---|---|
| 1 | **f2 — the share of a team's production tokens running on weights they own** | [G1]: no survey publishes it with a denominator. It is the multiplier the entire TAM rests on | [../strategy/market_sizing.md](../strategy/market_sizing.md), A3 |
| 2 | **Whether the veto is real, and whether ownership defuses it** | Inferred from one public backlash [S21] and zero interviews. It is the basis for four P0 features | [decision_making_unit.md](decision_making_unit.md), E13 |
| 3 | Which framing lands — tolerance-first or savings-first | Positioning, cheap to change, and the thing everyone wants to talk about first | [../strategy/positioning.md](../strategy/positioning.md), E5 |

---

## 2. Screening — before any question about problems

**Do not skip this to be polite.** An unqualified conversation costs an hour and produces a quote that will be believed later.

**Screen for P2 Marcus (the beachhead — run the first ten calls against this profile only):**

| # | Question | Qualifies if |
|---|---|---|
| S1 | *"Walk me through where your LLM calls actually execute today."* | They name their own GPUs or a self-hosted serving stack, unprompted |
| S2 | *"Of the tokens you served last month, roughly what share ran on weights you host yourselves?"* | **This is f2.** Any answer is data; a confident zero disqualifies for the beachhead and is still recorded |
| S3 | *"What did that cost last month, and who asks you about it?"* | A named person asks. If nobody asks, there is no trigger |
| S4 | *"How many models are resident in that pool right now?"* | Two or more. One model means no tier structure to route across |
| S5 | *"Who else calls this service?"* | Other teams. If it is only their own feature, there is no Ravi, no veto, and the organisational thesis is untested by this call |

**Persona variants.** P1 Priya: S1 qualifies, S3 usually returns *"nobody asks"* — record it, since her trigger is a feature decision rather than a cost directive. P3 Wen: additionally *"do you run your own evaluation?"* and *"have you built a router before?"* — a yes to the second makes her the build-versus-buy interview. P4 Dana: screen only for budget ownership; her interview is a different script (§5). P5 Ravi: screen by finding him **through** a qualified Marcus — a consuming engineer identified any other way is not in the structure being tested. P6 Sam: below the volume floor [S27] and not a customer; his interview is about the repository and the post, not the purchase.

---

## 3. Problem interview — fourteen questions, past behaviour only

**The rules, and they are the whole method.** Never mention routing, tiers, savings, CAMIR or a product before the respondent does. Never ask *would you*. Ask about the last time, the specific instance, what they actually did and what it cost them. If a question can be answered with an opinion about the future, it is the wrong question.

**Their world, and the cost they already carry**

1. *"Tell me about the last time somebody asked you to reduce inference cost. What exactly were you asked, and by whom?"*
2. *"What did you do in the week after that? Walk me through the actual steps."*
3. *"What happened in the end — did the number move?"*
4. *"When was the last time you changed which model an endpoint uses? What triggered it?"*
5. *"How did you decide that endpoint could use the smaller model? What did you look at?"*
6. *"Has anyone on your team ever measured what your smallest model can and cannot handle on your own traffic? Show me what came out of it if so."*

*(Question 6 is the highest-value question in the set. A "no" is the market. A "yes, and here is the spreadsheet" is a competitor — the internal one, Petal 5 in [../strategy/petal_diagram.md](../strategy/petal_diagram.md), which is the primary competitor in every deal.)*

**Quality, blame and the organisation**

7. *"Tell me about the last time a quality complaint came in on an LLM feature. How long did it take to work out what caused it?"*
8. *"Who found out first, and how?"*
9. *"When you last changed something in the shared inference service, how did the teams calling it find out?"*
10. *"Has a product team ever asked you to leave their endpoint alone? What did they say?"*

*(Questions 9 and 10 test the veto without naming it. If nobody has ever objected to a shared-service change, either the structure is different from what this pack assumes or the change was invisible — both are findings.)*

**Money, evidence and the last purchase**

11. *"What is the last infrastructure tool your team paid for? Walk me through how that got approved."*
12. *"Who signed it, and what did you have to show them?"*
13. *"Is there a budget line this would come out of, or would it be new money?"*
14. *"What have you already tried and abandoned in this area?"*

**Closing, always:** *"Who else should I be talking to?"* and *"Can I come back to you when I have something to show?"* — a "yes" to the second is a weak commitment; a name given to the first is a stronger one.

---

## 4. Banned questions

These are the ones that will be asked by default, and each produces data that feels good and means nothing.

| Banned | Why it fails | Ask instead |
|---|---|---|
| *"Would you use a tool that routes requests to cheaper models?"* | Hypothetical. Everyone says yes | Q4, Q5 |
| *"How much would you pay for 30% savings?"* | Invites a number nobody will honour | Q11, Q12 |
| *"Is inference cost a problem for you?"* | Leading, and answerable by anyone | Q1, Q3 |
| *"Do you care about quality?"* | Nobody says no | Q7, Q8 |
| *"Would your team object to routing?"* | Speculation about a colleague | Q10 |
| Anything containing "CAMIR" | Turns the interview into a demo | — |

---

## 5. Solution interview — a separate call, never the same one

Run only after the problem interview, only with a respondent who scored on §7's coding, and only with something to show (see [mvp_definition.md](mvp_definition.md)).

1. Show the **disqualification report** first, not the frontier. *"This is the output where we tell you not to buy. Under what circumstances would you believe it?"*
2. Show a **frontier curve** on public open-weight models. *"What is wrong with this? What would you need changed before it applied to you?"*
3. *"What would you have to see before you would run this on your own logs?"* — then: *"Would you do that this week? What would stop you?"*
4. *"Who in your organisation would have to agree, and which of them would say no first?"*
5. The commitment ask, in ascending order: run it on their own logs → give a named endpoint's shadow report → a paid pilot. **Take the highest one they will commit to with a date.** An expression of interest without a date is a no.

**For Dana (P4), a different script entirely.** Three questions: *"Show me the last infrastructure saving you approved — what did the evidence look like?"*, *"Who checked that nothing got worse, and how did you know they had?"*, and *"What would make you not believe a savings number?"*

---

## 6. Interview discipline

- **Two people, one talking.** The second takes verbatim notes, especially of the respondent's own nouns — the vocabulary in [../HANDOFF.md](../HANDOFF.md) §2.4 was chosen by us and has to survive contact.
- **Record their words, not the gist.** *"Nobody has ever measured that"* is data. *"They agreed measurement is important"* is a summary of a pitch.
- **Silence after an answer.** The second sentence is usually the real one.
- **Never correct them.** A respondent who says routing is a solved problem is telling you what the market believes.
- **End at 45 minutes** whether or not the script is finished.

---

## 7. Synthesis template — code every interview into this, same day

```
ID / persona hypothesis / date / role / company size
QUALIFIED: yes / no        f2 (share self-hosted, Q S2): ____%
Monthly inference spend: ____   Tiers resident: ____   Other teams calling: ____

TOP PAINS, in their words              (verbatim, ranked by what they spent time on)
WORKAROUND IN PLACE                    (which of the four: one model / endpoint assignment /
                                        hand rules / a one-off A/B — or something new)
HAS ANYONE MEASURED THE CEILING?       no / partial / yes -> if yes, what did they use
TRIGGER                                (the event that made them act, if any)
MUST-HAVE LANGUAGE                     (their sentence, not ours)
THE VETO                               (has a consuming team ever blocked a change: y/n, what happened)
WHO SIGNS                              (name of role, and what evidence they required last time)
SURPRISE                               (the thing that contradicted this pack)
COMMITMENT TAKEN                       (none / run on own logs / shadow report / paid pilot) + DATE
```

**Rules for coding.** Fill "surprise" every time; an interview with no surprise was probably a pitch. **A verbatim that could have been written before the call is discarded** — it is the interviewer's sentence coming back. Recode after every fifth interview and note what changed, because the thing that changes the coding scheme is the finding.

---

## 8. Pass/fail thresholds, declared now

Set before any interview so the result cannot be reinterpreted afterwards. These are the discovery-track thresholds from [experiment_board.md](experiment_board.md) E4, E5 and E13.

| Question | Sample | Pass | Fail | Consequence of failure |
|---|---|---|---|---|
| Does the segment exist (A3) | 20 screened | **≥ 6 qualify** on S1–S5 *and* spend ≥ $50k/month — [experiment_board.md](experiment_board.md) E4's definition — with f2 recorded for every call | < 6 qualify | The beachhead is smaller than [../strategy/market_sizing.md](../strategy/market_sizing.md)'s corridor; hybrid pools move up the roadmap (A9) |
| Has anyone measured their ceiling (Q6) | 15 qualified | **≥ 12 say no** | ≥ 6 say yes with a real artifact | The internal build is further along than assumed; Petal 5 is a stronger competitor than [../strategy/petal_diagram.md](../strategy/petal_diagram.md) prices |
| Is the veto real (Q10) | 15 qualified | **≥ 7 report a consuming team blocking or constraining a shared-service change** | ≤ 2 | The four P0 features are over-weighted and the roadmap in [../product/features_prioritized.md](../product/features_prioritized.md) is wrong at the top |
| Does ownership defuse it (solution Q4) | 10 | **≥ 6 name per-endpoint tolerance ownership as sufficient** | ≤ 2 | No feature closes it; the wedge needs rework, not the product |
| Framing (E5) | 15 | tolerance-first preferred **≥ 2:1** | savings-first preferred ≥ 2:1 | Positioning flips; the product does not |

---

## Recommended next 3

1. **Run the first ten calls against P2's screen only, and treat S2 as the deliverable.** Ten conversations collapse A3 and the persona set simultaneously, and f2 is the single number the whole sizing corridor rests on — currently derived from download proxies that count laptops [S30][G1].
2. **Find Ravi through Marcus, never directly.** The veto is a structural claim about two reporting lines, and a consuming engineer sourced any other way is not the person in the structure. This is the one interview that cannot be recruited from a list, and it tests the thesis that four P0 features are built on.
3. **Show the disqualification report before the frontier in every solution interview.** It is the artifact no competitor can show, it inverts the respondent's expectation of a sales call in the first minute, and their reaction to it is a sharper read on the positioning in [../strategy/positioning.md](../strategy/positioning.md) than any question about savings will produce.
