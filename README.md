# C14 Codex Task Manager

A free, open-source Codex skill for managing long-running projects as a pool of persistent tasks.

It helps Codex:

- assess whether the current context is Green, Amber, or Red;
- route work to the worker with the most relevant experience;
- avoid overlapping writers on the same files;
- pin active workers and retire completed ones;
- recycle a finite naming pool without losing task history;
- create and verify a successor before the manager retires itself.

The bundled default naming convention uses Japanese actresses' common Chinese names. You can replace the pool in `SKILL.md` with any project convention.

## Install

Clone the repository and copy the bundled skills into your Codex skills directory:

```bash
git clone https://github.com/liangsu1210/c14-codex-task-manager.git
cp -R c14-codex-task-manager/skills/* ~/.codex/skills/
```

Restart or refresh Codex so it discovers the skills.

## Use

Invoke the main manager:

```text
$c14-codex-task-manager
```

Example request:

```text
Use $c14-codex-task-manager to inspect the pinned workers, assign this Header bug
to the worker with the closest experience, and replace the manager if its context
is Red.
```

## Safety model

The skill separates read-only audits from task mutations. Creating, renaming, pinning, unpinning, archiving, or messaging another task requires active task-management authorization. It also keeps one writer per overlapping file set and verifies checkout ownership before assigning writes.

Some capabilities depend on Codex desktop task-management tools. If those tools are unavailable, the skill should report the limitation instead of pretending a lifecycle action succeeded.

## Repository layout

- `skills/c14-codex-task-manager/` — canonical skill

## License

MIT. Free to use, modify, and redistribute.

This is a community project and is not affiliated with or endorsed by OpenAI.
