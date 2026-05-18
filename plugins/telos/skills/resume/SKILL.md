---
name: telos-resume
description: "Activar SOLO si el usuario escribe `/telos:resume` o pide explícitamente retomar un proyecto existente bajo el ciclo de vida de Ingeniería del Propósito (inspecciona ramas, cambios locales, estado del repo). NO activar para resumir conversaciones, commits, documentos, ni reanudar tareas genéricas."
user-invocable: true
---

# /telos:resume

Carga el estado real del repo antes de planificar o ejecutar.

## Contexto

Este comando lanza la fase **resume** del ciclo de vida de proyecto definido en `telos-dev-core`. Para operaciones Git aplica las reglas de `telos-git-core`.

## Comportamiento

1. Inspecciona ramas y remotos.
2. Detecta cambios locales sin commit.
3. Identifica base branch correcta según tipo de trabajo.
4. Localiza entrypoints relevantes y dependencias clave.
5. Si hace falta, propone `/telos:sync`.

## Si falta información

Pregunta por tipo de trabajo, ID de Jira, branch base y objetivo actual.
