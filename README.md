# C14 Codex Task Manager

Coordinate persistent Codex tasks while preserving project state and one writer
per file set. The manager diagnoses the problem, gives workers bounded contracts,
and accepts their results.

## Completion modes

- **Review:** inspect and recommend without task mutations.
- **Dispatch:** deliver the requested brief and identify its acceptance owner.
- **Deliver:** coordinate through implementation and acceptance, using callbacks
  and bounded waits rather than stopping after dispatch.

Context compaction is a recovery signal, not an automatic retirement counter.
Creation and replacement require an explicit user request. A prepared handoff
can be useful without creating a new task. Operational ownership transfer is
verified separately from pin order, naming and browser-tab presentation.

Role preferences live in the skill's single preferences table. Explicit user
choices and current task-tool constraints take precedence. Existing task settings
are preserved; missing optional model metadata is reported as unknown. Worker
names support the bundled Red Chamber pool, existing custom pools and descriptive
titles. No fixed worker quota is required.

## Install

```sh
git clone https://github.com/C14-design/c14-codex-task-manager.git
cp -R c14-codex-task-manager/skills/c14-codex-task-manager ~/.codex/skills/
```

Refresh Codex skill discovery before a fresh invocation.

## Use

Invoke `$c14-codex-task-manager` with the intended outcome, for example:

- Review the pinned project workers and recommend ownership. Do not dispatch.
- Send this decided brief to the named worker; I will follow up there.
- Organize the existing workers to finish these fixes and accept their results.

The skill uses persistent task tools, not ephemeral agents posing as pinned tasks.
It does not authorize commits, pushes, deployments, merchant-data changes or
preview reconciliation. Retired tasks are renamed and unpinned, not archived
without an explicit request.

## Files

- `skills/c14-codex-task-manager/SKILL.md`: modes, authority and acceptance.
- `references/lifecycle.md` inside the skill: naming and authorized succession.
- `references/default-name-pool.json` inside the skill: multilingual name pool.
- `agents/openai.yaml` inside the skill: discovery UI metadata.

MIT. Community project, not affiliated with or endorsed by OpenAI.
