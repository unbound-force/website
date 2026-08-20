## Why

Dewey now supports RPM-based installation for Fedora, RHEL, and CentOS (implemented in dewey#59). The upstream README documents this installation method, but the website's Dewey documentation pages still only cover Homebrew and `go install` from source. Users on RPM-based Linux distributions have no guidance on the website for installing Dewey via their native package manager.

This change syncs the website documentation with the upstream Dewey README, addressing GitHub issue unbound-force/website#186. The milestone is release v0.16.0 (due 2026-08-21).

## What Changes

Add an RPM installation section to two documentation pages, positioned between the existing Homebrew and "install from source" sections:

1. **`content/docs/projects/dewey.md`** — Add RPM install block after the Homebrew cask section
2. **`content/docs/getting-started/knowledge.md`** — Add RPM install block after the Homebrew cask section, before the Embedding Model Alignment section

The RPM section covers:
- Downloading the `.rpm` package from GitHub Releases
- Installing with `sudo dnf install ./dewey_<version>_linux_amd64.rpm`
- Noting both `amd64` and `arm64` architectures are available
- Binary installs to `/usr/bin/dewey`

## Capabilities

### New Capabilities
- `RPM install documentation`: Website documents RPM-based installation for Fedora/RHEL/CentOS users

### Modified Capabilities
- `Installation sections`: Both Dewey documentation pages gain a new installation method between Homebrew and source install

### Removed Capabilities
- None

## Impact

- **Files modified**: `content/docs/projects/dewey.md`, `content/docs/getting-started/knowledge.md`
- **No layout, navigation, or style changes** — content-only additions within existing page sections
- **No build impact** — Markdown content only, no template or config changes

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: N/A

This is a documentation-only change to static Markdown content. No agent artifacts, communication protocols, or runtime behavior is affected.

### II. Composability First

**Assessment**: N/A

No software components are added, removed, or modified. The change adds installation instructions for an already-composable tool (Dewey).

### III. Observable Quality

**Assessment**: N/A

No machine-parseable output or provenance metadata is affected. This is static website content.

### IV. Testability

**Assessment**: N/A

No testable components are introduced. Validation is manual: `npm run build` must succeed and the new sections must render correctly.

### V. Security by Default

**Assessment**: N/A

The documented `sudo dnf install` command uses standard system package management. No security-sensitive changes are introduced.
