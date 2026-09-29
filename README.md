# shiny-gxp-governance

A governance and validation **template** for teams running R/Shiny applications
in a GxP-regulated environment. Fork it, fill in the `<...>` placeholders, and
you have a working governance framework for Shiny-based clinical data review.

## The problem it solves

Small and mid-sized biotech teams increasingly use R/Shiny for clinical data
review: exploratory dashboards, monitoring views, sometimes outputs that feed
regulated decisions. The statistical programming practice around this is often
SAS-centric, where validation habits (independent double programming, structured
review, documented sign-off) are well established — but those habits do not
transfer automatically to a self-service Shiny platform.

Typical failure modes without a framework:

- Anyone who can merge code can reach end users — there is no release gate.
- Nobody can say who is accountable for the correctness of a given number on a
  dashboard.
- Validation effort is either zero or uniform: every app is treated as
  submission-grade, or none are.
- Data permissions are copied into the platform, drift from the system of
  record, and silently over- or under-grant access.
- The whole platform depends on one person's accounts and memory.

This repository is a ready-to-adopt answer: role definitions, control gates, a
risk-tiered validation model, QC procedures, and record templates — all generic,
all with placeholders.

## What "GxP" and "validation" mean here

For readers new to regulated work:

- **GxP** is the umbrella term for the good-practice regulations that govern
  clinical and laboratory work in pharma (GCP, GLP, GMP, ...). Software that
  produces records or supports decisions under these regulations is expected to
  be **validated**: you must have documented evidence that it does what it is
  supposed to do, and that it keeps doing so under change control.
- **21 CFR Part 11** is the FDA rule for electronic records and signatures. It
  does not certify products; it sets expectations (access control, audit trails,
  record protection, ...) that your combination of software and procedures must
  meet. See `part-11-mapping.md`.
- Validation here is deliberately **risk-based**: an exploratory plotting app
  and a dashboard feeding a dose decision do not deserve the same ceremony. The
  three-tier model in `validation/GXP_CLASSIFICATION_RUBRIC.md` scales the
  required evidence to what the output is used for.

## Who this is for

Statistical programming and data science teams in small-to-mid biotech (or
similar settings) who:

- run, or are about to run, R/Shiny applications against clinical data,
- need a defensible validation and governance story without building one from
  scratch,
- want effort concentrated where the regulatory and safety risk actually is.

No GxP background is assumed, but the framework is written to survive an audit
by people who have one.

## Scope statement

This repository is a governance and validation **framework**: documents,
templates, and decision records. It is not software, and it is not itself
validated. **Adopting this framework does not make any system compliant and
confers no regulatory status.** Compliance comes from executing the procedures
described here against your own platform, keeping the evidence, and — where
required — having that evidence reviewed by your quality organization. Nothing
here replaces legal or regulatory advice.

## Relationship to the R Validation Hub

The [R Validation Hub](https://www.pharmar.org/) (an R Consortium ISC Working
Group, founded 2018 by the PSI AIMS SIG) publishes the community's reference
position on R in regulated work. Its flagship white paper, *A Risk-based
Approach for Assessing R Package Accuracy within a Validated Infrastructure*
(Nicholls, Bargo, Sims, January 2020,
[pharmar.org/white-paper](https://www.pharmar.org/white-paper/)), covers
risk-based assessment of **contributed R packages** and explicitly leaves
infrastructure validation and environment reproducibility/traceability to each
adopting organization. Its tooling work ({riskmetric}, {riskassessment},
{val.meter}) likewise targets package risk assessment.

This template operates at the layer the Hub's published scope leaves to you:
**application-level validation and platform-level governance** — who may release
an app, how much evidence each app needs, how QC independence works, and how the
platform's controls map to 21 CFR Part 11. The risk-based philosophy here is
aligned with, and references, the Hub's published approach. This project is
independent: it is not endorsed by, approved by, or affiliated with the R
Validation Hub, the R Consortium, or PHUSE.

## How to adopt

1. **Fork or copy** this repository into an organization-owned account
   (not a personal one — see the continuity section of
   `GOVERNANCE_TEMPLATE.md`).
2. **Fill the placeholders.** Every `<...>` token is something your organization
   must decide: role holders, repository names, identity provider, hosting
   platform.
3. **Assign the roles.** At minimum: one Platform Maintainer, and one App
   Author per application. Read the permission matrix in
   `GOVERNANCE_TEMPLATE.md` before assigning.
4. **Adopt the four control gates** in your hosting and source-control setup
   (`GOVERNANCE_TEMPLATE.md`, section on control gates). The gates are the
   enforcement mechanism; the documents are only the description.
5. **Classify every existing app** using
   `validation/GXP_CLASSIFICATION_RUBRIC.md` and record the tier in each app's
   metadata file. New apps declare a classification before development starts.
6. **Run platform validation once** using
   `validation/PLATFORM_VALIDATION_REPORT_TEMPLATE.md`. Until this exists and is
   signed, no app should carry a `gxp-support` or `gxp-critical` classification.
7. **Validate apps per tier.** `gxp-support` and `gxp-critical` apps need a
   completed validation memo (`validation/APP_VALIDATION_MEMO_TEMPLATE.md`) and
   RTM before release; `gxp-critical` additionally needs double programming of
   critical outputs (`qc/DOUBLE_PROGRAMMING_GUIDE.md`).
8. **Record decisions** as ADRs (`adr/ADR_TEMPLATE.md`) when you deviate from or
   extend this template.

## File map

| Path | Contents |
|------|----------|
| `GOVERNANCE_TEMPLATE.md` | Roles, permission matrix, four control gates, asset ownership, continuity, change control |
| `part-11-mapping.md` | 21 CFR Part 11 expectations mapped to mechanisms, for both commercial hosting (Posit Connect as the example) and self-hosted deployments |
| `validation/PLATFORM_VALIDATION_REPORT_TEMPLATE.md` | IQ/OQ report template for the platform layer, with revalidation triggers and dual signature |
| `validation/APP_VALIDATION_MEMO_TEMPLATE.md` | Per-application validation memo: OQ/PQ results, independent verification, conclusion, signatures |
| `validation/GXP_CLASSIFICATION_RUBRIC.md` | The three GxP tiers, decision tree, per-tier requirements, classification governance |
| `validation/RTM_TEMPLATE.md` | Requirements traceability matrix template and conventions |
| `qc/DOUBLE_PROGRAMMING_GUIDE.md` | Independent double-programming operating procedure (R and SAS tracks, XPT interchange, tolerance contract) |
| `qc/QC_REPORT_TEMPLATE.md` | Comparison report template for double programming / independent QC |
| `qc/PR_CHECKLIST.md` | Pre-merge/release checklist: security, metadata, validation gate, code quality |
| `adr/ADR_TEMPLATE.md` | Compressed Context/Decision/Rationale decision-record format |
| `adr/0001-…0005` | Five worked example decisions showing the format and the reasoning behind this framework's core choices |

## License and authorship

MIT License, copyright (c) 2026 Jaime Yan. See `LICENSE`.
Contributions are welcome — see `CONTRIBUTING.md`.
