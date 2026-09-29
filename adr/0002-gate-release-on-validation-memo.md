# ADR-0002 — Gate release on the validation memo

- **Status:** accepted (example)
- **Date:** YYYY-MM-DD

- **Context:** On a shared platform, merging application code and exposing the
  application to end users are easy to conflate into one event.
- **Decision:** Applications classified `gxp-support` or `gxp-critical` become
  reachable by end users only after the Platform Maintainer performs an
  explicit release action (Gate 4), which requires a completed and reviewed
  validation memo.
- **Rationale:** Making visibility a separate, deliberate act means a merged
  but unvalidated application is inert — end users cannot reach it — so the
  validation state, not the merge order, decides what users see.
- **Consequences:** App Authors cannot self-release; every release of a
  regulated-tier application leaves an approval record tied to a memo and a
  commit hash.
