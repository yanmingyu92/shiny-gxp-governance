# Shiny Platform Governance — Template

> **How to use this template.** Every `<...>` token is a placeholder your
> organization must replace. Role names (Platform Maintainer, App Author, QC
> Programmer, End User) are used verbatim across all documents in this
> repository — if you rename a role, rename it everywhere. This document
> describes the governance model; the control gates in §3 are the enforcement
> mechanism and must actually be configured in your tooling for the model to
> hold.

This document defines how the `<platform-name>` Shiny application platform is
governed: who can do what, which controls stand between a code change and a
production user, how assets are owned, and how the setup survives the
unavailability of any single person.

---

## 1. Roles and permission matrix

Four roles participate in the platform. Role membership is assigned by the
Platform Maintainer and reviewed periodically (at least annually, recorded in
`<review-record-location>`).

| Role | Held by | Scope |
|------|---------|-------|
| **Platform Maintainer** | `<role-holder>` | Maintains the platform code, the shared authentication mechanism, deployment approvals, and the application release/visibility gate |
| **App Author** | one per application | Owns the code, correctness, and validation of their own application(s) |
| **QC Programmer** | per `gxp-critical` engagement | Independently re-computes critical outputs under the independence rules in `qc/DOUBLE_PROGRAMMING_GUIDE.md`; must not be the App Author of the outputs under review |
| **End User** | any authorized staff member | Reaches authorized applications through `<company-sso>` only |

### Scope boundaries

The following capabilities are explicitly **outside** the Platform Maintainer's
scope, by design:

- **Data permissions** — governed exclusively by `<data-system-of-record>`;
  granted per user by `<data-administrator-role>` and enforced server-side by
  that system. The platform has no ability to grant, modify, or bypass them
  (see `adr/0001-delegate-data-authorization-to-system-of-record.md`).
- **Infrastructure** — servers, networking, and certificates are owned and
  operated by `<infrastructure-team>` (or the hosting vendor, for commercial
  hosting).
- **Organization administration** of the source-control organization — held by
  `<org-admin-role>`; deliberately separate from the Platform Maintainer so
  continuity does not depend on one account (see §5).
- **Credential custody** — held per Gate 3 (§3); break-glass recovery runs
  through `<break-glass-authority>` on an authorized management request (see §5).

### Permission matrix

| Capability | Platform Maintainer | App Author | QC Programmer | End User |
|------------|:------------------:|:----------:|:-------------:|:--------:|
| Read this governance repository | yes | yes | yes | yes |
| Read/write platform implementation source | yes | no | no | no |
| Read/write shared application repository (`<apps-repo>`) | yes | own apps only | own QC directories only | no |
| Modify the shared authentication mechanism | yes | no | no | no |
| Merge to protected release branches | yes (after review) | own apps, after Platform Maintainer review | no | no |
| Approve production deployments | yes | no | no | no |
| Execute production deployments | yes | no | no | no |
| Add/remove applications at the release visibility gate (Gate 4) | yes | no | no | no |
| Request release of an application | n/a | yes | no | no |
| Approve a GxP classification (see `validation/GXP_CLASSIFICATION_RUBRIC.md`) | yes | proposes only | no | no |
| Access applications via `<company-sso>` | yes | yes | yes | authorized apps only |

Separation of duties that this matrix enforces:

- An App Author cannot self-approve release to end users — release is a
  separate Platform Maintainer action (Gate 4).
- A production deployment requires an approval distinct from the code author
  (Gate 2).
- For `gxp-critical` outputs, the person verifying the numbers is not the
  person who produced them (QC Programmer independence rules).

---

## 2. Asset ownership model

| Asset | Visibility | Contents |
|-------|-----------|----------|
| `<apps-repo>` | Shared (internal) | Applications plus their QC evidence and validation memos |
| `<governance-repo>` | Broad internal read | This repository: governance, decision records, validation records, release log |
| Platform implementation | Privately maintained by the Platform Maintainer | Platform services, shared authentication mechanism, deployment automation, server-side configuration |

Separation rationale: operational detail stays with the Platform Maintainer's
privately maintained materials; application work happens in the shared
repository where App Authors collaborate; oversight records live in the
governance repository, where stakeholders can read them without access to
anything sensitive. See `adr/0005-disclosure-tiered-documentation.md`.

**All repositories belong to the `<your-org>` organization account, never to a
personal account.** This is a continuity requirement, not a preference (§5).

---

## 3. Control gates

Four independent gates stand between a code change and a production user. Each
gate is a configuration in your tooling, not a policy statement.

### Gate 1 — Branch protection and code ownership

