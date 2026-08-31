---
tag: vision-roadmap-pages
author: jay-flowers
category: pattern
created_at: 2026-08-31T14:22:23Z
identity: vision-roadmap-pages-20260831T142223-jay-flowers
tier: draft
---

When adapting source Markdown documents (like VISION.md or ROADMAP.md) from an upstream repository for publication as Hugo/Doks website pages, the heading-level adaptation strategy matters. For this website's Doks theme, the frontmatter `title` field generates the H1, so body content must start at H2. When the source document uses H1 as its title and H2 for sections, the body sections can be preserved at H2 — no shift needed for the section headings. Only the H1 title line is removed (replaced by frontmatter). Hugo's `_index.md` convention should be used for section landing pages (content/docs/section/_index.md). Cross-references between source documents (e.g., `[VISION.md](VISION.md)`) must be rewritten to Hugo routes (e.g., `[Vision](/docs/vision/)`), while external links (GitHub issues, discussions) that already use full URLs can be preserved as-is. Menu entries in menus.en.toml require both `[[docs]]` entries (for sidebar navigation with identifiers) and `[[main]]` entries (for top navbar), with weight values controlling ordering in each independently.
