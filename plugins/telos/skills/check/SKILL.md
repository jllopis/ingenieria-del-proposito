---
name: telos-check
description: "Activar SOLO si el usuario escribe `/telos:check` o pide explícitamente cerrar un cambio bajo Ingeniería del Propósito: revisa contra ficha de propósito horizonte por horizonte, ejecuta tests/lint/build, hace commit semántico con ID Jira, abre PR al branch correcto y opcionalmente captura una lección de propósito. NO activar para validaciones puntuales, lint aislado, health checks, ni revisiones de PR sin ficha de propósito previa."
user-invocable: true
---

# /telos:check

Cierra un cambio: revisa contra propósito + valida técnicamente + commit + PR + lección opcional.

## Contexto

Este comando lanza la fase **check** del ciclo de vida de proyecto definido en `telos-dev-core`. Es el único cierre del flujo: combina la revisión por horizontes (antes en `/telos:review`), la validación técnica, el cierre Git y la captura de aprendizaje (antes en `/telos:retro`). Para operaciones Git aplica las reglas de `telos-git-core`. Los criterios y plantillas viven en `telos-purpose-core`.

## Comportamiento

1. **Localiza la ficha de propósito** en `docs/REQUIREMENTS.md` o en la conversación. Si no existe ficha para este cambio, avisa al usuario antes de continuar: sin propósito explícito la revisión se reduce a impresiones. Ofrece crearla con `/telos:brief`.

2. **Revisión por horizontes (antes era /telos:review).** ANTES de tests y commit, evalúa la solución horizonte por horizonte. Usa la plantilla `purpose-review-template.md` de `telos-purpose-core/assets/`:

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

   Sé específico: cita líneas, decisiones de diseño o patrones concretos. No suavices violaciones. Si la propuesta cumple la función pero rompe arquitectura o restricción, la decisión es "Pedir cambios" y **detienes** el flujo aquí: informa y pide decisión antes de continuar.

3. **Tests/lint/build.** Ejecuta lo relevante para el repo. Fail-hard: si algo falla, detén, informa y pide decisión. No hay commit ni PR con tests rotos.

4. **Code review** de los cambios.

5. Aplica fixes necesarios y vuelve al paso 2 si el código cambió de forma sustantiva.

6. **Commit(s) semánticos** con ID de Jira (delega a `telos-git-core`).

7. **PR al branch correcto** (delega a `telos-git-core`):
   - feature/bugfix → `develop`
   - release/hotfix → `main`/`master`

8. **Lección de propósito (opcional, antes era /telos:retro).** Si el cambio fue significativo o dejó aprendizaje, ofrece al usuario capturarlo con la plantilla `purpose-retro-template.md` de `telos-purpose-core/assets/`:

   ```md
   ## Lección de propósito

   **Cambio:** [una frase]

   **Qué funcionó:**
   - [aspecto positivo vinculado a la ficha o al proceso]

   **Qué falló:**
   - [problema concreto y causa probable]

   **Qué faltó en la ficha:**
   - [horizonte vago, restricción omitida, contexto que habría ayudado]

   **Patrón reutilizable:**
   - [regla o criterio extraído, aplicable a cambios similares]
   ```

   Reglas:
   - No fuerces hallazgos. Si el cambio fue limpio, dilo y no inventes lección.
   - Vincula cada punto a algo concreto, no abstracciones.
   - El patrón reutilizable debe ser específico y accionable.
   - El usuario decide dónde guardarla (notas de equipo, docs/, etc.).

## Si falta información

Pregunta por tipo de trabajo (feature/bugfix/release/hotfix), branch destino y convención de commits.
