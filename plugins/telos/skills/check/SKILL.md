---
name: telos-check
description: "Activar SOLO si el usuario escribe `/telos:check` o pide explícitamente cerrar un cambio bajo Ingeniería del Propósito: revisa contra ficha de propósito horizonte por horizonte, ejecuta validación (tests/lint/build en modos dev-*, plantilla+referencias+aprobaciones en modo documental), cierra (commit+push+PR en dev-team, commit+push con ficha embebida en dev-solo, publicación+notificación en documental), y captura lección clasificándola como local/capacidad/paraguas con diff propuesto a REQUIREMENTS.md. Detecta cierre de fase del ROADMAP. Push es parte del cierre en modos dev-* con remote; override con `Push automático: no` en REQUIREMENTS.md. NO activar para validaciones puntuales, lint aislado, ni revisiones de PR sin ficha de propósito previa."
user-invocable: true
---

# /telos:check

Cierra un cambio: revisa contra propósito + valida + cierra (commit/PR/publicación según modo) + captura lección con promoción a fichas.

## Contexto

Este comando lanza la fase **check** del ciclo de vida definido en `telos-dev-core`. Es el único cierre del flujo. Lee el **modo del proyecto** (`dev-team` | `dev-solo` | `documental`) de `docs/REQUIREMENTS.md` y adapta cada paso. Las plantillas viven en `telos-purpose-core/assets/`.

## Comportamiento

### 1. Localiza ficha y modo

- Lee el modo del proyecto de `docs/REQUIREMENTS.md` (sección "Modo del proyecto").
- Localiza la ficha de propósito relevante (capacidad o paraguas) en `docs/REQUIREMENTS.md` o `docs/requirements/`.
- Si no hay ficha para este cambio, avisa: sin propósito explícito la revisión se reduce a impresiones. Ofrece crearla con `/telos:brief`.

### 2. Revisión por horizontes

ANTES de validación técnica/documental y cierre, evalúa horizonte por horizonte usando la plantilla `purpose-review-template.md` de `telos-purpose-core/assets/`:

```md
## Revisión contra propósito

**Funcional:** Cumple / No cumple — [observación concreta]
**Arquitectónico:** Cumple / No cumple — [observación concreta]
**Restricción:** Cumple / No cumple — [observación concreta]
**Autoría:** Cumple / No cumple — [observación concreta]

**Riesgos o trade-offs:**
- [riesgo]

**Decisión:** Aprobar / Aprobar con observaciones / Pedir cambios

**Correcciones sugeridas:**
- [corrección]
```

Sé específico: cita líneas, decisiones de diseño, secciones del documento. No suavices violaciones. Si la propuesta cumple la función pero rompe arquitectura o restricción → decisión "Pedir cambios" y **detén** el flujo. Informa y pide decisión antes de continuar.

### 3. Validación específica del modo

**Modos `dev-team` y `dev-solo`:**
- Ejecuta tests / lint / build relevantes.
- Fail-hard: si algo falla, detén e informa.

**Modo `documental`:**
- Plantilla aplicada correctamente (campos obligatorios completos).
- Referencias cruzadas íntegras (otros documentos citados existen y la cita es coherente).
- Aprobaciones requeridas presentes según workflow declarado en DESIGN.md.
- Ausencia de información restringida según el horizonte de restricción de la ficha.
- Fail-hard: si alguna validación falla, detén e informa.

### 4. Revisión humana

- `dev-team`: code review por al menos un revisor.
- `dev-solo`: el agente IA es el revisor (ya hecho en pasos 2-3). Salta.
- `documental`: lectura por aprobadores según workflow. Si la fase aún no llegó a "approved", marca la tarea como `review` y detén.

### 5. Fixes

Aplica correcciones necesarias. Si los cambios son sustantivos, vuelve al paso 2.

### 6. Cierre adaptado al modo

#### Modo `dev-team`

1. Commit(s) semánticos con ID de Jira (delega a `telos-git-core`).
2. **Push** de la rama a `origin` (con `-u` si es el primer push). Si el push falla, detén, informa y pide decisión (igual que un test roto). Si la rama no tiene remote configurado o el usuario declaró `**Push automático:** no` en `REQUIREMENTS.md`, omite el push y avisa.
3. PR al branch correcto:
   - feature/bugfix → `develop`
   - release/hotfix → `main`/`master`
4. **Cuerpo del PR usa la plantilla `telos-purpose-core/assets/pr-template.md`**:

   ```md
   ## Ficha de propósito
   [copia textual de la ficha de la Capacidad X o del Paraguas]

   ## Revisión por horizontes
   - Funcional: Cumple — [observación]
   - Arquitectónico: Cumple — [observación]
   - Restricción: Cumple con observación — [observación]
   - Autoría: Cumple — [observación]

   ## Cambios
   [summary de commits]

   ## Validación
   - Tests: [pasa / detalles]
   - Lint/build: [pasa / detalles]
   ```

   Esto hace visibles los criterios a los reviewers humanos y deja traza permanente en el sistema de PRs.

#### Modo `dev-solo`

