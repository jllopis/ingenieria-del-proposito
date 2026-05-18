---
name: telos-dev-core
description: "Ciclo de vida de proyecto del plugin telos en 3 fases (plan → exec → check), adaptable a tres modos: dev-team (equipo + PRs), dev-solo (un dev + agente IA, sin PRs), documental (entregables no-código). Integra telos-purpose-core en las tres fases y delega Git a telos-git-core cuando aplica. Orquestador de `/telos:plan`, `/telos:exec`, `/telos:check`."
user-invocable: false
metadata:
  version: "2.1.0"
---

# Dev Flow

## Purpose

Flujo compacto y explícito para llevar cualquier proyecto (código o documental) desde idea hasta entrega: **plan → exec → check**, gobernado por la ficha de propósito.

## When to use / Activation hints

Usa esta skill cuando el usuario pida:

- planificar una feature, un cambio o un cuerpo documental
- ejecutar un plan aprobado
- validar y cerrar un cambio (commit + PR si aplica, o publicación documental)

Para operaciones Git puras delega a `telos-git-core` (solo aplica a modos `dev-team` y `dev-solo`). Para la ficha atómica de propósito, usa `/telos:brief`.

## Modos de proyecto

El modo se declara al inicio del proyecto en `docs/REQUIREMENTS.md` (sección "Modo del proyecto") y condiciona el comportamiento de `plan`, `exec` y `check`.

### `dev-team` (default si hay equipo + repo Git)

- Stack: lenguaje + framework + repo Git con Git Flow
- Cierre: commit semántico + PR a `develop` o `main`/`master`, revisado por humanos
- Validación: tests / lint / build
- Artefactos: `docs/DESIGN.md`, `docs/REQUIREMENTS.md`, `docs/TASKS.md`, `docs/ROADMAP.md`
- Aplican íntegras las reglas de `telos-git-core`

### `dev-solo` (un desarrollador + agente IA)

- Stack: lenguaje + framework + repo Git, generalmente sin branches compartidas
- Cierre: **commit semántico cuyo mensaje incluye la ficha y la revisión por horizontes**. No PR.
- Validación: tests / lint / build (igual que `dev-team`)
- Artefactos: los mismos que `dev-team`
- `telos-git-core` aplica solo para naming de ramas/commits/tags; el flujo de PRs/reviewers se omite

### `documental` (entregables no-código)

- Stack: ninguno técnico. Almacén canónico puede ser local, Google Drive, Confluence, SharePoint, etc.
- Cierre: documento(s) en estado "aprobado" y comunicado a stakeholders. Versión registrada (Drive history, naming convention, git tag si aplica).
- Validación: cumplimiento de plantilla, integridad de referencias cruzadas, aprobaciones requeridas presentes, ausencia de información restringida. **No hay tests/lint/build**.
- Artefactos: `docs/DESIGN.md` (estructura del cuerpo documental), `docs/REQUIREMENTS.md` (fichas por documento o conjunto), `docs/TASKS.md` (backlog de documentos/secciones a producir o revisar), `docs/ROADMAP.md` (fases de redacción → revisión → aprobación → publicación)
- `telos-git-core` es opcional. Si el almacén canónico no es Git, las recetas de init/resume/sync no aplican.

## Inputs (ask if missing)

Si falta información necesaria en cualquier fase, **pregunta al usuario antes de actuar**.

Inputs comunes (todos los modos):

- objetivo o alcance
- modo del proyecto (`dev-team` | `dev-solo` | `documental`)

Solo para modos `dev-*`:

- tipo de trabajo: feature | bugfix | release | hotfix
- ID de Jira (si aplica)
- branch principal: main | master
- branch de integración: develop (solo `dev-team`)
- hosting: Bitbucket (por defecto) o GitHub (excepción)

Solo para `documental`:

- almacén canónico (local | Drive | Confluence | etc.)
- workflow de aprobación (quién aprueba, en qué orden)
- normativa aplicable (ENS, ISO 27001, GDPR, etc. si es seguridad)

## Outputs

- Planificación en archivos (rutas adaptables al modo):
  - `docs/DESIGN.md`
  - `docs/REQUIREMENTS.md`
  - `docs/TASKS.md`
  - `docs/ROADMAP.md`
- En modos `dev-*`: cambios de código implementados y validados, commits y (si `dev-team`) PRs.
- En modo `documental`: documentos producidos, validados y publicados al almacén canónico.

