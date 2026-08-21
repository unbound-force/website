---
tag: website-docs-maintenance
author: jay-flowers
category: pattern
created_at: 2026-08-21T20:19:02Z
identity: website-docs-maintenance-20260821T201902-jay-flowers
tier: draft
---

When adding new pages to the website reference section (content/docs/reference/), the reference section index page at content/docs/reference/_index.md manually lists all reference pages with descriptions. New pages will appear in Hugo's sidebar automatically but will NOT appear on the _index.md landing page without a manual update. Always include a task to update _index.md when adding reference pages. Similarly, when modifying the constitution (e.g., adding a new principle), check all pages that reference the principle count — common locations include getting-started/_index.md, architecture.md, blog posts like the-8-phase-pipeline.md, and role-based guides (tester.md, product-owner.md). The count appears in more places than expected.
