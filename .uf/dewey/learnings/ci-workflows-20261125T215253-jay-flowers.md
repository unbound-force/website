---
tag: ci-workflows
author: jay-flowers
category: gotcha
created_at: 2026-11-25T21:52:53Z
identity: ci-workflows-20261125T215253-jay-flowers
tier: draft
as_of: "unbound-force/website@3ba3fddbcfaca7f752ffbc84d3b42ee451a5e2aa"
evidence:
  - ".github/workflows/deploy-gh-pages.yml"
  - "unbound-force/website#456"
  - "session:ses_deb598052ffeQ15mUpnc8IJ46r"
---

unbound-force/website CI workflows (.github/workflows/deploy-gh-pages.yml on main, and ci.yml added in PR #456) pin actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd and actions/setup-node@6044e13b5dc448c55e2357c09f80417699197238 with version comments claiming "# v4", but gh api tag verification (2026-11-25, during review of PR #456) shows de0fac2e is the actions/checkout v6.0.2 release commit and 6044e13b is the actions/setup-node v6.2.0 release commit — both comments are off by two major versions. peaceiris/actions-hugo@75d2e84710de30f6ff7268e08f310b60ef14033f is correctly labeled (# v3, actual v3.0.0). The SHAs themselves are valid, immutable, official upstream release commits identical across both workflows (production parity holds; no security exposure), so only the comments are wrong — trust the SHA, not the comment. When bumping or auditing these pins, verify tag→SHA via gh api repos/<owner>/<repo>/git/ref/tags/<tag> (dereferencing annotated tags via git/tags/<sha>) rather than trusting the inline comment.
