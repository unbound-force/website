# Quickstart: Site Scaffold and Deployment

**Feature**: 001-site-scaffold | **Date**: 2026-02-23

## Prerequisites

- **Node.js** >= 20.11.0 (`node --version`)
- **Go** >= 1.23 (`go version`) -- required for Hugo module resolution
- **Git** (for cloning and CI/CD)

## Setup (after implementation)

```bash
# Clone the repository
git clone https://github.com/unbound-force/website.git
cd website

# Install dependencies
npm install

# Start development server
npm run dev
# → Site available at http://localhost:1313/
```

## Available Commands

| Command | Purpose |
|---------|---------|
| `npm run dev` | Start dev server with live reload at localhost:1313 |
| `npm run build` | Production build (output in `public/`) |
| `npm run preview` | Preview production build locally |
| `npm run format` | Format all files with Prettier |

## Project Layout

```
config/_default/     Hugo configuration (hugo.toml, params.toml, module.toml, menus/)
content/_index.md    Homepage frontmatter
layouts/home.html    Custom homepage template
assets/scss/common/  Brand colors and custom styles
static/              Favicons, CNAME file
```

## Key Files to Modify

| Task | File(s) |
|------|---------|
| Change site title or base URL | `config/_default/hugo.toml` |
| Change brand colors | `config/_default/params.toml` + `assets/scss/common/_variables-custom.scss` |
| Edit homepage content | `layouts/home.html` (template) + `content/_index.md` (frontmatter) |
| Edit navigation | `config/_default/menus/menus.en.toml` |
| Add custom styles | `assets/scss/common/_custom.scss` |

## Deployment

Automatic via GitHub Actions on push to `main`:
1. Workflow installs Node.js 22 + Hugo 0.155.1 (extended)
2. Runs `npm ci` then `hugo --minify --gc`
3. Deploys `public/` to GitHub Pages
4. Site available at `https://unboundforce.dev/`

## Verification Checklist

After implementation, verify:

- [ ] `npm install` completes without errors
- [ ] `npm run build` produces `public/` without errors or warnings
- [ ] `npm run dev` starts server at localhost:1313
- [ ] Homepage shows hero, features, projects, and CTA sections
- [ ] Dark mode toggle works correctly
- [ ] No external font requests in browser network tab
- [ ] Lighthouse performance score >= 90
