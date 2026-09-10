# CAMIR — Architecture index

**What this is** — the map of the ten architecture diagrams (D01–D10), what each one is the authority on, and the reading order for three different skeptics: the engineer who will build it, the security reviewer who will approve it, and the investor checking whether there is a system here at all.
**Why it exists** — CAMIR's product is a *measurement* system that happens to route, and the natural reading of "LLM router" is a proxy with an if-statement in it. Ten diagrams of a proxy would be ten pictures of the same box. These ten are chosen so that the parts a reviewer would otherwise assume — where the ceiling is computed, where the counterfactual is computed, which side of the open/paid line each component sits on, and whose perimeter each arrow crosses — are each drawn explicitly, because every one of those is a place a competitor's architecture quietly differs and no diagram admits it.
**How to read it** — D02 first, always: it is the loop the whole product is. Then D01 if you are asking *does this work*, D06 if you are asking *can we approve this*, D04 if you are asking *is there a real system*. Each diagram's caption ends with what a reviewer should notice, and that line is the diagram's actual claim.
**Depends on / feeds** — depends on [../../product/PRD.md](../../product/PRD.md) §1 and §5, [../deep_dives.md](../deep_dives.md), [../whitepaper.md](../whitepaper.md); feeds [../not_vaporware.md](../not_vaporware.md), [../techniques/decision_tree.md](../techniques/decision_tree.md), [../../visuals/visual_manifest.md](../../visuals/visual_manifest.md) and [../../narrative/pitch_deck.md](../../narrative/pitch_deck.md).

**Status: no component in these diagrams has been built.** These are design documents, not as-builts.

---

## The ten

| # | Diagram | The authority on | The claim it makes |
|---|---|---|---|
| [D01](D01.md) | **Qualify pipeline** — logs to first frontier | How a ceiling is produced before any routing exists | Measurement precedes routing, and can end in *do not deploy* |
| [D02](D02.md) | **The closed loop** — Classify → Dispatch → Judge → Attribute → Recalibrate | The product's core cycle and where it closes | The loop closes on `judgment_record` labels, per deployment, never network-wide |
| [D03](D03.md) | **Request path orchestration** | What fires, in what order, on one live request | The tolerance and pin are read *before* any model is chosen |
| [D04](D04.md) | **Durable records schema** | What is written and what it is keyed by | `tolerance_policy.owner` is a non-null column, not a report field |
| [D05](D05.md) | **Model pool, tier registry and cost metering** | Where cost per request actually comes from | The cost axis is derived, declared and versioned — not borrowed from a hosted price list |
| [D06](D06.md) | **Perimeter, privacy and the open/paid boundary** | Which arrows cross which trust boundary | Prompts never leave; the counterfactual is computed by open code inside the customer's perimeter |
| [D07](D07.md) | **Integrations and ecosystem** | Where CAMIR sits in a stack that already exists | CAMIR ships as a routing strategy inside LiteLLM, above vLLM/SGLang, not as a rival proxy |
| [D08](D08.md) | **Observability, drift and the breach path** | How a quality question becomes an attributed answer | Attribution lands on the caller's own span and survives CAMIR's removal |
| [D09](D09.md) | **Multi-endpoint scale and isolation** | How six teams share one router without sharing one tolerance | Endpoints are the isolation unit; a pin is per-endpoint and unilateral |
| [D10](D10.md) | **Human-in-the-loop and escalation** | Where humans are required, not merely permitted | Enforcement is blocked without a named tolerance owner |

---

## Reading paths

**The engineer who will build it.** D02 → D03 → D04 → D01 → D05. Stop at D05 if the cost derivation does not convince you; everything downstream is priced by it.

**The security or platform reviewer.** D06 → D04 → D09 → D10. D06 answers *does customer text leave*, D04 answers *what is retained*, D09 answers *can one team's policy affect another's*, D10 answers *who authorised the enforcement*.

**The investor or technical due-diligence reader.** D02 → D01 → D08. D02 shows there is a system rather than a script, D01 shows the disqualification path that no share-of-savings competitor can render, and D08 shows the failure mode that kills deployments and the component that prevents it.

**The prospective contributor** ([../../strategy/personas.md](../../strategy/personas.md) P3, P6). D06 for the open/paid line, then D05 and D08 for the two interfaces most worth replacing.

---

## Conventions used in all ten

| Convention | Meaning |
|---|---|
| **Solid arrow** | Synchronous call on the request path |
| **Dashed arrow** | Asynchronous, batch, or scheduled |
| **Subgraph `Customer perimeter`** | Inside the customer's network. Prompt text never crosses out of it |
| **`open` / `paid`** node suffix | Which side of the open-core boundary the component sits on ([../../product/PRD.md](../../product/PRD.md) §5.5) |
| **Cylinder node** | A durable record from D04 |
| Component names | Exactly the names fixed in [../../product/PRD.md](../../product/PRD.md) §1. No diagram invents a component |

Diagrams are **Mermaid source in markdown**. They need no renderer to be complete and no build step to be current; GitHub, most IDEs and the pack's own site render them live. That is why the visual layer does not duplicate them as HTML posters — see the A50 contract in [../../../../references/artifact-manifest.md](../../../../references/artifact-manifest.md).

---

## What these diagrams deliberately do not show

1. **A serving engine.** vLLM and SGLang exist, and routing is being absorbed into the serving stack itself [S11]. CAMIR sits above them and D07 draws that boundary rather than hiding it — it is also the sharpest structural threat to the company, and it is on the diagram rather than in a footnote.
2. **A cache.** N3. Caching composes upstream, removes 20–45% of production traffic and adversely selects the remainder toward the hard end [S36]. D01 shows the corpus drawn from post-cache traffic and says so.
3. **A training pipeline for the models themselves.** N1. CAMIR measures the pool; changing the pool is the customer's call and triggers a re-measurement, which is D02's `pool_manifest` edge.
4. **A multi-tenant SaaS control plane.** N4. D09 is multi-*endpoint*, not multi-*customer*; the control plane runs in the customer's VPC.

---

## Recommended next 3

1. **Validate every Mermaid fence by rendering it before this set is shown to anyone.** A diagram that does not parse is invisible in every reader that matters, and these ten carry the entire "this is a system, not a wrapper" argument — a silent parse failure is indistinguishable from an empty tech layer.
2. **Build D01 first and D03 second.** D01 is the qualify pipeline, which is the only path that returns a negative result cheaply and produces the artifact decomposition before any routing code exists. D03 is the request path, and its ordering — tolerance and pin read before tier selection — is the part that must be right on day one because it cannot be retrofitted after a deployment has been pinned.
3. **Put D06 in the first security review, unprompted.** The perimeter question decides whether P1 can use CAMIR at all [S28], and a reviewer who has to ask where prompts go has already priced the risk higher than the diagram would have.
