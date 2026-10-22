# Agent Team Lead

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

**One leader. Multiple coding agents. Work carried through to verified delivery.**

Agent Team Lead is a proposed cross-harness skill and supporting runtime for managing coding-agent teams. A coordinating agent owns planning, delegation, follow-up, recovery, and acceptance. Execution agents implement, test, and resolve review findings. The user (project owner) sets direction, sees progress, and handles decisions that require their authority.

The project focuses on cross-tool coordination when development is delegated to another agent tool, for example to preserve a coordinator’s limited subscription quota. Native subagents can handle in-tool parallel work; this project addresses differences in permissions, sessions, notifications, and recovery across tools. It extracts reusable workflows and scripts from practical experience. The motivating problem is manual relay: users copying a coordinator’s handoff prompt into an executor, then copying its report back. The intended workflow has the coordinator send handoffs, retrieve results, and request repairs directly.

It aims for verified delivery, useful progress updates, and keeping usage within available limits while reducing the user’s attention and coordination burden. It defines outcomes and responsibilities, leaves room for agent judgment, and reuses project quality checks and native automation.

## Project status

**An initial instruction-only skill is available in source. No bundled runtime, validated cross-tool adapter, or plugin is included yet.**

Both real-project experiments—parallel frontend migration and a webhook service—have ended. Their evidence informs the [first-version design](docs/v0-design.md) and [skill implementation record](docs/implementation-plan.md) (Chinese). The initial skill is not an end-to-end compatibility or reliability guarantee.

## Try the initial skill

The source package is [`skills/agent-team-lead`](skills/agent-team-lead/SKILL.md). Load that folder using your host’s supported skill installation method, then ask the agent to coordinate a development task in another agent tool. The current conversation becomes the coordinator and presents a kickoff brief before dispatch. An optional [task-record outline](skills/agent-team-lead/assets/task-record.md) helps projects without existing handoff records.

The skill provides instructions, not an execution or notification service. Actual cross-tool use requires supported worker lifecycle interfaces, suitable permissions, accessible project records, and a notification or agreed manual continuation path. No cross-tool path has been validated with this new skill yet.

## Responsibilities

| Role | Responsibility |
| --- | --- |
| User / project owner | Set outcomes and constraints, view progress, and decide matters outside delegated authority. |
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

This table describes responsibilities and the longer-term direction. The first version will validate one execution path and add only the mechanisms that path needs.

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

## Next steps

- Validate the skill on a concrete cross-tool path against the design’s user experience and acceptance scenarios; keep private retrospective material outside Git.
- Reuse existing host and project mechanisms first. Projects such as [delegate-skills](https://github.com/amElnagdy/delegate-skills), [acpx](https://github.com/openclaw/acpx), and [CLI Agent Orchestrator](https://github.com/awslabs/cli-agent-orchestrator) remain references; no third-party dependency or source-code reuse is selected for the first version.
- Select and justify one concrete execution path, verify executor-side automatic approvals and coordinator handover, then plan only the runtime mechanisms needed for that path.
- Measure human interventions, context-recovery effort, coordinator model invocations, and verified task outcomes. Do not claim savings before measurement.

## Contributing

Design feedback is welcome. Please keep proposals separate from validated behavior and use synthetic examples. Do not include credentials, private project paths, customer data, or unredacted execution logs.

## License

[MIT](LICENSE). Future reused components retain their applicable license notices.
