# PR Checklist — Release Gate

Confirm each item before merging a PR that takes an application live, or that
advances its metadata `status` from `dev` to `validating` or `validated`.
Per-tier validation requirements are defined in
`../validation/GXP_CLASSIFICATION_RUBRIC.md`.

## Security

- [ ] Authentication reuses the platform's **shared authentication mechanism** —
      no home-grown authentication logic in the application
- [ ] The app does not access filesystem data outside its own directory (data
      ships with the app, comes from `<data-system-of-record>`, or is provided
      by the platform)
- [ ] The app opens no additional network listeners beyond what the platform
      assigns

## Metadata

- [ ] The app metadata file (`app.yaml` or equivalent) is complete:
      `slug` / `title` / `owner` / `status` / `classification` / `users`
      (allowlisted end users, synced to the release gate at release)
- [ ] `status` is correct: new apps start as `dev`; `validating` is required
      before release; `validated` only with an approved memo
- [ ] `classification` is one of `exploratory` / `gxp-support` / `gxp-critical`
      and the delivered validation depth matches the requirements table in
      `../validation/GXP_CLASSIFICATION_RUBRIC.md`
- [ ] Any classification downgrade since the last release carries a written
      rationale in the validation memo

## Validation gate (gxp-support and above)

- [ ] `validation/VALIDATION_MEMO.md` is complete
      (per `../validation/APP_VALIDATION_MEMO_TEMPLATE.md`)
- [ ] All verification items (numerical / graphical / logical OQ, plus PQ)
      have actual results recorded — no empty cells
- [ ] `validation/RTM.md` exists, is current, and every requirement traces to
      a passing test case
- [ ] Independent verification evidence is in place: second-person re-check of
      key numbers (`gxp-support`), or double-programming comparison report
      with dual sign-off (`gxp-critical`, per `DOUBLE_PROGRAMMING_GUIDE.md`)
- [ ] For `gxp-critical`: the production QC export (XPT + manifest) for the
      current data cut is present under
      `validation/double_programming/production_export/`
- [ ] Memo conclusion is **approved** with all signatures in place
- [ ] The commit hash recorded in the memo matches the commit being released

## Code quality (all classifications)

- [ ] The automated test suite exists and passes
- [ ] The dependency lockfile (e.g. `renv.lock`) is committed and current
- [ ] CI checks are green
