# Website Recommendations

## Executive Summary

Build a minimal website for Unbound Force using **Hugo + Doks/Thulite**, deployed to **GitHub Pages**. This is the same stack used by the [complytime-website](https://github.com/sonupreetam/complytime-website/tree/feat/website-minimal), which is a near-identical use case (open source org with multiple Go tool repos, docs-heavy, community-focused). The site should start minimal -- a homepage, project pages, and team personas -- and grow organically as the projects mature.

---

## Recommended Stack

### Hugo + Doks Theme

| Component | Choice | Rationale |
|-----------|--------|-----------|
| Static site generator | [Hugo](https://gohugo.io/) | Fast builds, Go-native (matches team's Go expertise), excellent docs support |
| Theme | [Doks](https://getdoks.org/) (`@thulite/doks-core`) | Docs-first theme with search, dark mode, sidebar nav, mobile responsive, SEO built-in |
| Package manager | npm | Manages Hugo theme + dev dependencies (Prettier, Vite preview) |
| Deployment | GitHub Pages | Free, integrated with the existing GitHub org, simple Actions workflow |
| CI/CD | GitHub Actions | SHA-pinned actions, single workflow for build+deploy on push to `main` |

### Why This Stack Over Alternatives

| Alternative | Why Not |
|-------------|---------|
| Next.js / Astro / Gatsby | Overkill for a docs-heavy project site. These add a build toolchain (React/Svelte), longer build times, and complexity the project doesn't need yet. |
| Plain HTML/CSS | No search, no dark mode, no sidebar nav, no mobile responsiveness out of the box. High maintenance for docs. |
| Docusaurus | React-based, heavier than Hugo. Doks provides equivalent docs features with simpler infrastructure. |
| MkDocs / Sphinx | Python-based. Doesn't align with the team's Go/Node toolchain. |
| Jekyll | Slower builds, Ruby dependency, less active development than Hugo. |

---

## Recommended Site Structure

### Information Architecture

```
Homepage
├── Projects
│   ├── Gaze (side effect detection + CRAP scores for Go)
│   └── [Future projects as they emerge]
├── The Team (Agent Personas)
│   ├── Muti-Mind (Product Owner)
│   ├── Cobalt-Crush (Developer)
│   ├── Gaze (Tester)
│   ├── The Divisor (PR Reviewer)
│   └── Mx F (Manager)
├── Getting Started
│   ├── What is Unbound Force?
│   ├── How It Works (Swarm overview, Speckit, OpenCode, Specify)
│   └── Quick Start (getting the tools running)
└── Contributing
    └── How to contribute to the projects
```

### Content Mapping

| Page | Source | Notes |
|------|--------|-------|
| Homepage | New content | Hero section, value proposition, project cards, CTA |
| What is Unbound Force? | `unbound-force/unbound-force.md` Overview section | Adapted from the existing description |
| The Team personas | `unbound-force/unbound-force.md` Heroes section | Each persona gets its own page |
| Gaze project page | `unbound-force/gaze/README.md` | Installation, commands, architecture |
| Gaze documentation | `unbound-force/gaze/README.md` + specs | Detailed docs pulled from the repo |
| Contributing | New content | Standard contributing guide |

---

## Homepage Design

Take direct inspiration from the complytime-website `layouts/home.html`. The homepage should have these sections:

### 1. Hero Section
- **Badge**: "Open Source AI Agent Swarm"
- **Title**: Something like "Software Engineering, Unbound." or "The AI Agent Swarm for Software Teams"
- **Lead text**: Brief explanation of what Unbound Force is
- **CTA buttons**: "Get Started" + "View on GitHub"

### 2. Features Section (3-4 cards)
Highlight what makes Unbound Force unique:
- **Agent Personas**: Purpose-built AI roles (Product Owner, Developer, Tester, Reviewer, Manager)
- **Speckit Workflow**: Structured spec-to-implementation pipeline
- **Quality-First**: Integrated testing, review, and validation at every stage
- **Open Source**: Apache 2.0, built for the community

### 3. Projects Section
Project cards for each tool (currently just Gaze):
- **Gaze**: "Test quality analysis via side effect detection for Go. Static analysis of functions to detect all observable side effects and compute CRAP scores."

### 4. CTA Section
"Ready to try Unbound Force?" with Get Started and GitHub links.

---

## File System Layout

```
website/
├── assets/
│   └── scss/common/
│       ├── _variables-custom.scss    # Brand colors (see Color Palette below)
│       └── _custom.scss              # Custom card styles, feature styles
├── config/
│   └── _default/
│       ├── hugo.toml                 # Site title, baseURL, outputs
│       ├── params.toml               # Doks theme config (colors, search, repo link)
│       └── menus/
│           └── menus.en.toml         # Navigation menus
├── content/
│   ├── _index.md                     # Homepage frontmatter
│   ├── privacy.md                    # Privacy policy (optional)
│   └── docs/
│       ├── _index.md                 # Docs landing
│       ├── getting-started/
│       │   ├── _index.md             # What is Unbound Force?
│       │   └── quick-start.md        # How to get started
│       ├── projects/
│       │   ├── _index.md             # Projects overview
│       │   └── gaze.md               # Gaze project page
│       ├── team/
│       │   ├── _index.md             # The Heroes overview
│       │   ├── muti-mind.md          # Product Owner persona
│       │   ├── cobalt-crush.md       # Developer persona
│       │   ├── gaze-tester.md        # Tester persona
│       │   ├── the-divisor.md        # PR Reviewer persona
│       │   └── mx-f.md              # Manager persona
│       └── contributing/
│           └── _index.md             # How to contribute
├── layouts/
│   └── home.html                     # Custom homepage (hero + features + projects)
├── static/
│   ├── favicon.png
│   └── favicon-512x512.png
├── .github/
│   └── workflows/
│       └── deploy-gh-pages.yml
├── .gitignore
├── package.json
├── go.mod
├── AGENTS.md
├── LICENSE
└── README.md
```

---

## Color Palette Recommendation

The superhero/swarm theme suggests bold, energetic colors. Suggestion:

```scss
// Primary: Electric blue (force/energy theme)
$primary: #3b82f6;

// Accent: Violet (complements the superhero aesthetic)
$accent: #8b5cf6;

// Dark theme
$text-dark: "#e2e8f0";
$accent-dark: "#818cf8";

// Light theme
$text-light: "#0f172a";
$accent-light: "#3b82f6";
```

These can be adjusted later. The Doks theme supports both light and dark modes via `params.toml`.

---

## Implementation Plan

### Phase 1: Scaffold (Day 1)

1. Initialize Hugo + Doks in the website repo
   ```bash
   # In the website/ directory
   npm init -y
   # Install Doks dependencies (mirror complytime-website package.json)
   npm install thulite @thulite/doks-core @thulite/images @thulite/inline-svg @thulite/seo @tabler/icons
   npm install -D prettier vite
   ```

2. Create `go.mod`:
   ```
   module github.com/unbound-force/website
   go 1.23
   ```

3. Set up `config/_default/` (hugo.toml, params.toml, menus.en.toml)
   - Adapt from complytime-website, changing:
     - `title` = "Unbound Force"
     - `baseurl` = the GitHub Pages URL (e.g., `https://unbound-force.github.io/website/`)
     - `docsRepo` = `https://github.com/unbound-force/website`
     - Colors to the chosen palette
     - Social links to the unbound-force GitHub org

4. Create `.gitignore` (copy from complytime-website)

5. Create `.github/workflows/deploy-gh-pages.yml` (adapt from complytime-website, changing baseURL)

6. Create `layouts/home.html` (adapt from complytime-website, changing content to Unbound Force)

### Phase 2: Content (Day 1-2)

7. Write homepage content (`content/_index.md`)

8. Write Getting Started section:
   - `content/docs/getting-started/_index.md` -- What is Unbound Force?
   - `content/docs/getting-started/quick-start.md` -- How to get started with the tools

9. Write Projects section:
   - `content/docs/projects/_index.md` -- Projects overview
   - `content/docs/projects/gaze.md` -- Adapted from Gaze README

10. Write Team section:
    - `content/docs/team/_index.md` -- The Heroes overview
    - One page per persona, adapted from `unbound-force.md`

11. Write Contributing page

### Phase 3: Polish (Day 2-3)

12. Add custom SCSS (colors, card styles)
13. Add favicon/logo (can use a placeholder initially)
14. Test dark mode, mobile responsiveness, search
15. Set up GitHub Pages in repo settings
16. Deploy and verify

### Phase 4: Future Enhancements (Backlog)

- **Content sync**: Build a sync engine (like complytime's) to pull README content from project repos automatically
- **Custom domain**: Set up a custom domain if desired
- **Blog section**: Add a blog for project updates
- **Interactive demos**: Embed Gaze output examples or terminal recordings
- **Per-project docs**: As projects mature, add detailed documentation sections (API reference, tutorials, etc.)

---

## Key Decisions to Make

### 1. Domain / URL

| Option | URL | Notes |
|--------|-----|-------|
| GitHub Pages (default) | `https://unbound-force.github.io/website/` | Free, zero config, works immediately |
| GitHub Pages (org site) | `https://unbound-force.github.io/` | Requires renaming repo to `unbound-force.github.io` |
| Custom domain | `https://unboundforce.dev/` (or similar) | Requires domain purchase + DNS config |

**Recommendation**: Start with GitHub Pages default. Move to a custom domain later if desired.

### 2. Repo Name

The repo is currently named `website`. This works fine for a project site (`unbound-force.github.io/website/`). If you want the site at the org root (`unbound-force.github.io/`), rename the repo to `unbound-force.github.io`.

### 3. Content Sync

The complytime-website has an automated content sync engine that pulls docs from upstream repos. This is valuable but adds complexity. **Recommendation**: Skip this for v1. Manually maintain project pages. Add sync later when there are multiple projects with frequently changing docs.

### 4. Persona Page Depth

The `unbound-force.md` file has extensive descriptions of each persona. Options:
- **Full pages**: One page per persona with the complete description (good for SEO, thorough)
- **Single page**: All personas on one "The Team" page with anchor links (simpler, less navigation)

**Recommendation**: Start with one page per persona. The descriptions are substantial enough to warrant their own pages, and it makes navigation cleaner.

---

## Reference: Complytime Website Patterns to Reuse

These patterns from the complytime-website are directly applicable:

1. **`package.json` scripts**: `dev`, `build`, `preview`, `format` -- copy as-is
2. **`deploy-gh-pages.yml` workflow**: Copy and change baseURL
3. **`hugo.toml` structure**: Copy and change title, baseURL, copyright
4. **`params.toml` Doks config**: Copy and change colors, repo URL, social links
5. **`layouts/home.html` template**: Copy and rewrite content for Unbound Force
6. **`.gitignore`**: Copy as-is
7. **`CONTRIBUTING.md` structure**: Adapt for Unbound Force conventions

---

## What NOT to Do

- **Do not over-engineer v1.** The site should be functional and accurate, not feature-complete. Content is more important than custom design.
- **Do not write custom CSS when Doks provides the feature.** Check Doks docs first.
- **Do not create placeholder pages with "Coming Soon" content.** Only publish pages that have real content. Empty pages hurt credibility.
- **Do not duplicate content from READMEs verbatim.** Adapt it for a website audience (less technical setup detail, more "why should I care" framing).
- **Do not add a blog, analytics, or newsletter signup for v1.** These are distractions from the core goal of explaining what Unbound Force is.
