# ADR-0005 — Tier documentation by disclosure level

- **Status:** accepted (example)
- **Date:** YYYY-MM-DD

- **Context:** Governance records must be readable by managers and auditors,
  but the same repository must not become a map of the production environment
  (addresses, paths, commands, credentials).
- **Decision:** Documentation is split into three disclosure tiers: privately
  maintained operational documentation (full detail, need-to-know), the shared
  application repository (code plus validation evidence), and a broadly
  readable governance repository (roles, gates, decisions, validation status —
  sanitized by rule).
- **Rationale:** Oversight fails when records are locked away with the servers,
  and security fails when operational detail circulates widely; separating the
  two lets each audience read exactly what it needs.
- **Consequences:** Every document in the governance tier follows the
  sanitization rules (roles not names; no hostnames, ports, paths, commands,
  or credentials); when in doubt whether something belongs in the governance
  tier, it stays in the operational tier.
