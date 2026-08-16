![DeepLeaf Agronomist-1, of the Agronomist-X family: an open agentic vision-language model for
agriculture. Weights and code under Apache-2.0, training data under CC-BY-4.0. Specification
published; model and dataset November 2026.](assets/agronomist-1-social.jpg)

# DeepLeaf Agronomist-1

**An open agentic vision-language model for agriculture. Specification published now,
artefacts in November 2026.**

[![Licence: Apache 2.0](https://img.shields.io/badge/licence-Apache--2.0-blue.svg)](LICENSE)
[![Data: CC BY 4.0](https://img.shields.io/badge/data-CC--BY--4.0-blue.svg)](LICENSING.md)

**DeepLeaf Agronomist-1** is the first openly released member of the **Agronomist-X family**: an
agentic vision-language model for agriculture. It interprets field imagery,
reasons over structured field context — satellite indices, hyperlocal weather, soil
profiles, sensor and robotics telemetry — invokes narrow-AI models and tools, and returns
a grounded agronomic assessment with its evidence and its confidence.

It is an orchestrator. Through agentic tool use it calls narrow task-specific
models — a phenotyping YOLO, a disease detection API, anything exposed over a **CLI** or **HTTP
API** — and reaches data sources over **MCP**.

**It works with any harness, because the schema is open.** The interface contract is published
with the model, so you can build your own harness, flow engine, MCP servers and tools. DeepLeaf's
own harness, flow engine, MCP servers, agentic tools and Data Hub are proprietary and are not
part of this release.

**Coverage grows by curation, not by retraining.** It searches a curated reference library by
visual similarity and reasons over what comes back, so a new crop or condition is added as
reference material rather than trained into the weights. See [SPEC.md](SPEC.md).

**The model and dataset are not released yet.** This repository publishes the specification,
the licences, the governance, and the evaluation protocol, before any results exist.

| | |
|---|---|
| Specification | [SPEC.md](SPEC.md) |
| Evaluation protocol, pre-registered | [EVAL_PROTOCOL.md](EVAL_PROTOCOL.md) |
| Licence decision and reasoning | [LICENSING.md](LICENSING.md) |
| Responsible use | [RESPONSIBLE_USE.md](RESPONSIBLE_USE.md) |
| Target release | Late November 2026 |
| Be notified | https://deepleaf.io/agronomist-1 |

---

## Why publish a specification before a model

Because the evaluation protocol should be fixed before the results exist.

[EVAL_PROTOCOL.md](EVAL_PROTOCOL.md) states how we will measure — the split strategy, the
primary metric, the reporting strata, the abstention threshold, and what we will count as
failure. Once published it is frozen, and every amendment is logged with a date and a reason.

The model, the dataset and the training code are in preparation and follow in November 2026.

## What will be released

| Component | Licence | When |
|---|---|---|
| Model weights | Apache-2.0 | Nov 2026 |
| Schema — the interface contract | Apache-2.0 | Nov 2026 |
| Training and inference code | Apache-2.0 | Nov 2026 |
| Training, validation and test dataset | CC-BY-4.0 | Nov 2026 |
| Reference library, retrieved at inference time | CC-BY-4.0 | Nov 2026 |
| Documentation | CC-BY-4.0 | Now, and updated at release |

Base model: `Qwen/Qwen3-VL-4B-Instruct`, Apache-2.0. The reasoning, including why a proven
model was chosen over a newer one, is in [SPEC.md](SPEC.md).

## What is not in the release

Separate components of the DeepLeaf platform, which this model reaches rather than contains:
the realtime TFLite capture-quality model, climate-based early forecasting of pest and disease
pressure, the DeepLeaf Data Hub itself, proprietary narrow-AI models, robotics integrations, and
the national pesticide registry with its dose and safety-interval layer.

## Product, dose and compliance

Agronomist-X orchestrates product selection, legal dose, safety intervals and regulatory
compliance by querying an authoritative pesticide registry. The registry is not part of this
open release.

So the behaviour is conditional: with a registry tool connected the model composes a
recommendation from what the registry returns and cites it; with none connected it declines
rather than answering from memory.

What we guard against is not a dose, but an **ungrounded** dose. See [SPEC.md](SPEC.md).

If you are building something farmer-facing, the diagnosis is one input; the pesticide registry,
the legal check and the agronomist review are yours to add.
See [RESPONSIBLE_USE.md](RESPONSIBLE_USE.md).

## No results in this repository

There are none yet. Where a number would go, the specification says *will be reported*.

At release we will publish results across all five axes of the protocol — perception, tool use,
grounding, scope discipline and injection resistance — with the full per-class table, tool-use
scores broken out for tools the model never saw in training, and the measured injection success
rate.

Limitations are measured and published, not predicted. What is already known — because it
follows from the shape of this release rather than from a training run — is in
[SPEC.md](SPEC.md).

## DPG status

Not yet nominated. The Standard requires published artefacts before review can begin, so the
nomination will be filed when the model and dataset are released.

We will not describe this model as a Digital Public Good until the DPG Registry lists it.

## What would help most right now

Comments on the split strategy, the metric choice and the failure criteria in the evaluation
protocol go in this repository's Discussions.

We are looking for institutions willing to evaluate the model on their own crops at release, and
for openly licensed field imagery for crops we do not cover — imagery goes into the reference
library as well as the training corpus.

## Governance and contributing

[GOVERNANCE.md](GOVERNANCE.md) · [CONTRIBUTING.md](CONTRIBUTING.md) ·
[CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) · [SECURITY.md](SECURITY.md)

Contributions are inbound-equals-outbound: Apache-2.0 for code, CC-BY-4.0 for data and
documentation. No copyright assignment.

## Ownership

Copyright in the model, code and dataset is held by **DeepLeaf, Co.**, incorporated in
Delaware, United States (2024).

DeepLeaf operates through three entities:

| Entity | Jurisdiction | Since |
|---|---|---|
| DeepLeaf, Co. | Delaware, United States | 2024 |
| STE DeepLeaf SARLAU | Sidi Bennour, Morocco | 2023 |
| DeepLeaf LLC | Qatar Financial Centre, Qatar | 2025 |

Copyright in a work vests automatically on creation and requires no registration, so the
copyright statement in [LICENSE](LICENSE), [NOTICE](NOTICE) and this file constitutes the
ownership documentation.

The `DeepLeaf` name and logo are trademarks of DeepLeaf, Co., licensed separately from the
copyright grant.

See [NOTICE](NOTICE).

## Contact

- Discussion: this repository's Discussions tab
- Agronomic questions: hello@deepleaf.io
- Anything with a safety dimension: hello@deepleaf.io
