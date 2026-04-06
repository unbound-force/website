---
title: "The Divisor"
description: "The Divisor is the PR Reviewer Council — eight sub-personas for code review and content creation, serving as the Code Integrity Guardian."
lead: "The Architectural Conscience and Code Integrity Guardian"
date: 2026-02-23T00:00:00+00:00
draft: false
weight: 50
toc: true
---

![The Divisor — PR Reviewer Council trading card](/images/team/the-divisor.png)

## The Code Integrity Guardian

The Divisor is the PR Reviewer of the Unbound Force swarm — the architectural conscience and code integrity guardian. Their mission is to ensure that all code changes proposed by Cobalt-Crush, once validated by Gaze, adhere strictly to established architectural standards, best practices, security policies, and maintainability requirements.

The Divisor acts as the final technical gate before integration, translating high-level architectural standards into actionable review criteria. What makes The Divisor unique is its structure: it operates as a **council of eight dynamically discovered personas** -- eight canonical roles ship by default (five review, three content creation), with users able to add or remove personas freely. Each provides a different lens on every code submission.

## The Council

### The Guard — Intent and Cohesion

The Guard focuses on the _"Why"_ of the code. They ensure the pull request:

- Is not redundant
- Adheres to the original user intent and acceptance criteria
- Does not introduce feature bloat (**Zero-Waste Mandate**)
- Is cohesive and does not negatively impact adjacent modules (**Neighborhood Rule**)

### The Architect — Structure and Sustainability

The Architect focuses on the _"How"_ of the code. They perform structural and architectural reviews:

- Verifying adherence to strategic architecture principles (DRY, SOLID)
- Assessing long-term maintainability
- Preventing the introduction of technical debt
- Ensuring consistency with the approved plan and project conventions

### The Adversary — Resilience and Security

The Adversary focuses on _"Where it breaks."_ They act as a skeptical auditor:

- Seeking logical loopholes and edge cases
- Identifying security vulnerabilities (SQLi, XSS, insecure mTLS)
- Finding performance bottlenecks (O(n^2) loops, excessive API calls)
- Enforcing behavioral constraints and robust error handling

### The Operator (SRE) — Deployment and Operational Readiness

The Operator focuses on _"Can it ship and run?"_ They ensure the code is deployable, maintainable, and operable:

- Reviewing release pipeline integrity and reproducible builds
- Assessing dependency health (pinned versions, known CVEs, unused dependencies)
- Verifying configuration hygiene (no hardcoded paths, externalized secrets)
- Ensuring runtime observability (meaningful exit codes, actionable error messages, structured output)
- Evaluating upgrade and migration paths for breaking changes

### The Tester — Test Quality and Testability

The Tester focuses on _"Are the tests meaningful?"_ They ensure tests are not just present but valuable:

- Evaluating test architecture (arrange/act/assert structure, self-contained fixtures, table-driven tests)
- Assessing coverage strategy (contract surface over line coverage, observable side effects verified)
- Checking assertion depth (specific expected values, not just "no error" or nil checks)
- Verifying test isolation (no shared mutable state, no execution order dependencies, no external network access)
- Ensuring regression protection (spec-critical behavior locked down, known-good and known-bad scenarios covered)

## Content Creation Personas

Beyond code review, The Divisor includes three content creation personas that apply the same council-based quality model to written output. Each operates at a different temperature to match its domain — lower temperatures for precision-critical documentation, higher temperatures for creative public communication.

### The Scribe — Technical Documentation

The Scribe focuses on _"Does the documentation match the code?"_ They produce and review technical documentation with precision:

- READMEs, specification artifacts, CLI help text, and API reference documentation
- Ensuring documentation claims are verifiable against the actual codebase
- Maintaining terminology consistency across all documentation pages
- Flagging stale content that no longer reflects the current implementation

Temperature: **0.1** (precision). Technical documentation demands accuracy over creativity — every claim must be traceable to code.

### The Herald — Blog Posts and Announcements

The Herald focuses on _"Does this tell a compelling technical story?"_ They produce and review narrative content:

- Blog posts, release notes, changelogs, and feature announcements
- Structuring content with a narrative arc: problem, approach, evidence, conclusion
- Balancing technical depth with accessibility for a mid-level developer audience
- Avoiding time-sensitive language that becomes stale ("recently," "new," "just launched")

Temperature: **0.4** (controlled creativity). Blog posts need enough creative latitude to engage readers while staying grounded in technical accuracy.

### The Envoy — Public Relations and Communications

