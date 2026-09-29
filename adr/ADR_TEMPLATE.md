# Architecture Decision Record — Template

Decision records capture *why* the platform works the way it does, so a
successor (or an auditor) can understand a decision without re-litigating it.
Keep each record compressed: one sentence each for Context, Decision, and
Rationale. If a decision needs more prose than that, the decision is probably
two decisions.

**Sanitization reminder:** decision records in a shared or governance
repository refer to people by role (Platform Maintainer, App Author,
infrastructure team, vendor contact), never by personal name, and contain no
hostnames, addresses, ports, paths, commands, or credentials. Operational
detail belongs in privately maintained documentation.

Copy the block below to `adr/NNNN-<short-title>.md`.

---

# ADR-NNNN — `<short-title>`

- **Status:** proposed | accepted | superseded by ADR-NNNN
- **Date:** YYYY-MM-DD

- **Context:** *One sentence: the situation and forces that made a decision
  necessary.*
- **Decision:** *One sentence: what was decided.*
- **Rationale:** *One sentence: why this option over the alternatives.*
- **Consequences:** *(Optional) what becomes easier, what becomes harder, what
  this decision rules out.*
