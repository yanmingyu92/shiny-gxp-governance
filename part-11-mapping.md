# 21 CFR Part 11 Mapping — Template

This document maps the FDA's 21 CFR Part 11 expectations (electronic records;
electronic signatures) to concrete mechanisms of the governance framework.
Part 11 does not certify products — it sets expectations that your combination
of **software, configuration, and procedure** must meet. This mapping is a
template: replace `<...>` tokens and verify each row against your actual
deployment before relying on it.

Each mechanism is written for **both** hosting models:

- **Self-hosted** — you operate the entry point, services, logs, and backups.
- **Posit Connect** (the example commercial hosting platform) — the vendor
  operates the hosting layer and provides built-in audit logging, content
  versioning, and access controls.

*(The dual hosting-model coverage is an extension beyond the source practice,
which documented a self-hosted deployment; the Posit Connect column should be
verified against your vendor agreement and the vendor's current qualification
documentation.)*

## Shared responsibility

Some Part 11 expectations are met by the hosting platform itself; on
commercial hosting those controls sit with the vendor, and your evidence is
the vendor's qualification package (e.g. SOC reports, the vendor's own
validation/qualification documentation) plus your review of it. **What never
shifts to the vendor:** the correctness of application outputs, the validation
memos, classification decisions, release approvals, and the governance
procedures in this repository. The table's right-hand column states, per row,
where the boundary lies.

## Mapping table

| Part 11 expectation | Self-hosted mechanism | Posit Connect mechanism | Responsibility boundary |
|---|---|---|---|
| **§11.10(a) — Validation:** systems validated for accuracy, reliability, consistent intended performance | Platform IQ/OQ report (`validation/PLATFORM_VALIDATION_REPORT_TEMPLATE.md`) executed once and maintained via impact assessment; per-application memos with OQ/PQ and independent verification per tier (`validation/GXP_CLASSIFICATION_RUBRIC.md`) | Same application-layer validation; platform-layer IQ partially inherited from the vendor's qualification evidence, with adopter OQ covering the configuration you control (content settings, access rules) | Vendor: hosting-platform qualification. Adopter: all application validation, plus configuration-level OQ and the decision to rely on vendor evidence |
| **§11.10(c) — Record protection:** records protected to enable accurate and ready retrieval | Source, validation evidence, and governance records in org-owned repositories with branch protection; automated pre-deployment backups with tested restore (platform OQ-4/OQ-6) | Content versioning and the vendor's backup/retention provisions; validation evidence still kept in your own org-owned repositories, not only on the platform | Vendor: durability of hosted content. Adopter: retention of validation evidence, repositories, and tested restore of anything self-managed |
| **§11.10(d) — Access control:** system access limited to authorized individuals | SSO via `<company-sso>` as the only user entry point; per-user data authorization enforced server-side by `<data-system-of-record>`; application visibility limited by the Gate 4 release whitelist; administrator login retained only as break-glass | SSO/authentication integration of the hosting platform; per-content access settings implement Gate 4; data authorization unchanged (system of record) | Vendor: authentication plumbing and platform access enforcement. Adopter: who is granted access, Gate 4 release decisions, and data-permission governance in the system of record |
| **§11.10(e) — Audit trails:** secure, computer-generated, time-stamped audit trails of operator entries | Git history on protected branches (every code change with author and timestamp); protected-environment approval records (every deployment decision); release log (what went live, when, who approved); container/service logs on the production server (runtime behavior) | The hosting platform's built-in audit logging (user and content events) plus content version history; git history, approvals, and release log unchanged | Vendor: completeness and integrity of platform audit logs. Adopter: change and release records in source control, and periodic review that vendor logging is enabled and retained |
| **§11.10(g) — Authority checks:** only authorized individuals can use the system, alter records, or perform operations | The role permission matrix and four control gates (`GOVERNANCE_TEMPLATE.md`): merge requires code-owner review; deployment requires Platform Maintainer approval; release (visibility) is a separate action; QC independence for `gxp-critical` | Same governance gates implemented with the hosting platform's publisher/viewer permission model and your source-control protections | Adopter (both models): authority checks are a governance configuration, not a vendor feature |
| **§11.10(k) — Documentation control:** controlled distribution and revision of system documentation | This governance repository under branch protection and code ownership; change control of the governance document itself (`GOVERNANCE_TEMPLATE.md` §7); ADRs record material changes (`adr/ADR_TEMPLATE.md`) | Identical — documentation control lives in your repositories regardless of hosting | Adopter |
| **§11.50 — Signature manifestations:** signed records show the signer's name, date/time, and meaning of the signature | Signature tables in the platform report, application memos, and QC comparison reports (name/role, date, meaning via the document's conclusion section); signatures correlated with the approving pull-request review in git history | Identical — signature manifestations live in your validation documents, not in the hosting platform | Adopter |
| **§11.70 — Signature/record linking:** signatures linked to their records so they cannot be excised or transferred | Signatures are embedded in the version-controlled record they sign (memo, report), so git history binds signer, content, and commit hash together; the memo records the exact commit hash it validates | Identical — same version-controlled linking; hosted content versions provide an additional anchor for the released artifact | Adopter |

## Notes for adopters

- Where a row says "inherited from the vendor," the work is not zero: record
  which vendor evidence you reviewed, its date, and your conclusion, as part of
  the platform validation report. An unexamined vendor claim is not a control.
- Audit-trail and record-protection rows assume your repositories have branch
  protection and org ownership actually configured — Gate 1 and the asset
  ownership model in `GOVERNANCE_TEMPLATE.md` are the enforcement, this table
  is only the description.
- This mapping covers the sections most relevant to a Shiny review platform.
  If your use of the platform changes (e.g. you start producing signed
  regulatory records in the platform itself rather than in documents), extend
  this table before extending the use.
