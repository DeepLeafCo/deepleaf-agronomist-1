# Licensing Decision

**Decision: Apache-2.0 for code, architecture and weights. CC-BY-4.0 for the dataset.
No use-based restrictions anywhere.**

---

## The decision table

| Artefact | Licence | File |
|---|---|---|
| Training, evaluation, pre-processing, inference code | Apache-2.0 | `LICENSE` in the GitHub repo |
| Model architecture and definition code | Apache-2.0 | same |
| Model weights and parameters | Apache-2.0 | `LICENSE` in the HF model repo |
| Dataset: imagery and labels | CC-BY-4.0 | `LICENSE` in the HF dataset repo |
| Documentation | CC-BY-4.0 | stated in the repo README |
| Trademarks: DeepLeaf name and logo | Not licensed. Reserved. | `NOTICE` |

---

## Why Apache-2.0 and not OpenRAIL

OpenRAIL is a natural instinct for an agricultural model with real-world safety
implications. It permits free commercial use while attaching use-based restrictions, and
those restrictions propagate to derivatives. For a model that could be misapplied to
pesticide decisions, that looks responsible.

It is nonetheless the wrong choice here, for one decisive reason and two supporting ones.

**Decisive: it fails the DPG Standard.** The Standard requires model parameters to be under
terms conformant to the Open Source Definition. Use-based restrictions are incompatible
with the OSD, which is why licences like OpenRAIL are commonly described as open-weight but
restricted rather than open source. Choosing OpenRAIL means choosing not to be eligible for the
DPG Registry, and eligibility is a requirement of this release.

**Supporting: the restrictions would not do the work.** The realistic misuse here is not
malice, it is a well-intentioned integrator wiring the model into a spraying recommendation
without a pesticide registry behind it. A licence clause does not prevent that. Clear
documentation, a confidence-and-abstention mechanism, and an explicit statement that the model
returns no dose and no legal check are what prevent it.

**Supporting: restrictions block the adoption this release exists to enable.** Downstream
builders need to build commercial products on this model, adapt it, and keep full ownership and
portability of what they build. Every use-based restriction is a term their own customers and
investors have to diligence. Apache-2.0 removes that friction, and its express patent grant is a
benefit for companies building on top.

Responsible-use expectations still get written down — as `RESPONSIBLE_USE.md`, published,
prominent, and non-binding. That is the standard pattern for open models released under an
unrestricted licence.

References: [the DPG Standard on AI systems as DPGs](https://www.digitalpublicgoods.net/blog/ai-systems-as-dpgs),
[HF on OpenRAIL](https://huggingface.co/blog/open_rail). Content rephrased for compliance
with licensing restrictions.

---

## Why CC-BY-4.0 and not CC0 or CC-BY-SA

The DPG Standard accepts a range of Creative Commons licences for content and requires an
Open Definition conformant licence for data. It encourages licences permitting both
derivatives and commercial reuse.

- **CC-BY-4.0 — chosen.** Permits commercial use and derivatives, requires attribution.
  Attribution matters here: it is how the contribution stays traceable in downstream research
  and how contributors are credited. It is also the most familiar licence in agricultural and
  remote-sensing research, so it creates no friction for research institutions building on it.
- **CC0 — rejected.** Public domain dedication forfeits attribution, and attribution is how a
  dataset's provenance stays checkable once it has been recombined downstream.
- **CC-BY-SA-4.0 — rejected.** Share-alike creates compatibility problems when researchers
  combine this data with differently licensed corpora, which is the use this release exists to
  encourage. It also makes adoption harder for downstream commercial builders.
- **CC-BY-NC — rejected.** The DPG Standard does accept non-commercial licences for content,
  but this release depends on downstream builders shipping commercial products, so a
  non-commercial term would defeat the purpose.

Note the split: **Apache-2.0 for weights, CC-BY-4.0 for data.** Apache-2.0 is designed for
software and carries the patent grant that matters for model artefacts; CC-BY-4.0 is designed for
content and is what the DPG Standard and the research community expect for datasets.

---

## Where each licence lives

### GitHub repository root

```
LICENSE          Apache-2.0 full text, unmodified
NOTICE           copyright and trademark statement
RESPONSIBLE_USE.md   non-binding expectations
```

`NOTICE` content:

```
DeepLeaf Agronomist-1
Copyright 2024 DeepLeaf, Co.

This product includes software developed by DeepLeaf, Co.
Licensed under the Apache License, Version 2.0.

"DeepLeaf" and the DeepLeaf logo are trademarks of DeepLeaf, Co.
The Apache License does not grant permission to use these trademarks,
except as required for reasonable and customary use in describing the
origin of the work.

Training data for this model is published separately under
Creative Commons Attribution 4.0 International (CC-BY-4.0):
https://huggingface.co/datasets/DeepLeaf/DeepLeaf-FieldImages-Open
```

### Hugging Face model repository

`license: apache-2.0` in the model card YAML, plus a `LICENSE` file with the full text.
The card states that the licence covers the weights, and links the separately licensed
dataset.

### Hugging Face dataset repository

`license: cc-by-4.0` in the dataset card YAML, plus a `LICENSE` file.

Required attribution string for downstream use:

```
DeepLeaf-FieldImages-Open, DeepLeaf, Co., CC-BY-4.0.
https://huggingface.co/datasets/DeepLeaf/DeepLeaf-FieldImages-Open
```

---

## What this commits us to

- The Apache-2.0 text is the unmodified original. Adding a condition would make it a
  different, non-OSI licence.
- [RESPONSIBLE_USE.md](RESPONSIBLE_USE.md) is guidance, not a licence term, and cannot be read
  as a restriction on the grant.
- Every third-party data source's licence is verified individually at its primary source and
  recorded in the manifest. See [DATA_STATEMENT.md](DATA_STATEMENT.md).
- Everything in the model's lineage permits Apache-2.0 redistribution of derivatives. The base
  model choice and its licence reasoning are in [SPEC.md](SPEC.md).
- Code dependencies are licence-compatible, with no copyleft dependency in the inference path.