### Split de REQUIREMENTS.md en proyectos grandes

Si el proyecto acumula más de ~5 fichas de propósito, parte el fichero:

- `docs/REQUIREMENTS.md` queda como **índice** (ficha-paraguas + tabla de capacidades con enlaces).
- Cada ficha de capacidad vive en `docs/requirements/<capacidad-kebab>.md` (en `documental`: `docs/requirements/<documento>.md`).

`plan` propone el split cuando detecta que el proyecto va a tener >5 fichas; el usuario decide.

## Global rules

- Para operaciones Git (modos `dev-*`), aplica **telos-git-core**.
- `check` es **fail-hard**: si tests/lint/build fallan (modos `dev-*`) o si la validación documental falla (modo `documental`), no hay commit/PR/publicación.
- En `dev-team`: no commits directos a `develop`/`main`/`master`.
- `rebase` solo en ramas locales privadas.
- Si hay dudas de branch destino o de aprobador, **pregunta**.

## Integración con Ingeniería del Propósito

Este flujo incorpora la skill **telos-purpose-core** en las tres fases:

- **plan**: antes de crear documentación de proyecto, se formula la ficha de propósito con los cuatro horizontes (funcional, arquitectónico, restricción, autoría). Se invoca `/telos:brief` para producirla. Los horizontes son universales: aplican igual a un módulo de código que a una política de seguridad.
- **exec**: las decisiones (de implementación o redacción) se guían por la ficha. Antes de cada tarea, exec **recita explícitamente** qué horizontes toca esta tarea, para que la auto-revisión final tenga referencia clara.
- **check**: además de la validación técnica (modos `dev-*`) o documental (modo `documental`), se revisa el deliverable contra el propósito declarado antes del cierre, y se ofrece capturar una lección de propósito que pueda **promover** un horizonte a la ficha de capacidad o a la ficha-paraguas.

---

# Phase: plan

## Goal

Definir el modo del proyecto, formular el propósito-paraguas y crear diseño, requisitos, tareas y roadmap adaptados al modo.

## Steps

1. **Confirma el modo del proyecto** si no está declarado. Pregunta: ¿dev-team, dev-solo o documental? Si es ambiguo, propone uno con justificación y pide confirmación.
2. **Formula la ficha de propósito-paraguas** invocando `/telos:brief`. Los 4 horizontes describen el proyecto completo (no una tarea concreta).
3. Los horizontes actúan como criterios de aceptación globales: las fichas de capacidad heredan/refinan estos horizontes.
4. **Descompón en fichas de capacidad** (una `/telos:brief` por capacidad relevante). Cada ficha hereda restricciones del paraguas y añade las suyas.
5. Decide si usar layout monolítico (`docs/REQUIREMENTS.md` con todas las fichas) o split (paraguas en `REQUIREMENTS.md` + una por capacidad en `docs/requirements/<cap>.md`). Propone split si hay >5 capacidades.
6. Redacta `docs/DESIGN.md`:
   - Modo `dev-*`: arquitectura, data flow, APIs, decisiones, riesgos, non-goals.
   - Modo `documental`: estructura del cuerpo documental, jerarquía, referencias cruzadas, normativa aplicable, ciclo de aprobación.
7. Redacta `docs/REQUIREMENTS.md` con la sección "**Modo del proyecto**" como primer apartado, seguido del Goal, Scope, las fichas y los requirements globales.
8. Redacta `docs/TASKS.md`:
   - Modo `dev-*`: tareas de implementación (todo | doing | done), dependencias.
   - Modo `documental`: documentos/secciones a redactar o revisar (todo | drafting | review | approved | published).
9. Redacta/actualiza `docs/ROADMAP.md` con fases:
   - Modo `dev-*`: Phase 1 (skeleton), Phase 2 (capacidades), Phase 3 (producción)…
   - Modo `documental`: Phase 1 (estructura + plantillas), Phase 2 (redacción), Phase 3 (revisión cruzada), Phase 4 (aprobación), Phase 5 (publicación + comunicación).
10. Pide confirmación del plan antes de pasar a exec.

## Document templates

`docs/REQUIREMENTS.md` (cabecera obligatoria):

