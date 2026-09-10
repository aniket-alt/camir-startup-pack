# Journey — Edge-high · Wen Xu · "I'll bring my own judge. Publish your agreement numbers."

**What this is** — the power-user journey: an ML infrastructure lead with five tiers, an internal judge, an evaluation team and a bespoke router she already built once, using CAMIR by replacing four of its components and keeping the harness. It is the journey where every interface in the architecture is actually exercised rather than asserted.
**Why it exists** — Wen is the persona whose objection is *true*: she could build this in three weeks, and she has. A product that answers her by claiming superiority loses, because [S4] falsifies that claim in one citation. The pack needs a written answer to "why not build it" that survives someone who genuinely can — and if this journey cannot produce one, the edge-high segment is not addressable and the technical-reference strategy in [../../strategy/gtm.md](../../strategy/gtm.md) collapses.
**How to read it** — read §Act II (the three interfaces she swaps) and §Act IV (the build-versus-buy answer) and skip the rest if you are short. A skeptic should attack Act IV: it is an argument about maintenance capacity, not capability, and it is either honest or it is a rationalisation.
**Depends on / feeds** — depends on [../../strategy/personas.md](../../strategy/personas.md) P3, [../PRD.md](../PRD.md) §3 and §5.3, [../../research/survey.md](../../research/survey.md), [../../tech/whitepaper.md](../../tech/whitepaper.md); feeds [../ux_spec.md](../ux_spec.md), [../../tech/architecture/00_INDEX.md](../../tech/architecture/00_INDEX.md), [../../strategy/gtm.md](../../strategy/gtm.md) and [../../narrative/vc_memo.md](../../narrative/vc_memo.md).

---

## The profile

**Wen Xu.** ML infrastructure lead, high-volume consumer AI product. An evaluation team, an internal quality bar with numbers attached, and a GPU fleet she has to justify quarterly.

**Her pool is five tiers, not three:** a 3B, an 8B, a 32B, a 70B, and a **fine-tuned domain specialist** that is not comparable to any of them on a single competence ordering. This alone breaks most published routing work, which assumes a strong/weak pair [S1].

**Her history with the category.** She tried RouteLLM and abandoned it: two tiers were not enough, and the published numbers are MT-Bench numbers that did not reproduce on her traffic — which is exactly what [S2] predicts, since 3.66× on MT-Bench collapses to 1.41× on MMLU and 1.49× on GSM8K.

**Her trigger.** Not a saving. A *methodology she can audit* — a judging protocol that survives the 2026 reliability findings [S33][S34] and a cost axis derived for self-hosted pools rather than borrowed from hosted list prices [S7].

**Her opening line.** *"I'll plug in my own judge and my own tiers. What I want from you is the harness and the inter-judge agreement numbers. If you don't publish judge agreement, your frontier is noise."*

---

## Act I — The audit, before the install · Day 1

Wen does not run CAMIR first. She reads the code, and she reads it in a specific order that the repository layout has to anticipate:

1. **The judge harness.** She is checking one thing: is the temperature pinned at 0, is answer position fixed or permuted-and-averaged, is verbosity normalised, and is exact-match preferred where the task admits it. If any of those is a configurable default rather than a protocol, she closes the tab — because a harness that lets the user set temperature 1 will be used at temperature 1, and same-verdict reliability falls from >95% to ~70% [S34].
2. **The cost meter.** She is checking whether cost is GPU-seconds × amortised rate ÷ utilisation with the all-in multiplier declared, or whether it is a hosted price list with self-hosted numbers pasted in. [S7] does the latter and she knows it.
3. **The counterfactual.** She is checking that the savings computation is in the **open** half. If the vendor is the only party who can compute the number, it is the OpenRouter incentive shape with different branding [S17][S20].

She finds one thing she does not like: the default corpus stratification samples by endpoint and length band, and her traffic's difficulty is not correlated with length. She notes it. It becomes her first upstream patch on day 9.

---

## Act II — Three swaps and a registry · Days 2–4

