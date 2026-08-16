# DeepLeaf Agronomist-1 — Specification

**Status: specification. The artefact is not yet released.**

Published 7 August 2026. Model, dataset and training code follow in late November 2026.

This document states what we are building, what licences it will carry, and what it will and
will not do. The evaluation protocol is published separately and was fixed before any results
existed — see [EVAL_PROTOCOL.md](EVAL_PROTOCOL.md).

Nothing here is a benchmark claim. Where a result would go, it says *will be reported*.

---

## What it is

**DeepLeaf Agronomist-1** is the first openly released member of the **Agronomist-X family**: an
**agentic vision-language model for agriculture**.

It interprets field imagery, reasons over structured field context — satellite indices,
hyperlocal weather, soil profiles, sensor and robotics telemetry — invokes narrow-AI models and
tools, and returns a grounded agronomic assessment with its evidence and its confidence.

It is an orchestrator. Given a photograph alone it will still assess plant
health; given field context and tools it composes a substantially better answer, and that
composition is the capability being released.

| | |
|---|---|
| Artefact | `deepleaf-agronomist-1-4B` |
| Family | Agronomist-X |
| Task | Agentic multimodal agronomic reasoning with tool use |
| Type | Fine-tuned agentic vision-language model |
| Inputs | Field imagery, natural-language query, structured field context, MCP tool results |
| Outputs | Grounded assessment, MCP tool calls, cited evidence, calibrated confidence |
| Published size | 4B, fixed. 2B and 8B are run as published reference points, not as alternative releases — see [EVAL_PROTOCOL.md](EVAL_PROTOCOL.md) §8 |
| Weights licence | Apache-2.0 |
| Code licence | Apache-2.0 |
| Dataset licence | CC-BY-4.0 |
| Serving | Self-hostable. No call back to DeepLeaf. |
| Target release | Late November 2026 |
| Owner | DeepLeaf, Co. |

## What is in the release, and what is not

### How it orchestrates

The model is an orchestrator. Through agentic tool use it calls narrow, task-specific models —
a phenotyping YOLO, a disease detection API, anything you expose over a **CLI** or an **HTTP
API** — and reaches data sources over **MCP**. It decides which to call, composes what comes
back, and cites it.

**It works with any harness, because the schema is open.** The schema is the interface contract:
how tools are presented to the model, how it emits a call, and how it returns an assessment with
its evidence and confidence. It is published with the model. Read it and you can build your own
harness, your own flow engine, your own MCP servers and your own tools.

DeepLeaf runs it with our own harness and flow engine — versioned flows per scenario with intent
routing — reaching our Data Hub over our own MCP servers and agentic tools. **Those are
proprietary and are not in this release.** They are one implementation of a published contract.

### Retrieval over a living reference library

Agronomist-1 does not have to be retrained to recognise something new. Given a field photograph
it retrieves: it searches a curated reference library by visual similarity — symptom imagery,
expert-verified condition descriptions, host-specific notes — and reasons over what comes back,
citing it.

Subagents handle the decomposition. One retrieves visual references, one checks field context,
one composes the assessment.

Two consequences:

- **New crops and new conditions are added to the library, not trained into the weights.**
  Coverage grows by curation. That is faster than a training run and an agronomist can audit
  what changed.
- **New tasks come from new references and new tools.** Phenotyping, yield estimation and
  similar assessments are added by attaching a narrow model plus the reference material needed
  to interpret its output, rather than by producing a new checkpoint.

Fine-tuning stays supported, and the training code and data are published for it. It is the
fallback rather than the default route.

### What is released

**Released, all under open licences:**

- The agentic VLM — weights, architecture, training and inference code
- The **schema** — the documented interface contract, sufficient to build your own runtime
- The training, validation and test dataset

**Not part of this model, and not in this release.** These are separate components of the
DeepLeaf platform, reached over the interfaces above:

