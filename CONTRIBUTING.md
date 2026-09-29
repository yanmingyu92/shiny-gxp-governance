# Contributing

This repository is a **methodology template**, not software. Contributions
improve the framework: clearer procedures, better templates, additional worked
decision records, corrections.

## How to propose changes

- **Issues** for questions, gaps, or disagreements with the methodology.
- **Pull requests** for concrete changes. Keep PRs focused: one document, one
  concern. For material changes to the governance model itself, include a new
  decision record in `adr/` (using `adr/ADR_TEMPLATE.md`) explaining the
  context and rationale.

## Style rules

- Plain, declarative, precise language. No marketing register, no emoji.
- The audience is not assumed to be GxP experts: explain a regulated-work
  concept briefly where a newcomer needs it, then move on.
- Use `<...>` placeholder tokens for anything organization-specific. Templates
  must never ship filled-in examples from a real organization.
- Keep terminology consistent across files: role names (Platform Maintainer,
  App Author, QC Programmer, End User), tier names (`exploratory`,
  `gxp-support`, `gxp-critical`), the status lifecycle (`dev → validating →
  validated`), and the gate names (Gate 1–4) are fixed vocabulary — do not
  introduce synonyms.
- Cross-reference between documents rather than repeating content.

## Sanitization rules for contributions

Contributions must not contain:

- Hostnames, IP addresses, ports, file-system paths, environment-variable
  values, operational commands, or credentials.
- Vendor-internal configuration or proprietary platform details.
- Company names, product/compound/study identifiers, or app names from a real
  deployment.
- Personal names, other than the project author where attribution applies.
  Refer to people by role.

If you are describing a practice from a real deployment, generalize it first.
If you extrapolate beyond established practice, mark it inline
(e.g. "(extension beyond the source practice)").

## Scope rules

- In scope: governance models, validation methodology, QC procedures, record
  templates, decision records, regulatory-expectation mappings.
- Out of scope: platform implementations, deployment tooling, application
  code. This repository deliberately contains no executable software.
- No claims of regulatory endorsement, certification, or compliance conferred;
  no invented metrics or adoption claims.
