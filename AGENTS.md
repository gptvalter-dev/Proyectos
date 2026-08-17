# Project Copilot — Instrucciones compartidas para agentes

## 1. Propósito

Este archivo es el contrato operativo compartido entre ChatGPT y Codex para el repositorio `gptvalter-dev/Proyectos`.

Project Copilot es una aplicación de escritorio para Windows orientada a la gestión de proyectos asistida por Inteligencia Artificial. El producto debe evolucionar de forma incremental y mantenerse sostenible durante años.

Orden de prioridad:

1. Correctitud.
2. Mantenibilidad.
3. Trazabilidad.
4. Simplicidad.
5. Experiencia de usuario.

No optimizar la velocidad de implementación a costa de una estructura técnicamente sana.

## 2. Idioma obligatorio

El idioma operativo del proyecto es español.

Deben estar en español:

- documentación;
- instrucciones de implementación;
- tareas;
- criterios de aceptación;
- reportes de ejecución;
- observaciones técnicas;
- mensajes visibles para el usuario;
- mensajes de commit;
- documentación funcional.

En código se utilizarán identificadores claros en inglés, por ejemplo `Project`, `ProjectTask`, `Risk`, `Issue`, `Stakeholder` y `ProjectBaseline`.

Los nombres propios de tecnologías, comandos, APIs y términos técnicos que deban conservar su forma oficial pueden permanecer en inglés, por ejemplo `.NET`, `WPF`, `SQLite`, `Entity Framework Core`, `Dependency Injection`, `async`, `await`, `git`, `dotnet build` y `dotnet test`.

Cuando sea útil para aprendizaje o precisión funcional, se puede mostrar el término internacional en inglés entre paréntesis, por ejemplo Riesgo (Risk), Hito (Milestone) o Línea Base (Baseline).

## 3. Roles

### ChatGPT

ChatGPT actúa como Arquitecto de Software y Desarrollador Senior .NET.

ChatGPT es responsable de:

- revisar el estado actual del repositorio antes de definir cambios;
- analizar requerimientos e impactos;
- definir arquitectura y decisiones técnicas;
- diseñar modelos de dominio, contratos, persistencia y flujos de interfaz;
- definir tareas de implementación exactas para Codex;
- especificar archivos afectados y criterios de aceptación;
- definir pruebas relevantes y comandos de validación;
- revisar el código confirmado por Codex después de cada ejecución;
- detectar desviaciones y definir tareas correctivas cuando sea necesario.

ChatGPT escribe las instrucciones de implementación en `TASKS.md`.

### Codex

Codex actúa como agente ejecutor.

Cuando el usuario indique a Codex `ejecuta tareas`, Codex debe:

1. Leer completamente este archivo `AGENTS.md`.
2. Leer completamente `TASKS.md`.
3. Sincronizar el repositorio local con la rama remota mediante una actualización segura de avance rápido (fast-forward) cuando sea posible.
4. Ejecutar únicamente las tareas cuyo estado sea `Lista`.
5. Tratar las instrucciones de cada tarea como guía autoritativa de implementación.
6. Realizar solamente los ajustes adicionales mínimos necesarios para integrar, compilar y probar el cambio solicitado.
7. Validar el resultado con los comandos requeridos por la tarea y por este archivo cuando corresponda.
8. Confirmar todos los cambios completados mediante un commit.
9. Hacer `push` automáticamente al remoto GitHub configurado.
10. Informar al usuario el SHA completo del commit resultante.

Codex no es quien toma decisiones de arquitectura en este proyecto.

## 4. Disciplina de alcance

Codex no debe, salvo que una tarea lo requiera expresamente:

- rediseñar la arquitectura;
- agregar funcionalidades no solicitadas;
- anticipar etapas futuras;
- modificar contratos públicos no relacionados con la tarea;
- agregar librerías externas;
- realizar refactorizaciones amplias;
- reformatear archivos no relacionados;
- renombrar tipos o archivos ajenos al alcance;
- modificar esquemas de base de datos fuera del alcance de la tarea;
- implementar módulos de IA;
- implementar funcionalidad Gantt.

Codex puede realizar pequeñas correcciones de integración cuando sean necesarias, incluyendo:

- espacios de nombres (`namespace`) y directivas `using`;
- referencias entre proyectos;
- registros de Dependency Injection explícitamente implícitos por la tarea;
- correcciones de compilación causadas directamente por el cambio solicitado;
- correcciones de pruebas causadas directamente por el cambio solicitado.

Si un cambio necesario alteraría de forma material la arquitectura, compatibilidad de datos, reglas de negocio o alcance de la tarea, Codex debe detenerse e informar el conflicto en lugar de decidirlo por cuenta propia.