```md
# Requirements

**Modo del proyecto:** dev-team | dev-solo | documental
**Almacén canónico:** [git remote URL | Drive folder | Confluence space]
**Normativa aplicable:** [si procede]

## Goal
[del propósito-paraguas]

## Scope
In:  …
Out: …

## Ficha de propósito-paraguas
[4 horizontes del proyecto completo]

## Fichas de capacidad
[lista o enlaces si hay split]
```

`docs/DESIGN.md` (modo `dev-*`):

- Overview
- Architecture (diagramas si aplica)
- Data flow
- APIs / Interfaces
- Decisions (trade-offs)
- Risks / Non-goals

`docs/DESIGN.md` (modo `documental`):

- Overview del cuerpo documental
- Jerarquía y dependencias entre documentos
- Plantillas aplicables
- Normativa de referencia
- Workflow de aprobación
- Riesgos / Non-goals

`docs/TASKS.md`:

- Lista con estados según modo
- Dependencias
- Estimaciones ligeras si aporta valor

`docs/ROADMAP.md`:

- Fases con tareas y estado
- Lista corta de tareas en curso

## If missing info

Pregunta por modo, alcance, restricciones, prioridades y criterios de aceptación.

---

# Phase: exec

## Goal

Implementar (modos `dev-*`) o redactar (modo `documental`) el plan de forma incremental, trazable y alineada con el propósito.

## Steps

1. Selecciona la tarea prioritaria.
2. **Localiza la ficha relevante** (capacidad o paraguas según alcance) en `REQUIREMENTS.md` o `docs/requirements/`.
3. **Recita explícitamente los horizontes que esta tarea toca y los que no.** Formato:
   ```
   Tarea: T2.3 — [descripción]
   Ficha relevante: Capacidad X
   Horizontes que toca: Arquitectónico ("…"), Restricción ("…")
   Horizontes que NO toca: Funcional, Autoría
   ```
   Este contrato declarado al inicio reemplaza la "auto-revisión vaga": al cerrar la tarea solo comparas contra los horizontes listados.
4. Si la tarea es compleja y no tiene ficha clara, formula una breve con `/telos:brief` (funcional + restricción como mínimo).
5. Ejecuta:
   - Modo `dev-*`: implementa cambios pequeños y revisables.
   - Modo `documental`: redacta/edita la sección o documento.
6. **Auto-revisión contra los horizontes recitados** antes de avanzar. Si la implementación/redacción cumple la función pero rompe alguno de los horizontes declarados, corrige.
7. Actualiza `docs/TASKS.md` y `docs/ROADMAP.md` si cambia el estado.

## If missing info

Pregunta qué tarea atacar primero o solicita confirmación del orden.

---

# Phase: check

## Goal

Validar contra propósito + estándares (técnicos o documentales), cerrar (commit/PR/publicación) y capturar lecciones que **refinen las fichas**. Ver la skill `telos-check` para el detalle operativo.

## Steps (resumen)

1. Localiza ficha (capacidad o paraguas) y modo del proyecto.
2. **Revisión por horizontes** (plantilla en `telos-purpose-core/assets/purpose-review-template.md`).
3. Validación específica del modo:
   - `dev-*`: tests/lint/build (fail-hard).
   - `documental`: plantilla aplicada, referencias íntegras, aprobaciones presentes, sin info restringida.
4. Code review / peer review (modo `dev-team`) o lectura por aprobadores (modo `documental`).
5. Aplica fixes.
6. **Cierre adaptado al modo:**
   - `dev-team`: commit semántico + PR (delega a `telos-git-core`). El cuerpo del PR incluye ficha + revisión (plantilla `pr-template.md`).
   - `dev-solo`: commit semántico con la ficha + revisión **dentro del mensaje** (plantilla `commit-message-template.md`). Sin PR.
   - `documental`: publica al almacén canónico, registra versión, notifica a stakeholders.
7. **Lección de propósito** con clasificación obligatoria (local / capacidad / paraguas) y producción de diff sobre `REQUIREMENTS.md` si aplica.
8. **Retro de fase** si la tarea cerrada es la última `todo` de una fase del ROADMAP.

---

# Safety / Constraints

- No introducir secretos o credenciales.
- No reescribir historial en ramas compartidas.
- En `dev-team`: no commits directos a ramas protegidas.
- Fail-hard en `check` si la validación falla (tests o documental).
- En modo `documental`: ninguna publicación con campos obligatorios de plantilla sin rellenar.
