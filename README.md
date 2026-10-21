# Agent Team Lead

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

**One leader. Multiple coding agents. Work carried through to verified delivery.**

Agent Team Lead is a proposed cross-harness skill and supporting runtime for managing coding-agent teams. A coordinating agent owns planning, delegation, follow-up, recovery, and acceptance. Execution agents implement, test, and resolve review findings. The human sets direction, sees progress, and handles decisions that require their authority.

The project focuses on reducing human coordination and context switching, not simply launching more agents.

## Project status

**Design only. No installable skill, runtime, harness adapter, or plugin is included yet.**

Two ongoing real-project experiments inform this design: parallel frontend migration and a webhook service. They are not both complete, and their current behavior is not a compatibility or reliability guarantee. Implementation will begin after their delivery, recovery, and cleanup lessons have been reviewed.

## Responsibilities

| Role | Responsibility |
| --- | --- |
| Human / project owner | Set outcomes and constraints, view progress, and decide matters outside delegated authority. |
| Coordinating agent / leader | Own the task lifecycle, select executors, manage dependencies, resolve blockers, verify delivery, and arrange cleanup. |
| Execution agent | Implement within scope, run checks, complete the configured independent-review and repair loop, and leave evidence and handoff records. |
| Deterministic runtime | Track processes and events, validate records, deduplicate notifications, monitor health, and perform authorized lifecycle operations. |

The leader remains responsible for progress. A worker returning a result is a handoff, not automatic acceptance.

## Design principles

1. **Manage through completion.** Delegate coherent outcomes, follow up without repeated human prompts, and carry work through verification and cleanup. The leader coordinates judgment; it does not micromanage every command.
2. **Separate judgment from mechanics.** Scripts handle routine checks and state transitions. Models handle planning, diagnosis, review, and decisions. Normal monitoring should not repeatedly invoke a model.
3. **Use durable project state.** Keep scope, decisions, execution identifiers, candidate revisions, evidence, blockers, and next actions with the project. Conversations help explain history but are not the only recovery mechanism.
4. **Make progress visible.** Prefer native agent status and logs. Present observed activity and time since the last update; do not invent completion percentages or confuse process activity with business progress. No separate monitoring app is required by the design.
5. **Recover deliberately.** A quiet process is potentially stalled, not necessarily failed. Detect process exits and monitor failures, preserve the scene, retry notifications within bounds, and resume the same task where safe. Do not blindly redispatch work with side effects.
6. **Work within delegated authority.** Pre-authorize task-appropriate operations where the host supports them. Request decisions for genuine exceptions. Never assume one harness's permission settings authorize another harness or default to unrestricted shell access.
7. **Verify independently.** Bind delivery and review evidence to the task, run, and candidate revision. Check actual changes and relevant results; an exit code, worker report, or review label alone does not prove completion.
8. **Parallelize selectively.** Isolate working trees and task artifacts. Serialize shared accounts, mutable test resources, and integration where needed. Limit concurrency when acceptance becomes the bottleneck.
9. **Keep the human out of routine routing.** Report meaningful milestones, actionable blockers, and final outcomes. Native views or deterministic notifications provide lightweight progress without generating a model summary on every update.
10. **Keep the core portable.** Define coordinator and executor roles independently of products. Prefer reliable native lifecycle capabilities, with explicit script fallbacks and documented capability gaps.

## Intended architecture

| Layer | Intended contents |
| --- | --- |
| Portable skill | Leadership workflow, delegation contract, recovery decisions, acceptance, and reporting rules. |
| Shared runtime | Process and event handling, health checks, delivery-record validation, bounded notification retries, and cleanup. |
| Harness adapters | Start, resume, inspect, message, interrupt, and capability discovery using each tool's supported interfaces. |
| Project configuration | Requirement entry points, verification commands, review integration, environment references, permission boundaries, and shared-resource policy. |

Claude Code, Codex, Antigravity, OpenCode, and DeepSeek Harness are candidate integrations, not currently supported adapters. Native Codex subagents are another candidate execution path. Neither role is tied to a particular vendor.

The design does not promise equivalent capabilities across harnesses, unlimited unattended execution, or progress while all supporting processes are stopped. Event delivery needs acknowledgement and an explicit fallback when the coordinating session cannot receive a notification.

## Distribution plan

The intended distribution is one source repository with an Agent Skills package installable through `npx skills`, plus Codex and Claude Code plugin packaging. These are **planned**, not available installation methods. Runtime dependencies, host permissions, background-service setup, and adapter compatibility will be documented separately; installing a skill does not automatically provision them.

## Before implementation

- Finish and review both experiments, including permission blocking, quiet long-running commands, notification failure, recovery, and cleanup.
- Decide what to reuse from existing projects such as [delegate-skills](https://github.com/amElnagdy/delegate-skills), [acpx](https://github.com/openclaw/acpx), and [CLI Agent Orchestrator](https://github.com/awslabs/cli-agent-orchestrator). No dependency or code reuse is selected yet.
- Define a small task/run/event/delivery contract and capability matrix, then implement only the first proven execution path.
- Measure human interventions, context-recovery effort, coordinator model invocations, and verified task outcomes. Do not claim savings before measurement.

## Contributing

Design feedback is welcome. Please keep proposals separate from validated behavior and use synthetic examples. Do not include credentials, private project paths, customer data, or unredacted execution logs.

## License

[MIT](LICENSE). Future reused components retain their applicable license notices.