The Envoy focuses on _"Does this build trust?"_ They produce and review public-facing communications:

- Press releases, social media posts, community updates, and partnership announcements
- Calibrating claims to match the project's actual maturity level — never overstating capabilities
- Building credibility through honest scoping and transparent limitation acknowledgment
- Adapting tone for different audiences while maintaining the Unbound Force voice

Temperature: **0.5** (most creative). Public communications benefit from the most creative latitude, but The Envoy's credibility constraints prevent overclaims.

## Exclusive Ownership Boundaries

Each review dimension is owned by exactly one Divisor persona, eliminating duplicate findings across personas:

| Dimension                                   | Owner     |
| ------------------------------------------- | --------- |
| Secrets/credentials                         | Adversary |
| Dependency CVEs/supply chain                | Adversary |
| Error handling/resilience                   | Adversary |
| Test isolation/coverage/depth               | Tester    |
| Plan alignment/intent drift                 | Guard     |
| Zero-waste mandate                          | Guard     |
| Constitution alignment                      | Guard     |
| File permissions/hardcoded config           | SRE       |
| Efficiency/performance (O(n²), allocations) | SRE       |
| Architectural patterns/conventions          | Architect |

This ownership model ensures that a finding like "missing error handling" is raised by exactly one persona (Adversary), not duplicated across Adversary and Architect.

## Shared Severity Standard

All eight personas share a calibration standard defined in the `severity.md` convention pack (`.opencode/unbound/packs/severity.md`):

| Level        | Definition                                                                           | Auto-Fix?                    |
| ------------ | ------------------------------------------------------------------------------------ | ---------------------------- |
| **CRITICAL** | Data loss, security breach, build failure, constitutional violation. MUST NOT merge. | Report only                  |
| **HIGH**     | Significant risk or tech debt. Blocks review.                                        | Report only                  |
| **MEDIUM**   | Quality issue, should be addressed. Non-blocking.                                    | Auto-fix in Spec Review Mode |
| **LOW**      | Minor style or documentation improvement. Non-blocking.                              | Auto-fix in Spec Review Mode |

Each level includes domain-specific examples per persona — for example, an Adversary CRITICAL is "hardcoded production secret" while a Guard CRITICAL is "constitutional violation."

## Prior Learnings Integration

All eight Divisor personas include a "Prior Learnings" step at the start of their review workflow:

1. Search Dewey for learnings tagged with the repo name and relevant file paths
2. Include found learnings as prior knowledge in the review context
3. Graceful degradation when Dewey is not available — the step is skipped with an informational note

This means the review council learns from past reviews. If a particular pattern has been flagged before, the relevant persona will reference that history when evaluating new code.

## Collective Approval

The Divisor holds the ultimate authority to approve and merge code into the main branch. Approval requires collective consensus — **no outstanding REQUEST CHANGES from any persona** is mandatory. Gaze's functional validation serves as a prerequisite: the CI pipeline must be green before The Divisor begins review.

This structure ensures that only code which passes all eight lenses — intent, architecture, resilience, operational readiness, test quality, documentation accuracy, narrative clarity, and public communication — is deployed.

## How The Divisor Serves the Team

**For Developers (Cobalt-Crush):** Cobalt-Crush relies on The Divisor for the final technical sign-off. The multi-faceted feedback from the eight personas provides a holistic critique covering intent, structure, security, operational readiness, test quality, and content accuracy. Clear architectural standards minimize uncertainty, allowing Cobalt-Crush to code with confidence.

**For the Tester (Gaze):** Gaze relies on The Divisor to establish the architectural and non-functional requirements against which testability must be measured. Gaze's green light on functional tests is the entry criterion for The Divisor's review, ensuring review time is spent on high-level risk and structure rather than defect finding.

**For the Product Owner and Manager (Muti-Mind and Mx F):** Muti-Mind and Mx F are assured that every merged feature is technically sound, secure, and scalable. The Divisor acts as the ultimate guarantor of technical quality, ensuring that the velocity gained through prioritization and execution is sustainable and does not accrue crippling technical debt.

## Next Steps

- See [Common Workflows: Code Review](/docs/getting-started/common-workflows/#code-review) for how to invoke the review council and the review loop
- Read the [Developer getting-started guide](/docs/getting-started/developer/#convention-packs) for convention packs that The Divisor enforces
- Learn about [hero artifacts](/docs/getting-started/artifacts/) -- The Divisor produces `review-verdict` artifacts consumed by other heroes
