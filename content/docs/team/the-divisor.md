---
title: "The Divisor"
description: "The Divisor is the PR Reviewer Council — nine canonical sub-personas for code review and content creation, plus two repo-local instruction reviewers."
lead: "The Architectural Conscience and Code Integrity Guardian"
date: 2026-02-23T00:00:00+00:00
draft: false
weight: 50
toc: true
---

![The Divisor — PR Reviewer Council trading card](/images/team/the-divisor.png)

## The Code Integrity Guardian

The Divisor is the PR Reviewer of the Unbound Force swarm — the architectural conscience and code integrity guardian. Their mission is to ensure that all code changes proposed by Cobalt-Crush, once validated by Gaze, adhere strictly to established architectural standards, best practices, security policies, and maintainability requirements.

The Divisor acts as the final technical gate before integration, translating high-level architectural standards into actionable review criteria. What makes The Divisor unique is its structure: it operates as a **council of dynamically discovered personas** -- nine canonical roles ship by default (six review, three content creation), with users able to add or remove personas freely. The `unbound-force/unbound-force` repository adds two repo-local instruction reviewers, [The Linguist and The Determinist](#repo-local-personas), for a council of 11. Each persona provides a different lens on every code submission.

## Why a Council?

A single AI agent reviewing its own code — or even a single separate agent reviewing another's code — misses issues that only become visible from a different perspective. Security experts catch vulnerabilities that architects overlook. Test quality specialists identify coverage gaps that SREs miss. The council model exists because no single evaluative lens is sufficient for production-quality code.

The Divisor's architecture reflects two design principles. First, **doer/judge separation**: the Guard agent operates at temperature 0.1 with tool access restrictions (`write: false`, `edit: false`, `bash: false`), making it structurally unable to modify code. This is not a behavioral instruction — it is an architectural constraint. The judge physically cannot fix what it finds, forcing all findings through the normal implementation pipeline with full visibility.

Second, **layered feedback ordering**: computational checks (CI pipeline, [Gaze](/docs/team/gaze-tester/) static analysis) run before the Divisor Council reviews. Fast, deterministic, cheap checks filter out obvious issues before slower, more expensive semantic review begins. The council's time is spent on judgment calls, not compilation errors.

## The Council

### The Guard — Intent and Cohesion

The Guard focuses on the _"Why"_ of the code. They ensure the pull request:

- Is not redundant
- Adheres to the original user intent and acceptance criteria
- Does not introduce feature bloat (**Zero-Waste Mandate**)
- Is cohesive and does not negatively impact adjacent modules (**Neighborhood Rule**)
- **Gatekeeping Integrity**: Checks whether any quality gates, thresholds, severity definitions, or governance rules were modified to make an implementation pass. Modifications to gates without human authorization are CRITICAL-severity findings.

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
- **Gate Tampering**: Detects unauthorized modifications to coverage thresholds, CRAP scores, convention pack classifications, CI flags, agent temperature/tool-access settings, review iteration limits, or workflow gate markers.

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

## Triage Persona

### The Curator — Documentation & Content Pipeline Triage

The Curator focuses on _"Is the documentation keeping up with the code?"_ It detects documentation gaps during every code review and files tracking issues for the content agents to fill:

- Checking whether AGENTS.md and README are updated when user-facing code changes
- Filing `docs`-labeled GitHub issues in the website repository for missing documentation
- Identifying blog-worthy changes and filing `blog`-labeled issues
- Identifying tutorial-worthy workflows and filing `tutorial`-labeled issues
- Checking for duplicate issues before filing new ones

Temperature: **0.2** (judgment-based). The Curator makes classification decisions (is this change user-facing? does documentation exist?) that benefit from consistent, conservative judgment.

The Curator is the first Divisor persona with bash access, restricted to `gh issue list` and `gh issue create`. The Adversary's "Gate Tampering" check monitors for bash misuse — a trust architecture pattern where minimum capability is granted and a peer agent verifies compliance.

**Important**: The Curator triages — it identifies what needs documenting and files tracking issues. It does not write documentation, blog posts, or communications. Those responsibilities belong to The Scribe, The Herald, and The Envoy respectively.

## Repo-Local Personas

A repository can add its own personas next to the canonical nine. The `unbound-force/unbound-force` repository adds two that review the instruction text its agents execute: commands, agents, skills, convention packs, and `AGENTS.md`. Both depend on tooling that lives only in that repository (the instruction linter, the determinism pack, and `tool-inventory`), so `uf init` does not deliver them. With them, the council in that repository has 11 personas: 9 canonical and 2 repo-local.

### The Linguist — Instruction Quality

The Linguist (`divisor-linguist`) focuses on _"Will an agent read this the way the author meant?"_ It reviews instruction prose the way a compiler reviews code: first with deterministic evidence from the instruction linter, then with judgment where no tool can decide:

- Reporting instruction-lint findings: frontmatter, references, RFC 2119 marker and body agreement, vague qualifiers, token budgets, and near-duplicate paragraphs
- Checking instruction clarity: when an artifact runs, what counts as done, where inputs come from, when to stop and ask, what happens on failure, and whether external text is treated as data
- Quoting the text for every finding, with the concrete misreading an executing agent could make

Temperature: **0.1** (precision). Its shell access is limited to the linter wrapper and read-only `git diff` and `git status`.

### The Determinist — Deterministic Execution

The Determinist (`divisor-determinism`) focuses on _"Does work that runs the same way every time live in a tool?"_ It owns determinism pack rules DE-001 through DE-008 and applies the twice-test to every changed step: if a competent agent follows the step twice and gets the same result, the step belongs in a tool.

- Flagging fixed procedures written as prose, output templates the agent is told to reproduce, and judgment-free chains of tool calls (DE-001, DE-003, DE-005)
- Checking that instruction changes record their line and token deltas (DE-004), using `instruction-lint --base`
- Checking that prose-to-tool migrations test their seams: registration, documented output shape, and round trips (DE-007), using `tool-inventory --seams`
- Checking that delegated work carries its `task_classification` and skill contract through the delegation tool's parameters (DE-008), using the linter's IL-042 hint
- Naming the existing tool that should own each mechanical step, using `tool-inventory --match`

Temperature: **0.1** (precision). It cites tool output instead of re-deriving it, and its shell access is limited to the linter wrapper and `tool-inventory`. Read [The Determinist](/blog/the-determinist/) for why the review moved onto tool-backed evidence, and [Verifying Deterministic Instruction](/docs/tutorials/verifying-deterministic-instruction/) to run the same checks before you push.

## Exclusive Ownership Boundaries

Each review dimension is owned by exactly one Divisor persona, eliminating duplicate findings across personas:

| Dimension                                   | Owner                                                                                                                   |
| ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Secrets/credentials                         | Adversary                                                                                                               |
| Dependency CVEs/supply chain                | Adversary                                                                                                               |
| Error handling/resilience                   | Adversary                                                                                                               |
| Test isolation/coverage/depth               | Tester                                                                                                                  |
| Plan alignment/intent drift                 | Guard                                                                                                                   |
| Zero-waste mandate                          | Guard                                                                                                                   |
| Constitution alignment                      | Guard                                                                                                                   |
| File permissions/hardcoded config           | SRE                                                                                                                     |
| Efficiency/performance (O(n²), allocations) | SRE                                                                                                                     |
| Architectural patterns/conventions          | Architect                                                                                                               |
| Instruction lint and clarity (repo-local)   | Linguist                                                                                                                |
| Determinism rules DE-001–DE-008 (repo-local) | Determinist                                                                                                            |
| **The Curator**                             | Documentation & content pipeline triage — gap detection, website issue filing, blog/tutorial opportunity identification |

This ownership model ensures that a finding like "missing error handling" is raised by exactly one persona (Adversary), not duplicated across Adversary and Architect. The same split applies to instruction review: the linter's determinism hints (IL-040 to IL-042) go to The Determinist, and every other lint rule goes to The Linguist.

## Shared Severity Standard

All Divisor personas share a calibration standard defined in the `severity.md` convention pack (`.opencode/uf/packs/severity.md`):

| Level        | Definition                                                                           | Auto-Fix?                    |
| ------------ | ------------------------------------------------------------------------------------ | ---------------------------- |
| **CRITICAL** | Data loss, security breach, build failure, constitutional violation. MUST NOT merge. | Report only                  |
| **HIGH**     | Significant risk or tech debt. Blocks review.                                        | Report only                  |
| **MEDIUM**   | Quality issue, should be addressed. Non-blocking.                                    | Auto-fix in Spec Review Mode |
| **LOW**      | Minor style or documentation improvement. Non-blocking.                              | Auto-fix in Spec Review Mode |

Each level includes domain-specific examples per persona — for example, an Adversary CRITICAL is "hardcoded production secret" while a Guard CRITICAL is "constitutional violation."

## Prior Learnings Integration

All Divisor personas include a "Prior Learnings" step at the start of their review workflow:

1. Search Dewey for learnings tagged with the repo name and relevant file paths
2. Include found learnings as prior knowledge in the review context
3. Graceful degradation when Dewey is not available — the step is skipped with an informational note

This means the review council learns from past reviews. If a particular pattern has been flagged before, the relevant persona will reference that history when evaluating new code.

## Collective Approval

The Divisor holds the ultimate authority to approve and merge code into the main branch. Approval requires collective consensus — **no outstanding REQUEST CHANGES from any persona** is mandatory. Gaze's functional validation serves as a prerequisite: the CI pipeline must be green before The Divisor begins review.

This structure ensures that only code which passes every lens — intent, architecture, resilience, operational readiness, test quality, documentation governance, documentation accuracy, narrative clarity, and public communication, plus instruction quality and deterministic execution where the repo-local personas are enrolled — is deployed.

## How The Divisor Serves the Team

**For Developers (Cobalt-Crush):** Cobalt-Crush relies on The Divisor for the final technical sign-off. The multi-faceted feedback from the council's personas provides a holistic critique covering intent, structure, security, operational readiness, test quality, documentation governance, and content accuracy. Clear architectural standards minimize uncertainty, allowing Cobalt-Crush to code with confidence.

**For the Tester ([Gaze](/docs/team/gaze-tester/)):** Gaze relies on The Divisor to establish the architectural and non-functional requirements against which testability must be measured. Gaze's green light on functional tests is the entry criterion for The Divisor's review, ensuring review time is spent on high-level risk and structure rather than defect finding.

**For the Product Owner and Manager (Muti-Mind and Mx F):** Muti-Mind and Mx F are assured that every merged feature is technically sound, secure, and scalable. The Divisor acts as the ultimate guarantor of technical quality, ensuring that the velocity gained through prioritization and execution is sustainable and does not accrue crippling technical debt.

## Next Steps

- See [Common Workflows: Code Review](/docs/getting-started/common-workflows/#code-review) for how to invoke the review council and the review loop
- Read the [Developer getting-started guide](/docs/getting-started/developer/#convention-packs) for convention packs that The Divisor enforces
- Learn about [hero artifacts](/docs/getting-started/artifacts/) -- The Divisor produces `review-verdict` artifacts consumed by other heroes
