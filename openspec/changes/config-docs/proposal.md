## Why

The `uf config` command group provides unified configuration management with 3 subcommands (`init`, `show`, `validate`) and a layered loading system covering 7 sections (setup, scaffold, embedding, sandbox, gateway, doctor, workflow). It also removed hardcoded model frontmatter from 24 agent files. None of this is documented on the website.

The `common-workflows.md` page already mentions `.uf/config.yaml` but doesn't document the `uf config` commands that manage it.

This change addresses [GitHub issue #58](https://github.com/unbound-force/website/issues/58).

## What Changes

A new documentation page for the `uf config` command covering:

1. **Command surface**: `init`, `show`, `validate` — with descriptions and usage examples
2. **Layered loading**: CLI flags > env vars > repo config (`.uf/config.yaml`) > user config (`~/.config/uf/config.yaml`) > compiled defaults, with precedence examples
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

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

Documents `uf config` commands sourced from the actual upstream implementation at `unbound-force/unbound-force/internal/config/`. The layered loading order (CLI flags > env vars > repo config > user config > compiled defaults) is verified against the source code comment in `config.go`. The 7 configuration sections are verified against the `Config` struct fields. All content must be cross-referenced with `uf config --help` and `uf config show` output at implementation time.

### II. Minimal Footprint

**Assessment**: PASS

Single Markdown page under the Reference section using Doks built-in sidebar navigation. No custom layouts, CSS, or JavaScript. The Reference section is being established by the `cli-reference-and-positioning` change (PR #73) — this change adds a second page to that section, justifying its existence.

### III. Visitor Clarity

**Assessment**: PASS

Configuration is a reference topic — placing it alongside the CLI reference in the Reference section is intuitive. Cross-references from `common-workflows.md` (which already mentions `.uf/config.yaml`) ensure discoverability. The page ends with cross-links to related content.
