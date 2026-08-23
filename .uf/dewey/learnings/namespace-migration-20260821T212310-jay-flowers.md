---
tag: namespace-migration
author: jay-flowers
category: gotcha
created_at: 2026-08-21T21:23:10Z
identity: namespace-migration-20260821T212310-jay-flowers
tier: draft
---

During the slash command namespace migration (opsx/slash-command-docs-update, August 2026), the code review council caught a critical gap: frontmatter fields (title, description, lead) in documentation pages were not being updated alongside body content. The initial implementation preserved blog post frontmatter as an explicit design decision (D6), but this exemption was incorrectly extended to docs pages like code-review-tutorial.md. Frontmatter fields in docs pages are user-facing — they appear in SEO metadata, social media previews, and page headers — and must be updated when command names change. Blog post frontmatter is a more nuanced case: the slug field controls the URL, so titles can be updated safely if the slug is explicitly set. The lesson is that "preserve frontmatter" should be scoped precisely (blog titles that generate URLs vs. docs page metadata that is purely descriptive) rather than applied as a blanket rule.
