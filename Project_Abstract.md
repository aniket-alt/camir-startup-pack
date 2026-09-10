# Cost-Aware Multi-Model Inference Router (CAMIR)

**Project Abstract — CMPE 295A**

**Team**
- Abhishek Darji (abhishek.darji@sjsu.edu)
- Aniket Anil Naik (aniketanil.naik@sjsu.edu)
- Tamizh Selvan Manivannan (tamizhselvan.manivannan@sjsu.edu)

**Project Advisor:** Prof. Vijay Eranti

**September 2026**

---

## Abstract

Large Language Model (LLM) inference is increasingly deployed as a shared service
handling a wide mix of request types, from simple factual lookups to complex
multi-step reasoning. Because model capability scales with size, and larger models
carry proportionally higher compute cost and latency, production systems face a
persistent tension between response quality and the cost of serving every request
at maximum capability.

Most deployed systems route every prompt to a single fixed model, regardless of the
prompt's actual complexity, since request difficulty is not known in advance. This
is wasteful when applied uniformly to large models, since a substantial share of
real-world prompts do not require frontier-level reasoning to be answered
correctly. Conversely, routing everything to a smaller, cheaper model risks
unacceptable quality loss on the harder subset of traffic, and existing routing
research is rarely evaluated in a way that is reproducible outside large
cloud-hosted model pools.

In this project, we propose and evaluate a Cost-Aware Multi-Model Inference Router
(CAMIR) that classifies each incoming prompt by complexity and dynamically routes
it to the smallest model tier predicted to answer it correctly, using a pool of
small, medium, and large models. The system compares two routing strategies, a
feature-based classifier and a cascade/escalation controller, against fixed-model
baselines on a mixed-difficulty benchmark, producing an empirical cost-quality
frontier. The outcome will be a reproducible, low-cost demonstration that
intelligent request routing can substantially reduce aggregate inference cost while
preserving output quality within an acceptable tolerance.
