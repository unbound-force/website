---
tag: config-docs
category: gotcha
created_at: 2026-05-02T21:32:14Z
identity: config-docs-1
tier: draft
---

When the Unbound Force website has multiple OpenSpec changes that create the same directory or section (e.g., content/docs/reference/), each branch must independently scaffold the section because branches are isolated. The config-docs change and cli-reference-and-positioning change both needed content/docs/reference/_index.md and a menus.en.toml entry. When both PRs merge to main, the second merge will see the files already exist. However, the _index.md content may differ between branches — the config-docs branch should only list pages that exist on its branch, not pages from other unmerged PRs. The code review caught a dead link to /docs/reference/cli/ in the _index.md because that page only exists on the cli-reference-and-positioning branch.
