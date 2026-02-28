# Quick Start: Documentation Content Pages

**Branch**: `002-documentation-content` | **Date**: 2026-02-27 | **Spec**: [spec.md](spec.md) | **Plan**: [plan.md](plan.md)

## Prerequisites

- Node.js >= 20.11.0
- Go >= 1.23 (for Hugo module resolution)
- Dependencies installed: `npm install`
- Access to source repos: `unbound-force/unbound-force` and `unbound-force/gaze`

## Implementation Order

### 1. Create Directory Structure

```bash
mkdir -p content/docs/getting-started
mkdir -p content/docs/projects
mkdir -p content/docs/team
mkdir -p content/docs/contributing
```

### 2. Docs Root Index

Create `content/docs/_index.md` — minimal frontmatter, no body content.

### 3. Getting Started Section (P1)

Create 2 pages sourced from `unbound-force/unbound-force.md`:

- `content/docs/getting-started/_index.md` — "What is Unbound Force?" adapted from overview
- `content/docs/getting-started/quick-start.md` — covers Specify, OpenCode, and Swarm equally

### 4. Projects Section (P1)

Create 2 pages:

- `content/docs/projects/_index.md` — brief project listing
- `content/docs/projects/gaze.md` — Gaze project page focused on `/gaze` OpenCode command, cross-links to blog article

### 5. Team Section (P2)

Create 6 pages sourced from `unbound-force/unbound-force.md` "The Heroes":

- `content/docs/team/_index.md` — heroes intro + composite image (`/images/team/the-unbound-force.jpeg`)
- 5 persona pages, each with trading card image from `static/images/team/`

### 6. Contributing Section (P2)

Create 1 page:

- `content/docs/contributing/_index.md` — guidelines from AGENTS.md conventions

### 7. Re-enable "Get Started" Button

Uncomment the `[doks.navbar.button]` section in `config/_default/params.toml`:

```toml
[doks.navbar.button]
  enable = true
  text = "Get Started"
  url = "/docs/getting-started/"
```

### 8. Verification

```bash
npm run build              # Must succeed with zero errors
npm run dev                # Visually verify:
                           #   - All 12 pages render
                           #   - Sidebar shows all sections in order
                           #   - Getting Started -> Quick Start navigation works
                           #   - Gaze page shows /gaze command, links to blog
                           #   - Team pages display trading card images
                           #   - Team index shows composite image
                           #   - Contributing page has GitHub links
                           #   - Dark mode renders correctly
                           #   - All internal links work
```
