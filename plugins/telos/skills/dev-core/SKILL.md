---
name: dev-core
description: "Ciclo de vida de proyecto en fases: init, resume, plan, exec, check, sync. Integra Ingeniería del Propósito en plan, exec y check, y delega Git a git-core."
user-invocable: false
metadata:
  version: "1.0.0"
---

# Dev Flow

## Purpose

Flujo compacto y explícito para desarrollo en fases: **init**, **resume**, **plan**, **exec**, **check** y **sync**.

## When to use / Activation hints

Usa esta skill cuando el usuario pida:

- iniciar un proyecto desde cero
- retomar un repositorio existente
- planificar una feature (diseño, requisitos, historias, roadmap)
- ejecutar un plan aprobado
- validar con tests/lint/build y cerrar con commit+PR
- sincronizar cambios remotos en la rama local

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
- Commits y PR creados según Git Flow

## Global rules

- Para operaciones Git, aplica **git-core**.
- `check` es **fail-hard**: si tests/lint/build fallan, no hay commit ni PR.
- No commits directos a `develop`/`main`/`master`.
- `rebase` solo en ramas locales privadas.
- Si hay dudas de branch destino, **pregunta**.

## Integración con Ingeniería del Propósito

Este flujo incorpora la skill **purpose-core** en tres fases:

- **plan**: antes de crear documentación de proyecto, se formula la ficha de propósito con los cuatro horizontes (funcional, arquitectónico, restricción, autoría).
- **exec**: las decisiones de implementación se guían por la ficha de propósito. Ante alternativas, prevalece la que mejor cumple los horizontes.
- **check**: además de tests/lint/build, se revisa la solución contra el propósito declarado antes del commit.

Se usan los comandos `/telos:brief`, `/telos:review` y `/telos:retro` cuando estén disponibles. Si no, se aplican los mismos criterios manualmente.

---

# Phase: init

## Goal

Iniciar un proyecto nuevo con base funcional mínima.

## Steps

1. Confirma stack, nombre del proyecto y repo remoto (si aplica).
2. Inicializa el repo (si no existe) y estructura mínima.
3. Instala dependencias y crea el bootstrap mínimo.
4. Ejecuta un smoke test o instrucción de arranque.
5. Documenta cómo ejecutar localmente.

## If missing info

Pregunta por stack, nombre, repositorio remoto, branch principal y si se usa Git Flow.

---

# Phase: resume

## Goal

Cargar el estado real del repo antes de planificar o ejecutar.

## Steps

1. Inspecciona ramas y remotos.
2. Detecta cambios locales sin commit.
3. Identifica base branch correcta según tipo de trabajo.
4. Localiza entrypoints relevantes y dependencias clave.
5. Si hace falta, propone `sync`.

## If missing info

Pregunta por tipo de trabajo, ID de Jira, branch base y objetivo actual.

---

# Phase: plan

## Goal

Definir el propósito del cambio y crear diseño, requisitos, tareas y roadmap.

## Steps

1. **Formula la ficha de propósito.** Antes de documentar, define:
   - **Funcional**: qué problema desaparece o qué capacidad nueva aparece.
   - **Arquitectónico**: qué principio de diseño debe protegerse.
   - **Restricción**: qué no puede ocurrir bajo ninguna circunstancia.
   - **Autoría**: cómo debe leerse, mantenerse o evolucionar el resultado.
   Usa `/telos:brief` si está disponible.
2. Clarifica alcance y restricciones a partir de la ficha. Los cuatro horizontes actúan como criterios de aceptación: si la solución los cumple, se acepta; si viola alguno, se rechaza.
3. Redacta `docs/DESIGN.md` — el diseño debe responder a los horizontes declarados.
4. Redacta `docs/REQUIREMENTS.md`.
5. Redacta `docs/TASKS.md`.
6. Redacta/actualiza `docs/ROADMAP.md`.
7. Pide confirmación del plan antes de ejecutar.

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
2. **Antes de implementar, verifica que la ficha de propósito está clara para esta tarea.** Si la tarea es compleja y no tiene propósito explícito, formula uno breve (funcional + restricción como mínimo).
3. Implementa cambios pequeños y revisables.
4. **Auto-revisión contra propósito**: comprueba que la implementación respeta los cuatro horizontes antes de avanzar. Si cumple la función pero rompe arquitectura o restricción, corrige antes de continuar.
5. Actualiza `docs/TASKS.md` y `docs/ROADMAP.md` si cambia el estado.

## If missing info

Pregunta qué tarea atacar primero o solicita confirmación del orden.

---

# Phase: check

## Goal

Validar calidad contra propósito y estándares técnicos, y cerrar con commit + PR.

## Steps

1. **Revisión contra propósito.** Evalúa la solución horizonte por horizonte:
   - ¿Cumple el fin funcional?
   - ¿Protege el principio arquitectónico?
   - ¿Respeta las restricciones?
   - ¿Se lee y mantiene como se esperaba?
   Usa `/telos:review` si está disponible.
   Si algún horizonte no se cumple: **detén**, informa y pide decisión antes de continuar.
2. Ejecuta tests/lint/build relevantes.
3. Si falla algo: **detén**, informa y pide decisión.
4. Realiza code review de los cambios.
5. Aplica fixes necesarios.
6. Crea commit(s) semánticos con ID de Jira.
7. Crea PR al branch correcto:
   - feature/bugfix -> `develop`
   - release/hotfix -> `main`/`master`
8. Opcionalmente, captura aprendizaje con `/telos:retro` si el cambio fue significativo.

## If missing info

Pregunta por tipo de trabajo, branch destino y convención de commits.

---

# Phase: sync

## Goal

Traer cambios remotos e integrar localmente.

## Steps

1. `git fetch`.
2. Verifica estado local limpio.
3. Aplica modo:
   - **merge** (default) para ramas compartidas.
   - **rebase** solo en ramas locales privadas.
   - **ff-only** si no hay cambios locales.
4. Resuelve conflictos y valida estado.

## If missing info

Pregunta por el modo deseado (merge/rebase/ff-only).

---

# Safety / Constraints

- No introducir secretos o credenciales.
- No reescribir historial en ramas compartidas.
- No commits directos a ramas protegidas.
- Fail-hard en `check` si tests/lint/build fallan.
