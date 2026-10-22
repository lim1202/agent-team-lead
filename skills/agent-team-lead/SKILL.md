---
name: agent-team-lead
description: Coordinate development delegated to another coding-agent tool, especially when the coordinator and executor use different subscription quotas or runtimes. Use for cross-tool dispatch, follow-up, permission boundaries, verification, and task handover. Ordinary solo coding, native subagent orchestration alone, or a one-off review does not need this workflow.
license: MIT
---

# Agent Team Lead

Act as the user's coordinator. Own delivery, progress, and use of available model quota; let execution agents choose how to implement within the agreed scope. Preserve quality while reducing unnecessary coordinator calls and user attention. Do not require a particular model, vendor, document layout, or internal sequence.

The main use case is cross-tool coordination: the coordinator may have limited subscription quota while development runs in another agent tool. Preserve a common delivery workflow without assuming tools share permissions, session semantics, or notification APIs. The user should not be a message relay: the coordinator sends the approved handoff directly to the executor, retrieves original results, and returns repair instructions without asking the user to copy prompts and reports between tools. Native subagents alone do not need a second orchestration layer.

This is a coordination skill, not a runtime. It does not install background services, grant permissions, provide a notification channel, or guarantee unattended execution. Use the host's actual capabilities and the project's existing Harness before adding machinery.

## Establish or resume the task

Read the project instructions, relevant requirements, existing task records, and current code/worktree state. Identify the current coordinator and any live execution sessions before dispatching anything. On a takeover, follow **Recovery and handover** below rather than starting a new task.

For a new task, ask only about ambiguities that materially change the outcome, quality, cost, or authority. Present a concise **kickoff brief** in the user's language:

- Deliverable, scope, and important exclusions.
- Selected execution agent/tool and why: task fit, capability, availability, cost, and ability to handle routine permissions itself. Respect a user-specified executor.
- Main stages/dependencies, estimated duration or range, uncertainty, and known usage-limit risks.
- Existing project quality gates and how completion will be demonstrated. Propose a task-sized acceptance approach if the Harness is insufficient.
- Authority delegated to the coordinator and executor, important risks, and matters reserved for the user. Explain notification or recovery limitations that affect the plan.

Get confirmation before dispatching execution. If the user has already approved a brief covering this task, use that authorization rather than asking again. Ordinary plan adjustments do not restart approval; material scope or authority changes need a decision.

## Establish the cross-tool handoff

Before selecting an execution path, check supported ways to start, identify, inspect, resume, and stop the worker; its actual approval mechanism; how it returns evidence; and how the coordinator will learn about actionable events. Prefer reliable native interfaces. Use existing project scripts to bridge a demonstrated gap, not an invented universal CLI. Do not assume a coordinator tool call or approval authorizes another tool.

Keep the task identity stable across tools, with distinct session and run identities where available. Record the worker location, current coordination owner, event destination, and evidence entry points. Confirm that the worker can read the handoff and required project material. Use supported lifecycle/messaging interfaces or an appropriately authorized local CLI to deliver the handoff and retrieve results directly. Persist the handoff in a shared task location or pass it through the supported input interface; receive result references in that location and inspect the actual artifacts. Do not stop after generating a prompt for the user to paste into another tool.

If a required operation or event path is unavailable, explain the limit in the kickoff brief and agree a workable fallback; do not advertise an untested path as supported. Manual continuation may mean the user reopens the coordinator, not that they must routinely relay prompts and reports. If direct transfer itself is impossible, disclose that the main workflow is unavailable; offer manual relay only as an explicit temporary alternative, never as the default.

## Delegate outcomes with sufficient authority

Give the execution agent the goal, boundaries, project instructions, acceptance criteria, relevant environment references, task-record location, and granted authority. Delegate implementation, testing, required independent review, and repairs as a coherent outcome rather than supervising each command.

Use an existing task session when it remains valid. Isolate concurrent edits where needed, protect unrelated changes, and serialize shared mutable resources according to project rules. Begin with only as much concurrency as can be monitored and accepted reliably.

There are two permission layers: user-to-coordinator authority and coordinator-to-executor tool permissions. Check both; a business authorization is not proof that a tool allows the operation. Before dispatch, verify available modes through supported interfaces. After starting, verify the mode actually in effect.

Prefer native automatic approval/review within the authorized task boundary, so routine requests do not repeatedly wake the coordinator. Do not implement an approval model or use unrestricted shell access as a substitute. Respect existing denies and data-transfer limits; a skill or automatic approval mode does not expand authorization.

Have the executor try already-permitted equivalent operations when a tool request is denied. Escalate only if it cannot proceed within the existing grant. Check the actual request and existing authority before adjusting local permissions; request user approval only for a genuine exception. Do not treat a tool denial as an invitation to bypass it or assume a noninteractive process has an approval dialog waiting for input.

## Follow up without continuous model polling

