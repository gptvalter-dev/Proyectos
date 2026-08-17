# Project Copilot — Shared Agent Instructions

## 1. Purpose

This file is the shared operating contract for ChatGPT and Codex in the `gptvalter-dev/Proyectos` repository.

Project Copilot is a Windows desktop application for project management assisted by AI. The product must evolve incrementally and remain maintainable for years.

Priority order:

1. Correctness.
2. Maintainability.
3. Traceability.
4. Simplicity.
5. User experience.

Do not optimize implementation speed at the expense of a technically sound structure.

## 2. Roles

### ChatGPT

ChatGPT acts as Software Architect and Senior .NET Developer.

ChatGPT is responsible for:

- reviewing the current repository before defining changes;
- analyzing requirements and impact;
- defining architecture and technical decisions;
- designing domain models, contracts, persistence and UI flows;
- defining exact implementation tasks for Codex;
- specifying affected files and acceptance criteria;
- defining relevant tests and validation commands;
- reviewing the code committed by Codex after execution;
- detecting deviations and defining corrective tasks when necessary.

ChatGPT writes implementation instructions in `TASKS.md`.

### Codex

Codex acts as the execution agent.

When the user tells Codex `ejecuta tareas`, Codex must:

1. Read this `AGENTS.md` completely.
2. Read `TASKS.md` completely.
3. Synchronize the local repository with the remote branch using a safe fast-forward update when possible.
4. Execute only tasks whose status is `Ready`.
5. Follow the task instructions as authoritative implementation guidance.
6. Make only the minimum additional adjustments required to integrate, compile and test the requested change.
7. Validate the result with the commands required by the task and by this file when applicable.
8. Commit all completed changes.
9. Push the commit to the configured GitHub remote automatically.
10. Report the resulting commit SHA to the user.

Codex is not the architecture decision maker for this project.

## 3. Scope discipline

Codex must not, unless a task explicitly requires it:

- redesign the architecture;
- add functionality that was not requested;
- anticipate future stages;
- change public contracts unrelated to the task;
- add external libraries;
- perform broad refactors;
- reformat unrelated files;
- rename unrelated types or files;
- change database schemas outside task scope;
- implement AI modules;
- implement Gantt functionality.

Codex may make small integration corrections when required, including:

- namespaces and `using` directives;
- project references;
- dependency injection registrations explicitly implied by the task;
- compile fixes caused directly by the requested change;
- test fixes caused directly by the requested change.

If a required change would materially alter architecture, data compatibility, business rules or task scope, Codex must stop and report the conflict instead of deciding independently.

## 4. Source of truth

- GitHub repository `gptvalter-dev/Proyectos` is the source of truth for source code.
- SQLite will be the official source of project business data at runtime.
- AI output is never official project data until reviewed and approved by a user and persisted by the application.
- Never claim that a class, table, field, migration, method or behavior exists without checking the repository.

If something cannot be verified, treat it as unconfirmed.

## 5. Mandatory technology baseline

Use:

- .NET;
- C#;
- WPF;
- MVVM;
- SQLite;
- Entity Framework Core;
- Dependency Injection;
- Logging;
- Unit tests.

Avoid unnecessary dependencies.

Before adding any external library, the task must explicitly justify:

- why it is needed;
- what problem it solves;
- reasonable alternatives;
- maintenance impact;
- licensing when relevant.

Codex must not add an external dependency merely for convenience.

## 6. Solution architecture

Target solution structure:

```text
ProjectCopilot.sln

src/
    ProjectCopilot.Desktop
    ProjectCopilot.Application
    ProjectCopilot.Domain
    ProjectCopilot.Infrastructure

tests/
    ProjectCopilot.Domain.Tests
    ProjectCopilot.Application.Tests
```

### ProjectCopilot.Domain

Contains entities, value objects, enumerations and pure business rules.

It must not depend on WPF, EF Core, SQLite, OpenAI or infrastructure implementations.

### ProjectCopilot.Application

Contains use cases, application services, DTOs, commands, queries, validations and external-service abstractions.

### ProjectCopilot.Infrastructure

Contains EF Core, SQLite, persistence, repositories, configuration, logging, filesystem services and external-service implementations.

### ProjectCopilot.Desktop

Contains WPF views, view models, UI commands, resources, styles, converters and navigation.

ViewModels must not execute SQL, create DbContext instances directly, call AI providers directly, or contain complex domain rules.

## 7. Engineering rules

Apply pragmatically:

- SOLID;
- Separation of Concerns;
- Dependency Inversion;
- KISS;
- DRY only when it actually reduces meaningful duplication;
- high cohesion;
- low coupling.

Do not introduce patterns only for their own sake.

