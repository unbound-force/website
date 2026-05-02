## Why

The `uf config` command group provides unified configuration management with 3 subcommands (`init`, `show`, `validate`) and a layered loading system covering 7 sections (setup, scaffold, embedding, sandbox, gateway, doctor, workflow). It also removed hardcoded model frontmatter from 24 agent files. None of this is documented on the website.

The `common-workflows.md` page already mentions `.uf/config.yaml` but doesn't document the `uf config` commands that manage it.

This change addresses GitHub issue #58.

## What Changes

A new documentation page for the `uf config` command covering:

1. **Command surface**: `init`, `show`, `validate` — with descriptions and usage examples
2. **Layered loading**: User config (`~/.config/uf/config.yaml`) > repo config (`.uf/config.yaml`) > env vars, with precedence examples
3. **Configuration sections**: The 7 sections with key settings and defaults
4. **Common customizations**: Platform-specific package manager, embedding model, gateway port, skip list

## Capabilities

### New Capabilities
- `docs/reference/config`: Comprehensive configuration documentation page

### Modified Capabilities
- `docs/getting-started/common-workflows`: Updated to reference `uf config` commands alongside existing `.uf/config.yaml` mentions

### Removed Capabilities
- None

## Impact

- 1 new content page
- 1 existing page updated (common-workflows.md cross-reference)

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

Documentation page — no effect on hero communication.

### II. Composability First

**Assessment**: PASS

Documents `uf config` as a standalone capability. The configuration system works independently — users can use `uf` without ever running `uf config` (defaults apply).

### III. Observable Quality

**Assessment**: PASS

Documents `uf config show` and `uf config validate` which provide observable, machine-parseable output of the effective configuration and validation results.

### IV. Testability

**Assessment**: N/A

No code changes. Validation is `npm run build` + visual review.