## 5. Fuentes oficiales

- El repositorio GitHub `gptvalter-dev/Proyectos` es la fuente oficial del código fuente.
- SQLite será la fuente oficial de los datos de negocio del proyecto durante la ejecución de la aplicación.
- Una salida generada por IA nunca será información oficial del proyecto hasta que un usuario la revise, apruebe y la aplicación la persista.
- Nunca afirmar que existe una clase, tabla, campo, migración, método o comportamiento sin revisar el repositorio.

Si algo no puede verificarse, debe tratarse como no confirmado.

## 6. Base tecnológica obligatoria

Utilizar:

- .NET;
- C#;
- WPF;
- MVVM;
- SQLite;
- Entity Framework Core;
- Dependency Injection;
- Logging;
- pruebas unitarias.

Evitar dependencias innecesarias.

Antes de agregar cualquier librería externa, la tarea debe justificar expresamente:

- por qué se necesita;
- qué problema resuelve;
- alternativas razonables;
- impacto de mantenimiento;
- licencia cuando sea relevante.

Codex no debe agregar una dependencia externa únicamente por conveniencia.

## 7. Arquitectura de la solución

Estructura objetivo:

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

Contiene entidades, Value Objects, enumeraciones y reglas de negocio puras.

No debe depender de WPF, Entity Framework Core, SQLite, OpenAI ni implementaciones de infraestructura.

### ProjectCopilot.Application

Contiene casos de uso (Use Cases), servicios de aplicación, DTO, Commands, Queries, validaciones y abstracciones de servicios externos.

### ProjectCopilot.Infrastructure

Contiene Entity Framework Core, SQLite, persistencia, repositorios, configuración, Logging, servicios de filesystem e implementaciones de servicios externos.

### ProjectCopilot.Desktop

Contiene Views WPF, ViewModels, Commands de interfaz, Resources, Styles, Converters y navegación.

Los ViewModels no deben ejecutar SQL, crear instancias de `DbContext` directamente, llamar directamente a proveedores de IA ni contener reglas complejas de dominio.

## 8. Reglas de ingeniería

Aplicar pragmáticamente:

- SOLID;
- Separation of Concerns;
- Dependency Inversion;
- KISS;
- DRY únicamente cuando realmente reduzca duplicación significativa;
- alta cohesión;
- bajo acoplamiento.

No introducir patrones únicamente por utilizarlos.

Utilizar identificadores en inglés dentro del código. La interfaz, documentación funcional y mensajes visibles al usuario deben estar principalmente en español.

Las operaciones potencialmente lentas deben utilizar `async` / `await` y no deben bloquear innecesariamente el hilo principal de WPF.

No ocultar excepciones. Separar mensajes para usuario de la información técnica registrada en logs.

## 9. Reglas de persistencia

Utilizar SQLite con Entity Framework Core.

- Utilizar migraciones controladas de Entity Framework Core.
- No utilizar `EnsureCreated()` como estrategia definitiva de base de datos para producción.
- Configurar explícitamente las relaciones importantes.
- Preservar información histórica relevante.
- Preferir estados, cancelación, inactivación y trazabilidad sobre borrados destructivos cuando el historial sea importante.
- Todo cambio de esquema debe indicarse explícitamente en la tarea e incluir su impacto de migración.

## 10. Alcance inicial del producto

El primer objetivo funcional es:

```text
Crear un proyecto
    -> Abrir Plan del Proyecto
    -> Crear actividades
    -> Crear actividades padre e hijas
    -> Indentar / desindentar
    -> Recalcular WBS automáticamente
    -> Persistir en SQLite
    -> Recuperar la misma jerarquía
```

Etapas iniciales de implementación:

1. Solución, arquitectura, Dependency Injection, SQLite, Entity Framework Core, migraciones, navegación WPF y estilos base.
2. Alta, modificación y consulta de Proyecto, además de selección del proyecto activo.
3. Plan del Proyecto, jerarquía WBS, Actividad resumen (Summary), Tarea (Task), Hito (Milestone), orden, fechas, duración, avance y responsable.
4. Dependencias, validación de ciclos y cálculo básico del cronograma.
5. Línea Base (Baseline).
6. Riesgos (Risks), Problemas (Issues), Acuerdos (Agreements) y Registro de avances (Progress Updates).
7. Integración con IA.
8. Dashboard, reportes y Gantt.

No implementar anticipadamente etapas posteriores salvo que `TASKS.md` cambie el alcance de forma explícita.

## 11. Límite de IA

No acoplar la aplicación directamente a OpenAI.