1. Commit semántico (delega naming a `telos-git-core`).
2. **El mensaje de commit incluye ficha + revisión** (plantilla `telos-purpose-core/assets/commit-message-template.md`):

   ```
   <tipo>(<scope>): <resumen imperativo>

   ## Ficha de propósito
   [copia textual de la ficha relevante]

   ## Revisión por horizontes
   - Funcional: Cumple — [observación]
   - Arquitectónico: Cumple — [observación]
   - Restricción: Cumple con observación — [observación]
   - Autoría: Cumple — [observación]

   Refs: [Jira ID si aplica]
   ```

   Sin PR. La traza queda en el historial de commits, que es el sustituto del PR cuando no hay reviewers humanos.

#### Modo `documental`

1. Publica el documento al almacén canónico (Drive, Confluence, local + git tag, etc.).
2. Registra la versión (Drive history, naming convention, git tag, según almacén).
3. Notifica a stakeholders según workflow declarado: aprobadores, equipo afectado, compliance.
4. Si el almacén es git, commit **y push** con la plantilla de `dev-solo` (ficha + revisión en mensaje).
5. Actualiza estado en `TASKS.md` a `published`.

### 7. Lección de propósito con clasificación obligatoria

Tras el cierre, ofrece capturar lección. **Clasifica obligatoriamente** el alcance:

```
¿Esta lección es...
  (a) local      → solo afecta a este cambio; guárdala en notas/journal
  (b) capacidad  → modifica un horizonte de la ficha de Capacidad X
  (c) paraguas   → modifica un horizonte de la ficha-proyecto
  (d) sin lección → el cambio fue limpio, no hay aprendizaje significativo
```

Usa la plantilla `purpose-retro-template.md`:

```md
## Lección de propósito

**Cambio:** [una frase]
**Alcance:** local | capacidad <X> | paraguas
**Qué funcionó:** [vinculado a la ficha o al proceso]
**Qué falló:** [problema concreto + causa]
**Qué faltó en la ficha:** [horizonte vago, restricción omitida, contexto que habría ayudado]
**Patrón reutilizable:** [regla específica y accionable]
```

**Si la clasificación es (b) capacidad o (c) paraguas:** produce un **diff concreto** sobre `docs/REQUIREMENTS.md` (o `docs/requirements/<cap>.md` si hay split) refinando el horizonte afectado. Preséntalo al usuario para aprobar/editar antes de aplicar. Ejemplo:

```diff
 ### Capacidad 3 — Evaluación semántica
 ...
 **Restricción:**
 - Cada observación cita la sección o campo origen.
 - No se acepta una observación sin cita.
 - Prompt y modelo versionados.
+- Las observaciones del LLM tienen TTL: deben re-evaluarse si el modelo o el prompt cambia (lección de PR #42).
```

**Si la clasificación es (a) local:** guarda como nota en `docs/lessons/YYYY-MM-DD-<slug>.md` o donde el usuario prefiera. No toca fichas.

**Si la clasificación es (d) sin lección:** no fuerces hallazgos. Cierre limpio.

### 8. Retro de fase (si aplica)

Lee `docs/ROADMAP.md`. Si la tarea que se está cerrando es la **última `todo` / `doing` de una fase**, antes del paso 7 (lección normal), lanza una **retro de fase** más profunda:

```md
## Retro de fase — Phase N: [nombre]

**Qué descubriste sobre el dominio que no sabías al planificar:**
- [...]

**Horizontes de la ficha-paraguas que necesitan refinarse:**
- [...]

**Tareas de Phase N+1 que sobran / faltan / cambian de orden:**
- [...]

**Patrones reutilizables a nivel de fase:**
- [...]
```

Produce **dos diffs**:
1. Sobre `REQUIREMENTS.md` (refinar paraguas o fichas de capacidad).
2. Sobre `ROADMAP.md` (ajustar Phase N+1).

Presenta ambos para aprobación antes de aplicar. Esta es la retro más valiosa del proyecto: convierte aprendizaje de fase en plan revisado.

## Si falta información

Pregunta por modo del proyecto (si no está en REQUIREMENTS.md), tipo de trabajo (feature/bugfix/release/hotfix), branch destino, convención de commits, o (modo documental) workflow de aprobación y almacén canónico.

## Reglas

- No saltes el paso 2 (revisión por horizontes). Es el corazón del check.
- No cierres con horizontes en violación. Detén el flujo y pide decisión.
- En `dev-team`, el PR debe llevar la plantilla. Sin ella, el cambio es invisible para reviewers humanos.
- En `dev-solo`, el commit message debe llevar la ficha embebida. Sin ella, no hay traza de propósito.
- En modos `dev-*` con remote configurado, **push es parte del cierre**, no un paso opcional posterior. La traza compartida es el entregable. Excepción: si `REQUIREMENTS.md` declara `**Push automático:** no`, el agente omite el push y avisa al usuario para que lo lance manualmente.
- En `documental`, no hay publicación sin aprobaciones presentes.
- La clasificación de lección (paso 7) es obligatoria, no opcional. "Sin lección" es una opción válida.
- En cierre de fase, retro de fase tiene precedencia sobre retro normal.
- Adapta el idioma al del usuario.
