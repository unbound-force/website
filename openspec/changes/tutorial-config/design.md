## Context

The website has a config reference page (PR #75) covering the command surface, layered loading, and 7 config sections. Issue #67 requests a tutorial that walks through customization scenarios. The reference page answers "what can I configure?" — the tutorial answers "how do I configure this for my setup?"

## Goals / Non-Goals

### Goals
- Create a hands-on tutorial for common customization scenarios
- Show `uf config init` → edit → `uf config validate` → `uf config show` workflow
- Include platform-specific examples (Fedora/RHEL, macOS, custom embedding models)
- Demonstrate precedence with a concrete override scenario

### Non-Goals
- Duplicate the full config reference (all 7 sections in detail) — link to the reference page
- Cover every config key — focus on the most commonly customized settings
- Cover sandbox or gateway config in depth (those have their own reference pages)

## Decisions

1. **Page location**: `content/docs/getting-started/config-tutorial.md` — tutorials belong in Getting Started. Weight after common-workflows and code-review-tutorial.

2. **Scenario-driven structure**: Each section after the setup is a "How do I...?" scenario with a problem, a YAML snippet, and a verification step. This is more useful than walking through sections sequentially.

3. **Precedence example**: Show a concrete case where user config sets one value and repo config overrides it. Use `uf config show` output to prove the merge.

4. **Frontmatter**: Standard docs page frontmatter (title, description, lead, date, weight, toc).

5. **Cross-references**: Link to config reference page (`/docs/reference/config/`) if it exists at implementation time.

## Content Sources

Authoritative upstream sources:
- Config implementation: `unbound-force/unbound-force/internal/config/config.go`
- CLI help: `uf config --help`, `uf config init --help`, `uf config show --help`, `uf config validate --help`
- Config reference page: `content/docs/reference/config.md` (if PR #75 merged)

All config keys and section names must be verified against the upstream implementation at implementation time.

## Risks / Trade-offs

- **Risk**: Config sections may expand with new features, making the tutorial stale. Mitigated by focusing on stable, commonly-used settings rather than exhaustive coverage.
- **Trade-off**: Scenario-driven vs. sequential walkthrough. Chose scenarios — they answer the reader's actual question ("how do I change X?") rather than forcing them to read through unrelated sections.
