# ADR-0001 — Delegate data authorization to the system of record

- **Status:** accepted (example)
- **Date:** YYYY-MM-DD

- **Context:** The platform needs to ensure users only see data they are
  permitted to see, and the data system of record already maintains per-user
  permissions.
- **Decision:** The platform keeps no permission database of its own; every
  data request is authorized server-side by the data system of record, and the
  platform merely passes the user's session credential along.
- **Rationale:** A copied ("shadow") permission store inevitably drifts from
  the source of truth and must be audited in two places; delegating means
  revoking a user's access takes effect on their next request with no
  platform-side change.
- **Consequences:** The platform's release/visibility gate (Gate 4) controls
  only which applications are reachable, never which data a user receives;
  platform validation can treat data authorization as an external, separately
  governed control.
