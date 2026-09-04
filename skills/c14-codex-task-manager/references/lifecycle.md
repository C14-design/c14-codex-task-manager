# Naming and authorized succession

Read only when a naming or lifecycle change is actually requested.

## Preferences

Read an existing project `.codex/c14-codex-task-manager.json` override, then the
existing global `~/.codex/c14-codex-task-manager.json`. Do not create either file
merely by invoking this skill. Reuse a convention already stated by the user.
When none exists, use descriptive names for the current authorized operation;
ask about a custom naming scheme only when it materially matters. Save new
preferences only if the user requests persistence and specifies its scope.

Supported existing keys are `naming_mode`, `name_language`,
`resolved_name_language`, and `name_pool`. Preserve their existing schema.
Modes: `red_chamber_women`, `custom_pool`, `descriptive`; read legacy
`default_actresses` as `red_chamber_women`. Languages: `auto`, `zh`, `ja`, `en`.
For auto, use substantive user messages, not tool output. Do not rename existing
tasks because an incidental message changes language.

The bundled [default-name-pool.json](default-name-pool.json) contains aliases and
canonical IDs. Reserve `wang-xifeng` for the manager. Select a currently available
worker identity from the configured pool, checking every alias. Preserve custom
spelling. A title beginning with a name plus a suffix also reserves it; a retired
`退役｜...｜原...` title does not. Refresh relevant names before mutation. If a
collision cannot be resolved, defer that name change without blocking unrelated
work. Do not retire tasks just to obtain a name.

## Creation and transfer

1. Confirm explicit creation/replacement authority. A context warning alone is
   insufficient. Read the entrypoint role preferences and current tool contract.
2. Resolve a saved project with `list_projects`; use the tool's environment
   default unless the user specified the authoritative existing checkout. Never
   invent a branch or assume a ready thread ID from a pending client ID.
3. Prepare a concise current handoff. Create without forking history unless
   requested. Use a temporary successor title while the original name is owned.
4. Send the precise assignment and callback requirement. Wait for acknowledgement
   in bounded calls. Do not transfer overlapping writes before acknowledgement.
5. Verify the successor understands source, owned files and pending acceptance;
   transfer ownership explicitly and notify affected workers once.
6. Retire the previous owner only after safe transfer: rename as
   `退役｜<主模块>·<主要贡献>｜原<名字>` and unpin; never archive without request.
7. Finalize the configured successor title and pool order. Use the explicit reorder
   API where available; preserve every unrelated task's relative order. Refresh
   the complete pinned list required by that API, not all historical tasks.
8. If the user asked to deliver a browser tab to the successor, use `open_in_codex`
   with its thread ID. Otherwise include the relevant URL in the handoff. Do not
   restart the preview. Queued delivery is not visual confirmation.

Report operational transfer and presentation separately. If acknowledgement or
ownership transfer fails, retain the previous owner and pause dependent writes.
If only title, pin order or tab delivery fails, retain a functioning pinned owner,
report the presentation gap, and continue safe work. Never claim an unverified
step succeeded.
