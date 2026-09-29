# GxP Classification Rubric

Every application on the platform declares one of three GxP tiers. The tier is
a statement about the **intended use of the output**, not about the technology,
the data source, or how careful the author feels. The declared tier scales the
validation depth required before release.

Applications are treated in the spirit of **GAMP 5 (2nd edition) Category 4
(configured products)**: standard, qualified platform components (runtime,
entry point, authentication mechanism) configured with application-specific
code, data, and metadata. Validation effort is scaled to the risk each
application poses to patient safety, data integrity, and decision quality.

## The three tiers

| Tier | Meaning |
|------|---------|
| `exploratory` | Non-GxP. Exploratory internal analysis; outputs support neither regulated submissions/decisions nor internal clinical/scientific decisions |
| `gxp-support` | GxP-relevant. Outputs support internal clinical/scientific decisions (e.g. a monitoring dashboard shared with clinical colleagues) |
| `gxp-critical` | GxP-critical. Outputs directly support a regulated submission, a regulated record, or a decision with safety/regulatory impact (e.g. safety monitoring decisions, dose decisions, submission content) |

## Decision tree

Ask in order; stop at the first yes:

1. **Does the output directly support a regulated submission, record, or
   decision?** → `gxp-critical`
2. **Does the output support internal clinical/scientific decisions?** →
   `gxp-support`
3. **Otherwise** → `exploratory`

When in doubt between two tiers, take the higher one. Upgrades are always
allowed and cheap; downgrades are the expensive direction (see governance
rules below).

## Requirements per tier

| Requirement | `exploratory` | `gxp-support` | `gxp-critical` |
|-------------|---------------|---------------|----------------|
| Code review | Peer review via pull request | Peer review via pull request | Peer review via pull request |
| Automated tests | Pass in CI | Pass in CI | Pass in CI |
| Requirements + RTM | Not required | Required (`RTM_TEMPLATE.md`) | Required |
| Validation memo | Not required | Required (`APP_VALIDATION_MEMO_TEMPLATE.md`) | Required |
| Independent verification | Not required | **Independent re-check of key numbers by a second person** (targeted verification, not full re-implementation) | **Independent double programming** of critical outputs (`../qc/DOUBLE_PROGRAMMING_GUIDE.md`) with comparison report and dual sign-off |
| PQ (user confirmation) | Not required | Required | Required |
| After a data refresh | No action required | Re-run automated tests **and** re-check key numbers | Re-run the double-programming comparison; append to the comparison report |
| Periodic review | Not required | Re-affirm fitness at least annually, and after platform changes affecting the app | Re-affirm fitness at least annually, and after platform changes affecting the app |

Rationale for the split: independent double programming — a second programmer
re-computing results from the specification alone — is the strongest evidence
available, but it roughly doubles the cost of what it covers. Reserving it for
outputs where an undetected error has regulatory or safety impact keeps the
framework proportionate and sustainable for small teams. Everything else is
covered by review, automated tests, and targeted re-checks.

## Governance rules

- **Declared in metadata.** The tier is recorded in the application's metadata
  file (`app.yaml` field `classification`, or your equivalent). Legal values
  are exactly `exploratory`, `gxp-support`, `gxp-critical`; enforce this in CI
  so an undeclared or misspelled tier fails the build
  (see `adr/0004-tier-gxp-classification-in-app-metadata.md`).
- **Proposed by the App Author, approved by the Platform Maintainer** during
  pull-request review, before development starts — the tier determines the
  depth of every subsequent validation step.
- **Upgrades are always allowed.** Any app may voluntarily meet a higher
  tier's requirements.
- **Downgrades require a written rationale**, recorded in the app's validation
  memo (deviations section), and approved by the Platform Maintainer.
- **Release follows the tier.** End-user access is granted only when the
  requirements for the declared tier are met; for `gxp-support` and above this
  means an approved validation memo before release at Gate 4
  (see `../GOVERNANCE_TEMPLATE.md`).
- **Status lifecycle.** An app's metadata `status` moves `dev → validating →
  validated`. `validating` means validation is underway and the app must not
  be released; `validated` means the memo is approved and release at Gate 4 is
  permitted.
