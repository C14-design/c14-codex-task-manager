---
name: c14-codex-task-manager
description: >-
  Manage a persistent Codex task pool end to end: assess context health, inventory
  and pin tasks, assign work to the best-matched existing worker, create named
  workers or successors, retire and rename completed tasks by their main
  contribution, and replace the manager itself when its context becomes stale.
  Use when the user invokes $c14-codex-task-manager, asks Codex to distribute or
  manage tasks/chats/threads, asks who should take work, asks to balance pinned
  workers, asks for context check plus succession, asks to create or rename a
  task, or asks to retire, pin, unpin, replace, or continue a task safely.
---

# C14 Codex Task Manager

Operate one project-scoped pool of persistent Codex tasks with an explicit lifecycle. Treat “task”, “thread”, and “chat” as the same persistent object.

## Respect authority

- Treat review, inventory, explanation, and context assessment as read-only.
- Mutate tasks only when the user requests task management or invokes this skill for active management.
- Treat active management as authorization for safe Red-status manager succession unless the user says review-only or forbids creating a task.
- Use persistent thread tools for named or pinned workers. Do not silently replace them with ephemeral subagents.
- Never archive a retired task unless the user explicitly asks. Retirement normally means rename plus unpin.
- Preserve any explicitly requested model and reasoning profile. If unavailable, use the configured default and report the limitation.

## Default worker pool

- Unless the user specifies otherwise, maintain three active persistent direct-report workers for the managed project.
- Unless the user specifies otherwise, create or replace workers with model `gpt-5.6-luna` and reasoning effort `xhigh` (Luna Extra High).
- Treat the three-worker target as a normal operating default, not a reason to create filler work: create workers only when there is a real bounded assignment, and keep the pool below three when no safe, useful assignment exists.

## Resolve the project namespace

1. Identify the calling task, Codex project, repository root, current checkout, and every Git worktree belonging to that repository.
2. List project tasks at the maximum supported limit. When the list may omit older tasks, supplement it with the local Codex thread index in read-only mode.
3. Treat the main checkout and its worktrees as one naming namespace, but evaluate checkout compatibility separately for write assignments.
4. Fail closed on naming or ownership when available sources cannot establish project-wide truth.

## Audit context health

Judge context economics from observable signals; never invent a percentage or token remainder.

- **Green** — one coherent goal, reliable current state, bounded continuation.
- **Amber** — finish the current atomic step, then hand off before another substantial feature.
- **Red** — stop adding scope and create a successor before more implementation.

Pressure signals include compaction replacing exact details, three or more completed feature clusters, a major topic switch, repeated source-state reconstruction, contradictory stale history, or repeated misunderstandings caused by old context.

Finish an in-progress atomic or safety-critical action before succession unless context loss already threatens correctness.

## Build the worker roster

Record only what affects assignment:

- active title and naming identity;
- active, idle, waiting, blocked, completed, or retired status;
- pinned state;
- repository checkout/worktree and write suitability;
- context health;
- strongest completed modules;
- current file ownership, blocker, and pending verification.

Derive expertise from actual work, not the worker’s display name.

## Assign work

1. Exclude retired, wrong-checkout, blocked, Red-context, and file-conflicting tasks.
2. Prefer a worker that already completed closely related work in the same module.
3. Prefer the authoritative checkout required by the task.
4. Prefer Green over Amber context, then an idle or less recently loaded worker.
5. Keep one writer per overlapping file set. Use another worker only for read-only acceptance or independent diagnosis.
6. Do not manufacture filler work merely to balance the pool.
7. Create a fresh worker only when no eligible existing worker is safe or the best worker needs succession.

Send a bounded brief containing the exact objective, non-goals, authoritative handoff, checkout, protected-state constraints, owned files, acceptance criteria, required verification, blockers, and approval boundaries.

Every dispatch must also include the manager thread ID and a completion callback requirement. When the assignment is finished, the worker must use persistent thread messaging to notify the manager once with its final result, changed files, verification evidence, unresolved gaps, and blockers. Do not rely on the manager polling for completion.

After a dispatch succeeds, the manager returns to idle. Do not call `wait_threads`, repeatedly read the worker, or otherwise monitor intermediate progress unless the user explicitly asks for monitoring or the worker reports that it needs attention, approval, or clarification.

When the worker's completion notification arrives, the manager owns final acceptance. Review the result against the original scope and non-goals, inspect relevant outputs or diffs, verify the claimed checks in proportion to risk, and either accept the work or send one bounded correction brief. Do not report the assignment complete to the user before this acceptance succeeds.

Pin the manager and actively assigned workers. Unpin retired tasks. Avoid changing unrelated projects’ pins.

## Configure naming on first use

There is no reliable post-install hook, so perform naming onboarding on the first invocation of this skill.

1. Look for a project override at `<project-root>/.codex/c14-codex-task-manager.json`, then a global preference at `${CODEX_HOME:-~/.codex}/c14-codex-task-manager.json`.
2. If neither exists and the user has not already stated a convention, ask them to choose:
   - the default Japanese-actress pool with random selection (recommended);
   - a custom pool of their favorite actresses or other names;
   - ordinary descriptive task titles.
