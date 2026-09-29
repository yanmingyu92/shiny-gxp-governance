# ADR-0003 — Split validation into platform and application layers

- **Status:** accepted (example)
- **Date:** YYYY-MM-DD

- **Context:** Validating every application end-to-end — including
  authentication, deployment, and logging — would repeat the same platform
  checks in every memo, and any platform change would force re-validation of
  every application.
- **Decision:** The platform layer (authentication enforcement, data channel,
  deployment and rollback, logging, backups) is validated once by the Platform
  Maintainer and re-examined by impact assessment on platform change; each
  application is validated separately by its App Author for the correctness of
  what it displays, referencing the platform report rather than repeating it.
- **Rationale:** The split removes re-validation churn while sharpening
  accountability: platform-layer assurance is stable and owned centrally, and
  the correctness of every displayed number has exactly one named owner.
- **Consequences:** Application memos must cite the platform validation report
  version they rely on; platform changes require an impact assessment listing
  which platform items were re-validated.
