---
tag: website
author: jay-flowers
category: gotcha
created_at: 2026-11-25T19:42:05Z
identity: website-20261125T194205-jay-flowers
tier: draft
as_of: "website@e481e6bbbe0d05dcd3a609769e355f32b66e0329"
evidence:
  - "dewey:layouts/_partials/footer/script-footer-custom.html"
  - "session:ses_debd78b13ffeal4xjdNkx7wiah"
---

In unbound-force/website, the Doks theme's footer partial (node_modules/@thulite/doks-core/layouts/_partials/footer/footer.html) renders .Site.Menus.footer and the repo does NOT override it — the only footer-area override is layouts/_partials/footer/script-footer-custom.html, a scripts hook. [[footer]] menu entries (e.g. "Apache 2.0 License") render live on every page. Before recording layout facts as decision rationale in a design doc, verify against the built output in public/ — PR #453's design.md claimed "the footer partial is overridden and renders no footer menu," which was verifiably false against the PR's own build output.
