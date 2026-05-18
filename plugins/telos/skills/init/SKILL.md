---
name: telos-init
description: "Activar SOLO si el usuario escribe `/telos:init` o pide explícitamente iniciar un proyecto nuevo bajo el ciclo de vida de Ingeniería del Propósito (telos-dev-core: stack, repo, bootstrap, smoke test). NO activar para inicializar CLAUDE.md (existe la skill built-in `init`), `git init` puntual, o cualquier inicialización que no sea un proyecto completo bajo telos."
user-invocable: true
---

# /telos:init

Inicia un proyecto nuevo con base funcional mínima.

## Contexto

Este comando lanza la fase **init** del ciclo de vida de proyecto definido en `telos-dev-core`. Para operaciones Git aplica las reglas de `telos-git-core`.

## Comportamiento

1. Confirma stack, nombre del proyecto y repo remoto (si aplica).
2. Inicializa el repo (si no existe) y estructura mínima.
3. Instala dependencias y crea el bootstrap mínimo.
4. Ejecuta un smoke test o instrucción de arranque.
5. Documenta cómo ejecutar localmente.

## Si falta información

Pregunta por stack, nombre, repositorio remoto, branch principal y si se usa Git Flow.
