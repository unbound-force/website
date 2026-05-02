---
tag: cli-reference-and-positioning
category: pattern
created_at: 2026-05-02T19:39:27Z
identity: cli-reference-and-positioning-3
tier: draft
---

For the Unbound Force website's Hugo/Doks theme, section index pages (`_index.md`) should use `weight: 10` in their frontmatter for consistency with all existing section indexes. The top-level sidebar ordering is controlled by `config/_default/menus/menus.en.toml` menu entries, not by the frontmatter weight. Setting the section index weight to match the menu weight (e.g., weight: 35) is harmless but inconsistent with the established pattern. The code review Architect flagged this as a LOW finding.
