---
tag: gaze-multi-language-docs
author: jay-flowers
category: context
created_at: 2026-08-23T18:05:25Z
identity: gaze-multi-language-docs-20260823T180525-jay-flowers
tier: draft
---

When editing the homepage card badge in layouts/home.html for the Unbound Force website, the badge HTML pattern appears multiple times (once per project card). The edit must include enough surrounding context to uniquely identify the target card — specifically the parent anchor tag with the project-specific href. For example, to edit the Gaze badge, include the `<a href="/docs/projects/gaze/"` line in the oldString context. Without this, the edit fails with "Found multiple matches for oldString." This applies to all three project cards (Gaze, Dewey, Replicator) in the homepage template.
