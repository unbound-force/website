# Feature Specification: Site Scaffold and Deployment

**Feature Branch**: `001-site-scaffold`
**Created**: 2026-02-23
**Status**: Draft
**Input**: User description: "Build the Hugo + Doks site infrastructure, homepage, CI/CD pipeline, and custom domain configuration for unboundforce.dev. Adapt from complytime-website reference implementation."

## Clarifications

### Session 2026-02-23

- Q: What Lighthouse performance score target should the site meet? → A: >= 90
- Q: What accessibility compliance level should the site target? → A: WCAG 2.1 AA
- Q: How should domain redirects (theunbound.dev, theunboundforce.dev, thegaze.dev) be implemented? → A: DNS registrar forwarding (fully out of scope for this repo)

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Site Builds and Deploys (Priority: P1)

As a contributor, I want to clone the repo, run `npm install` and `npm run build`, and get a successful production build so that I can develop locally and trust that CI will also pass.

**Why this priority**: Nothing else works until the site builds. This is the foundation for all subsequent content and styling work.

**Independent Test**: Run `npm install && npm run build` from a clean clone. The build completes without errors and produces output in `public/`.

**Acceptance Scenarios**:

1. **Given** a clean clone of the repo, **When** I run `npm install`, **Then** all dependencies install without errors (Node.js >= 20.11.0 required).
2. **Given** dependencies are installed, **When** I run `npm run build`, **Then** Hugo produces a production build in `public/` without errors.
3. **Given** dependencies are installed, **When** I run `npm run dev`, **Then** a local dev server starts at `http://localhost:1313/` with live reload.
4. **Given** dependencies are installed, **When** I run `npm run preview`, **Then** I can preview the production build locally.

---

### User Story 2 - Homepage Communicates Value (Priority: P1)

As a first-time visitor landing on unboundforce.dev, I want to immediately understand what Unbound Force is, what it offers, and how to get started so that I can decide whether it's relevant to me within seconds.

**Why this priority**: The homepage is the first impression and the gateway to everything else. Without it, the site has no purpose.

**Independent Test**: Load the homepage in a browser. Within 5 seconds of scanning, the visitor should be able to answer: "What is Unbound Force?" and "What do I do next?"

**Acceptance Scenarios**:

1. **Given** I navigate to the homepage, **When** the page loads, **Then** I see a hero section with a clear title, a brief description of Unbound Force, and CTA buttons for "Get Started" and "View on GitHub".
2. **Given** I am on the homepage, **When** I scroll past the hero, **Then** I see a features section with 3-4 cards highlighting what makes Unbound Force unique (Agent Personas, Speckit Workflow, Quality-First, Open Source).
3. **Given** I am on the homepage, **When** I scroll to the projects section, **Then** I see a card for Gaze with an accurate description derived from the Gaze repository README.
4. **Given** I am on the homepage, **When** I scroll to the bottom, **Then** I see a closing CTA section with links to get started and view the GitHub organization.
5. **Given** I am on the homepage, **When** I toggle dark mode, **Then** all sections render correctly with appropriate colors in both light and dark themes.

---

### User Story 3 - CI/CD Deploys Automatically (Priority: P1)

As a maintainer, I want pushes to the `main` branch to automatically build and deploy the site to GitHub Pages so that the live site stays in sync with the repository.

**Why this priority**: Without automated deployment, the site requires manual publishing which creates drift between the repo and the live site.

**Independent Test**: Push a commit to `main` and verify the GitHub Actions workflow completes successfully and the site is updated at the deployment URL.

**Acceptance Scenarios**:

1. **Given** a commit is pushed to `main`, **When** GitHub Actions triggers, **Then** the workflow installs Node.js and Hugo, runs `hugo --minify --gc`, and deploys to GitHub Pages.
2. **Given** the workflow configuration, **When** I inspect the action references, **Then** all GitHub Actions use SHA-pinned versions (not mutable tags).
3. **Given** the workflow completes, **When** I visit the deployment URL, **Then** the site is live and reflects the latest commit.

---

### User Story 4 - Custom Domains Work (Priority: P2)