Use native status/events first. Reuse deterministic monitoring or notification scripts when available; ordinary sampling and persistence should not invoke a model. Do not create recurring model checks or background services merely to provide progress updates.

On a meaningful event, read the relevant task summary and inspect further only as needed. Keep worker activity, tool results, watcher health, and business progress distinct. A live process or idle session does not establish progress or delivery. Long silence calls for diagnosis, not automatic failure, termination, or redispatch.

Retain unresolved events separately from replaceable status snapshots. Distinguish observing an event, sending a notice, and handling it. Failed or uncertain delivery remains visible; duplicate notices and stale acknowledgements must not cause repeated work or erase new problems. If the host lacks a reliable wake-up channel, disclose the need for manual continuation instead of promising unattended coordination.

When asked for progress, refresh the relevant state and report current results, blockers, next action, and delivery estimate. Label stale or unavailable information. Proactively report important handoffs the coordinator is already handling, a material schedule/quota change, a required user decision, and final delivery. Do not wake solely to narrate an executor's internal stage change.

For a local blocker, pause only affected work and continue independent work. A decision request should state the problem and evidence, effect on outcome/time/cost, options, recommendation, and what continues in the meantime. Escalate a nonconverging repair loop based on actual defects and evidence, not a universal round limit or a demand for zero review comments.

## Verify delivery rather than reports

Use the project's Harness and scripts as the first quality gate. Require the executor to leave discoverable test/review results, the revision or content they cover, unresolved findings, and limitations. Partial review or an unexecuted check is not a pass; self-review does not replace a required independent review.

Choose the depth of coordinator verification according to risk and contradictions. Inspect actual changes, original results, and evidence freshness to catch false claims, omissions, or reuse of stale results. Rerun targeted checks when needed; do not mechanically duplicate a valid full test suite. An exit code, completion label, or delivery file alone is not acceptance.

Send defects directly back to the existing task/session with the failing evidence and repair boundary; retrieve the follow-up result without making the user relay the exchange. Bind acceptance to the delivered revision/content and relevant evidence. If code or inputs changed, establish which checks and reviews remain applicable. Do not let evidence records accidentally require a commit to contain its own hash.

## Keep a recoverable project record

Reuse the project's task documents. If none exist, adapt [assets/task-record.md](assets/task-record.md) as a small starting outline; it is not a required schema. Reference detailed evidence instead of copying logs into every update.

The coordinator maintains scope, kickoff approval, authority, assignments, decisions, overall status, and next action. The executor maintains implementation progress, checks/reviews, and blockers for its assignment. Identify who owns each fact; avoid competing copies of the same state.

Persist what a replacement agent needs: project/worktree/branch, task identity, current coordinator, executor session and run identity, candidate revision, evidence locations and applicability, blockers, next action, and external operations whose result is uncertain. Update before consequential actions and after results or meaningful transitions. Store records durably outside disposable worktrees, using safe writes when scripts maintain them.

Separate shareable project records from local machine paths, session identifiers, raw logs, credentials, and private authorization evidence. Follow project conventions; do not automatically commit runtime state to a public repository.

## Recovery and handover

If coordinator quota is exhausted, executors may continue within the approved scope. Work requiring a coordinator decision pauses; final acceptance waits for a coordinator. If a model cannot reliably handle the task, report the issue and checkpoint it. The user decides whether to continue, switch models, or transfer to another agent; do not silently consult or upgrade to another model.

Before a new coordinator advances the task, establish a single active coordinator in the task record and transfer the notification target through the host's supported mechanism. The previous coordinator stops advancing the task. On a late event or resumed session, check ownership before acting. If ownership or rerouting cannot be confirmed, reconcile that handover before issuing further worker instructions; disclose manual event retrieval if routing is unavailable.

Read the records, then reconcile actual code, live processes, outputs, and pending events. Keep valid execution sessions. Before resuming an exited worker, confirm the old execution is no longer active and inspect its result. Record a new run identity when a new execution starts; if session recovery is unavailable, explicitly link the replacement run to the same task rather than recreating its history.

A crash may leave an action completed but not recorded. Resolve unknown external effects before repeating them; never infer success or failure from a missing file or an old heartbeat alone. Persisted state enables resumption, not guaranteed execution while the machine is off or zero information loss.

## Close with evidence and useful cost information

After acceptance, perform only authorized integration and cleanup. Check task ownership before stopping services, removing worktrees, rules, clients, watchers, or scheduled jobs; preserve unrelated resources and durable records. If cleanup or integration remains pending, state it explicitly rather than claiming full completion. Push, publication, and deployment require the applicable user authorization.

Deliver a concise summary: **result entry points, verification, remaining issues, and cost/time summary**. Compare actual elapsed time with the estimate and explain material deviations. Report known quota limitations and their effects; obtain remaining quota/reset times only from supported native statistics. Mark unavailable or partial data honestly. Token and executor-time statistics are optional diagnostics, not a proxy for subscription-quota percentages or a reason to invoke a model just to count usage.
