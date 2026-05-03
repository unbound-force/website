---
tag: blog-sandbox-isolation
category: gotcha
created_at: 2026-05-03T16:51:29Z
identity: blog-sandbox-isolation-1
tier: draft
---

When writing blog posts about the Unbound Force sandbox for the website, be careful with credential statements in the Google Cloud / Vertex AI section. The sandbox has two credential paths that are easy to conflate: the gateway path (where no credentials are mounted — the gateway runs on the host and proxies requests) and the direct mount fallback path (where service account key files or gcloud ADC directories are mounted read-only into the container). Writing "no credentials are mounted" and then describing credential mounts in the next sentence is a Content Accuracy contradiction that the code review Guard will flag. Always distinguish the gateway path from the fallback credential paths.