- The release branch (`<release-branch>`, typically `main`) is protected on all
  project repositories: no direct pushes; changes arrive via pull request.
- A code-ownership mechanism (e.g. `CODEOWNERS`) assigns the Platform
  Maintainer as required reviewer for the shared authentication mechanism,
  deployment configuration, and governance files; App Authors are code owners
  of their own application directories.
- A change to shared platform code cannot merge without Platform Maintainer
  review.

### Gate 2 — Protected deployment approval

- The production deployment workflow targets a protected deployment
  environment that requires explicit Platform Maintainer approval before it
  runs.
- Every production deployment is therefore an approved, recorded event.
  Record each one in `<release-log-location>` (what, when, commit, approver,
  deployer).

### Gate 3 — Secrets isolation

- Credentials are stored in the secret store of the source-control or hosting
  platform: write-only through the UI, never readable back, injected only at
  deploy time.
- Server access material (e.g. SSH keys) lives outside every repository, on
  authorized machines only.
- No credential material of any kind is committed to any repository — the
  governance repository included.

### Gate 4 — Release visibility gate

- Applications become reachable by end users only after an explicit release
  action owned solely by the Platform Maintainer (a visibility whitelist on a
  self-hosted entry point, or content access settings on a commercial hosting
  platform such as Posit Connect).
- This gate controls **application visibility only**. Data-level authorization
  is a separate, independent layer governed by `<data-system-of-record>` — a
  released application still returns no data to a user without permissions
  there.
- An application classified `gxp-support` or `gxp-critical` is not released
  until its validation memo is complete and reviewed (see
  `validation/APP_VALIDATION_MEMO_TEMPLATE.md`). Merging code is not releasing:
  visibility is a separate, deliberate act
  (see `adr/0002-gate-release-on-validation-memo.md`).

---

## 4. Validation responsibility split

Validation is split into two layers with different owners and lifecycles
(see `adr/0003-split-validation-platform-vs-application.md`).

### Platform validation — owned by the Platform Maintainer

Covers the platform itself: authentication enforcement, the data-access
channel, deployment and rollback mechanics, logging, and backups.

- Established **once**, using
  `validation/PLATFORM_VALIDATION_REPORT_TEMPLATE.md`, and remains in effect
  long-term.
- Re-examined when the platform changes: the Platform Maintainer performs an
  **impact assessment** per change and re-validates only impacted items
  (triggers listed in the report template).

### Application validation — owned by each App Author

Covers what an application shows: numbers, figures, tables, and derived logic.

- One validation memo per application classified `gxp-support` or
  `gxp-critical`, stored alongside the application in `<apps-repo>`, using
  `validation/APP_VALIDATION_MEMO_TEMPLATE.md`.
- Validation depth scales with the declared GxP tier — see
  `validation/GXP_CLASSIFICATION_RUBRIC.md`. `exploratory` apps require no
  memo: code review and passing automated tests suffice.
- Re-validated whenever the application's data logic changes.

This split keeps platform-level assurance stable (no re-validation churn per
application) while making each App Author personally accountable for the
correctness of what their application displays.

---

## 5. Organizational continuity

The platform must survive the unavailability of any single individual.

- **Org ownership.** All shared repositories belong to the `<your-org>`
  organization account. Repository ownership never depends on the Platform
  Maintainer's personal account.
- **Dormant admin takeover.** `<org-admin-role>` holds organization owner
  rights and can grant repository access to a successor without any action
  from the current Platform Maintainer. The same role is the designated
  successor contact for privately maintained operational materials.
- **Break-glass recovery.** `<break-glass-authority>` (e.g. the hosting
  vendor's administration team, or IT for self-hosted infrastructure) can
  restore administrative access to the production environment on an authorized
  management request. Because operational configuration and deployment tooling
  reside with the platform itself, infrastructure-level recovery plus
  repository-level takeover jointly cover continuity — no separately sealed
  credential package is required. Operational documentation maintained by the
  Platform Maintainer describes recovery procedures at a level a qualified
  successor can execute.
- **Bus-factor review.** Continuity provisions are re-checked at least annually
  as part of the periodic review (see `validation/GXP_CLASSIFICATION_RUBRIC.md`,
  periodic review row).

---

## 6. GxP mapping

The mapping between 21 CFR Part 11 expectations and the mechanisms above —
for both commercial hosting and self-hosted deployments — lives in
`part-11-mapping.md`.

---

## 7. Change control for this document

Changes to this governance document require a pull request with Platform
Maintainer approval (Gate 1). Material changes to the governance model are
recorded as decision records in `adr/` using `adr/ADR_TEMPLATE.md`.
