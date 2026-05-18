---
name: telos-exec
description: "Activar SOLO si el usuario escribe `/telos:exec` o pide explícitamente ejecutar un plan bajo Ingeniería del Propósito. Antes de cada tarea, recita explícitamente qué horizontes de la ficha toca la tarea y cuáles no. Adaptado a tres modos: dev-team y dev-solo (implementación de código), documental (redacción/edición de documentos). NO activar para ejecutar comandos shell, scripts, queries SQL, ni para implementación que no sigue la metodología de propósito."
user-invocable: true
---

# /telos:exec

Implementa o redacta el plan de forma incremental, trazable y alineada con el propósito declarado.

## Contexto

Este comando lanza la fase **exec** del ciclo de vida definido en `telos-dev-core`. Las decisiones se guían por los cuatro horizontes de la ficha relevante. Lee el modo del proyecto (`dev-team` | `dev-solo` | `documental`) de `docs/REQUIREMENTS.md` y adapta el comportamiento.

## Comportamiento

1. **Selecciona la tarea prioritaria** de `docs/TASKS.md`.

2. **Localiza la ficha relevante** (capacidad o paraguas según alcance) en `docs/REQUIREMENTS.md` o `docs/requirements/`. Si la tarea no tiene ficha clara y es compleja, formula una breve con `/telos:brief` antes de implementar (funcional + restricción como mínimo).

3. **Recita explícitamente los horizontes que esta tarea toca y los que no.** Este contrato declarado al inicio reemplaza la "auto-revisión vaga": al cerrar la tarea solo comparas contra los horizontes listados.

   Formato:
   ```
   Tarea: T2.3 — [descripción corta]
   Ficha relevante: [Capacidad X o Paraguas]
   Horizontes que toca esta tarea:
     - Arquitectónico: "[cita textual del horizonte]"
     - Restricción: "[cita textual del horizonte]"
   Horizontes que NO toca: Funcional, Autoría
   ```

   Si la tarea toca los 4 horizontes, lístalos los 4. Si la tarea es trivial y no toca ninguno (típico: rename, formato), declara "tarea sin horizontes activos" y procede sin auto-revisión.

4. **Ejecuta:**
   - Modos `dev-team` / `dev-solo`: implementa cambios pequeños y revisables. Commits intermedios solo si la tarea es larga.
   - Modo `documental`: redacta o edita la sección/documento en el almacén canónico (local, Drive vía MCP, etc.). Aplica la plantilla si la capacidad usa una.

5. **Auto-revisión contra los horizontes recitados.** Antes de marcar la tarea como completa:
   - Para cada horizonte tocado: ¿la implementación/redacción lo respeta?
   - Si cumple la función pero rompe un horizonte tocado → corrige antes de avanzar.
   - Si no estás seguro de un horizonte, déjalo señalado para `/telos:check` y avisa al usuario.

6. **Actualiza** `docs/TASKS.md` y `docs/ROADMAP.md` si cambia el estado. En `documental`, los estados son `todo | drafting | review | approved | published`; en `dev-*` son `todo | doing | done`.

## Si falta información

Pregunta qué tarea atacar primero o solicita confirmación del orden. Si no hay ficha relevante y la tarea es no-trivial, pide producir una primero con `/telos:brief`.

## Reglas

- No saltes el paso 3 (recitar horizontes). Es lo que hace la auto-revisión del paso 5 honesta.
- No avances a la siguiente tarea con un horizonte tocado que no se cumple. Corrige o detén el flujo.
- En `documental`, los "cambios pequeños y revisables" son secciones cortas con sentido completo, no documentos enteros de una sola vez.
- Adapta el idioma al del usuario.
