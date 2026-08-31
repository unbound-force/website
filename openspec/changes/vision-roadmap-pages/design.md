## Context

The Unbound Force organization maintains `VISION.md` and `ROADMAP.md` in the `unbound-force/unbound-force` repository. These documents define the project's purpose, principles, and trajectory. Currently they are only accessible to developers who browse the source repository — website visitors have no way to discover them.

The website uses Hugo with the Doks theme (`@thulite/doks-core`). Content lives in `content/docs/` as Markdown files organized into sections. Navigation is configured in `config/_default/menus/menus.en.toml` with separate `[[docs]]` (sidebar) and `[[main]]` (top nav bar) entries.

## Goals / Non-Goals

### Goals
- Publish Vision and Roadmap as first-class documentation pages on unboundforce.dev
- Place both pages prominently: top of docs sidebar and in the main navigation bar
- Preserve all source content substance — adapt only formatting for Hugo/Doks compatibility
- Ensure all cross-references (GitHub issues, discussions, org links) render as working hyperlinks

### Non-Goals
- Auto-syncing content from the upstream repository (manual updates are acceptable)
- Custom layouts or shortcodes — use standard Doks Markdown rendering
- Restructuring existing documentation sections or their weights
- Adding a dedicated "About" or "Community" section — Vision and Roadmap are standalone docs sections

## Decisions

### D1: Standalone sections, not nested under an existing section

Each document becomes its own top-level docs section (`content/docs/vision/_index.md` and `content/docs/roadmap/_index.md`) rather than being nested under "Getting Started" or a new "About" section. This keeps the sidebar clean, gives each page its own section heading, and avoids creating a parent section that would only contain two children.

### D2: Section index files (`_index.md`) for content

Using `_index.md` (section index) rather than a plain `page.md` ensures Hugo treats each as a section root. This is the Doks convention for top-level sidebar entries and allows future sub-pages if the content grows.

### D3: Weight ordering — Vision before Roadmap, both before Getting Started

- Vision: sidebar weight 5, main nav weight 2
- Roadmap: sidebar weight 6, main nav weight 3
- Getting Started remains at sidebar weight 10, Blog remains at main nav weight 5

This positions Vision and Roadmap as the first things visitors see, establishing context before they dive into practical guides.

### D4: Content adaptation approach

Source Markdown is adapted minimally:
- Add Hugo YAML frontmatter (`title`, `description`, `lead`, `date`, `weight`, `toc`, `draft`)
- Convert H1 (`#`) to H2 (`##`) since the `title` frontmatter generates the H1
- Shift all heading levels down by one accordingly
- Replace `[VISION.md](VISION.md)` cross-reference in ROADMAP with `[Vision](/docs/vision/)`
- Preserve all external GitHub URLs as-is (they're already fully qualified)
- Preserve all list structures, bold text, and inline formatting

### D5: No custom CSS or layout overrides

Both pages use standard Doks Markdown rendering. The Doks theme handles table of contents, heading anchors, dark mode, and responsive layout automatically. No custom SCSS, shortcodes, or template overrides needed.

## Risks / Trade-offs

### Content drift
The website pages are static copies, not synced from the upstream repository. If `VISION.md` or `ROADMAP.md` changes upstream, the website will become stale. **Mitigation**: This is acceptable for now — these documents change infrequently, and auto-sync would add complexity disproportionate to the update frequency.

### Sidebar prominence
Placing Vision and Roadmap above Getting Started may surprise visitors who expect practical docs first. **Mitigation**: The titles are self-explanatory, and Getting Started remains easily accessible at weight 10. Visitors looking for practical guidance can find it quickly.

### Heading level shift
Shifting all headings down by one level to accommodate Hugo's H1-from-title convention could affect readability if the source documents use deeply nested headings. **Mitigation**: Both source documents use H2 and H3 only, so the shift produces H2 and H3 (plus occasional H4) — well within readable range.
