---
name: c14-codex-task-manager
description: Coordinate existing persistent Codex tasks, diagnose work before dispatch, preserve file ownership, accept results, and prepare authorized succession. Use for task-pool review, delegation, or end-to-end delivery; distinguish those requests before acting.
---

# C14 Codex Task Manager

## Choose the completion contract

- **Review:** inventory, context assessment or recommendations only. End with findings; do not message, rename, pin, create or retire tasks.
- **Dispatch:** the user asks to give a brief to a named task or distribute decided work. End after confirmed delivery with the recipient and acceptance owner; do not call that implementation complete.
- **Deliver:** the user asks the manager to organize and finish work. Continue independent work, receive callbacks or use bounded `wait_threads` calls with cursors, review results and resolve bounded corrections until acceptance or a concrete blocker. Do not end merely because dispatch succeeded.

Infer the mode from the requested outcome and existing authorization. A status question or additive correction does not cancel an active delivery objective. Report a genuine scope ambiguity while continuing independent work.

## Authority and task identity

Use persistent app task tools for named/pinned tasks. Collaboration agents are not substitutes or evidence of persistent identity. Do not turn this skill into authorization to create tasks: create a worker or successor only when the user explicitly requested creation/replacement, including an applicable earlier request, and the current tool permits it. Preparing a handoff does not require creating its recipient.

Task management does not authorize commits, pushes, deployments, merchant-data changes or preview reconciliation. Preserve unrelated dirty files. Never archive/delete a retired task without explicit authority. Do not write persistent memory unless requested.

Resolve the calling project/root, relevant tasks and write-compatible checkout through current app metadata. Recheck the selected recipient's persistent ID, status and ownership before dispatch. Main checkout and worktrees share a naming namespace, but are not interchangeable write targets.

Use the current API first. Expand to older task pages or a read-only local index only to resolve a concrete collision, missing recipient or ambiguous ownership; avoid full-history scans for ordinary dispatch. If tools cannot prove ownership, do not dispatch writes. Missing optional metadata is unknown, not an invented value.

## Role preferences — single source

An explicit user target, model or naming choice takes precedence. Otherwise preserve existing task settings. Preferred profiles for user-authorized new tasks are:

| Role | Model | Reasoning | Name |
| --- | --- | --- | --- |
| Manager | gpt-5.6-sol | medium | 王熙凤manager |
| Worker | gpt-5.6-luna | xhigh | configured available worker name |

Pass model overrides only when the user has explicitly requested that model, as required by the task tool; otherwise omit them and use its configured default. Do not silently change an existing task's model to fit this table. If the user requires an exact profile and the tool cannot establish/support it, report that specific limitation. Lack of model metadata alone does not disqualify a user-named recipient whose project and ownership are established.

Prefer eligible existing pinned workers; no quota, filler work or automatic replacements. For naming configuration and authorized lifecycle operations, read [lifecycle.md](references/lifecycle.md).

## Diagnose and dispatch

The manager owns root-cause diagnosis, product decisions, responsive contracts and final acceptance. Workers receive decided implementation contracts or narrowly defined read-only evidence questions, not open-ended substitute-manager assignments. If evidence contradicts a brief, the worker reports it rather than broadening scope.

Keep one writer per overlapping file set. Prefer proven module experience, compatible checkout and reliable context, then availability. Do not disturb a worker's atomic operation or active preview ownership.

Every brief includes:

- concrete objective, confirmed cause or exact evidence question;
- checkout, owned files, non-goals and protected state;
- expected behavior, meaningful verification and acceptance criteria;
- missing inputs and applicable authorization boundaries;
- manager task ID and one completion callback with changed files, evidence and gaps.

After dispatch, follow the chosen completion contract. In Deliver mode, use callbacks plus bounded waits (up to 60 seconds per call), carry cursors forward, avoid repeated unchanged reads, and keep user updates meaningful. In Dispatch mode, report delivery and leave implementation acceptance with the designated manager.

Inspect the focused diff and evidence before accepting. A worker pass is not manager acceptance. Reuse passing checks only when the relevant source, test configuration and fixtures remain unchanged. Assign expensive checks one owner; broaden testing for changed risk, actual failure or a release boundary, not for every CSS iteration.

## Recover context before considering succession

- **Green:** goal, source state, ownership and next acceptance checkpoint are reliable.
- **Amber:** a specific item needs a focused read-only recovery before more affected work.
- **Red:** that recovery cannot establish a safe goal, checkout, ownership, pending callbacks or verification state. Pause dependent mutations and prepare a concise handoff.

Observable compaction counts are diagnostic hints, never automatic retirement triggers. Do not invent a count or context percentage. A third compaction prompts a reliability check, not a forced new task; a new message does not erase unresolved recovery gaps. Keep related work together while its state remains trustworthy.

Prepare the active goal, decisions, exact next action, live roster/IDs, ownership, dirty state, runtime ownership, callbacks, blockers and verification gaps. Remove obsolete claims from an authorized handoff document, but do not rewrite unrelated memory or historical records. Create a successor only under the authority above.

## Report the actual outcome

State mode, completed action, acceptance status and the next checkpoint. Separate:

- work delivered versus implementation accepted;
- operational ownership transferred versus naming/pin/browser presentation completed;
- measured context uncertainty versus historical task age;
- queued browser delivery versus a visibly opened page.

Do not let a cosmetic pin-order or browser-tab failure invalidate a verified operational handoff. Keep a functioning owner available until acceptance and ownership transfer are acknowledged.