- The realtime TFLite capture-quality model that checks distance and focus on device
- Climate-based early forecasting of pest and disease pressure
- The DeepLeaf Data Hub itself — satellite, hyperlocal weather, soil
- Proprietary narrow-AI models
- Robotics integrations
- The national pesticide registry, and the legal dose, interval and compliance layer

It reasons over the context reached through MCP; it does not ship the services that produce it.

## Product, dose and compliance

Agronomist-X handles product selection, legal dose, pre-harvest and re-entry intervals,
maximum-application limits and regulatory compliance. In DeepLeaf's commercial deployment the
model orchestrates them by querying an authoritative national pesticide registry, enforcing the
legal concentration and seasonal limits, with an agronomist in the loop.

Those capabilities are **orchestrated, not memorised**. They depend on an authoritative source
being connected, and no registry ships with this open release.

The model's behaviour is therefore conditional, by design:

| Registry tool connected | Behaviour |
|---|---|
| Yes | Composes a recommendation from what the registry returns, and cites it |
| No | Declines, and says why, rather than answering from parametric memory |

**The failure we guard against is not producing a dose. It is producing an *ungrounded* dose** —
one invented rather than looked up. An invented dose is dangerous in a way an invented crop name
is not, which is why it carries a disqualifying threshold in
[EVAL_PROTOCOL.md](EVAL_PROTOCOL.md) §10.

If you are building farmer-facing advice, connect a registry for your jurisdiction and keep an
agronomist in the loop. MCP exists for exactly that. See
[RESPONSIBLE_USE.md](RESPONSIBLE_USE.md).

---

## Base model decision

**`Qwen3-VL-4B-Instruct`, Apache-2.0.**

The release is 4B. DeepLeaf committed to a small open model that anyone can run on their own
hardware, and 4B is the size that keeps that promise while leaving enough capacity for
multi-turn tool use.

There is a tension here. Agentic reliability — choosing the right tool, forming valid arguments,
staying grounded across turns — degrades faster as models shrink than perception accuracy does,
and that pressure points toward 8B. We accept a measured cost at 4B in exchange for a model that
runs on modest hardware, and publish 2B and 8B reference numbers so the size of that cost is
visible. See [EVAL_PROTOCOL.md](EVAL_PROTOCOL.md) §8.

### Why a 2025 model rather than the newest one

Our compute is a fixed allocation on NVIDIA A100 (sm80) and the release date is fixed at late
November 2026. Under those two constraints the correct base model is the one with no unresolved
engineering questions, not the one at the top of a leaderboard.

`Qwen3-VL` uses conventional attention with a standard vision encoder. That means:

- **A100 is proven.** No exotic kernels. FlashAttention-2 works on sm80.
- **The tooling is settled.** ms-swift supports it including Megatron-parallel SFT, DPO and
  GRPO; LLaMA-Factory supports it; Unsloth supports it including vision RL; there is a
  dedicated community fine-tuning repository; and multiple independent step-by-step
  fine-tuning walkthroughs have been published against it.
- **Thousands of people have already fine-tuned it.** Failure modes are known and searchable.
- **Sizes fit the need.** Dense 2B, 4B, 8B and 32B, so scaling is a config change.

The newer alternatives each carry an unresolved risk against these constraints:

| Alternative | Why it is not the primary choice |
|---|---|
| `Qwen3.5` (Feb 2026) | 3:1 hybrid stack with Gated DeltaNet linear attention. A100 kernel support is unverified for our setup, and there is an open LLaMA-Factory defect where the Unsloth booster fails during LoRA setup for `model_type=qwen3_5` vision models. Strong model; wrong time to be first. |
| `Gemma 4 E2B / E4B` | Per-Layer Embeddings inherited from Gemma 3n plus MatFormer-style nesting. Unconventional at exactly the layer a fine-tune touches. |
| `Gemma 4 26B-A4B / 31B` | Conventional enough, but too large for a "small" published artefact and heavy to full-fine-tune within one A100 node. |
| `North-Micro-Vision-2.4B` | Weeks old, vLLM support still pending, and an OCR/document-heavy training curriculum that is a poor match for fine-grained natural images. |

