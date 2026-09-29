# Requirements Traceability Matrix (RTM) — Template

Copy this file to `apps/<app-slug>/validation/RTM.md` and keep it current
throughout the application lifecycle. Required for **`gxp-support`** and
**`gxp-critical`**; optional for `exploratory` (see
`GXP_CLASSIFICATION_RUBRIC.md`).

The RTM closes the loop between what the application must do and the evidence
that it does it:

```
requirement  <-->  test case  <-->  recorded result
```

Every requirement must trace to at least one test case, and every test case to
a recorded result.

| Req ID | Requirement description | Risk (L/M/H) | Test case ID | Test method | Result | Status |
| --- | --- | --- | --- | --- | --- | --- |
| REQ-001 | *Example: the app displays the per-subject values of `<measurement>` by visit.* | M | OQ-N1 | automated (`tests/testthat/test-<topic>.R`) | | pending |
| REQ-002 | *Example: baseline records are visually distinguishable from follow-up records.* | M | OQ-G1 | manual | | pending |
| REQ-003 | *Example: only released, authorized users can open the app.* | H | PQ-1 | manual (pre-release deployment) | | pending |
| REQ-004 | *Example (`gxp-critical`): the primary-endpoint summary dataset matches the independent QC re-computation.* | H | DP-1 | diffdf / PROC COMPARE | | pending |

## Conventions

- **Req ID** — stable identifier from the requirements list; **never reuse a
  retired ID**. Retire a row by marking it, not by deleting it.
- **Risk** — impact of a wrong result for this requirement: L (cosmetic /
  convenience), M (misleading internal analysis), H (regulatory or safety
  impact). High-risk rows identify the candidates for independent verification.
- **Test case ID** — references a validation-memo section (`OQ-N1`, `OQ-G1`,
  `OQ-L1`, `PQ-1`, ...), an automated test file, or a double-programming
  comparison entry (`DP-n`) for `gxp-critical`.
- **Test method** — prefer automated methods where feasible; use `manual` for
  visual/UX confirmations and record the evidence in the memo. For the SAS
  double-programming track, the archived PROC COMPARE output is the evidence
  (see `../qc/DOUBLE_PROGRAMMING_GUIDE.md`).
- **Status** — `pass` / `fail` / `pending`. **Every row must be `pass` before
  the memo conclusion can be `approved`.** Explain any `fail` row in the memo's
  deviations section.
- **Change control** — on any change to data logic: mark affected rows, re-run
  the linked tests, and record the new results **with the new commit hash**.
  An RTM row without a current commit reference is not evidence.
