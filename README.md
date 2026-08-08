# C14 Codex Task Manager

A Codex skill for running a long-lived project through one manager task and a small pool of persistent worker tasks.

It is designed for projects where work arrives as a continuous stream of related fixes, reviews, and component-level changes. Instead of opening a fresh chat for every small request, the skill keeps one manager as the project brain and delegates bounded assignments to three reusable Luna workers.

## The problem it solves

Long-running Codex projects tend to develop the same operational problems:

- one task accumulates too much implementation detail and becomes expensive to recover;
- new tasks lose important checkout, runtime, design, and safety context;
- several workers accidentally edit the same files;
- work is assigned without regard to a worker's previous module experience;
- a worker finishes, but nobody performs final acceptance against the original brief;
- tasks are replaced simply because they are old or were compacted once;
- manager succession loses pinned order or the active preview URL.

C14 Codex Task Manager treats those as coordination problems rather than prompt-writing problems.

## The operating model

```text
Manager task
├── Luna worker 1
├── Luna worker 2
└── Luna worker 3
```

The manager is the durable control plane. It retains the program goal, task roster, file ownership, protected state, runtime ownership, acceptance history, and the next checkpoint.

The default pool contains three persistent workers using `gpt-5.6-luna` with `xhigh` reasoning. Workers are not disposable one-shot agents: the manager reuses them for closely related modules when their checkout and context remain reliable.

### Manager responsibilities

- understand the user's current request and define a bounded objective;
- choose the worker with the closest proven experience;
- maintain one writer per overlapping file set;
- include the authoritative checkout, constraints, non-goals, and verification in every dispatch;
- protect dirty work, external state, and runtime ownership;
- wait for one completion callback instead of polling workers continuously;
- perform final acceptance and issue one bounded correction when necessary;
- manage task naming, pinning, retirement, and succession.

### Worker responsibilities

- treat the manager's dispatch brief as the primary context;
- work only in the assigned checkout and owned files;
- preserve protected and unrelated state;
- implement and verify the bounded assignment;
- send one completion callback containing changed files, evidence, gaps, and blockers;
- never claim manager-level acceptance for its own work.

This separation lets workers stay focused while the manager preserves cross-assignment continuity.

## Assignment lifecycle

```text
user request
    ↓
manager resolves scope, ownership, and safety
    ↓
best-matched worker receives a bounded brief
    ↓
worker implements and verifies
    ↓
worker sends one completion callback
    ↓
manager accepts or sends one correction brief
    ↓
manager records the next checkpoint and returns to idle
```

The manager does not poll idle workers. A successful dispatch is not reported as completed work; completion exists only after the callback and final acceptance pass.

## Context lifecycle

The skill uses Green, Amber, and Red as economic and reliability states, not as age limits.

- **Green** — the program goal, authoritative source, ownership, protected state, and next action are reliable. Continue in the same task.
- **Amber** — finish the current atomic assignment, then hand off before adding more scope.
- **Red** — stop adding scope and create a confirmed successor before further implementation.

A task is not Amber or Red merely because it has many turns, completed several related features, or experienced one compaction.

Workers and managers use different thresholds:

- A worker can be replaced sooner because its bounded dispatch is cheap to reconstruct. After repeated compaction, it finishes its current assignment and hands off before receiving another.
- A manager stays longer because replacing it discards valuable program-level knowledge. It becomes Red only when a focused read-only recovery can no longer reconstruct the goal, checkout, ownership, callback state, protected runtime, and next acceptance checkpoint, or when recovery cost repeatedly overtakes useful coordination.

This keeps component-polish workflows in one coherent program while preventing repeated context reconstruction from wasting time and tokens.

## Worker selection and file safety

Before dispatch, the manager excludes retired, blocked, Red-context, wrong-checkout, and file-conflicting tasks. It then prefers:

1. prior experience in the same module;
2. the correct checkout or worktree;
3. Green context over Amber context;
4. an idle or less recently loaded worker.

Only one worker may own an overlapping file set at a time. A second worker may perform read-only acceptance or an independent diagnosis, but must not become a concurrent writer.

## Persistent task pool

Active tasks stay pinned in a stable order:

1. manager;
2. Luna worker 1;
3. Luna worker 2;
4. Luna worker 3.

When a worker or manager is replaced, its successor takes the same logical slot. Retired tasks are renamed with their main contribution and unpinned rather than deleted, so history remains available and names can return to the pool.

The bundled default naming mode uses Japanese actress names and supports Chinese, Japanese, and English aliases. Projects may instead configure a custom pool or ordinary descriptive titles.

## Succession and preview continuity

A manager never retires before its successor is created, pinned, and acknowledged.

The handoff includes the active roster, file ownership, callback state, safety constraints, runtime ownership, and exact next action. When browser QA is in progress, it also carries the exact preview URL. After acknowledgement, Codex opens or queues that same URL for the successor without restarting the preview.

Pinned order is then normalized as manager first, followed by the existing workers in their prior relative order.

## Safety model

The skill separates read-only assessment from task mutation.

- Reviews, inventory, explanations, and context audits are read-only.
- Creating, renaming, pinning, unpinning, retiring, or messaging tasks requires active task-management authority.
- The manager must verify checkout and worktree compatibility before assigning writes.
- Runtime restart or stop operations require ownership checks.
- Retired tasks are not archived or deleted unless the user explicitly asks.
- Project secrets, transcripts, repository state, and personal data are never stored in the naming configuration.

Some lifecycle features depend on Codex desktop task-management and browser-routing tools. If those tools are unavailable, the skill reports the limitation instead of pretending the action succeeded.

## Install

Clone the repository and copy the bundled skill into your Codex skills directory:

```bash
git clone https://github.com/liangsu1210/c14-codex-task-manager.git
cp -R c14-codex-task-manager/skills/* ~/.codex/skills/
```

Restart or refresh Codex so it discovers the skill.

## Use

Invoke the manager:

```text
$c14-codex-task-manager
```

Example:

```text
Use $c14-codex-task-manager to inspect the active worker pool, assign this bug to
the worker with the closest module experience, preserve file ownership, and
replace the manager only if its context is genuinely Red.
```

On first use, choose the default actress pool, a custom name pool, or descriptive task titles. The selected language remains stable for the project unless it is explicitly reconfigured.

## Repository layout

- `skills/c14-codex-task-manager/SKILL.md` — lifecycle and dispatch rules
- `skills/c14-codex-task-manager/agents/openai.yaml` — Codex UI metadata
- `skills/c14-codex-task-manager/references/default-name-pool.json` — multilingual default name pool

## License

MIT. Free to use, modify, and redistribute.

This is a community project and is not affiliated with or endorsed by OpenAI.
