# Security Policy

`Agronomist-1` is an agentic model: it generates open-ended text and it calls tools over data
supplied by whoever deploys it. That widens the security surface beyond a conventional model, so
prompt injection and tool-call exploitation are explicitly in scope below.

## Reporting a vulnerability

**Do not open a public issue.**

Report privately through either:

- GitHub private vulnerability reporting, via the **Security** tab on this repository
- Email: hello@deepleaf.io

Please include what you found, how to reproduce it, what an attacker could achieve, and any
suggested fix.

## What to expect

| Stage | Target |
|---|---|
| Acknowledgement | Within 3 working days |
| Initial assessment | Within 10 working days |
| Fix or mitigation plan | Communicated with the assessment |
| Coordinated disclosure | 90 days from the report, or sooner by agreement |

We will keep you informed, credit you in the advisory unless you prefer otherwise, and tell
you before we publish.

## Supported versions

No model weights or code have been released yet, so there is nothing to support. A version
support policy will be published alongside the first release in November 2026.

When weights do ship, older versions stay published for reproducibility but will not receive
fixes. A security-relevant defect in a published version is recorded in that version's model
card, and the version stays published.

## In scope

- Vulnerabilities in the code in this repository, including the inference path
- Dependency vulnerabilities we can act on
- Malicious input handling — crafted images or payloads causing crashes, resource exhaustion or
  code execution
- **Practical prompt injection.** An input, tool result or document that reliably causes the
  model to ignore its instructions, produce an out-of-scope output such as a pesticide dose, or
  emit a tool call the user did not ask for. Include the payload and the success rate.
- **Tool-call exploitation.** Any way to induce a call to a tool not in the supplied schema, or
  to smuggle unintended arguments into a legitimate call.
- Anything indicating training data has leaked personal information into the weights
- Integrity problems with published artefacts, such as a checksum mismatch

## Out of scope

- Vulnerabilities in DeepLeaf's commercial platform. Those go to hello@deepleaf.io and are
  handled separately; this repository is not the right channel.
- **Model accuracy problems.** A wrong prediction is not a security issue. Open a normal
  issue — they are welcome, see
  [RESPONSIBLE_USE.md](RESPONSIBLE_USE.md).
- Adversarial examples in the general sense. Any vision model can be fooled by crafted input,
  and any language model can be jailbroken with enough effort. Report it if you find an attack
  that is practical in a field deployment and causes real harm — a reliable route to an
  unsafe agronomic recommendation qualifies. A contrived single-shot jailbreak with no
  realistic deployment path is a research finding rather than a vulnerability.
- Vulnerabilities in a third party's product that happens to use this model.

## Artefact integrity

No artefacts have been published yet. When weights ship they will carry checksums, and the
verification command and published digests will be documented here.

Weights will be published as safetensors rather than pickle formats. Pickle deserialisation can
execute arbitrary code, so if you encounter `.bin` or `.pth` weights claiming to be this model
from any source, treat them as untrusted — including now, before any official release exists.

## Data protection reports

If you believe the published dataset contains personal data, an image published without valid
consent, or anything that allows re-identification of a specific farm, report it to
hello@deepleaf.io. These are treated with the same urgency as a security report, and
the takedown process is described in the dataset card.
