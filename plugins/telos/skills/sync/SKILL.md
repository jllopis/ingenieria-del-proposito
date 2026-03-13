---
name: sync
description: "Dev flow: sincronizar — trae cambios remotos e integra localmente con merge, rebase o ff-only."
user-invocable: true
---

# /telos:sync

Trae cambios remotos e integra localmente.

## Contexto

Este comando lanza la fase **sync** del ciclo de vida de proyecto definido en `dev-core`. Para operaciones Git aplica las reglas de `git-core`.

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
