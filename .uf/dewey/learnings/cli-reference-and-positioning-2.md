---
tag: cli-reference-and-positioning
category: gotcha
created_at: 2026-05-02T19:39:23Z
identity: cli-reference-and-positioning-2
tier: draft
---

The Unbound Force website homepage (`content/_index.md`) contains only YAML frontmatter with no body content — the homepage is rendered entirely by `layouts/home.html`. The homepage template has no stack/tools table. The stack table actually lives on `content/docs/getting-started/_index.md` (the "What is Unbound Force?" page) and `content/docs/getting-started/quick-start.md`. This was a HIGH finding in the cli-reference-and-positioning spec review: tasks targeted `content/_index.md` for a stack table update, but that file has no stack table. When writing specs that modify the homepage or stack table, always verify the actual file locations by reading the files rather than assuming from page URLs.
