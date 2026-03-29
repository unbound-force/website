---
title: "Hero Artifacts"
description: "The 7 inter-hero artifact types, the standard envelope format, and how artifacts flow through the hero lifecycle."
lead: "How heroes communicate — structured artifacts with a standard envelope format that enables autonomous, auditable collaboration."
date: 2026-03-29T00:00:00+00:00
draft: false
weight: 75
toc: true
---

## How Heroes Communicate

Heroes in the Unbound Force swarm communicate through **artifacts** -- structured files that follow a standard envelope format. This is the practical implementation of [Constitution Principle I: Autonomous Collaboration](/docs/getting-started/constitution/#principle-i-autonomous-collaboration): heroes collaborate through well-defined artifacts rather than runtime coupling or synchronous interaction.

Every artifact is self-describing. A consumer hero can interpret any artifact without consulting the producing hero, because the envelope contains all the metadata needed: who produced it, when, what type it is, and what version of the schema it follows. This makes hero communication asynchronous, auditable, and resilient to individual hero unavailability.

## Envelope Format

Every inter-hero artifact uses a standard envelope with 7 fields:

| Field            | Description                                                                                       |
| ---------------- | ------------------------------------------------------------------------------------------------- |
| `hero`           | Name of the producing hero (e.g., `gaze`, `the-divisor`, `muti-mind`)                             |
| `version`        | Version string of the producing hero                                                              |
| `timestamp`      | ISO 8601 timestamp of when the artifact was produced                                              |
| `artifact_type`  | One of the 7 defined types (see [Artifact Types](#artifact-types) below)                          |
| `schema_version` | Semantic version of the artifact's schema (e.g., `1.0.0`)                                         |
| `context`        | Contextual references: current branch, commit SHA, and backlog item reference                     |
| `payload`        | The artifact-specific data object -- structure varies by type but conforms to a registered schema |

The envelope format ensures that every artifact is machine-parseable (Constitution [Principle III: Observable Quality](/docs/getting-started/constitution/#principle-iii-observable-quality)) and includes provenance metadata for traceability.

## Artifact Types

The swarm defines 7 artifact types. Each has a designated producer hero and one or more consumer heroes:

| Type                  | Producer            | Consumers                     | Description                                                                                      |
| --------------------- | ------------------- | ----------------------------- | ------------------------------------------------------------------------------------------------ |
| `quality-report`      | Gaze                | Mx F, Muti-Mind, Cobalt-Crush | Test results, contract coverage data, CRAP scores, and risk analysis from quality validation     |
| `review-verdict`      | The Divisor         | Mx F, Cobalt-Crush, Muti-Mind | APPROVE or REQUEST CHANGES verdict with per-persona findings from the review council             |
| `backlog-item`        | Muti-Mind           | Mx F, Cobalt-Crush            | Feature or bug descriptions with priority scores (5-dimension composite) and acceptance criteria |
| `acceptance-decision` | Muti-Mind           | Mx F, Cobalt-Crush            | ACCEPT, REJECT, or CONDITIONAL decision with rationale after reviewing the completed increment   |
| `metrics-snapshot`    | Mx F                | Muti-Mind                     | Velocity, quality trends, review efficiency, and CI health metrics for the current period        |
| `coaching-record`     | Mx F                | All heroes                    | Learning feedback, actionable recommendations, and convention pack update suggestions            |
| `workflow-record`     | Swarm Orchestration | Mx F, Muti-Mind               | Complete workflow trace with per-stage timings, artifacts produced, and iteration history        |

Each artifact type has a registered JSON schema that defines the structure of its `payload` field. Schemas are versioned independently (the `schema_version` envelope field), allowing artifact formats to evolve without breaking consumers.

## Lifecycle Stage Mapping

Artifacts flow through the [6-stage hero lifecycle](/docs/getting-started/common-workflows/#new-feature-end-to-end). Each stage produces specific artifact types:

| Stage         | Hero                 | Artifacts Produced                                       |
| ------------- | -------------------- | -------------------------------------------------------- |
| **Define**    | Muti-Mind            | `backlog-item`                                           |
| **Implement** | Cobalt-Crush         | Code and tests (not envelope-formatted artifacts)        |
| **Validate**  | Gaze                 | `quality-report`                                         |
| **Review**    | The Divisor          | `review-verdict`                                         |
| **Accept**    | Muti-Mind            | `acceptance-decision`                                    |
| **Reflect**   | Mx F / Orchestration | `metrics-snapshot`, `coaching-record`, `workflow-record` |

The implement stage produces code rather than envelope-formatted artifacts. All other stages produce at least one artifact that feeds into subsequent stages or into [Mx F's](/docs/team/mx-f/) cross-hero analysis.

After a complete lifecycle, the reflect stage produces three artifacts that capture what happened (workflow-record), how it went (metrics-snapshot), and what to improve (coaching-record). These feed back into the next cycle's define stage through Muti-Mind's data-driven prioritization.

## Next Steps

- Read the [Constitution](/docs/getting-started/constitution/) to understand the principles that govern artifact communication
- See [Common Workflows](/docs/getting-started/common-workflows/) for the full 6-stage hero lifecycle
- Explore the [Team](/docs/team/) pages to see how each hero produces and consumes artifacts
