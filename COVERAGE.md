# Coverage

DeepLeaf's commercial platform covers **83 crops fully and 24 further crops partially**. The
open model covers a subset of that, because it is trained only on openly licensed imagery.

**We are not publishing a crop count for the open model before the corpus is assembled.** A
class enters the released label set only if it clears the support threshold in
[EVAL_PROTOCOL.md](EVAL_PROTOCOL.md) §3, so the achieved coverage is a result, not a target. It
is published at release: which crops, which conditions, and the support count for each.

The families below are the shape of the platform's coverage, and indicate where the open
model's coverage will be drawn from.

---

## What the two levels mean

**Fully covered.** The crop's principal diseases, pests and nutrient deficiencies are in the
released label set, and each clears the minimum per-class support threshold in the evaluation
protocol. Per-class metrics are published so you can judge any individual condition yourself.

**Partially covered.** The crop is recognised and some conditions are supported, but the
condition set is incomplete. **Treat partially covered crops as screening only** — an absence of
findings does not mean an absence of disease, because the condition may simply not be in the
label set.

**Not covered.** The model will still produce a fluent, confident-sounding answer for a crop it
has never seen, and that answer will be unreliable. **Enforce the covered-crop list in your own
application.** Do not rely on the model to recognise that it is out of its depth.

---

## Crop families

Grouped as the platform organises them. The enumeration of the crops the open model actually
covers is published with the model, once the support threshold has been applied to the final
corpus.

| Family | Includes |
|---|---|
| Greenhouse and vegetables | Tomato, pepper, potato, eggplant |
| Cereals and field crops | Wheat, barley, maize, sugar beet |
| Citrus | Orange, lemon, other citrus |
| Orchard and arboriculture | Apple, pear, peach, apricot, cherry, pomegranate, almond, olive, date palm |
| Vines | Grape, including table grape |
| Root, bulb and cucurbit vegetables | Onion, garlic, carrot, cucumber, melon, watermelon |
| Forage and livestock feed | Alfalfa, forage crops |
| Tropical and export crops | Cocoa, coffee, banana, cassava, rice |

Where DeepLeaf's imagery is densest, and so where the open model's coverage is most likely to
be strongest: **tomato, sugar beet, maize, pepper, wheat, barley, alfalfa and forage.** Which of
these clears the support threshold on openly licensed data is a result, published at release.

If your crop is not listed, the first route is retrieval rather than retraining: add reference
imagery and condition descriptions for that crop to the reference library and the model can
work from them. Fine-tuning remains available, and the training code and data are published
for it.

---

## Conditions

Three families, counted per crop. The same disease on a different host is a distinct condition,
because symptoms, thresholds and management differ by crop.

**Diseases** — the largest share.
Fungal and oomycete: anthracnose, early and late blight, powdery and downy mildews, leaf spots
(Cercospora, Alternaria, Septoria), rusts, scab, smut, rice blast, banana Sigatoka, cocoa black
pod, and Fusarium, Verticillium and Panama wilts. Bacterial and viral: bacterial leaf spot and
canker, angular leaf spot, blights, mosaic and leaf-curl viruses, yellow and sterility mosaic,
swollen-shoot virus.

**Pests** — sucking and chewing: aphids, whiteflies, thrips, spider mites, mealybugs, scales,
plant- and leafhoppers; fruit and shoot borers, bollworm, armyworm, cutworm, weevils, leaf
miners, beetles and caterpillars.

**Nutrient deficiencies** — nitrogen, phosphorus, potassium, calcium, magnesium, iron,
manganese, zinc, boron, and banded chlorosis.

Per-condition support counts and per-class metrics are published at release. The list above is
the platform's condition taxonomy; which of these conditions reach the open model's label set is
determined by the support threshold. Check the support column before depending on any individual
condition.

---

## Geographic distribution

Training data is weighted toward North and West African field conditions. Expect degradation on
visually distinct cultivars, unfamiliar growing systems, and different soil and light
backgrounds.

This is a property of the corpus. If you are deploying outside that distribution, validate on
your own material before trusting it, then extend the reference library with local material —
see the retrieval section of [SPEC.md](SPEC.md) — and fine-tune if that is not enough.

---

## What coverage does not include

Coverage is about **recognition**, not advice. For every covered crop and condition, the model
still returns no product, no dose, no pre-harvest interval and no regulatory check. See
[RESPONSIBLE_USE.md](RESPONSIBLE_USE.md).

Coverage also says nothing about severity. The model identifies a condition; it does not
quantify how far it has progressed or how much yield is at risk.
