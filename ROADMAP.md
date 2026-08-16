# Roadmap

Two dates. What is published now, what follows, and what could move.

---

## Now — specification

Published in this repository:

- [SPEC.md](SPEC.md) — what is being built, the base model and the reasoning behind it, intended
  and out-of-scope use, and the constraints that are known before any results exist
- [EVAL_PROTOCOL.md](EVAL_PROTOCOL.md) — how it will be measured, fixed before any training run
- [COVERAGE.md](COVERAGE.md) — target crops and conditions
- [DATA_STATEMENT.md](DATA_STATEMENT.md) — what the dataset will contain and how it is licensed
- [LICENSING.md](LICENSING.md) — the licence decision and why
- [RESPONSIBLE_USE.md](RESPONSIBLE_USE.md) — known failure modes, prompt injection, deployment obligations
- Governance, contribution terms, conduct and security policy

Not published: model weights, training or inference code, the dataset, any evaluation result.

## November 2026 — artefacts

- Model weights, in safetensors
- Training, evaluation and inference code
- The schema — the documented interface contract, enough to build your own harness
- The training, validation and test dataset under CC-BY-4.0
- Full results against the protocol above, across all five axes
- Nomination filed with the Digital Public Goods Registry

---

## Why the gap

DPG recognition requires an AI system's training data to be published under an open licence,
alongside its code and weights. Two things take time: preparing a dataset that meets that bar,
and building a corpus of agronomic reasoning trajectories, which does not exist openly and has
to be constructed and reviewed.

## What could move the November date

Published in advance, so a slip is visible early.

| Risk | Effect |
|---|---|
| Consent for open publication takes longer than expected on some sources | Narrower dataset, or a later release |
| The trajectory corpus needs more agronomist review time than budgeted | Later release. This is the longest lead item. |
| Results miss the failure thresholds in the evaluation protocol | We narrow the published coverage, or delay. The criteria are fixed before the runs. |
| A licence cannot be verified at primary source for a material data source | That source is excluded, reducing coverage |

If the date moves, we will say so here and say why, early.

## Beyond the first release

Not commitments, and deliberately not dated:

- Additional crops and conditions, prioritised by what people building on this actually ask for.
  These are added to the reference library by curation first — see the retrieval section of
  [SPEC.md](SPEC.md) — with fine-tuning as the fallback route.
- Reference-library material for local hosts and conditions, and guidance on curating your own
- Fine-tuning recipes and adapters for local crops and languages
- `Agronomist-2`, revisiting the base-model choice once newer architectures are proven on the
  hardware available to us

If you need a crop, a condition, a language or a deployment target that is missing, open a
Discussion. That is how it gets prioritised.
