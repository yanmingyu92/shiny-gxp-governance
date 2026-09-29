# Application Validation Memo — Template

> One memo per application, stored alongside the application in `<apps-repo>`
> (suggested: `apps/<app-slug>/validation/VALIDATION_MEMO.md`). Required for
> **`gxp-support`** and **`gxp-critical`**; `exploratory` apps do not need a
> memo (see `GXP_CLASSIFICATION_RUBRIC.md`). Fill every section; delete
> guidance in *italics* before sign-off.

## 1. Purpose and scope

*What this application shows, and which parts of its behavior this validation
covers. Scope is limited to the application layer: data loading, calculations,
plot/table rendering, access-control integration. Platform-layer controls
(authentication enforcement, deployment, logging) are qualified once at the
platform level and referenced in Section 3 — not re-verified here.*

- Application name: `<app-name>`
- Validation scope:

## 2. Application metadata

| Field | Value |
| --- | --- |
| Application slug | `<app-slug>` |
| Classification (exploratory / gxp-support / gxp-critical) | |
| Status at validation time (dev / validating / validated) | `validating` |
| Commit hash at validation time | |
| Validation environment (local / pre-release deployment) | |

## 3. Platform validation reference

*Installation-level assurance is covered at the platform layer; cite the
platform validation this application builds on. Do not re-execute platform
checks here.*

- Platform validation report version / date:
- Platform validation report location: `<governance-repo>`

## 4. Requirements reference

- Requirements list location: `<requirements-doc>`
- RTM location: `apps/<app-slug>/validation/RTM.md` (see `RTM_TEMPLATE.md`)

## 5. OQ results — functional and logical testing

### 5.1 Automated tests

*Reference the automated test suite; record the commit and date of the passing
run.*

- Suite location: `apps/<app-slug>/app/tests/` — result:
- Run commit / date:

### 5.2 Numerical correctness

*Cross-check headline counts/statistics against an independent source (hand
check, reference script, or source listings) on a known dataset.*

| ID | Test data | Expected result | Actual result | Pass/Fail |
| --- | --- | --- | --- | --- |
| OQ-N1 | | | | |

### 5.3 Graphical correctness

*Verify at least one representative figure or table against independently
computed values.*

| ID | Test data | Expected result | Actual result | Pass/Fail |
| --- | --- | --- | --- | --- |
| OQ-G1 | | | | |

### 5.4 Logical correctness

*Exercise filters, derivations, and edge cases (empty groups, missing values,
single-subject strata) and record expected vs. observed.*

| ID | Test data | Expected result | Actual result | Pass/Fail |
| --- | --- | --- | --- | --- |
| OQ-L1 | | | | |

## 6. PQ results — user confirmation

*Confirmation by intended End Users, on real or representative data in a
pre-release deployment not visible to general users, that the application
answers the intended business question correctly. Required for `gxp-support`
and above.*

| ID | Scenario | Tester | Result | Pass/Fail |
| --- | --- | --- | --- | --- |
| PQ-1 | | | | |

## 7. Independent verification evidence

*Per classification (see `GXP_CLASSIFICATION_RUBRIC.md`):*

- *`gxp-support`: record the independent re-check of key numbers — who
  verified, what was re-computed from source data, and the outcome.*
- *`gxp-critical`: reference the double-programming comparison report
  (`qc/QC_REPORT_TEMPLATE.md`, filled at
  `apps/<app-slug>/validation/double_programming/COMPARISON_REPORT.md`); the
  report carries its own dual sign-off.*

- Verification evidence:

## 8. Deviations and known issues

*Any failed or waived items — including any classification-downgrade rationale
(downgrades require written justification, see `GXP_CLASSIFICATION_RUBRIC.md`)
— with disposition and follow-up.*

- None / …

## 9. Conclusion

- [ ] **approved** — all verification items passed; the application may be
  released at Gate 4
- [ ] **rejected** — failures exist; rework and re-validation required

Conclusion notes:

## 10. Signatures

| Role | Name | Signature | Date |
| --- | --- | --- | --- |
| Verifier (performed the validation) | | | |
| App Author | | | |
| Platform Maintainer (release approval) | | | |

*For `gxp-critical` applications, the App Author / QC Programmer dual sign-off
on critical outputs is recorded in the comparison report (Section 7).*

## 11. History

*Append a dated row on every re-validation (data-logic changes re-execute the
memo in whole or in part; cosmetic changes need only a note here).*

| Date | Commit | Change | Sections re-executed | By |
| --- | --- | --- | --- | --- |
| | | | | |
