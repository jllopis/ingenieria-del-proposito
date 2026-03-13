---
name: plan
description: "Dev flow: planificar — define propósito, diseño, requisitos, tareas y roadmap antes de implementar."
user-invocable: true
---

# /telos:plan

Define el propósito del cambio y crea diseño, requisitos, tareas y roadmap.

## Contexto

Este comando lanza la fase **plan** del ciclo de vida de proyecto definido en `dev-core`. Integra Ingeniería del Propósito como primer paso. Para operaciones Git aplica las reglas de `git-core`.

## Comportamiento

1. **Formula la ficha de propósito.** Antes de documentar, define los cuatro horizontes (funcional, arquitectónico, restricción, autoría). Usa `/telos:brief` para producir la ficha.
2. Los cuatro horizontes actúan como criterios de aceptación: si la solución los cumple, se acepta; si viola alguno, se rechaza.
3. Clarifica alcance y restricciones a partir de la ficha.
4. Redacta `docs/DESIGN.md` — el diseño debe responder a los horizontes declarados.
5. Redacta `docs/REQUIREMENTS.md` — las fichas de propósito sustituyen a user stories + criterios de aceptación.
6. Redacta `docs/TASKS.md`.
7. Redacta/actualiza `docs/ROADMAP.md`.
8. Pide confirmación del plan antes de ejecutar.

## Si falta información

Pregunta por alcance, restricciones, prioridades y criterios de aceptación.