As a visitor, I want to access the site via `unboundforce.dev` and have alternative domains (`theunbound.dev`, `theunboundforce.dev`) redirect to it, and `thegaze.dev` redirect to the Gaze project page.

**Why this priority**: Custom domains establish brand identity and professionalism, but the site is functional on GitHub Pages URLs without them.

**Independent Test**: Navigate to each domain and verify the correct behavior (primary site loads or redirect occurs).

**Acceptance Scenarios**:

1. **Given** DNS is configured, **When** I navigate to `https://unboundforce.dev/`, **Then** the homepage loads directly (this is the primary domain).
2. **Given** DNS is configured, **When** I navigate to `https://theunbound.dev/`, **Then** I am redirected to `https://unboundforce.dev/`.
3. **Given** DNS is configured, **When** I navigate to `https://theunboundforce.dev/`, **Then** I am redirected to `https://unboundforce.dev/`.
4. **Given** DNS is configured, **When** I navigate to `https://thegaze.dev/`, **Then** I am redirected to the Gaze project page on unboundforce.dev.
5. **Given** the repo contains a `CNAME` file, **When** GitHub Pages processes the deployment, **Then** it serves the site on the custom domain with HTTPS.

---

### User Story 5 - Brand Styling Applied (Priority: P2)

As a visitor, I want the site to have a distinctive visual identity with the Unbound Force brand colors so that it feels professional and cohesive.

**Why this priority**: Styling can be applied after the site is functional. The Doks theme provides a solid default, so custom styling is an enhancement, not a blocker.

**Independent Test**: Load the site and verify the brand color palette is applied consistently across both light and dark modes.

**Acceptance Scenarios**:

