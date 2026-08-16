# Data Statement

What the training dataset will contain, where it comes from, and under what terms it is
published. This is a specification; the dataset itself follows in November 2026 with a full
dataset card.

The Digital Public Goods Standard requires an AI system's training, validation and test data to
be published under an open licence alongside its code and weights. That requirement shaped this
release.

---

## Licence

**CC-BY-4.0**, conformant to the Open Definition, permitting derivatives and commercial reuse.

Attribution string for downstream use:

```
DeepLeaf-FieldImages-Open, DeepLeaf, Co., CC-BY-4.0.
https://huggingface.co/datasets/DeepLeaf/DeepLeaf-FieldImages-Open
```

Reasoning for CC-BY-4.0 over CC0, CC-BY-SA and CC-BY-NC is in [LICENSING.md](LICENSING.md).

---

## Two components

`Agronomist-1` is an agentic orchestrator, so labelled imagery alone is insufficient to train or
evaluate it. The corpus has two parts.

| Component | Contents | Trains |
|---|---|---|
| **Imagery** | Field photographs with condition labels and per-image provenance | Perception |
| **Trajectories** | Multi-turn agronomic tasks with structured field context, tool schemas, tool calls, tool results and grounded assessments | Tool use, grounding, scope discipline, injection resistance |

### Imagery

Field photographs taken in real growing conditions — variable light, cluttered backgrounds, local
cultivars, ordinary phone cameras. Not laboratory imagery on uniform backgrounds. Publicly
available plant-disease data is overwhelmingly the latter, and models trained on it do not hold
up on a photograph a farmer actually takes.

Two sources:

1. **Openly licensed public datasets**, each verified individually at its primary source.
2. **A DeepLeaf contribution**, published only where documented consent covering open publication
   was obtained from the contributing farmer or cooperative.

### Trajectories

No open corpus of agronomic reasoning trajectories exists, so this component is constructed.
It will include:

- A published **tool schema** covering the data classes the platform orchestrates: satellite
  indices, weather series, soil profiles, sensor and robotics telemetry, knowledge lookup
- **Field-context payloads** in realistic ranges and correlations, synthetic or openly licensed,
  never customer data
- **Grounded assessments** citing the evidence they rest on
- **Negative examples**, weighted as heavily as the positives: out-of-scope requests where the
  correct behaviour is refusal; adversarial context where the correct behaviour is to flag stale
  or implausible data; **prompt-injection attempts embedded in tool results**, where the correct
  behaviour is to treat the content as data and ignore the instruction; and tasks needing no tool
  at all, so the model does not learn to call one reflexively

Which portions are synthetic, how they were generated, and the agronomist review applied are
stated explicitly in the dataset card.

---

## Licence verification

**Every source is verified at its primary source, not from a mirror or a secondary citation.**

Licence tags on dataset hosting platforms are self-declared by whoever uploaded the repository
and are frequently wrong. A re-upload tagged CC0 does not make the underlying dataset CC0.

For each source the manifest records: the primary source URL, the licence as stated there, the
SPDX identifier, the date verified, and a copy of the licence text as found. Anything that cannot
be verified at primary source is excluded.

**One consequence:** widely used plant-disease datasets sometimes carry no
licence at all at their origin, despite mirrors asserting one. Those are excluded from this
corpus regardless of how commonly they are used elsewhere. A third-party tag is not evidence of
the licence at origin, and building a DPG nomination on one would put the whole submission at
risk.

---

## Consent

Imagery contributed by DeepLeaf is published only with documented consent covering open
publication under CC-BY-4.0, including commercial reuse by third parties.

Contributors may request removal at any time. The takedown process, its timescale, and the
limit on it — copies already distributed under CC-BY-4.0 cannot be recalled, so removal applies
going forward — are documented in the dataset card.

---

## Privacy

- Every image screened for identifiable people, faces, identifying hands, vehicle plates,
  documents and signage
- All EXIF metadata stripped, including GPS, device serial numbers and owner fields
- **Precise coordinates are not published.** Provenance is recorded at coarse regional
  granularity, because precise location combined with disease status is commercially sensitive
  information about an identifiable farm
- No farmer names, phone numbers, cooperative membership identifiers or plot identifiers appear
  in the imagery or the manifest
- Field-context payloads in the trajectory component are synthetic or openly licensed, never
  derived from customer data

---

## Known limitations of the corpus

Stated in advance, because they follow from how agricultural data is collected.

- **Class imbalance.** Condition frequency reflects what appears in the field, not a balanced
  design. Per-class counts are published.
- **Label confidence has a ceiling.** Some pathogens cannot be distinguished visually without
  laboratory confirmation. The proportion that is lab-confirmed will be stated.
- **Geographic concentration** in North and West African conditions. Not globally representative.
- **Near-duplicate frames.** Multiple images of the same plant exist. Splits are made at field
  level, not image level, and the residual cross-split duplicate rate is published.
- **Synthetic field context** is realistic but not real. It cannot capture every correlation
  present in genuine sensor and satellite data.

---

## Format

Non-proprietary throughout. Imagery in a standard image format; labels, manifest and
trajectories in CSV and JSONL. Downloadable in full over plain HTTP.

Full field-level documentation, per-source provenance and per-class counts are published in the
dataset card at release.
