## Why

The configuration reference page (PR #75) documents the `uf config` command surface and layered loading model. But engineers who want to customize their environment — especially those on non-macOS platforms or with non-default embedding models — need a tutorial that walks through common customization scenarios step by step: create a config file, understand the sections, set platform-specific values, validate, and verify.

This change addresses [GitHub issue #67](https://github.com/unbound-force/website/issues/67).

## What Changes

A new tutorial page at `content/docs/getting-started/config-tutorial.md` covering:

1. **Prerequisites**: `uf` CLI installed
2. **Creating a config file**: `uf config init` walkthrough
3. **Understanding the 7 sections**: setup, scaffold, embedding, sandbox, gateway, doctor, workflow
4. **Layered loading**: 5-tier precedence with concrete examples
5. **Common customizations**: Fedora/RHEL package manager, embedding model, gateway port, skip list
6. **Validating and viewing config**: `uf config validate` and `uf config show`

## Capabilities

### New Capabilities
- `docs/getting-started/config-tutorial`: Tutorial page for environment customization with uf config

### Modified Capabilities
- None

### Removed Capabilities
- None

## Impact

- 1 new tutorial page (Markdown content)
- No layout, style, or configuration changes
- Cross-references to config reference docs (PR #75) and getting-started pages

## Constitution Alignment

Assessed against the Unbound Force website project constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

All commands, config keys, and section names will be verified against `uf config --help`, `uf config show`, and the upstream implementation at `unbound-force/unbound-force/internal/config/` at implementation time. The layered loading precedence (CLI flags > env vars > repo config > user config > compiled defaults) was verified in the config-docs change (PR #75).

### II. Minimal Footprint

**Assessment**: PASS

Single Markdown tutorial page under the existing Getting Started section. No custom layouts, CSS, JavaScript, or dependencies.

### III. Visitor Clarity

**Assessment**: PASS

Step-by-step tutorial with copy-pasteable YAML examples for each customization scenario. The precedence examples give visitors a concrete mental model of how config layers merge.
