# Contributing to DeepLeaf Agronomist-1

Thanks for being here. This model exists to be adapted, corrected and built on.

`Agronomist-1` is an agentic orchestrator, so useful contributions span more than perception:
tool schemas, grounding failures, refusal behaviour and injection findings all matter as much as
label corrections.

## The most valuable contributions

In rough order of how much they help:

1. **Agronomic label corrections.** If a dataset image is mislabelled, or a condition is
   misidentified, tell us. A wrong label in the training data propagates into every
   downstream model, and agronomic expertise is the scarcest input to this project. Open an
   issue with the `data-correction` label, or write to hello@deepleaf.io.
2. **Documented failure modes.** A reproducible case where the model is confidently wrong is
   worth more than a feature. Include the image if you have the right to share it, the crop, the
   region, the assessment it gave and the correct one.
3. **Tool-use and grounding failures.** Wrong tool chosen, malformed arguments, a call to a
   tool that was not offered, or an assessment that contradicts the context it was given.
   Include your tool schema and the full trace. These are as valuable as perception errors and
   far less commonly reported.
4. **Working prompt injections.** If external content can steer the model, report it privately
   via `SECURITY.md`, not in a public issue.
5. **Field validation on new conditions.** Ran it on your crops in your country and measured
   what happened? Publish it. Even a negative result is useful, and we will link it.
6. **Consented open imagery and reference material.** Field photographs you hold publication
   rights to, with documented consent, especially for under-represented crops and regions, and
   expert-verified condition descriptions for them. Both the reference library and the training
   corpus draw on these.
7. **Deployment work.** Export targets, quantisation, serving efficiency, integration
   examples. Anything that lowers the hardware bar widens who can host this.
8. **Localisation.** Condition names in local languages, including Darija, Arabic, French,
   Hausa and Swahili. Farmers cannot act on a label they cannot read.
9. **Code and documentation fixes.**

## Before you open a pull request

- Open an issue first for anything substantial, so effort is not wasted on an approach we
  cannot merge.
- Small fixes — typos, broken links, obvious bugs — go straight to a pull request.

## Licensing of contributions

Inbound equals outbound. By submitting a contribution you agree it is licensed on the same
terms as the component it touches:

- Code: Apache-2.0
- Data and documentation: CC-BY-4.0

There is no separate contributor licence agreement and no copyright assignment. Keep your
copyright; just license it on the same terms so the chain stays clean for the DPG nomination.

**Do not contribute anything you do not have the right to license that way.** This matters
more than usual here: a single image with unclear rights, or a snippet copied from a
restrictively licensed codebase, jeopardises the whole open release. If you are unsure about
an image's provenance or consent status, say so in the pull request rather than assuming.

For image contributions we will ask you to confirm:

- You hold the rights, or the rights holder consented
- Consent covers open publication under CC-BY-4.0, including commercial reuse by third parties
- No identifiable people, documents or other personal data are visible
- EXIF metadata including GPS has been stripped, or you consent to us stripping it

## Development setup

There is no code in this repository yet. The training, evaluation and inference code is
published with the model in November 2026, and the setup, lint, type-check and test commands
will be documented here at that point.

Until then, contributions are to the documentation, the specification, the evaluation protocol,
and the agronomic content — see the list above.

## Code expectations

- Match the style already in the repository rather than introducing a new one
- Keep files focused and reasonably sized; split by responsibility when they grow
- Log at meaningful boundaries — inputs, branches, external calls, errors, state changes —
  with enough context to reconstruct what happened. Never log secrets or personal data.
- Add a test for any bug you fix, so it stays fixed
- Update the documentation in the same pull request as the code
- If your change affects model behaviour, include before-and-after evaluation numbers

## Changes to the model or the data

Higher bar, because these affect everyone downstream.

- A weight change requires the full evaluation suite, reported per class and per stratum, not
  a single headline number
- A label-taxonomy change requires review by a practising agronomist
- A dataset schema change is a major version bump and needs a migration note
- A change that improves the average while degrading a specific crop or region will be
  questioned. Say so up front if that is the trade.

## Pull request checklist

- [ ] Linked issue, for anything non-trivial
- [ ] Tests pass locally
- [ ] Documentation updated
- [ ] For model changes: evaluation results included, per class and per stratum
- [ ] For data changes: provenance and licence recorded in the manifest
- [ ] For image contributions: rights and consent confirmed above
- [ ] No secrets, credentials, endpoints or personal data in the diff
- [ ] Commit messages explain why, not just what

## Reviews

Reviewed by a maintainer, usually within one week. Agronomic contributions may
take longer because they go to an agronomist for review.

We aim to explain a rejection rather than close silently. If a pull request goes quiet, a
polite nudge is welcome.

## Conduct

[CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) applies everywhere this project operates, including
the Hugging Face Community tab.

## Security

Do not open a public issue for a vulnerability. See [SECURITY.md](SECURITY.md).
