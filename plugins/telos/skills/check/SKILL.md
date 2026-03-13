---
name: check
description: "Dev flow: validar + commit + PR — revisa contra propósito, ejecuta tests y cierra con commit semántico."
user-invocable: true
---

# /telos:check

Valida calidad contra propósito y estándares técnicos, y cierra con commit + PR.

## Contexto

Este comando lanza la fase **check** del ciclo de vida de proyecto definido en `dev-core`. Combina revisión contra propósito, validación técnica y cierre Git. Para operaciones Git aplica las reglas de `git-core`.

## Comportamiento

1. **Revisión contra propósito.** ANTES de tests y commit, evalúa la solución horizonte por horizonte. Usa `/telos:review` para la evaluación formal. Si algún horizonte no se cumple: **detén**, informa y pide decisión.
2. Ejecuta tests/lint/build relevantes.
3. Si falla algo: **detén**, informa y pide decisión. Fail-hard: no hay commit ni PR si los tests no pasan.
4. Realiza code review de los cambios.
5. Aplica fixes necesarios.
6. Crea commit(s) semánticos con ID de Jira.
7. Crea PR al branch correcto:
   - feature/bugfix → `develop`
   - release/hotfix → `main`/`master`
8. Opcionalmente, captura aprendizaje con `/telos:retro` si el cambio fue significativo.

## Si falta información

Pregunta por tipo de trabajo, branch destino y convención de commits.
