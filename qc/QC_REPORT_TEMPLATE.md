# QC / Double-Programming Comparison Report — Template

> Template for the independent-verification comparison record. Use for
> `gxp-critical` double programming (see `DOUBLE_PROGRAMMING_GUIDE.md`); it
> can also record the second-person re-check for `gxp-support` apps, with the
> method sections trimmed. **Append a new dated entry on every comparison run
> (initial validation and each data refresh) — never overwrite previous
> entries.**

- Application (slug): `<app-slug>`
- Classification: `gxp-critical`

## Comparison entry — YYYY-MM-DD

| Field | Value |
| --- | --- |
| Comparison date | |
| Data version / data cut | |
| Production result commit | |
| QC result commit | |
| QC specification version | `SPEC.md` @ `<commit>` |
| Track used (R / SAS / mixed) | |
| Comparison tool | diffdf / PROC COMPARE |
| Numeric tolerance | `1e-8` *(default; justify any looser value in SPEC.md)* |
| Result | pass / fail |

### Scope

*Which critical outputs (per SPEC.md) were compared in this entry.*

- Datasets compared:

### Comparison output summary

*Track A (R): summarize the diffdf output — datasets compared, rows/columns
compared, number of unresolved differences (expected: 0 for pass). Track B
(SAS): reference the archived PROC COMPARE output by file name under
`compare_output/` and summarize its findings.*

```

```

### Discrepancies and resolution

*Reference entries in `DISCREPANCY_LOG.md`. Every difference must be resolved
and cross-referenced here before sign-off.*

- None / …

### Conclusion

- [ ] pass — production and independent QC results agree within tolerance
- [ ] fail — unresolved differences remain; **release is blocked**

### Sign-off (dual signature required)

| Role | Name | Signature | Date |
| --- | --- | --- | --- |
| App Author (production programmer) | | | |
| QC Programmer | | | |
