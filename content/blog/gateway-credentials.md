---
title: "Stop Leaking Cloud Credentials to Your AI Containers — uf gateway Credential Isolation"
description: "Running AI agents in containers means forwarding cloud credentials into the container. uf gateway eliminates this — your container sees localhost and a dummy API key while the gateway handles authentication on the host."
lead: "Your container sees localhost, not Google Cloud. One local proxy handles Vertex AI, Bedrock, and Anthropic authentication — zero cloud credentials inside the container."
slug: "gateway-credentials"
date: 2026-05-03T00:00:00+00:00
draft: false
weight: 65
toc: true
categories: ["Engineering"]
tags: ["gateway", "security", "credentials", "vertex-ai", "bedrock"]
contributors: ["Unbound Force"]
---

## The Problem

Running AI agents in containers requires LLM API access. The agent needs to call Claude, but the cloud credentials — Google Cloud OAuth tokens, AWS session credentials, Anthropic API keys — live on the host machine. Getting those credentials into the container is where things go wrong.

The standard approach is environment variable forwarding. Mount `ANTHROPIC_API_KEY` into the container, or bind-mount the gcloud config directory, or inject `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Each provider has its own credential format, its own SDK expectations, and its own expiry behavior. What works on your macOS laptop breaks on the CI server. What works with Anthropic direct requires a completely different setup for Vertex AI.

The result is a matrix of provider-specific container configurations:

- **Vertex AI**: OAuth tokens expire every 60 minutes. Bind-mount the gcloud directory, handle token refresh inside the container, hope the container's `gcloud` binary is compatible with the host's config format.
- **Bedrock**: AWS session credentials require SigV4 request signing. Forward 3+ environment variables, install the AWS SDK inside the container, deal with region and credential chain resolution.
- **Anthropic direct**: Simpler (one API key), but now you have a long-lived secret mounted inside a container where an AI agent has shell access.

Every credential you forward into the container is attack surface. Every provider-specific workaround is maintenance burden. And when tokens expire mid-session, the agent fails with cryptic auth errors that have nothing to do with the code it was writing.

## The Solution: One Proxy, Three Clouds

`uf gateway` runs a local reverse proxy on the host. It accepts standard Anthropic Messages API requests on `localhost:53147` and translates + authenticates upstream to whichever cloud provider you use. The container never touches a real credential — it sends requests to localhost with a placeholder API key, and the gateway swaps in the real credentials on the host side.

```bash
uf gateway --detach    # start the proxy in the background
```

The gateway auto-detects your provider from environment variables:

| Priority | Provider | Detection |
|----------|----------|-----------|
| 1 | **Vertex AI** | `CLAUDE_CODE_USE_VERTEX=1` + `ANTHROPIC_VERTEX_PROJECT_ID` |
| 2 | **Bedrock** | `CLAUDE_CODE_USE_BEDROCK=1` |
| 3 | **Anthropic** | `ANTHROPIC_API_KEY` |

Once running, any container on the host can call `http://localhost:53147` with a dummy token. The gateway intercepts the request, injects real credentials, translates the request format for the upstream provider, and forwards it. The container's code never changes regardless of which provider you use.

## How It Works

The request flow is straightforward:

```text
Container                    Host                        Cloud
─────────                    ────                        ─────
Anthropic API request  ───►  uf gateway (localhost:53147)
                             ├─ Inject credentials
                             ├─ Translate request format
                             └─ Forward upstream    ───►  Vertex AI / Bedrock / Anthropic
                                                    ◄───  Response
                       ◄───  Strip provider headers
                             Return standard response
```

For **Vertex AI**, the gateway handles the heaviest translation: rewriting URLs to the Vertex AI endpoint format, moving the model name from the JSON body to the URL path, injecting the `anthropic_version` header, and filtering out Vertex-specific SSE events (`vertex_event`, `ping`) that would confuse the Anthropic SDK client.

For **Bedrock**, the gateway implements SigV4 request signing directly — no AWS SDK dependency. The entire signing implementation is about 200 lines of Go, eliminating a large transitive dependency from the container image.

For **Anthropic direct**, the gateway simply injects the API key as the `x-api-key` header. Even this simple case benefits from credential isolation — the container never sees the actual key.

## Tokens Refresh Themselves

OAuth tokens and session credentials expire. If your agent runs for 2 hours, the credentials it started with will be stale long before it finishes. The gateway handles this transparently.

**Vertex AI tokens** are assumed to last 55 minutes (5-minute safety margin before the typical 60-minute expiry). A background goroutine refreshes the token by calling `gcloud auth application-default print-access-token` on the host — where gcloud is authenticated and working.

**Bedrock credentials** refresh every 50 minutes using the AWS CLI on the host.

**Proactive refresh**: When a request arrives and the current token will expire within 5 minutes, the gateway starts a non-blocking refresh before forwarding the request. The request uses the existing (still-valid) token while the refresh runs in the background. By the next request, the fresh token is ready.

**Deduplication**: If multiple requests arrive simultaneously during the refresh window, the gateway uses `sync.Mutex.TryLock()` to ensure only one refresh runs at a time. Concurrent requests get the existing token — no thundering herd of `gcloud` invocations.

## Before and After

| Aspect | Before Gateway | With Gateway |
|--------|---------------|--------------|
| **Env vars forwarded** | 4-6 (provider-specific) | 2 (`ANTHROPIC_BASE_URL`, `ANTHROPIC_API_KEY`) |
| **Credential mounts** | gcloud dir, service account keys, AWS config | None |
| **SDK dependencies** | AWS SDK, gcloud CLI inside container | None |
| **Token management** | Container-side refresh logic | Host-side, automatic |
| **Provider switching** | Rebuild container config | Change host env vars, restart gateway |
| **Container image** | Provider-specific variants | Single universal image |

The container sees the same two environment variables regardless of whether you use Vertex AI, Bedrock, or Anthropic direct. Switching providers is a host-side configuration change — the container image and agent code remain identical.

## Sandbox Integration

If you use `uf sandbox` for containerized development sessions, the gateway integration is automatic. When `uf sandbox start` detects a cloud provider in your environment (Vertex AI or Bedrock env vars), it starts the gateway before launching the container. The container receives `ANTHROPIC_BASE_URL=http://host.containers.internal:53147` and `ANTHROPIC_API_KEY=gateway` — no manual gateway setup required.

```bash
uf sandbox start    # gateway auto-starts if Vertex/Bedrock detected
```

## Scope

The gateway proxies Anthropic Messages API requests only. It is not a general-purpose HTTP proxy — it does not forward arbitrary traffic, non-Anthropic API calls, or direct model downloads. For API access beyond the Anthropic Messages API, forward the relevant credentials directly.

## Get Started

Install the Unbound Force CLI and start the gateway:

```bash
brew install unbound-force/tap/unbound-force
uf gateway --detach
```

Your container sees localhost. Your credentials stay on the host. Tokens refresh themselves.

## See Also

- [Quick Start](/docs/getting-started/quick-start/) -- Install and verify the toolchain
- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow with the `uf` CLI
- [Common Workflows](/docs/getting-started/common-workflows/) -- End-to-end feature and review flows
