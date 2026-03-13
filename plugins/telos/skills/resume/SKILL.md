---
name: resume
description: "Dev flow: retomar un proyecto existente — inspecciona ramas, cambios locales y estado del repo."
user-invocable: true
---

# /telos:resume

Carga el estado real del repo antes de planificar o ejecutar.

## Contexto

Este comando lanza la fase **resume** del ciclo de vida de proyecto definido en `dev-core`. Para operaciones Git aplica las reglas de `git-core`.

## Comportamiento

1. Inspecciona ramas y remotos.
2. Detecta cambios locales sin commit.
3. Identifica base branch correcta según tipo de trabajo.
4. Localiza entrypoints relevantes y dependencias clave.
5. Si hace falta, propone `/telos:sync`.

## Si falta información

Pregunta por tipo de trabajo, ID de Jira, branch base y objetivo actual.
