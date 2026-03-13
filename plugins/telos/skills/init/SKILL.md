---
name: init
description: "Dev flow: iniciar un proyecto nuevo con base funcional mínima (stack, repo, bootstrap, smoke test)."
user-invocable: true
---

# /telos:init

Inicia un proyecto nuevo con base funcional mínima.

## Contexto

Este comando lanza la fase **init** del ciclo de vida de proyecto definido en `dev-core`. Para operaciones Git aplica las reglas de `git-core`.

## Comportamiento

1. Confirma stack, nombre del proyecto y repo remoto (si aplica).
2. Inicializa el repo (si no existe) y estructura mínima.
3. Instala dependencias y crea el bootstrap mínimo.
4. Ejecuta un smoke test o instrucción de arranque.
5. Documenta cómo ejecutar localmente.

## Si falta información

Pregunta por stack, nombre, repositorio remoto, branch principal y si se usa Git Flow.