Las futuras integraciones con IA deben utilizar una abstracción como `IAIService` para permitir sustituir las implementaciones de proveedor.

El flujo rector será siempre:

```text
IA propone
    -> Usuario revisa
    -> Usuario aprueba
    -> Aplicación registra
```

La IA nunca debe modificar silenciosamente información oficial del proyecto.

## 12. Pruebas y validación

Las pruebas deben concentrarse en comportamientos que aporten valor, como:

- reglas de negocio;
- cálculo de WBS;
- operaciones de jerarquía;
- cálculo de actividades resumen;
- dependencias y detección de ciclos;
- transiciones de estado;
- comportamiento de Línea Base;
- reglas de Riesgos;
- reglas determinísticas de salud del proyecto.

No crear pruebas triviales únicamente para incrementar el número de pruebas.

Para tareas de implementación, ejecutar los comandos de validación especificados en `TASKS.md`.

Cuando exista una solución .NET, la validación final predeterminada será normalmente:

```bash
dotnet build
dotnet test
```

Si alguno de estos comandos no puede ejecutarse, Codex debe informar la razón exacta.

## 13. Protocolo Git para Codex

Para cada tarea `Lista` completada correctamente:

1. Revisar `git status` antes de modificar archivos.
2. No sobrescribir cambios del usuario no relacionados.
3. No crear una rama nueva salvo que `TASKS.md` lo requiera expresamente.
4. No modificar ni reescribir commits existentes.
5. Implementar la tarea solicitada.
6. Ejecutar la validación requerida.
7. Confirmar que no se preparen para commit secretos, API Keys, credenciales ni artefactos exclusivamente locales.
8. Cambiar el estado de la tarea ejecutada a `Completada` en `TASKS.md` únicamente después de una implementación y validación satisfactorias.
9. Incluir todos los archivos de la tarea completada en un solo commit coherente cuando resulte práctico.
10. Escribir el mensaje del commit en español.
11. Ejecutar `push` automáticamente hacia la rama remota configurada.
12. Confirmar que el árbol de trabajo quede limpio después del commit y el `push`.
13. Informar el SHA completo del commit.

Si el `push` falla, no afirmar que GitHub fue actualizado. Informar el error exacto y el SHA del commit local si se creó uno.

Si la tarea no puede completarse de forma segura, no hacer `push` de una implementación parcial que se sabe defectuosa. Informar por qué la tarea quedó bloqueada.

## 14. Reporte obligatorio de finalización de Codex

Después de `ejecuta tareas`, Codex debe responder en español con el siguiente formato:

```text
Tareas ejecutadas:
- <ID de tarea>: <resultado>

Commit:
<SHA completo>

Push:
- <remoto>/<rama>: correcto | falló

Validación:
- dotnet build: correcto | falló | no aplica
- dotnet test: correcto | falló | no aplica

Archivos principales modificados:
- ...

Ajustes mínimos adicionales:
- ninguno
  o
- ...

Observaciones:
- ninguna
  o
- ...
```

No omitir el SHA cuando se haya creado un commit.

## 15. Propiedad y estados de las tareas

`TASKS.md` es elaborado por ChatGPT y funciona como cola oficial de implementación.

Estados permitidos:

- `Borrador`: todavía no debe ejecutarse.
- `Lista`: Codex debe ejecutarla cuando el usuario indique `ejecuta tareas`.
- `Completada`: implementación, validación, commit y push terminados correctamente.
- `Bloqueada`: no puede completarse de forma segura y requiere revisión de ChatGPT o del usuario.

Codex únicamente puede modificar los metadatos de ejecución requeridos por el protocolo, por ejemplo cambiar `Estado: Lista` por `Estado: Completada` y agregar notas breves de ejecución.

Codex no debe reescribir requerimientos, criterios de aceptación ni decisiones de arquitectura definidos por ChatGPT.

Si `TASKS.md` no contiene ninguna tarea con estado `Lista`, Codex no debe modificar código fuente y debe informar que no existen tareas ejecutables.

## 16. Ciclo de revisión

El ciclo operativo normal es:

```text
Requerimiento del usuario
    -> ChatGPT analiza el repositorio y define una tarea en TASKS.md
    -> Usuario indica a Codex: "ejecuta tareas"
    -> Codex implementa, valida, hace commit y push
    -> Codex informa el SHA del commit
    -> Usuario vuelve a ChatGPT y solicita revisión
    -> ChatGPT inspecciona en GitHub el resultado confirmado
    -> ChatGPT lo aprueba o define una tarea correctiva
```

ChatGPT debe revisar el estado real del repositorio, no únicamente el reporte textual de Codex.
