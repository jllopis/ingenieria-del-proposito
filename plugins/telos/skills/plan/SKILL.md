---
name: telos-plan
description: "Activar SOLO si el usuario escribe `/telos:plan` o pide explícitamente planificar un cambio o proyecto bajo Ingeniería del Propósito. Soporta tres modos: dev-team (equipo + PRs), dev-solo (un dev + agente IA, sin PRs), documental (entregables no-código). Pregunta el modo al inicio, formula propósito-paraguas + fichas por capacidad, y genera DESIGN/REQUIREMENTS/TASKS/ROADMAP adaptados. NO activar para planning de sprint, scheduling de tareas, ni planes genéricos sin metodología de propósito."
user-invocable: true
---

# /telos:plan

Planifica un cambio significativo o un proyecto completo bajo Ingeniería del Propósito.

## Contexto

Este comando lanza la fase **plan** del ciclo de vida definido en `telos-dev-core`. Integra Ingeniería del Propósito como primer paso y adapta los artefactos al modo del proyecto. Para operaciones Git (cuando aplican) usa `telos-git-core`.

## Comportamiento

### Paso 0 — Confirma el modo del proyecto

Lee `docs/REQUIREMENTS.md` si existe. Si no hay sección "Modo del proyecto", pregunta:

> ¿Modo del proyecto?
> - **dev-team**: equipo de desarrollo, Git Flow, PRs con reviewers
> - **dev-solo**: un desarrollador + agente IA, sin PRs (la ficha y revisión van en el mensaje de commit)
> - **documental**: produces documentos (políticas, procedimientos, registros), sin tests/lint/build

Si el contexto lo sugiere claramente (p.ej. el usuario menciona "estoy solo", "es una política de seguridad", "tenemos varios devs"), propone uno con justificación y pide confirmación.

### Paso 0.5 — Si hay docs preexistentes, pregunta su origen temporal

Si encuentras docs en `docs/` (diseño funcional, especificaciones, propuestas, briefings), **NO asumas que reflejan el proyecto actual**. Antes de destilar nada, pregunta:

> He detectado los siguientes docs preexistentes: [lista]. ¿Reflejan…
> - (a) el **estado actual y objetivos vigentes** del proyecto?
> - (b) una **visión futura o aspiracional** (p.ej. propuesta dependiente de aprobación, fase posterior, escenario hipotético)?
> - (c) un **mix**: parte refleja el ahora, parte el futuro?
>
> Lo necesito para no contaminar el propósito con expectativas que no apliquen al alcance actual.

Si la respuesta es (b) o (c), pide al usuario que delimite **qué partes son "ahora"** antes de continuar. Un doc llamado "Diseño funcional v0.1" puede describir el proyecto comprometido o solo el ideal aspiracional; sin esa distinción, la ficha-paraguas hereda restricciones que no se aplican.

### Paso 1 — Ficha de propósito-paraguas

Invoca `/telos:brief` con alcance **paraguas**. La ficha describe el proyecto completo (4 horizontes globales). Los horizontes se aplican universalmente:

- **dev-*:** funcional = capacidad técnica; arquitectónico = principios de diseño; restricción = lo que no debe ocurrir; autoría = legibilidad/mantenimiento del código.
- **documental:** funcional = qué decisión/proceso habilita el cuerpo documental; arquitectónico = cómo encaja en el sistema documental existente; restricción = qué no puede contener; autoría = quién es responsable, cómo se aprueba y mantiene.

`brief` validará la ficha-paraguas horizonte por horizonte con el usuario antes de devolverla cerrada (ver "Validación por horizonte" en `telos-brief`). **No avances al paso 2 hasta que los 4 horizontes estén explícitamente confirmados.** Una ficha-paraguas con horizontes sin validar contamina todas las fichas de capacidad que hereden de ella.

### Paso 2 — Descompón en fichas de capacidad

Para cada capacidad relevante, invoca `/telos:brief` con alcance **capacidad**. Cada ficha hereda restricciones del paraguas y añade las suyas.

En `documental`, una "capacidad" suele ser un documento o un conjunto documental coherente (p.ej. "Política de control de accesos", "Procedimiento de gestión de incidentes", "Registro de tratamientos GDPR").

### Paso 3 — Decide layout

- Monolítico: todas las fichas en `docs/REQUIREMENTS.md`
- Split (si >5 capacidades): paraguas en `REQUIREMENTS.md` (índice) + una por capacidad en `docs/requirements/<cap>.md`

Propone split si el proyecto va a tener muchas capacidades.

### Paso 4 — Redacta artefactos según modo

**`docs/REQUIREMENTS.md`** (cabecera obligatoria en todos los modos):

```md
# Requirements

**Modo del proyecto:** dev-team | dev-solo | documental
**Almacén canónico:** [git remote URL | Drive folder | Confluence space | etc.]
**Push automático:** yes | no  *(default: yes en modos dev-* con remote configurado; ver `telos-check` paso 6)*
**Normativa aplicable:** [si procede: ENS, ISO 27001, GDPR, ...]

## Goal
[del propósito-paraguas]

## Scope
In:  …
Out: …

## Ficha de propósito-paraguas
[ficha completa con 4 horizontes]

## Fichas de capacidad
[lista o enlaces si hay split]
```

**`docs/DESIGN.md`**:

- Modo `dev-*`: Overview / Architecture / Data flow / APIs / Decisions / Risks / Non-goals
- Modo `documental`: Overview del cuerpo documental / Jerarquía y dependencias / Plantillas aplicables / Normativa de referencia / Workflow de aprobación / Riesgos / Non-goals

**`docs/TASKS.md`**:

- Modo `dev-*`: tareas con estados `todo | doing | done`
- Modo `documental`: documentos/secciones con estados `todo | drafting | review | approved | published`

**`docs/ROADMAP.md`**:

- Modo `dev-*`: Phase 1 (skeleton) → Phase 2 (capacidades) → Phase 3 (producción) → ...
- Modo `documental`: Phase 1 (estructura + plantillas) → Phase 2 (redacción) → Phase 3 (revisión cruzada) → Phase 4 (aprobación) → Phase 5 (publicación + comunicación)

### Paso 5 — Confirmación

Pide confirmación del plan antes de pasar a `/telos:exec`. No comiences a ejecutar sin OK del usuario.

## Si falta información

Pregunta primero por modo, luego por alcance, restricciones, prioridades y criterios de aceptación.

## Reglas

- No saltes el paso 0. Sin modo declarado el plan se vuelve ambiguo.
- En `documental`, no fuerces docs/DESIGN.md a parecer arquitectura de software. Su contenido es estructura documental.
- En `dev-solo`, deja claro que no habrá PRs y que las fichas + reviews irán en mensajes de commit.
- Adapta el idioma al del usuario.
