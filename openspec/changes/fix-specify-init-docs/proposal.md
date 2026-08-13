## Why

Issue [#214](https://github.com/unbound-force/website/issues/214): `uf init` now invokes `specify init --here --integration opencode --offline` instead of bare `specify init` (upstream change: [unbound-force/unbound-force#216](https://github.com/unbound-force/unbound-force/issues/216)). The website documentation must reflect this behavioral change to remain accurate.

The key behavioral differences are:

- **`--here`**: Constrains `.specify/` creation to the current directory (no parent directory search)
- **`--integration opencode`**: Explicitly targets OpenCode as the integration
- **`--offline`**: Non-interactive mode, no network calls during initialization

Currently, the Sub-Tool Initialization section (`developer.md`, lines 317-327) does not mention specify initialization at all — it lists config.yaml creation, Dewey init, and opencode.json configuration but omits the specify scaffolding step. This is a content gap, not a content error.

## What Changes

### New Capabilities
- None — no new website features

### Modified Capabilities
- `developer.md Sub-Tool Initialization`: Add specify initialization as a documented sub-tool step, describing the behavioral model (offline, directory-scoped, OpenCode-targeted)
- `cli.md init section`: Add a note that `uf init` also initializes the Speckit specification framework via specify
- `common-workflows.md setup flow`: Update the sub-tool initialization summary to include specify initialization

### Removed Capabilities
- None

## Impact

**Files affected** (content changes only — no templates, SCSS, or configuration):

| File | Section | Change |
|------|---------|--------|
| `content/docs/getting-started/developer.md` | Sub-Tool Initialization (lines 317-327) | Add specify initialization bullet describing offline, directory-scoped behavior |
| `content/docs/reference/cli.md` | init section (lines 28-43) | Add mention that init also runs specify initialization |
| `content/docs/getting-started/common-workflows.md` | Setup flow (line 436) | Update sub-tool initialization summary to include specify |

**Framing guidance**: Document the behavioral change from the user's perspective — what `uf init` does differently — rather than exposing internal `specify init` CLI flags. Users run `uf init`, not `specify init` directly. If the internal invocation is mentioned, frame it as implementation context, not a user-facing command.

## Constitution Alignment

Assessed against the Unbound Force website constitution (`.specify/memory/constitution.md`).

### I. Content Accuracy

**Assessment**: PASS

This change is directly required by Principle I: "When upstream repository content changes, the corresponding website pages MUST be reviewed and updated to stay in sync." The `uf init` behavioral change is a verifiable upstream change that the docs must reflect.

### II. Minimal Footprint

**Assessment**: PASS

The change is scoped to updating existing Markdown content in 3 files. No new pages, templates, SCSS, JavaScript, or dependencies. Uses the existing documentation structure — adds a bullet point to the Sub-Tool Initialization section rather than creating new sections or pages.

### III. Visitor Clarity

**Assessment**: PASS

The update improves visitor understanding of what `uf init` does during sub-tool initialization. Framing the specify step at the behavioral level (offline, directory-scoped) rather than exposing raw CLI flags serves the visitor's needs without adding implementation noise.


