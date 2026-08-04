---
name: context-check
description: Compatibility entry for assessing Codex task context health and deciding whether to continue or hand off. Use when the user invokes $context-check or /context-check, asks whether a task or context is too long, or asks when the current manager should create a fresh successor.
---

# Context Check Compatibility Entry

Read `../c14-codex-task-manager/SKILL.md` completely, then execute its context-health and manager-succession workflow.

Keep a plain `$context-check` invocation read-only. Create or rename a successor only when the user requests active task management, invokes `$c14-codex-task-manager`, or explicitly asks to continue in a fresh task.