3. Default `name_language` to `auto`; allow an explicit `zh`, `ja`, or `en` override.
4. For a custom pool, accept plain display strings or localized records with a stable `id` and `zh`, `ja`, and `en` aliases. Trim whitespace, remove duplicate IDs and aliases, preserve the user’s display spelling, and require at least one usable entry.
5. Ask whether to save the choice globally or only for the current project. Explain the exact target path and write it only after the user chooses. A project setting overrides a global setting.
6. If the user declines persistence, keep the choice for the current task only and explain that a future fresh task may ask again.
7. Do not create or rename a worker until first-use naming is resolved. Allow the user to reconfigure it later on request.

Store only `{"naming_mode":"default_actresses|custom_pool|descriptive","name_language":"auto|zh|ja|en","resolved_name_language":"zh|ja|en","name_pool":[]}`. Omit `resolved_name_language` until `auto` is resolved, and populate `name_pool` only for `custom_pool`. Do not place transcripts, project secrets, repository state, or personal account data in this configuration.

## Resolve the display language

1. Use an explicit `zh`, `ja`, or `en` configuration first.
2. For `auto`, reuse the project’s existing `resolved_name_language` when present.
3. Otherwise, detect the dominant language of recent substantive user-authored chat messages.
4. Ignore system/developer text, ambient browser context, webpages, tool output, logs, code, quoted material, and attachment contents.
5. If conversation evidence is inconclusive, use an exposed Codex UI locale; otherwise fall back to English.
6. Resolve once per managed project. Do not switch because of an occasional message in another language, and do not rename existing tasks automatically.
7. Persist the resolved language only with the same authorization and scope rules as the naming configuration.

Render `zh` as the common Chinese name, `ja` as the Japanese name, and `en` as the common English romanization. Treat a plain custom-pool string as language-neutral and display it verbatim.

## Name active workers

Use the project setting, then the global setting, then the current-task choice. In actress modes, randomly select an available canonical record and render it in the resolved language. In descriptive mode, use a concise title derived from the bounded assignment.

Before every create or rename:

1. Refresh app and local-index titles for the full project namespace.
2. Treat a non-retired title as reserving a canonical candidate when it equals or starts with any of that candidate’s configured `zh`, `ja`, or `en` aliases plus a suffix, separator, feature, date, or number.
3. Do not treat a correctly formatted retired title beginning with `退役｜` as reserving the original name.
4. When using the bundled default, read `references/default-name-pool.json` and compare every alias while keeping `id` as the cross-language identity.
5. Filter the configured canonical records to project-wide available IDs, randomly select one, and render the resolved-language alias.
6. Refresh again immediately before mutation and reroll from the remaining available IDs if the selected candidate became reserved.

If the pool is exhausted, retire eligible completed tasks and repeat the check, or ask the user to expand/change the pool. Never add numbers merely to bypass a collision.

## Retire and recycle a name

Retire only after work is complete, superseded, safely handed off, or proven unsuitable for the authoritative checkout.

1. Read the latest state and identify the task’s career-defining module and contribution.
2. Write a short factual summary rather than a chronology.
3. Rename it as `退役｜<主模块>·<主要贡献>｜原<名字>`; for example, `退役｜Header·玻璃菜单｜原上白石萌音`.
4. Unpin it. Do not archive or delete it without explicit authorization.
5. Return the original name to the pool only after the rename succeeds.

Do not retire a task merely because it is temporarily idle.

## Create a worker or successor

1. Clean the project’s current handoff in place when one exists. Verify it against current source and external state; remove solved, stale, superseded, and unrelated history.
2. Keep only the active goal, exact next action, decisions, constraints, dirty files, runtime ownership, blockers, and verification gaps.
3. Create the task in the current project and authoritative checkout. Do not fork conversation history unless explicitly requested.
4. Give it the bounded brief and point it to the current-state handoff.
5. Rename it with a freshly verified name, pin it, and dispatch the bounded brief with the manager thread ID and completion callback requirement.
6. After dispatch succeeds, return to idle without waiting for a progress snapshot. Claim only that assignment succeeded; claim task completion only after the worker notifies the manager and final acceptance passes.

## Replace the manager itself

When the manager is Amber, finish the atomic step and prepare succession before new scope. When Red, create a fresh manager under active-management authorization.

The manager handoff must also include:

- live worker roster and task IDs;
- ownership, status, last useful cursor, and blocker for each worker;
- pin state and retirement eligibility;
- exact next dispatch or follow-up;
- project safety rules and pending approvals.

Create and pin the successor before retiring the old manager. Wait once for acknowledgement. Then rename the old manager using its career summary and unpin it. If creation or acknowledgement fails, keep the old manager pinned and report the gap.

## Report compactly

For an audit, return:

- `Context status: Green | Amber | Red`
- `Active manager:` task title
- `Assignment:` worker → bounded objective
- `Lifecycle changes:` created, renamed, pinned, unpinned, or none
- `Next checkpoint:` normally the worker's completion notification, or one other concrete event

For read-only context assessment, also state `Handoff needed: No | After current step | Now`.

Do not dump transcripts, chronological logs, or speculative context percentages. Do not write persistent memory unless the user explicitly asks.
