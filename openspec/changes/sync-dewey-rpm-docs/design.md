## Context

Dewey v0.16.0 adds RPM package distribution for Fedora, RHEL, and CentOS (dewey#59). The upstream README already documents this installation method. The website has two pages with Dewey installation instructions that need updating:

1. `content/docs/projects/dewey.md` — Project overview page with a concise installation section
2. `content/docs/getting-started/knowledge.md` — Detailed getting-started guide with full installation walkthrough

Both pages currently document Homebrew (macOS) and `go install` (source). The RPM method fills the gap for Linux users on RPM-based distributions who want a binary install without building from source.

Per the proposal, all constitution principles are N/A — this is a documentation-only change with no impact on agent behavior, composability, or observable output.

## Goals / Non-Goals

### Goals
- Add RPM installation instructions to both Dewey documentation pages
- Position RPM section between Homebrew and "install from source" sections (matching the upstream README's ordering)
- Cover: download from GitHub Releases, `dnf install` command, architecture availability (`amd64`/`arm64`), install path (`/usr/bin/dewey`)
- Match the tone and detail level of each page (concise on project page, more detailed on getting-started page)

### Non-Goals
- Adding Debian/APT package instructions (not yet supported upstream)
- Modifying the Homebrew or source install sections
- Adding RPM instructions to any pages beyond the two identified
- Linking to specific release versions (use `<version>` placeholder)

## Decisions

### D1: RPM section placement

**Decision**: Insert RPM section after Homebrew, before "install from source" on both pages.

**Rationale**: This matches the upstream Dewey README ordering (Homebrew → RPM → source) and presents installation methods from most convenient to most manual.

### D2: Use `<version>` placeholder in download URL

**Decision**: Use `dewey_<version>_linux_amd64.rpm` with a placeholder rather than hardcoding a specific version.

**Rationale**: Hardcoded versions go stale. The placeholder directs users to check GitHub Releases for the latest version, which is always correct.

### D3: Detail level varies by page

**Decision**: The project page (`dewey.md`) gets a minimal RPM block (download + install command). The getting-started page (`knowledge.md`) gets a slightly more detailed section with architecture note and install path.

**Rationale**: The project page is an overview; the getting-started page is a walkthrough. Each page's RPM section should match the existing detail level of its sibling install sections.

## Risks / Trade-offs

- **Low risk**: Content-only Markdown changes. No build, layout, or navigation impact.
- **Version placeholder**: Users must look up the current version on GitHub Releases. This is acceptable — the alternative (hardcoding a version) creates maintenance burden and goes stale.
- **No automated link checking**: The GitHub Releases URL pattern is not validated by the build. If the release naming convention changes, the documented URL will be wrong. This is a pre-existing pattern (the upstream README has the same constraint).