The performance we give up is concentrated in OCR, document understanding, charts and agentic
behaviour. None of that is relevant to identifying a lesion on a leaf. The reliability we gain
is directly relevant to shipping in November on the hardware we actually hold.

`Qwen3.5` is recorded as the candidate to revisit for `Agronomist-2`, by which point its A100
support and tooling will be settled or we will be on newer hardware.

### Why licence is the deciding criterion

The DPG Standard requires model parameters under terms conformant to the Open Source
Definition. A derivative inherits its base model's terms, so a base model with use
restrictions makes the derivative ineligible for DPG recognition regardless of how the
fine-tune is licensed.

### Shortlist considered

| Base model | Sizes | Licence | Eligible | Assessment |
|---|---|---|---|---|
| **Qwen3-VL** | 2B, 4B, 8B, 32B dense; 30B-A3B, 235B-A22B | Apache-2.0 | Yes | **Selected.** Conventional attention, A100-proven, most mature fine-tuning tooling of any open VLM. |
| Qwen3.5 | 0.8B–397B, dense and MoE | Apache-2.0 | Yes | Better benchmarks, unresolved A100 and tooling risk. Revisit for `Agronomist-2`. |
| Gemma 4 | E2B, E4B, 26B-A4B, 31B | Apache-2.0 | Yes | Relicensed to Apache-2.0 April 2026. E-variants use PLE and MatFormer nesting; larger variants too big for a small artefact. |
| North-Micro-Vision | 2.4B | Apache-2.0 | Yes | Weeks old, vLLM pending, OCR/document-biased curriculum. |
| InternVL3.5 | 1B–241B-A28B | Verify per checkpoint | Probably | Strong on fine-grained perception; second choice if Qwen3-VL disappoints. |
| Ministral-3-3B, SmolVLM2, Phi-3.5-vision | 1.6–4.2B | Apache-2.0 / MIT | Yes | Viable small baselines, weaker than Qwen3-VL at equal size. |
| LFM2.5-VL | 1.6B | LFM v1.0 | **No** | Not an open licence. Fails the weights requirement. |
| Any Llama-derived VLM | — | Llama Community | **No** | Use restrictions. |
| Gemma 3 and earlier | — | Gemma Terms | **No** | Use restrictions. Gemma 4 onwards is Apache-2.0. |

### Why the published leaderboards were not the deciding input

This model needs two capabilities that standard benchmarks measure unevenly.

**Agentic tool use** is measured, and it is directly relevant. Progress on tool-calling and
instruction following through 2026 transfers to this task, so those benchmarks carry real
signal for base-model selection.

**Fine-grained agronomic perception** is not measured anywhere. No benchmark in the standard
VLM suites evaluates plant-health assessment, and OCR, chart and document scores — which
dominate compact-VLM leaderboards — say little about discriminating lesion texture, chlorosis
pattern or leaf-margin shape. General VQA and MMMU are weak predictors here.

What we expect to matter, and what the bake-off therefore measures on our own data:

- Reliability of tool selection and argument construction under realistic field context
- Grounding: whether the model's assessment actually follows from the data it was given
- The vision encoder's fine-grained representation quality on natural images
- Native or high resolution handling, because early-stage lesions are small and a fixed
  low-resolution square crop discards the symptom
- Robustness to real capture conditions rather than to clean imagery

### Selection is confirmed empirically

The base model family is a starting hypothesis, and the pre-registered bake-off in
[EVAL_PROTOCOL.md](EVAL_PROTOCOL.md) §8 can overturn it. Results are published in full, including
for candidates we reject.

What that bake-off settles is the **training approach** at 4B — LoRA rank and target modules,
whether the vision tower stays frozen, and resolution. It also produces reference numbers we
publish alongside the release:

- `Qwen3-VL-2B` and `Qwen3-VL-8B`, run identically, so the cost of shipping 4B is a published
  number rather than a claim. These are reference points, not alternative releases.