**Swap 1 · the judge interface.** Her internal judge — an ensemble with a human-labelled calibration set her eval team maintains — plugs in behind the same interface as the shipped judges. CAMIR computes **her** inter-judge agreement the same way it computes its own, and attaches it to every `frontier_run`. This is the whole reason she is here: not that CAMIR's judge is better, but that **her judge's agreement statistic is computed by code she did not write and can be handed to a skeptic** [S13]. Her ensemble comes back at 0.86 against the ~0.76 field baseline [S33] — a number she has never had, because she had no neutral harness to produce it.

**Swap 2 · the tier registry.** Five tiers in the `pool_manifest`, each with weights, hardware, resident VRAM and a per-tier utilisation figure. The specialist is the interesting entry: it shares a GPU with the 8B through multi-LoRA, so its **resident cost is adapter-sized, not model-sized** [S31], and the manifest records that rather than double-counting a card. Distinct base models do *not* share weights this way, and the manifest distinguishes the two cases — a detail that is invisible to a three-tier deployment and load-bearing for hers.

**Swap 3 · the cost model.** Her fleet is reserved-instance, not on-demand, with a committed-use discount and a measured 63% utilisation. She replaces the cost-model implementation, not a parameter. The `frontier_run` records which cost model produced the axis, so a curve of hers is never silently comparable to a curve of Marcus's — the incomparability [S13] complains of, closed by construction rather than by convention.

**Not swapped: the frontier builder, the artifact guard, the oracle ceiling probe.** She keeps these deliberately. They are the parts she would have had to write and would have written worse, and the artifact guard is the part she did not know she needed.

---

## Act III — Where she is genuinely stretched · Days 5–8

**The non-nested finding.** Her ceiling probe runs 40,000 requests × 5 tiers = 200,000 generations over three nights. The **non-nested tier report** comes back large: on 8.7% of requests a *smaller* tier was correct where a *larger* one failed, and on her specialist the figure is 21% against the 70B. Tiers are not a competence ordering (PR10), so routing on her pool is a genuine per-request assignment problem — and this is the first hard evidence she has ever had that her bespoke threshold-on-one-axis router was structurally wrong, not merely undertrained.

**The co-failure ceiling bites.** Her oracle ceiling is *lower* than she expected, because her models co-fail: across 67 frontier models, failures overlap, which bounds what any combination strategy can achieve [S15]. Her intuition had been "five tiers means more headroom than three." It does not, and the probe says so with a number.

**The artifact decomposition, on a pool with an eval team.** This is the beat that decides whether CAMIR's technical thesis is real or a story told to people who cannot check it. Wen's team already controls generation budgets and parsing carefully — so if the guarded-versus-unguarded gap is near zero on her pool, the claim in [../../tech/whitepaper.md](../../tech/whitepaper.md) is true only of sloppy harnesses.

Her gap comes back at **6 points**, against Marcus's 14 and Priya's 19. It is not zero and it is not 31. The honest reading, which the report states rather than spins: **the artifact share shrinks as harness quality rises, which is exactly what [S5] implies and is the correct shape for a real effect.** A finding that did not shrink for a team with an eval function would have been suspicious. Six points on 40,000 requests is still money and is still something her team did not find on its own.

**The classifier ablation, honestly reported.** She has the training data to try the classifier route properly. Against **calibrated confidence** — not against a fixed model (PR3, [S8]) — her trained classifier gains **1.4 percentage points** on ceiling recovery. That is inside the band [S4] reports across 21 methods, where the best remedies bought up to 2.13 points. **CAMIR reports this as a null-ish result and keeps the cascade primary.** Wen believes the rest of the pack more because of it.

**The one place CAMIR is behind her.** She asks about routing on **prefill activations** [S10] — the internal-state signal that self-hosting makes available and hosted routers structurally cannot reach. CAMIR does not have it; it is on the roadmap as the one credible escape from the predictability bottleneck (PR5). She is now interested in the roadmap for a reason that is not commercial, which is how a technical reference is actually recruited.

---

## Act IV — Why she does not just build it · Day 9

Her objection stands and **the answer is not to deny it**: she could build this in three weeks. She *did*, once. Her bespoke router is still running, maintained by nobody, calibrated against a benchmark from last year.

