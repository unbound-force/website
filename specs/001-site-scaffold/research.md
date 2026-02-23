# Research: Site Scaffold and Deployment

**Feature**: 001-site-scaffold | **Date**: 2026-02-23

## 1. Reference Implementation (complytime-website)

### Decision
Mirror the complytime-website (`complytime/website`, branch `feat/website-minimal`) architecture exactly, adapting only content, colors, and domain-specific values.

### Rationale
The complytime-website is a near-identical use case (open source org, Go tool repos, docs-heavy, Hugo + Doks). It is a proven, working implementation that has already solved the Thulite/Doks integration challenges (module mounts, homepage override, GitHub Pages deployment). Reusing this pattern eliminates guesswork.

### Alternatives Considered
- **Start from Doks quickstart**: Would require discovering module mount patterns, homepage override approach, and CI workflow independently. Higher risk of misconfiguration.
- **Use a different Hugo theme**: No benefit. Doks provides search, dark mode, sidebar nav, mobile responsiveness, and SEO out of the box.

### Key Findings

**Package dependencies** (from complytime-website `package.json`):
- `thulite ^2.6.3` (core framework)
- `@thulite/doks-core ^1.8.3` (docs theme)
- `@thulite/images ^3.3.1` (image processing)
- `@thulite/inline-svg ^1.2.0` (SVG icons)
- `@thulite/seo ^2.4.1` (SEO optimization)
- `@tabler/icons ^3.34.1` (icon set)
- `prettier ^3.6.2` (dev, formatting)
- `vite ^7.0.6` (dev, preview server)

**npm scripts**:
- `dev`: `hugo server --disableFastRender --noHTTPCache`
- `build`: `hugo --minify --gc`
- `preview`: `vite preview --outDir public`
- `format`: `prettier **/** -w -c`

**Hugo version**: 0.155.1 extended (installed via `peaceiris/actions-hugo` in CI)

**Go version**: 1.23 (in `go.mod`)

**go.mod dependency**: `gopkg.in/yaml.v3 v3.0.1` (required for Hugo module resolution)

---

## 2. Module Mount Architecture

### Decision
Use `config/_default/module.toml` with explicit mount declarations for all Thulite packages, with `excludeFiles = "home.html"` on the doks-core layouts mount.

### Rationale
Thulite/Doks packages install into `node_modules/` but Hugo needs them in its virtual filesystem. Module mounts bridge this gap. The `home.html` exclusion is mandatory when using a custom homepage layout to prevent conflicts with the Doks default.

### Alternatives Considered
- **Hugo Modules (Go-based)**: Would add Go module complexity and require `go get` commands. The npm-based mount approach is simpler and matches the Thulite ecosystem's intended workflow.

### Mount Categories
1. **content**: Local `content/` only
2. **data**: doks-core data + local data
3. **layouts**: Local layouts (first priority) + doks-core (excluding home.html) + thulite core + seo + images + inline-svg
4. **i18n**: doks-core i18n + local i18n
5. **archetypes**: doks-core archetypes + local archetypes
6. **assets**: thulite core + doks-core + tabler icons + images + local assets
7. **static**: doks-core static + local static

---

## 3. Homepage Template Pattern

### Decision
Use a custom `layouts/home.html` with three Hugo template blocks: `{{ define "main" }}`, `{{ define "sidebar-prefooter" }}`, and `{{ define "sidebar-footer" }}`.

### Rationale
These are the standard Doks block names for template inheritance. The `main` block contains the hero section (above the fold), `sidebar-prefooter` contains the feature/project cards, and `sidebar-footer` contains the closing CTA. This pattern is used by complytime-website and integrates cleanly with Doks' base template.

### Alternatives Considered
- **Markdown-only homepage**: Doks does not support a rich homepage layout via Markdown. The custom template is the documented approach.
- **Hugo shortcodes**: Would add complexity and fragment the homepage logic across multiple files.

### Content Sections (Unbound Force)
1. **Hero**: Badge ("Open Source AI Agent Swarm"), title, lead text, CTA buttons (Get Started + View on GitHub)
2. **Features** (4 cards): Agent Personas, Speckit Workflow, Quality-First, Open Source
3. **Projects** (1 card): Gaze -- side effect detection + CRAP scores for Go
4. **Closing CTA**: "Ready to try Unbound Force?" with action links

---

## 4. Gaze Project Description (Content Accuracy)

### Decision
Describe Gaze as: a static analysis CLI tool for Go that detects observable side effects in functions (P0-P2 tiers) and computes CRAP scores by combining cyclomatic complexity with test coverage.

### Rationale
This accurately reflects the current state of the Gaze repository. The description must NOT mention GazeCRAP (contract-aware coverage), transitive side effect analysis, or P3-P4 detection, as these are not yet implemented. Per Constitution Principle I (Content Accuracy), only implemented features may be described.

