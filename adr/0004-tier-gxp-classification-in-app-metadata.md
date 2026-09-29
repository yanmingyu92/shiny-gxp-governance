# ADR-0004 — Declare the GxP tier in application metadata

- **Status:** accepted (example)
- **Date:** YYYY-MM-DD

- **Context:** Validation depth must scale with the risk of each application's
  output, but "how much validation did this app need" is otherwise tribal
  knowledge that lives in email and memory.
- **Decision:** Every application declares its tier (`exploratory`,
  `gxp-support`, or `gxp-critical`) in its machine-readable metadata file,
  proposed by the App Author and approved by the Platform Maintainer, with the
  legal value set enforced by CI.
- **Rationale:** A declared, machine-checkable field makes validation depth
  auditable at a glance and lets automation reject releases whose evidence does
  not match the declared tier.
- **Consequences:** Classification changes are code-reviewed like any other
  change; downgrades must carry a written rationale in the validation memo.