The answer has three parts, and only the third is durable:

| Part | The argument | How strong |
|---|---|---|
| 1. Build cost | Three weeks of her team's time | **Weak.** She has three weeks |
| 2. Coverage | The artifact guard, the co-failure framing and the non-nested report are things she did not build and did not know to build | **Moderate.** Real, but she knows them now — this argument is spent on first contact |
| 3. **Maintenance capacity** | The frontier is a dated measurement that decays on every pool change, model upgrade and traffic shift. Her last router died of exactly this: it was correct when written and wrong six months later, and nobody owned the re-measurement. **The scarce resource is not capability, it is the standing obligation to re-run.** [S29] means the tier landscape moves every quarter | **The real answer.** It is why this is a subscription and not a script |

**What she buys** is not the router — the router is open and she has it either way. She buys per-deployment classifier training and gate fitting, tolerance policy management across her endpoint owners, attribution she can defend in a capacity review, and a party whose job is to notice when the curve moves. See the open-core boundary in [../PRD.md](../PRD.md) §5.5.

**Her actual first contribution is not money.** On day 9 she opens a pull request replacing the corpus stratifier with difficulty-band sampling, and on day 30 she gives a conference talk containing the sentence *"we measured our own oracle ceiling and 6 points of it was our harness."* That sentence shortens Marcus's evaluation by two weeks, which is the entire strategic value of the edge-high segment — a value that survives her never buying anything.

---

## Components that fired, in order

`judge interface` (**her judge**) → `pool manifest / tier registry` (**5 tiers, multi-LoRA resident cost**) → cost model (**her implementation**) → `replay corpus builder` → `oracle ceiling probe` → `artifact guard` → `judge harness` (**hers, agreement computed by CAMIR**) → `non-nested tier report` → `frontier builder` → `difficulty classifier` → `classifier route` (**ablation vs calibrated confidence, reported null-ish**) → `confidence gate` → `cascade route` (**kept primary**) → `cost meter` → `tolerance policy engine` (**per endpoint owner**) → `savings attributor` → `drift monitor` → `recalibration scheduler` → `frontier diff`.

**Every interface in the architecture is exercised by this journey.** That is what makes it the integration test for the design: if any component here cannot be replaced, the "one system, six people" claim in [../PRD.md](../PRD.md) §2 is false and CAMIR is three products.

---

## What would have lost her

Ranked, because each is a real product decision that could still be taken wrongly:

1. **A judge with a configurable temperature.** One line in a config file, and the frontier becomes a coin flip [S34]. She checks this in the first ten minutes.
2. **A closed savings computation.** If she cannot re-derive the number, she is being told her own bill by the party billing her [S17][S20].
3. **A claim of router superiority.** She has read [S4]. A landing page saying "our router is smarter" ends the evaluation before the harness is ever run — the reason N7 in the PRD is the strongest renunciation in the pack.
4. **A hosted-price cost axis.** Her fleet is reserved-instance at 63% utilisation. A curve priced on hosted list prices is not wrong for her, it is meaningless for her [S7].
5. **A feature that moved from open to paid.** Once. Ever. [S23] is the post-mortem.

---

## Recommended next 3

1. **Make the judge interface and the cost-model interface the first two extension points documented**, ahead of the router itself. They are what Wen audits in her first ten minutes, and the repository's ordering is the first credibility signal she receives — before any measurement runs.
2. **Publish CAMIR's own inter-judge agreement on the project's front page**, alongside the field's ~76% baseline [S33]. It is a number no competitor publishes, it costs nothing to disclose, and it converts the pack's judging protocol from a claim into an artifact a skeptic can check.
3. **Write the maintenance-capacity argument into the sales narrative explicitly**, because it is the only one of the three build-versus-buy answers that does not expire on first contact. [../../strategy/positioning.md](../../strategy/positioning.md) and [../../narrative/vc_memo.md](../../narrative/vc_memo.md) should carry it in the same words used here: the scarce resource is the standing obligation to re-measure, not the ability to write the router.
