# Governance

Accountable roles are named, so it is clear who decides what and who to hold to it. Individual
contributors beyond those roles are not named publicly, as a matter of personal privacy.

## Current state

**DeepLeaf Agronomist-1** is maintained by **DeepLeaf, Co.** This is a company-led open
project, not a foundation-governed one.

| Role | Who | Decides |
|---|---|---|
| Project lead | El Mahdi Aboulmanadel, Founder, DeepLeaf, Co. | Direction, releases, licensing, partnerships |
| Technical lead | Lahcen Ait Houmaid, CTO, DeepLeaf, Co. | Architecture, training, evaluation, merges |
| Agronomic review | Dr Brahim Ezzahiri, Head of Agronomy, DeepLeaf, Co. | Label taxonomy, condition definitions, diagnostic correctness |
| Maintainers | The DeepLeaf team | Code review, issue triage, releases |

Contact: hello@deepleaf.io

**Authorised representative.** Where a named individual must speak for the project — the
attestation on a Digital Public Goods nomination, for example — that is El Mahdi Aboulmanadel.

## What this means for you

Two consequences of company-led governance.

**You do not need our permission.** Apache-2.0 and CC-BY-4.0 mean you can fork the model,
retrain it, ship a commercial product on it, and never speak to us. There is no key to
revoke, no API to shut off, no term we can change retroactively on a version you already
hold. That independence is the point, and it is why the licences carry no use restrictions.

**We can be wrong, and you can route around us.** If we reject your contribution or take the
model somewhere you disagree with, the fork is a legitimate answer rather than a hostile act.
The published training data and training code are what make a fork viable in practice.

## How decisions get made

- **Routine changes** — bug fixes, documentation, tooling: any maintainer merges after review.
- **Model changes** — new weights, changed behaviour: technical lead decides, with full
  evaluation results published in the pull request.
- **Label taxonomy changes**: agronomic review has the final say. Engineering does not
  overrule agronomy on what a disease is.
- **Licensing, releases, partnerships**: project lead.
- **Anything affecting the DPG nomination**: project lead, since indicators 7 through 9 are
  formally attested by an authorised representative and that attestation has to stay true.

Substantial decisions are made in public — in issues or Discussions — so the reasoning is
inspectable even when the decision is ours.

## Release process

Semantic versioning.

| Change | Bump |
|---|---|
| Breaking change to the inference API or dataset schema | Major |
| New weights, new conditions, new crops, additive API | Minor |
| Bug fixes, documentation, label corrections | Patch |

Each release gets a git tag, written release notes, updated evaluation results, and a model
card revision. Old weight versions stay published so results published against them remain
reproducible. We do not delete or silently replace weights.

## Roadmap

Public, in Discussions: https://github.com/DeepLeafCo/deepleaf-agronomist-1/discussions

The roadmap is shaped by what downstream builders actually need as they build on this. If you
need a crop, a condition or a deployment target that is missing, say so there — that is how it
gets prioritised.

## Where this could go

If external adoption justifies it, moving to shared governance — a technical steering
committee with contributors from outside DeepLeaf, or stewardship under a neutral foundation
— is a reasonable next step and we are open to it. It is not the current state.

Reasonable triggers to revisit: sustained contribution from multiple independent
organisations, or a public-sector deployment that needs governance assurances.

## Conduct and enforcement

[CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md). Enforcement sits with the project lead, El Mahdi
Aboulmanadel, with reports to hello@deepleaf.io.

Where a report concerns the project lead, escalate to **Lahcen Ait Houmaid**, so that a complaint
is never handled by the person it concerns. Write to hello@deepleaf.io marked for his attention.

Both routes currently share one mailbox. If you would prefer a route that does not, say so in a
report and we will arrange a direct channel.
