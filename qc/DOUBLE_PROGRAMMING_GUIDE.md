# Double Programming Guide

Independent double programming — a second programmer re-computing results from
the specification alone, with a programmatic comparison — is the strongest
available evidence that a computed output is correct. This guide defines how to
apply it to Shiny applications **in a risk-proportionate way**: only the
critical outputs of `gxp-critical` applications are double-programmed;
everything else follows the `gxp-support` controls (code review, automated
tests, targeted re-check of key numbers — see
`../validation/GXP_CLASSIFICATION_RUBRIC.md`).

**Small teams are not expected to double-program everything.** Double
programming is required for `gxp-critical` apps only, and only for the critical
outputs identified in the risk assessment (primary endpoints, safety-critical
variables, complex derivations). The scope decision itself is documented in the
QC specification and reviewed by the Platform Maintainer.

## 1. Independence rules

Independence is what makes the evidence meaningful:

- The **QC Programmer** writes the re-computation from the **specification
  alone** (requirements, RTM, QC specification of derivations and rounding) —
  never from the production code. Reading the production application code
  before the comparison invalidates the exercise.
- The QC Programmer must be a **different person** from the App Author who
  wrote the production code.
- Neither side sees the other's intermediate results before the comparison is
  run.
- Discrepancies are recorded, investigated, and resolved; **both programmers
  sign** each resolution.

## 2. Compare datasets, not screenshots

Compare the **result datasets behind the displayed numbers** — the aggregated
data feeding each plot or table — not screenshots or rendered pixels. Visual
rendering is covered by the OQ graphical checks in the validation memo; double
programming verifies the numbers.

Common principles for both tooling tracks:

- Sort both sides by the comparison **keys** before comparing.
- Apply a documented **tolerance contract**: numeric tolerance default `1e-8`,
  specified in the QC specification and applied identically on both sides.
  Differences in row/column counts, labels, or types are failures unless
  explicitly waived in the specification.

## 3. Comparison tracks

### Track A — R (diffdf)

- The QC Programmer re-computes in R; comparison uses **diffdf** (the
  R-ecosystem counterpart of SAS PROC COMPARE) with the tolerance contract.
- Wrap the comparison as an automated test so CI re-runs it as a regression
  lock on every change; the test fails on any unresolved difference.

### Track B — SAS (PROC COMPARE)

For QC Programmers whose primary tool is SAS — common in statistical
programming teams:

- The QC Programmer independently re-computes the critical result datasets
  from source data in SAS, from the specification alone.
- The production side exports its result datasets as **XPT** (§4).
- The QC Programmer compares locally with PROC COMPARE, applying the tolerance
  contract (the `CRITERION=` option expresses the same tolerance as diffdf's),
  both sides sorted by key first.
- The comparison output is archived as the comparison record.
- The SAS track runs **locally, not in CI** (CI runners have no SAS); results
  are recorded in the comparison report exactly as in Track A.

### Track selection

- The track follows the QC Programmer's primary tool. **Mixed mode is
  allowed** (e.g. safety-critical variables double-programmed in SAS, the rest
  in R); record the choice in the comparison report.
- Independence rules, the specification, dual sign-off, and the data-refresh
  clause (§6) are identical for both tracks.

## 4. Interchange format and export conventions

**Format: XPT (SAS Transport v5).** R writes it (e.g. with a package such as
`haven`); SAS reads it natively. Rationale:

- **CSV loses types and labels** — a comparison that cannot distinguish the
  integer `1` from the string `"1"` is weaker evidence.
- **`sas7bdat` cannot be written from R** — it is a proprietary binary format;
  XPT is the open, regulatory-submission-standard interchange.

Export conventions for `gxp-critical` apps:

- Every result dataset behind a displayed critical number is exported, into
  **one directory per data cut**
  (`apps/<app-slug>/validation/double_programming/production_export/<data-cut>/`).
- Every export ships a **manifest** in the same directory: dataset list, the
  specification item each dataset maps to, row counts, export timestamp, and
  the commit of the export code.
- QC-side datasets go to a parallel `qc_export/<data-cut>/` directory (XPT or
  native SAS format).
- Exports are **produced by running code, never by manual export from the
  UI**. The export code lives in the repository so every export is
  re-computable from source data.

## 5. Directory layout

```
apps/<app-slug>/validation/double_programming/
├── SPEC.md                  # Which outputs are re-computed: precise
│                            # derivations, filters, rounding, tolerance
├── qc_script/               # Independent re-computation code (R or SAS).
│                            # Depends only on source data + SPEC.md
├── production_export/<data-cut>/   # Production result datasets + manifest
├── qc_export/<data-cut>/           # Independently produced QC datasets
├── compare_output/          # Archived comparison output (SAS track)
├── COMPARISON_REPORT.md     # Filled from QC_REPORT_TEMPLATE.md
└── DISCREPANCY_LOG.md       # Running log of discrepancies and resolutions
```

## 6. Data-refresh clause

Dashboards update with each data cut. On **every data refresh**:

1. Re-run the QC re-computation against the new data cut.
2. Re-run the comparison (Track A: diffdf, locally and via CI; Track B:
   re-export the production XPT for the new cut and re-run PROC COMPARE).
3. **Append** a dated entry to the comparison report (data version, commits,
   result summary) — never overwrite previous entries; the report is an
   append-only record.

A refresh with unresolved differences **blocks release** of the updated
application.

## 7. Discrepancy resolution

- Every comparison difference gets a discrepancy-log entry: description, root
  cause (production bug / QC bug / specification ambiguity), resolution, and
  the commit that fixed it.
- Specification ambiguities are resolved by clarifying the specification — the
  clarification is part of the evidence.
- The comparison report is complete only when all discrepancies are resolved
  and both the App Author and the QC Programmer have signed.