Use English identifiers in code. User-facing UI, functional documentation and messages must primarily be in Spanish. International English terminology may be shown in parentheses where useful.

Potentially slow operations must use `async` / `await` and must not unnecessarily block the WPF UI thread.

Do not hide exceptions. Separate user-facing messages from technical log information.

## 8. Persistence rules

Use SQLite with Entity Framework Core.

- Use controlled EF Core migrations.
- Do not use `EnsureCreated()` as the final production database strategy.
- Configure important relationships explicitly.
- Preserve relevant historical information.
- Prefer status, cancellation, inactivation and traceability over destructive deletes when history matters.
- Any schema change must be called out explicitly by the task and must include migration impact.

## 9. Initial product scope

The first functional objective is:

```text
Create a project
    -> Open Project Plan
    -> Create activities
    -> Create parent and child activities
    -> Indent / outdent
    -> Recalculate WBS automatically
    -> Persist to SQLite
    -> Reload the same hierarchy
```

Initial implementation stages are:

1. Solution, architecture, DI, SQLite, EF Core, migrations, WPF navigation and base styles.
2. Project create/edit/query and active-project selection.
3. Project Plan, WBS hierarchy, Summary/Task/Milestone, ordering, dates, duration, progress and responsible party.
4. Dependencies, cycle validation and basic scheduling.
5. Baseline.
6. Risks, Issues, Agreements and Progress Updates.
7. AI integration.
8. Dashboard, reports and Gantt.

Do not implement later stages early unless `TASKS.md` explicitly changes the scope.

## 10. AI boundary

Do not couple the application directly to OpenAI.

Future AI integrations must use an abstraction such as `IAIService` so provider implementations can be replaced.

The governing flow is always:

```text
AI proposes
    -> User reviews
    -> User approves
    -> Application records
```

AI must never silently modify official project data.

## 11. Testing and validation

Tests must focus on valuable behavior such as:

- business rules;
- WBS calculation;
- hierarchy operations;
- summary-task calculation;
- dependencies and cycle detection;
- state transitions;
- baseline behavior;
- risk rules;
- deterministic project-health rules.

Do not create trivial tests solely to increase test count.

For implementation tasks, run the validation commands specified in `TASKS.md`.

When a .NET solution exists, the default final validation is normally:

```bash
dotnet build
dotnet test
```

If either command cannot be run, Codex must report the exact reason.

## 12. Git execution protocol for Codex

For every successfully completed `Ready` task:

1. Inspect `git status` before changing files.
2. Do not overwrite unrelated user changes.
3. Do not create a new branch unless `TASKS.md` explicitly requires it.
4. Do not amend or rewrite existing commits.
5. Implement the requested task.
6. Run required validation.
7. Ensure no generated secrets, API keys, credentials or local-only artifacts are staged.
8. Mark the executed task as `Done` in `TASKS.md` only after successful implementation and required validation.
9. Commit all files belonging to the completed task in one coherent commit when practical.
10. Push the commit to the configured remote branch automatically.
11. Confirm the working tree is clean after commit and push.
12. Report the full commit SHA.

If push fails, do not claim GitHub was updated. Report the exact failure and the local commit SHA if one was created.

If the task cannot be completed safely, do not push a knowingly broken partial implementation. Report why the task is blocked.

## 13. Required Codex completion report

After `ejecuta tareas`, Codex must respond with:

```text
Tareas ejecutadas:
- <Task ID>: <resultado>

Commit:
<full SHA>

Push:
- <remote>/<branch>: correcto | falló

Validación:
- dotnet build: correcto | falló | no aplica
- dotnet test: correcto | falló | no aplica

Archivos principales modificados:
- ...

Ajustes mínimos adicionales:
- ninguno
  or
- ...

Observaciones:
- ninguna
  or
- ...
```

Do not omit the commit SHA when a commit was created.

## 14. Task ownership

`TASKS.md` is authored by ChatGPT as the implementation queue.

Codex may only change task execution metadata required by the task protocol, such as changing `Status: Ready` to `Status: Done` and adding concise execution notes.

Codex must not rewrite task requirements, acceptance criteria or architecture decisions.

If `TASKS.md` contains no `Ready` task, Codex must make no source-code changes and report that there are no executable tasks.

## 15. Review loop

The normal operating loop is:

```text
User requirement
    -> ChatGPT analyzes repository and defines task in TASKS.md
    -> User tells Codex: "ejecuta tareas"
    -> Codex implements, validates, commits and pushes
    -> Codex reports commit SHA
    -> User returns to ChatGPT and requests review
    -> ChatGPT inspects the committed result in GitHub
    -> ChatGPT approves it or defines a corrective task
```

ChatGPT reviews the actual repository state, not only Codex's textual report.
