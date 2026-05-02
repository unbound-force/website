---
title: "Gateway"
description: "LLM reverse proxy for credential isolation — auto-detects Vertex AI, Bedrock, or Anthropic and injects host-side credentials into upstream requests."
lead: "The uf gateway command runs a local reverse proxy that serves the Anthropic Messages API. Containers and sandboxed sessions connect to localhost instead of cloud endpoints — no credential forwarding required."
date: 2026-05-02T00:00:00+00:00
draft: false
weight: 30
toc: true
---

## Why a Gateway?

Running AI agents inside containers creates a credential isolation problem. The agent needs to call the Anthropic Messages API, but the cloud credentials (Google Cloud OAuth tokens, AWS session credentials, or Anthropic API keys) live on the host machine. Forwarding credentials into containers is fragile and insecure — credential files change location across platforms, tokens expire, and mounting secret directories creates attack surface.

The gateway solves this by running on the host and proxying API requests. The container sees `localhost:53147` and a dummy token. The gateway intercepts each request, injects real credentials, translates the request for the upstream provider, and forwards it. The container never touches a real credential.

## Commands

### gateway (start)

Start the gateway. Auto-detects the cloud provider from environment variables and begins listening for Anthropic Messages API requests.

```bash
uf gateway                    # foreground, auto-detect provider
uf gateway --detach           # background (daemon mode)
uf gateway --port 9090        # custom port
uf gateway --provider vertex  # override auto-detection
```

| Flag | Description |
|------|-------------|
| `--detach` | Run in the background as a daemon |
| `--port <int>` | Port to listen on (default `53147`) |
| `--provider <string>` | Provider override: `anthropic`, `vertex`, or `bedrock` (auto-detected if omitted) |

### gateway status

Show the running gateway's provider, port, PID, and uptime. Prints "No gateway running." if no gateway is found.

```bash
uf gateway status
```

### gateway stop

Terminate a running background gateway and remove its PID file. Prints "No gateway running." if no gateway is found.

```bash
uf gateway stop
```

## Provider Auto-Detection

The gateway scans environment variables to detect your cloud provider. The first match wins:

| Priority | Provider | Detection Condition |
|----------|----------|-------------------|
| 1 | **Vertex AI** | `CLAUDE_CODE_USE_VERTEX=1` + `ANTHROPIC_VERTEX_PROJECT_ID` set |
| 2 | **Bedrock** | `CLAUDE_CODE_USE_BEDROCK=1` |
| 3 | **Anthropic** | `ANTHROPIC_API_KEY` set |

Vertex AI is checked first because a developer may have both `ANTHROPIC_API_KEY` and Vertex env vars set — the more specific provider wins.

Use `--provider` to override auto-detection if the environment variables don't match your intent.

### Vertex AI

The gateway translates standard Anthropic Messages API requests into Google Cloud's Vertex AI format:

- **URL rewriting**: Routes requests to `https://{region}-aiplatform.googleapis.com/v1/projects/{project}/locations/{region}/publishers/anthropic/models/{model}:streamRawPredict` (streaming) or `:rawPredict` (non-streaming)
- **Model field removal**: The model name is moved from the JSON body to the URL path (Vertex AI convention)
- **Header injection**: Adds `anthropic_version: vertex-2023-10-16` to the JSON body
- **Header stripping**: Removes `anthropic-beta` and `anthropic-version` headers (Vertex rejects them)
- **SSE event filtering**: Drops `vertex_event` and `ping` events from the streaming response — these are Vertex-specific control events that confuse Anthropic SDK clients
- **Authentication**: Injects a Google Cloud OAuth bearer token via `gcloud auth application-default print-access-token`

#### Synthetic Model Catalog

The gateway exposes a `/v1/models` endpoint that returns the Vertex-available Claude models with capabilities metadata. This allows tools that query the models list (like OpenCode) to discover available models without calling Google Cloud directly. The catalog includes model family, context window size, and supported features (vision, tool use, extended thinking).

### AWS Bedrock

The gateway signs requests with AWS SigV4 authentication:

- **No AWS SDK dependency**: The gateway implements SigV4 signing directly (canonical request, string-to-sign, signature calculation) without importing the AWS SDK
- **Credential sources**: Uses the standard AWS credential chain (`AWS_ACCESS_KEY_ID` + `AWS_SECRET_ACCESS_KEY`, or `AWS_SESSION_TOKEN` for assumed roles)
- **Region**: Uses `AWS_REGION` or defaults to `us-east-1`

### Anthropic Direct

When `ANTHROPIC_API_KEY` is set (and no Vertex/Bedrock env vars are present), the gateway passes requests through to `api.anthropic.com` with the API key injected as the `x-api-key` header. This mode is useful when you want credential isolation without provider translation — the container still sees localhost and a dummy token.

## Token Refresh

For providers with expiring credentials (Vertex AI and Bedrock), the gateway runs background refresh goroutines:

- **Vertex AI**: Refreshes the OAuth token by calling `gcloud auth application-default print-access-token`. Tokens are assumed to last 55 minutes (5-minute safety margin before the typical 60-minute expiry).
- **Bedrock**: Refreshes AWS session credentials every 50 minutes.
- **Proactive refresh**: If a request arrives and the token expires within 5 minutes, a non-blocking refresh is attempted before forwarding. This prevents requests from failing due to just-expired tokens.
- **Deduplication**: Uses `sync.Mutex.TryLock()` to ensure only one refresh runs at a time. Concurrent requests that trigger refresh use the existing (soon-to-expire) token while the refresh completes.

Anthropic direct mode does not use token refresh — API keys do not expire.

## Daemon Mode

When started with `--detach`, the gateway runs as a background daemon:

- **PID file**: Written to `.uf/gateway.pid` for process management
- **Log file**: Output redirected to `.uf/gateway.log`
- **Process detection**: Uses a `_UF_GATEWAY_CHILD` environment variable sentinel to distinguish the parent (which daemonizes) from the child (which runs the server)
- **Cleanup**: `uf gateway stop` reads the PID file, sends SIGTERM, and removes the PID file

## Sandbox Integration

The gateway integrates with `uf sandbox` for containerized development sessions. When `uf sandbox start` detects a cloud provider in the environment (Vertex AI, Bedrock), it automatically starts the gateway before launching the container:

1. Gateway starts on the host (if not already running)
2. Container receives `ANTHROPIC_BASE_URL=http://host.containers.internal:53147` and `ANTHROPIC_AUTH_TOKEN=gateway-proxy` instead of real credential mounts
3. The agent inside the container makes standard Anthropic API calls to the gateway URL
4. The gateway translates and authenticates upstream

This means `uf sandbox start` is all you need — the gateway is transparent. No manual gateway setup required when using the sandbox.

## See Also

- [Quick Start](/docs/getting-started/quick-start/) -- Install and verify the toolchain
- [Developer Guide](/docs/getting-started/developer/) -- Daily workflow with the `uf` CLI
- [Common Workflows](/docs/getting-started/common-workflows/) -- End-to-end feature and review flows
