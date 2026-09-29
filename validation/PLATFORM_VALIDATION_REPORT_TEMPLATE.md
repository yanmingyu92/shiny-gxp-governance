# Platform Validation Report — Template

**Status: `<DRAFT | APPROVED>`**

<!--
HOW TO USE THIS TEMPLATE
- This report validates the PLATFORM layer only: entry point, authentication
  enforcement, data-access channel, deployment pipeline, logging, backups.
  Individual applications are validated separately by their App Authors using
  APP_VALIDATION_MEMO_TEMPLATE.md.
- Fill in every section, then remove the guidance comments.
- When complete, set Status to APPROVED and record the approval in
  <release-log-location> if it coincides with a deployment.
- SANITIZATION: this report must not contain hostnames, IP addresses, ports,
  file-system paths, environment-variable values, credentials, or operational
  commands. Describe components by role ("the production entry point", "the
  data system of record") and reference the privately maintained operational
  documentation for detail.
-->

## 1. Purpose and scope

<!-- State what this validation covers (the shared platform: entry point,
authentication, data-access channel, deployment pipeline, logging, backups)
and what it explicitly excludes (the content and logic of individual
applications, covered by per-application validation memos). If you run on a
commercial hosting platform (e.g. Posit Connect), state which controls are
inherited from the vendor and which vendor qualification evidence (e.g. SOC
reports) you rely on — see part-11-mapping.md. -->

Platform: `<platform-name>`
Hosting model: `<self-hosted | Posit Connect | other>`
Scope: *to be completed*
Excluded: application-layer content and logic (validated per application).

## 2. Platform version and commit

<!-- Record the exact validated state. The report is valid only for the state
recorded here — later changes go through the impact assessment in section 6. -->

| Item | Value |
|------|-------|
| Source repository | `<platform-repo>` |
| Branch / commit hash | *to be completed* |
| Version tag (if any) | *to be completed* |
| Deployment date | *to be completed* |

## 3. Installation Qualification (IQ) — environment checks

<!-- Confirm the environment is installed as designed. Suggested checks:
- running service/container inventory matches the orchestration definition
  (self-hosted), or deployed content matches the intended manifest
  (commercial hosting)
- all services healthy / passing post-deployment health checks
- network exposure: only the intended HTTPS entry point is reachable;
  internal services are not directly reachable from the network
- TLS certificate valid and within its validity period
- log collection and rotation in effect
Record check, expected, observed, pass/fail per line. Describe locations by
role (e.g. "production server"), never by address. -->

| # | Check | Expected | Observed | Result |
|---|-------|----------|----------|--------|
| IQ-1 | Service inventory matches orchestration definition | | | |
| IQ-2 | Health endpoints / post-deployment checks pass | | | |
| IQ-3 | Network exposure limited to the HTTPS entry point | | | |
| IQ-4 | TLS certificate valid and in period | | | |
| IQ-5 | Log collection and rotation in effect | | | |

## 4. Operational Qualification (OQ) — functional checks

<!-- Confirm the platform functions as intended. Minimum coverage below; add
rows as needed. Reference evidence kept with the operational records
(screenshots, log excerpts) by location, not by pasting sensitive content. -->

| # | Test case | Procedure summary | Expected | Observed | Result |
|---|-----------|-------------------|----------|----------|--------|
| OQ-1 | Authentication interception: unauthenticated access to a protected application is redirected to `<company-sso>` / rejected | | | | |
| OQ-2 | Unauthorized access denied: an authenticated user requesting data outside their granted scope is denied by `<data-system-of-record>` | | | | |
| OQ-3 | Release visibility gate effectiveness: an application not released at Gate 4 is not reachable by end users | | | | |
| OQ-4 | Deploy + rollback drill: deploy, confirm health checks pass, roll back from the pre-deployment backup, confirm prior state restored | | | | |
| OQ-5 | Logging: application and access logs are written and retained per `<retention-policy>` | | | | |
| OQ-6 | Backup: automated pre-deployment backups exist and are restorable | | | | |

## 5. Conclusion and signatures

<!-- State the overall conclusion: platform fit for intended use, or deviations
with their disposition. Dual signature: the person who executed the
qualification and a distinct approver (per the governance model in
GOVERNANCE_TEMPLATE.md). -->

*Conclusion: to be completed.*

| Role | Name / handle | Date | Signature |
|------|---------------|------|-----------|
| Executed by (Platform Maintainer) | | | |
| Approved by | `<approver-role>` | | |

## 6. Revalidation triggers

<!-- This report remains in effect until a triggering event. For each platform
change, perform an impact assessment and re-validate only the impacted items
(partial revalidation); append the outcome below or issue a new revision. -->

Revalidation (full or impact-assessed partial) is triggered by:

- Change to the shared authentication mechanism or SSO integration flow.
- Change to the data-access channel (interface contract or permission model of
  `<data-system-of-record>`).
- Change to entry-point routing or the network exposure model.
- Change to the deployment pipeline, backup, or rollback mechanics.
- Vendor-side changes affecting authentication or authorization behavior
  (including hosting-platform upgrades on commercial hosting).
- Infrastructure changes affecting the production environment (OS, container
  runtime, certificate chain).
- Any security incident affecting the platform.

| Trigger event | Impact assessment | Items re-validated | Date | By |
|---------------|-------------------|--------------------|------|----|
| *to be completed* | | | | |