1. **Given** I load the site in light mode, **When** I inspect the visual styling, **Then** the primary color is electric blue (#3b82f6) and accent elements use violet (#8b5cf6).
2. **Given** I load the site in dark mode, **When** I inspect the visual styling, **Then** colors adjust appropriately for dark backgrounds while maintaining the brand identity.
3. **Given** the SCSS files, **When** I inspect `_variables-custom.scss`, **Then** brand colors are defined using Bootstrap-compatible variables.
4. **Given** the SCSS files, **When** I inspect `_custom.scss`, **Then** only styles that Doks does not provide natively are present (homepage cards, section spacing).
5. **Given** the site uses Doks default fonts, **When** I inspect the CSS, **Then** no external font loading (Google Fonts or similar) is configured.

---

### Edge Cases

- What happens when a visitor accesses the site with JavaScript disabled? The site should still render (Hugo generates static HTML).
- What happens when the GitHub Pages deployment fails? The previous version of the site remains live (GitHub Pages behavior).
- What if the CNAME file conflicts with GitHub Pages settings? The CNAME file in `static/` must match the domain configured in GitHub Pages repo settings.
- What if a visitor accesses `http://` (non-HTTPS)? GitHub Pages enforces HTTPS redirect automatically for custom domains.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The repository MUST contain a valid `package.json` with `dev`, `build`, `preview`, and `format` scripts that work with the Doks/Thulite stack. Required dependencies and versions are specified in plan.md Technical Context.
- **FR-002**: The repository MUST contain a `go.mod` with module path `github.com/unbound-force/website` and Go version 1.23.
- **FR-003**: The repository MUST contain a `.gitignore` that excludes `public/`, `resources/_gen/`, `.hugo_build.lock`, `hugo_stats.json`, `node_modules/`, and OS-specific files.
- **FR-004**: Hugo configuration MUST be in `config/_default/` using TOML format, with `hugo.toml` for core settings, `params.toml` for Doks theme parameters, and `module.toml` for Hugo module mounts that wire Thulite npm packages into Hugo's virtual filesystem.
- **FR-005**: The `hugo.toml` MUST set `title` to "Unbound Force" and `baseurl` to `https://unboundforce.dev/`.
- **FR-006**: The `params.toml` MUST configure Doks with auto color mode, sticky navbar, flex search enabled, and the correct `docsRepo` URL (`https://github.com/unbound-force/website`).
- **FR-007**: The `params.toml` MUST set brand colors: `textDark = "#e2e8f0"`, `accentDark = "#818cf8"`, `textLight = "#0f172a"`, `accentLight = "#3b82f6"`.
- **FR-008**: The `menus/menus.en.toml` MUST define initial navigation sections (Getting Started, Projects, Team, Contributing) in `[[docs]]` entries, a top-level nav in `[[main]]`, a GitHub social link in `[[social]]`, and footer links in `[[footer]]`.
- **FR-009**: The homepage MUST be composed of `content/_index.md` (frontmatter only) and `layouts/home.html` (full HTML template). The `module.toml` MUST exclude Doks' default `home.html` from the doks-core layouts mount (`excludeFiles = "home.html"`) to prevent conflicts with the custom homepage.
- **FR-010**: The homepage template MUST include four sections: hero (badge, title, lead, CTA buttons), features (3-4 cards), projects (Gaze card), and closing CTA.
- **FR-011**: The Gaze project card on the homepage MUST accurately describe Gaze based on the actual repository README (side effect detection + CRAP scores for Go).
- **FR-012**: Custom SCSS MUST be limited to `assets/scss/common/_variables-custom.scss` (brand colors) and `assets/scss/common/_custom.scss` (homepage card and section styles).
- **FR-013**: The site MUST NOT use external fonts (Google Fonts or similar). Doks default fonts MUST be used.
- **FR-014**: The CI/CD workflow MUST be at `.github/workflows/deploy-gh-pages.yml`, triggered on push to `main`, using SHA-pinned GitHub Actions.
- **FR-015**: The workflow MUST install Node.js (>= 20), Hugo (extended edition >= 0.155.1), run `npm ci`, and build with `hugo --minify --gc --baseURL "https://unboundforce.dev/"`.
- **FR-016**: A `CNAME` file containing `unboundforce.dev` MUST be placed in `static/` for GitHub Pages custom domain configuration.
- **FR-017**: Domain redirects MUST be documented: `theunbound.dev` and `theunboundforce.dev` redirect to `unboundforce.dev`; `thegaze.dev` redirects to the Gaze project page. Redirects are handled via DNS registrar URL forwarding and are fully out of scope for this repository. No redirect logic, Hugo aliases, or additional GitHub Pages repos are required.
- **FR-018**: The site MUST meet WCAG 2.1 AA accessibility standards. All images MUST have alt text, all interactive elements MUST be keyboard-navigable, and color contrast MUST meet the 4.5:1 minimum ratio for normal text.

### Key Entities

- **Hugo Configuration**: The set of TOML files in `config/_default/` that control site behavior, theme settings, and navigation.
- **Homepage Template**: The `layouts/home.html` file that defines the homepage structure using Hugo template blocks (`main`, `sidebar-prefooter`, `sidebar-footer`).
- **SCSS Customization**: The two SCSS files that override Doks theme defaults for brand identity.
- **CI/CD Workflow**: The GitHub Actions YAML file that automates build and deployment.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `npm install && npm run build` succeeds on a clean clone with zero errors and zero warnings.
- **SC-002**: The homepage renders all four sections (hero, features, projects, CTA) correctly in both light and dark mode.
- **SC-003**: The CI/CD workflow passes on push to `main` and deploys the site to GitHub Pages.
- **SC-004**: The site is accessible at `https://unboundforce.dev/` with valid HTTPS.
- **SC-005**: All GitHub Actions in the workflow use SHA-pinned versions (no mutable tag references).
- **SC-006**: The Gaze project card description on the homepage is accurate when cross-referenced against the Gaze repository README.
- **SC-007**: No external font requests appear in the browser network tab when loading the site.
- **SC-008**: Custom SCSS is confined to exactly two files (`_variables-custom.scss` and `_custom.scss`) with no unnecessary overrides of Doks defaults.
- **SC-009**: The deployed homepage achieves a Lighthouse performance score of >= 90 on mobile and desktop.
- **SC-010**: The deployed site meets WCAG 2.1 AA compliance (minimum 4.5:1 color contrast for text, keyboard navigability, alt text on all images, valid ARIA landmarks).
