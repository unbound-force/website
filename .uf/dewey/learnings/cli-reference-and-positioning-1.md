---
tag: cli-reference-and-positioning
category: gotcha
created_at: 2026-05-02T19:39:18Z
identity: cli-reference-and-positioning-1
tier: draft
---

When creating OpenSpec proposals for the Unbound Force website, the Constitution Alignment section must assess against the website project constitution at `.specify/memory/constitution.md` (Content Accuracy, Minimal Footprint, Visitor Clarity) — not the org-level constitution from `unbound-force/unbound-force` (Autonomous Collaboration, Composability First, Observable Quality, Testability). This was a CRITICAL finding in the spec review for cli-reference-and-positioning: all 6 Divisor reviewers flagged the wrong constitution. The OpenSpec config.yaml context block itself referenced the org constitution, which may have caused the initial artifact generator to use the wrong one. Future proposals should always check which constitution governs the specific project.
