---
name: telos-dev-core
description: "Ciclo de vida de proyecto del plugin telos en 3 fases: plan, exec, check. Integra Ingeniería del Propósito (telos-purpose-core) en las tres fases y delega operaciones Git (init de repo, resume de estado, sync, branching, PRs) a telos-git-core. Orquestador de los comandos `/telos:plan`, `/telos:exec`, `/telos:check`."
user-invocable: false
metadata:
  version: "2.0.0"
---

# Dev Flow

## Purpose

Flujo compacto y explícito para desarrollo: **plan → exec → check**, gobernado por la ficha de propósito.

## When to use / Activation hints

Usa esta skill cuando el usuario pida:

- planificar una feature (diseño, requisitos, tareas, roadmap)
- ejecutar un plan aprobado
- validar con tests/lint/build y cerrar con commit+PR

Para operaciones Git (inicializar un repo, retomar uno existente, sincronizar con remoto, branching, PRs, releases) delega a `telos-git-core`. Para la ficha atómica de propósito, usa `/telos:brief`.

## Inputs (ask if missing)

Si falta información necesaria en cualquier fase, **pregunta al usuario antes de actuar**.

Inputs comunes:

- objetivo o alcance
- tipo de trabajo: feature | bugfix | release | hotfix
- ID de Jira (si aplica)
- branch principal: main | master
- branch de integración: develop
- hosting: Bitbucket (por defecto) o GitHub (excepción)

## Outputs

- Planificación en archivos:
  - `docs/DESIGN.md`
  - `docs/REQUIREMENTS.md`
  - `docs/TASKS.md`
  - `docs/ROADMAP.md`
- Cambios de código implementados y validados
- Commits y PR creados según Git Flow (delega a `telos-git-core`)

## Global rules

- Para operaciones Git, aplica **telos-git-core**.
- `check` es **fail-hard**: si tests/lint/build fallan, no hay commit ni PR.
- No commits directos a `develop`/`main`/`master`.
- `rebase` solo en ramas locales privadas.
- Si hay dudas de branch destino, **pregunta**.

## Integración con Ingeniería del Propósito

Este flujo incorpora la skill **telos-purpose-core** en las tres fases:

- **plan**: antes de crear documentación de proyecto, se formula la ficha de propósito con los cuatro horizontes (funcional, arquitectónico, restricción, autoría). Se invoca `/telos:brief` para producirla.
- **exec**: las decisiones de implementación se guían por la ficha. Ante alternativas, prevalece la que mejor cumple los horizontes.
- **check**: además de tests/lint/build, se revisa la solución contra el propósito declarado antes del commit, y se ofrece capturar una lección de propósito al cierre.

---

# Phase: plan

## Goal

Definir el propósito del cambio y crear diseño, requisitos, tareas y roadmap.

## Steps

1. **Formula la ficha de propósito.** Antes de documentar, define los cuatro horizontes (funcional, arquitectónico, restricción, autoría). Invoca `/telos:brief`.
2. Los cuatro horizontes actúan como criterios de aceptación: si la solución los cumple, se acepta; si viola alguno, se rechaza.
3. Clarifica alcance y restricciones a partir de la ficha.
4. Redacta `docs/DESIGN.md` — el diseño debe responder a los horizontes declarados.
5. Redacta `docs/REQUIREMENTS.md` — las fichas de propósito sustituyen a user stories + criterios de aceptación.
6. Redacta `docs/TASKS.md`.
7. Redacta/actualiza `docs/ROADMAP.md`.
8. Pide confirmación del plan antes de ejecutar.

## Document templates

`docs/DESIGN.md`:

- Overview
- Architecture (diagramas si aplica)
- Data flow
- APIs / Interfaces
- Decisions (trade-offs)
- Risks / Non-goals

`docs/REQUIREMENTS.md`:

- Goal
- Scope (in/out)
- Fichas de propósito (una por cambio o capacidad relevante, con los cuatro horizontes: funcional, arquitectónico, restricción y autoría — actúan como criterios de aceptación)
- Functional requirements (nivel de proyecto, alimentados por los horizontes funcionales de las fichas)
- Non-functional requirements (nivel de proyecto, alimentados por los horizontes de restricción y arquitectónico)
- Constraints / Dependencies

`docs/TASKS.md`:

- Task list con estados (todo | doing | done)
- Dependencias
- Estimaciones ligeras si aporta valor

`docs/ROADMAP.md`:

- Fases (Phase 1, Phase 2, ...)
- Tareas por fase con estado (todo | doing | done)
- Lista corta de tareas en curso

## If missing info

Pregunta por alcance, restricciones, prioridades y criterios de aceptación.

---

# Phase: exec

## Goal

Implementar el plan de forma incremental, trazable y alineada con el propósito declarado.

## Steps

1. Selecciona la tarea prioritaria.
2. **Antes de implementar, verifica que la ficha de propósito está clara para esta tarea.** Si la tarea es compleja y no tiene propósito explícito, formula uno breve (funcional + restricción como mínimo) con `/telos:brief`.
3. Implementa cambios pequeños y revisables.
4. **Auto-revisión contra propósito**: comprueba que la implementación respeta los cuatro horizontes antes de avanzar. Si cumple la función pero rompe arquitectura o restricción, corrige antes de continuar.
5. Actualiza `docs/TASKS.md` y `docs/ROADMAP.md` si cambia el estado.

## If missing info

Pregunta qué tarea atacar primero o solicita confirmación del orden.

---

# Phase: check

## Goal

Validar calidad contra propósito y estándares técnicos, cerrar con commit + PR, y capturar lección si procede. Ver la skill `telos-check` para el detalle operativo.

## Steps (resumen)

1. **Revisión contra propósito** horizonte por horizonte (plantilla en `telos-purpose-core/assets/purpose-review-template.md`).
2. Tests/lint/build (fail-hard).
3. Code review y fixes.
4. Commit semántico con ID Jira (delega a `telos-git-core`).
5. PR al branch correcto (delega a `telos-git-core`).
6. **Lección de propósito opcional** (plantilla en `telos-purpose-core/assets/purpose-retro-template.md`).

---

# Safety / Constraints

- No introducir secretos o credenciales.
- No reescribir historial en ramas compartidas.
- No commits directos a ramas protegidas.
- Fail-hard en `check` si tests/lint/build fallan.