- A vision-only classifier baseline, measured on perception only. It cannot orchestrate, so it
  exists to establish whether the perception task needs a VLM at all and to bound how much of
  any headline number is perception rather than orchestration.

### Contingency

If `Qwen3-VL-4B` underperforms our published failure thresholds, the fallback is
`InternVL3.5` at a comparable size — also conventional architecture, also well supported,
historically strong on fine-grained perception. Licence must be confirmed per checkpoint
before use.

We change the base model before we change the size. If 4B cannot meet the thresholds on any
suitable base, that becomes a published finding and a revised date.

There is no open engineering question on the selected path.

---

## Agronomic review

The label taxonomy, condition definitions and diagnostic correctness are reviewed by
**Dr Brahim Ezzahiri, Head of Agronomy at DeepLeaf, Co.**, a plant pathologist with a research
career in plant disease science.

This is a gate. Engineering does not overrule agronomy on what a disease is, and
a taxonomy change requires agronomic sign-off — see [GOVERNANCE.md](GOVERNANCE.md). A model that
learns a confidently wrong label is worse than one that learns nothing, and the only defence
against that is a qualified human deciding what the labels mean.

## Coverage

Target crops and conditions: [COVERAGE.md](COVERAGE.md).

DeepLeaf's commercial platform covers 83 crops fully and a further 24 partially. **The open
model covers a subset of those**, because it is trained only on openly licensed imagery and a
class enters the released label set only if it clears the support threshold in
[EVAL_PROTOCOL.md](EVAL_PROTOCOL.md) §3.

We are not publishing a crop count before the corpus is assembled. The achieved coverage — which
crops, which conditions, and the support count for each — is published at release.

"Fully covered" means the crop's principal diseases, pests and nutrient deficiencies are in the
released label set and each clears the minimum support threshold. "Partially covered" means the
crop is recognised and some conditions are supported, but the condition set is incomplete —
treat those crops as screening only.

The model will return a confident-looking answer for a crop it has never seen. Integrators
must enforce the supported crop list in their own application.

For a crop or condition outside the released label set, the first route is retrieval rather than
retraining: add reference imagery and condition descriptions to the reference library and the
model can work from them. Fine-tuning remains available, and the training code and data are
published for it. See *Retrieval over a living reference library* above.

## Training data

Specification: [DATA_STATEMENT.md](DATA_STATEMENT.md).

The training, validation and test set will be published under CC-BY-4.0 with per-image
provenance, because the DPG Standard requires an AI system's training data to be openly
licensed and available. Preparing a dataset that meets that bar takes time, and it is one of the
two reasons the artefacts follow in November rather than accompanying this specification.

No image enters the corpus without a verified licence recorded at its primary source.

## Relationship to DeepLeaf's commercial platform

Two different artefacts, deliberately.

DeepLeaf's commercial Agronomist-X deployment is trained on 2.7 million field images across 83
crops and over 1,000 crop-specific conditions, and it orchestrates the full platform: the
proprietary narrow-AI models, the enhanced satellite pipeline, hyperlocal weather and soil
services, on-field robotics, and a national pesticide registry with legal dose and
safety-interval logic.

`Agronomist-1` is the orchestrator core of that same family, trained only on openly licensed
data so the whole artefact can be given away, and released so that others attach their own
models and data over MCP. It will cover fewer crops and conditions and
will perform less well than the commercial deployment, and it arrives with no tools attached.
That is the trade for being open end to end.

Do not compare their numbers. Different corpora, different tool access, different purpose.

---

## Intended use

- An orchestrator core for an agricultural AI system, with your own narrow models, satellite,
  weather, soil or robotics data attached over MCP
- Reasoning over multi-source field context to produce a grounded assessment a human then acts on
- Screening field imagery to flag likely problems for human follow-up
- A component inside a larger advisory product that adds the agronomic and regulatory layer
- Self-hosted deployment where imagery and field data must stay on an organisation's own systems
- Tooling for extension officers, cooperatives and agronomists
- Agricultural research: disease surveillance, incidence mapping, phenotyping support
- Extending coverage by curating reference material for local crops, cultivars and conditions
- Fine-tuning for local crops, cultivars, conditions and locally available data sources, where
  curation is not enough

