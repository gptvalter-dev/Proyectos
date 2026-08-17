# Project Copilot — Implementation Tasks

This file is the implementation queue authored by ChatGPT and executed by Codex under the rules in `AGENTS.md`.

## Status values

- `Ready`: Codex must execute the task when the user says `ejecuta tareas`.
- `Done`: task completed, validated, committed and pushed.
- `Blocked`: task cannot be completed safely and requires review by ChatGPT/user.
- `Draft`: not executable yet.

## Active tasks

There are currently no `Ready` tasks.

The next implementation task will be added by ChatGPT after reviewing the repository and the user's requested scope.

## Task template

```text
### PC-XXXX — <Title>

Status: Ready

Objective:
<exact objective>

Scope:
- ...

Out of scope:
- ...

Files expected:
- ...

Implementation requirements:
1. ...

Validation:
- dotnet build
- dotnet test

Acceptance criteria:
- ...

Execution notes:
- Filled by Codex after successful execution.
```
