---
name: telos-exec
description: "Activar SOLO si el usuario escribe `/telos:exec` o pide explícitamente ejecutar un plan bajo Ingeniería del Propósito (implementación guiada por la ficha de propósito, con auto-revisión contra los cuatro horizontes). NO activar para ejecutar comandos shell, scripts, queries SQL, ni para implementación que no sigue la metodología de propósito."
user-invocable: true
---

# /telos:exec

Implementa el plan de forma incremental, trazable y alineada con el propósito declarado.

## Contexto

Este comando lanza la fase **exec** del ciclo de vida de proyecto definido en `telos-dev-core`. Las decisiones de implementación se guían por los cuatro horizontes de la ficha de propósito. Para operaciones Git aplica las reglas de `telos-git-core`.

## Comportamiento

1. Selecciona la tarea prioritaria.
2. **Busca la ficha de propósito** para la tarea actual en `docs/REQUIREMENTS.md` o en la conversación. Si no existe o es ambigua, pide al usuario crearla con `/telos:brief` antes de implementar.
3. Implementa cambios pequeños y revisables.
4. **Auto-revisión contra propósito**: comprueba que la implementación respeta los cuatro horizontes antes de avanzar. Si cumple la función pero rompe arquitectura o restricción, corrige antes de continuar.
5. Actualiza `docs/TASKS.md` y `docs/ROADMAP.md` si cambia el estado.

## Si falta información

Pregunta qué tarea atacar primero o solicita confirmación del orden.
