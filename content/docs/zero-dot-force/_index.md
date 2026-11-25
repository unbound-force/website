---
title: "Zero Dot Force"
description: "Zero Dot Force is the junior hero team of Unbound Force — the incubator where new tools start and team up with the senior heroes."
lead: "The junior hero team of Unbound Force. New tools start here, train alongside the senior heroes, and grow into the swarm."
date: 2026-11-25T00:00:00+00:00
draft: false
weight: 25
toc: true
---

## The Juniors

[Zero Dot Force](https://github.com/zero-dot-force) is Unbound Force's incubator. Where Muti-Mind, Cobalt-Crush, Gaze, The Divisor and Mx F are the senior heroes, Zero Dot Force is the junior team: smaller, newer tools that each do one job and check the quality of AI-written code and tests.

Each junior project lives in its own repository and installs on its own, the same [composability](/docs/vision/) rule the senior heroes follow. Several of them team up with a senior hero: they extend what that hero can see, or carry a hero's practice into a standalone package.

## Projects

| Project                                                            | What it does                                                                                    | Trains with                            | Get it                                                                 |
| ------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------- | -------------------------------------- | ---------------------------------------------------------------------- |
| [vibe-check](https://github.com/zero-dot-force/vibe-check)         | Coupling, cohesion and dependency-cycle metrics for Go, Python and TypeScript, emitted as JSON. | —                                      | [Releases](https://github.com/zero-dot-force/vibe-check/releases)      |
| [reading-stone](https://github.com/zero-dot-force/reading-stone)   | TypeScript analyzer for Gaze: side effects, coverage and test-to-assertion mapping.             | [Gaze](/docs/team/gaze-tester/)        | [npm](https://www.npmjs.com/package/reading-stone)                     |
| [snake-eyes](https://github.com/zero-dot-force/snake-eyes)         | Python analyzer for Gaze: side effects, coverage and test-to-assertion mapping.                 | [Gaze](/docs/team/gaze-tester/)        | [PyPI](https://pypi.org/project/snake-eyes-analyzer/)                  |
| [review-council](https://github.com/zero-dot-force/review-council) | Six specialist AI reviewers that review a change in parallel and return one verdict.            | [The Divisor](/docs/team/the-divisor/) | [Lola module](https://github.com/zero-dot-force/review-council#readme) |

### vibe-check

Vibe-check measures design and architectural quality. It computes Robert C. Martin's package-coupling metrics — afferent and efferent coupling, instability, abstractness and distance from the main sequence — plus LCOM4 cohesion and circular-dependency detection, and writes the results as JSON. It detects a project's language from its markers (`go.mod`, `pyproject.toml`, `tsconfig.json`); Python and TypeScript analysis uses the external [ripwire](https://github.com/redhat-et/ripwire) tool.

```bash
go install github.com/zero-dot-force/vibe-check/cmd/vibe-check@latest
vibe-check analyze .
```

Gaze tells you whether your tests are good; vibe-check tells you whether your design is sound. Its path to general availability is tracked on the [roadmap](/docs/roadmap/#vibe-check-ga).

### reading-stone

Reading-stone is the TypeScript analyzer backend for [Gaze](/docs/projects/gaze/). Gaze spawns it as a subprocess and talks to it over the JSON-RPC 2.0 [external analyzer protocol](/docs/projects/gaze/#external-analyzers); Gaze keeps the CLI, scoring and reports. Reading-stone reports side effects with contractual/incidental classification, complexity, coverage and test-to-assertion mapping for TypeScript code.

```bash
npm i -g reading-stone
```

### snake-eyes

Snake-eyes is the Python analyzer backend for [Gaze](/docs/projects/gaze/), built on the same protocol as reading-stone. It brings side-effect detection, classification signals, complexity, coverage and test-to-assertion mapping to Python 3.11+ codebases.

```bash
uv tool install snake-eyes-analyzer
```

### review-council

Review-council is a standalone take on [The Divisor's](/docs/team/the-divisor/) review council, packaged as a [Lola](https://github.com/LobsterTrap/lola) module so it works in Claude Code, Cursor, Gemini CLI and OpenCode without the rest of Unbound Force. Six reviewer personas — the Guard, Architect, Adversary, Tester, Operator and Curator — review a change in parallel. The council checks every finding against the actual files, drops fabricated evidence, and returns one verdict: **APPROVE** or **REQUEST CHANGES**.

```bash
lola mod add https://github.com/zero-dot-force/review-council.git
lola install review-council
```

## Next Steps

- Browse the [Zero Dot Force organization on GitHub](https://github.com/zero-dot-force)
- Meet the senior heroes on [The Team](/docs/team/) page
- See how the juniors fit the plan on the [Roadmap](/docs/roadmap/)
