---
tag: tutorial-code-review
category: pattern
created_at: 2026-05-03T19:39:37Z
identity: tutorial-code-review-1
tier: draft
---

When creating tutorial pages for the Unbound Force website, the weight value in Hugo frontmatter determines sidebar ordering. The getting-started section uses weights from 10 (_index) to 85 (constitution). Tutorials that are follow-ups to reference pages should use a weight value just above the reference page they extend — for example, the code review tutorial at weight 72 appears right after common-workflows at weight 70. This co-locates the reference and tutorial in the sidebar navigation, matching how readers naturally progress from command reference to hands-on walkthrough.
