# Project Copilot — Tareas de implementación

Este archivo es la cola oficial de implementación elaborada por ChatGPT y ejecutada por Codex bajo las reglas definidas en `AGENTS.md`.

## Estados permitidos

- `Borrador`: la tarea todavía no debe ejecutarse.
- `Lista`: Codex debe ejecutar la tarea cuando el usuario indique `ejecuta tareas`.
- `Completada`: tarea implementada, validada, confirmada mediante commit y enviada a GitHub mediante push.
- `Bloqueada`: la tarea no puede completarse de forma segura y requiere revisión de ChatGPT o del usuario.

## Tareas activas

Actualmente no existen tareas con estado `Lista`.

La siguiente tarea de implementación será agregada por ChatGPT después de revisar el repositorio y el alcance solicitado por el usuario.

## Plantilla de tarea

```text
### PC-XXXX — <Título>

Estado: Lista

Objetivo:
<objetivo exacto>

Alcance:
- ...

Fuera de alcance:
- ...

Archivos esperados:
- ...

Requerimientos de implementación:
1. ...

Validación:
- dotnet build
- dotnet test

Criterios de aceptación:
- ...

Notas de ejecución:
- Completadas por Codex después de una ejecución satisfactoria.
```
