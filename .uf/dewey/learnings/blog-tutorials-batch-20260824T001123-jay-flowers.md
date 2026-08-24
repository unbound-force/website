---
tag: blog-tutorials-batch
author: jay-flowers
category: gotcha
created_at: 2026-08-24T00:11:23Z
identity: blog-tutorials-batch-20260824T001123-jay-flowers
tier: draft
---

When generating blog posts and tutorials in parallel batches for the Unbound Force website, cross-document consistency is the primary review failure mode. In the blog-tutorials-batch change, 5 out of 5 Divisor agents returned REQUEST CHANGES on the first code review iteration, with the most critical findings being inconsistencies between blog posts and their companion tutorials: different YAML schema formats (map-based vs list-based stores), different provider names (vertex-ai vs vertexai), different config paths (.dewey/ vs .uf/dewey/), different CLI flags (--json vs --format=json), and different model names (gemini-2.5-flash vs claude-sonnet-4-20250514). When parallel agents independently generate content from the same upstream issues, they produce internally consistent but mutually inconsistent documents. The fix is to either generate the tutorial first and pass it as context to the blog agent, or do a cross-document alignment pass before the code review step. Also, always verify cross-reference slugs against actual file slugs — the dewey-knowledge-stores blog referenced two non-existent blog post slugs that had to be corrected to the actual slugs (dewey-vs-karpathy, dewey-curator).