### Alternatives Considered
- **Include planned features with caveats**: Rejected. Constitution explicitly prohibits "Coming Soon" or aspirational content.
- **Copy README verbatim**: Rejected. Constitution Principle III requires adapting content for website audience with "why should I care" framing.

### Verified Facts
- Written in Go, requires Go 1.24+
- Two commands: `gaze analyze` (side effects) and `gaze crap` (CRAP scores)
- Side effects categorized into P0/P1/P2 tiers (P3/P4 defined but NOT detected)
- CRAP formula: `complexity^2 * (1 - coverage/100)^3 + complexity`
- Default CRAP threshold: 15
- Output formats: text and JSON
- Apache 2.0 license
- Early maturity (no releases published)

---

## 5. CI/CD Workflow

### Decision
Use GitHub Actions with SHA-pinned action references, deploying to GitHub Pages on push to `main`.

### Rationale
This is the standard GitHub Pages deployment pattern. SHA-pinned actions satisfy FR-014 and SC-005 security requirements.

### SHA-Pinned Actions (from complytime-website)
| Action | SHA | Purpose |
|--------|-----|---------|
| `actions/checkout` | `de0fac2e4500dabe0009e67214ff5f5447ce83dd` | Checkout repository |
| `actions/setup-node` | `6044e13b5dc448c55e2357c09f80417699197238` | Install Node.js |
| `peaceiris/actions-hugo` | `75d2e84710de30f6ff7268e08f310b60ef14033f` | Install Hugo |
| `actions/upload-pages-artifact` | `7b1f4a764d45c48632c6b24a0339c27f5614fb0b` | Upload build output |
| `actions/deploy-pages` | `d6db90164ac5ed86f2b6aed7e0febac5b3c0c03e` | Deploy to Pages |

### Alternatives Considered
- **Netlify / Vercel**: Adds external dependency. GitHub Pages is free and integrated with the existing GitHub org.
- **Tag-based action references**: Rejected per FR-014 (SHA-pinned required for supply chain security).

---

## 6. Brand Styling

### Decision
Use electric blue (#3b82f6) as primary and violet (#8b5cf6) as accent, configured via Bootstrap CSS variables in `_variables-custom.scss`. No external fonts.

### Rationale
Colors are specified in FR-007. The complytime-website uses custom Google Fonts (DM Sans, JetBrains Mono) but the Unbound Force spec explicitly prohibits external fonts (FR-013). Doks default fonts will be used instead.

### Differences from complytime-website
| Aspect | ComplyTime | Unbound Force |
|--------|-----------|---------------|
| Primary color | Cyan (#0891b2) | Electric blue (#3b82f6) |
| Accent color | Cyan dark (#06b6d4) | Violet (#818cf8 dark / #8b5cf6 light) |
| Fonts | DM Sans + JetBrains Mono (Google Fonts) | Doks defaults (no external fonts) |
| SCSS scope | Full Tailwind-like color scales | Minimal brand variables only |

### Alternatives Considered
- **Full Tailwind-style color scales**: Rejected. The Minimal Footprint principle requires simplest implementation. Only the colors needed for `params.toml` and Bootstrap overrides should be defined.

---

## 7. Custom Domain Configuration

### Decision
Place a `CNAME` file containing `unboundforce.dev` in `static/`. Domain redirects for `theunbound.dev`, `theunboundforce.dev`, and `thegaze.dev` are handled via DNS registrar URL forwarding (fully out of scope for this repository).

### Rationale
GitHub Pages reads the CNAME file to configure the custom domain. The redirect domains are handled at the DNS level, not in the repository, per the clarification session.

### Alternatives Considered
- **GitHub Pages default URL**: Would work but doesn't establish brand identity. CNAME is trivial to add.
- **In-repo redirect logic**: Rejected during clarification. DNS registrar forwarding is simpler and out of scope.

---

## 8. .gitignore Pattern

### Decision
Adapt the complytime-website `.gitignore`, removing complytime-specific entries (sync-content binary, proposals/, check_workflow_action_shas.py).

### Rationale
Standard Hugo + Node.js ignore patterns. Must exclude `public/`, `resources/_gen/`, `.hugo_build.lock`, `hugo_stats.json`, `node_modules/`, and OS files per FR-003.

---

## 9. Content Structure (Scaffold Only)

### Decision
Create only `content/_index.md` (homepage frontmatter) in this spec. Documentation content pages (Getting Started, Projects, Team, Contributing) are out of scope -- they are covered by spec 002.

### Rationale
Spec 001 is infrastructure scaffold. The section directories and `_index.md` files for docs sections will be created by spec 002. The homepage needs `content/_index.md` for frontmatter (title, description, lead) consumed by `layouts/home.html`.

### Alternatives Considered
- **Create placeholder section directories**: Rejected. Constitution prohibits placeholder content. Creating empty sections would trigger build warnings and serve no purpose until spec 002 fills them.
