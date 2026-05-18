---
name: telos-sync
description: "Activar SOLO si el usuario escribe `/telos:sync` o pide explícitamente sincronizar cambios remotos bajo el ciclo de vida de Ingeniería del Propósito (merge/rebase/ff-only sobre la rama actual). NO activar para sincronización de datos entre sistemas, sync de archivos, sync de calendarios, ni `git pull` puntual sin contexto de telos."
user-invocable: true
---

# /telos:sync

Trae cambios remotos e integra localmente.

## Contexto

Este comando lanza la fase **sync** del ciclo de vida de proyecto definido en `telos-dev-core`. Para operaciones Git aplica las reglas de `telos-git-core`.

## Comportamiento

1. `git fetch`.
2. Verifica estado local limpio.
3. Aplica modo:
   - **merge** (default) para ramas compartidas.
   - **rebase** solo en ramas locales privadas.
   - **ff-only** si no hay cambios locales.
4. Resuelve conflictos y valida estado.

## Si falta información

Pregunta por el modo deseado (merge/rebase/ff-only).
