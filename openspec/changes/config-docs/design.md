## Context

The `uf config` command group was added via the `opsx/unified-config` change. It unified 7 previously separate configuration concerns into a single config struct with layered loading. The website's `common-workflows.md` page already mentions `.uf/config.yaml` but only in the context of workflow configuration — it doesn't document the `uf config` commands themselves.

## Goals / Non-Goals

### Goals
- Document all 3 subcommands (`init`, `show`, `validate`) with usage examples
- Explain the layered loading system with precedence rules
- Describe the 7 configuration sections with key settings
- Provide common customization examples (different platforms, models, ports)
- Update `common-workflows.md` to reference `uf config` commands

### Non-Goals
- Exhaustive documentation of every config key (the generated template from `uf config init` serves this purpose)
- Tutorial-style walkthrough (tracked in GitHub issue #67: Configuration Tutorial)
- Config migration guide from old `.uf/sandbox.yaml` format

## Decisions

1. **Single page under reference section**: Place at `content/docs/reference/config.md`. Configuration is a reference topic.

2. **Section overview as a table**: Use a table showing each of the 7 sections with purpose and key settings. Users can scan to find what they need.

3. **Precedence diagram**: Show the 5-tier loading order (CLI flags > env vars > repo config > user config > compiled defaults) with a clear "wins" indicator. This is the most common source of confusion. The order is verified against the `config.go` source code comment.

4. **Common customizations as code blocks**: Show before/after YAML snippets for the 4 most common customizations: package manager, embedding model, gateway port, skip list.

## Risks / Trade-offs

- **Risk**: Config sections may expand as new features are added. Table structure allows easy extension.
- **Trade-off**: Light reference vs. deep tutorial. Chose reference — the tutorial is a separate spec (issue #67).

## Content Sources

Authoritative upstream source files for documentation content:
- Config struct and loading: `unbound-force/unbound-force/internal/config/config.go`
- Config init template: `unbound-force/unbound-force/internal/config/template.go`
- Config validation: `unbound-force/unbound-force/internal/config/validate.go`
- CLI help: `uf config --help`, `uf config init --help`, `uf config show --help`, `uf config validate --help`

The precedence order (CLI flags > env vars > repo config > user config > compiled defaults) is documented in the `config.go` package comment and must be verified at implementation time.