Intended users: developers and agritech companies, agricultural enterprises, agronomists and
extension officers, cooperatives, research institutes, and government agricultural services.

## Out of scope

- Autonomous treatment decisions with no human review
- Pesticide recommendations or doses without a national registry layer
- Regulatory, certification or residue-compliance determinations
- Automated decisions on credit, insurance, subsidy or land use affecting a farmer
- Crops or conditions outside the published coverage list
- Any deployment where a wrong output causes harm that nobody checks for

---

## Limitations

**Limitations are measured and published, not predicted.**

Every axis in [EVAL_PROTOCOL.md](EVAL_PROTOCOL.md) produces a number, and all of them are
published at release, including per-class tables, per-stratum breakdowns, and robustness curves
under degraded imagery. Where a stratum falls materially below the overall figure it is named
here, in this section.

What *is* known now, because it follows from the shape of this release rather than from any
result:

- **No pesticide registry is included.** Product, dose and safety intervals are orchestrated
  from an authoritative source, and no such source ships here. Connect one for your
  jurisdiction. See the section above.
- **Coverage is bounded by the released label set.** The model will answer for a crop or
  condition outside that set, and the answer will not be reliable. Enforce the list in
  [COVERAGE.md](COVERAGE.md) in your own application.
- **Smaller than the commercial deployment.** Fewer parameters, and trained on a different
  corpus. Do not transfer numbers between the two.
- **Laboratory confirmation remains the standard for some pathogens.** Visual assessment,
  human or machine, has a ceiling where the differential diagnosis requires culture or assay.

## Uncertainty behaviour

The model will return a calibrated confidence with every assessment and will **abstain** rather
than force an answer below a published threshold. It also declines to produce a product, dose or
safety interval when no authoritative registry is connected, rather than inventing one.

Abstention is a correct output, not a failure. Applications should surface it as "unclear, ask
a human". An interface that hides abstention and shows only an answer has removed the safety
mechanism.

---

## Results

**Will be reported at release**, against the protocol pre-registered in
[EVAL_PROTOCOL.md](EVAL_PROTOCOL.md), including:

- Overall metrics
- Full per-class perception tables, including classes where the model performs badly
- Tool-use results reported separately for seen and novel tool schemas
- Grounding, scope-discipline and injection-resistance results, including a non-zero injection
  success rate if that is what we measure
- Per-stratum breakdowns by crop, region, image-quality tier, growth stage and device class
- Calibration error and abstention rate by stratum
- The full bake-off results for every base-model candidate, including rejected ones

No results are published in this document because none exist yet.

---

## Licensing

| Component | Licence |
|---|---|
| Code | Apache-2.0 |
| Model architecture | Apache-2.0 |
| Model weights | Apache-2.0 |
| Training dataset | CC-BY-4.0 |
| Documentation | CC-BY-4.0 |
| `DeepLeaf` name and logo | Trademarks, not licensed. See [NOTICE](NOTICE). |

No use-based restrictions on any component. Responsible-use expectations are published as
non-binding guidance in [RESPONSIBLE_USE.md](RESPONSIBLE_USE.md) specifically so they do not
constrain the licence grant. Full reasoning in [LICENSING.md](LICENSING.md).

## DPG status

Not yet nominated. The DPG Standard requires the training data, training code and weights to
be published before an AI system can be reviewed, so the nomination will be filed when the
artefacts are released in November 2026.

We will not describe this model as a Digital Public Good until the DPG Registry lists it.

## Contact

- Technical questions and issues: https://github.com/DeepLeafCo/deepleaf-agronomist-1/issues
- Agronomic corrections: hello@deepleaf.io
- Anything with a safety dimension: hello@deepleaf.io
