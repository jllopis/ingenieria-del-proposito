---
name: telos-check
description: "Activar SOLO si el usuario escribe `/telos:check` o pide explícitamente validar un cambio bajo Ingeniería del Propósito (revisa contra ficha de propósito, ejecuta tests/lint/build y cierra con commit semántico + PR). NO activar para validaciones puntuales, lint aislado, health checks, ni revisiones de PR sin ficha de propósito previa."
user-invocable: true
---

# /telos:check

Valida calidad contra propósito y estándares técnicos, y cierra con commit + PR.

## Contexto

Este comando lanza la fase **check** del ciclo de vida de proyecto definido en `telos-dev-core`. Combina revisión contra propósito, validación técnica y cierre Git. Para operaciones Git aplica las reglas de `telos-git-core`.

## Comportamiento

1. **Localiza la ficha de propósito** en `docs/REQUIREMENTS.md` o en la conversación. Si no existe ficha para este cambio, avisa al usuario antes de continuar: sin propósito explícito la revisión se reduce a impresiones.
2. **Revisión contra propósito.** ANTES de tests y commit, evalúa la solución horizonte por horizonte contra la ficha localizada. Usa `/telos:review` para la evaluación formal. Si algún horizonte no se cumple: **detén**, informa y pide decisión.
3. Ejecuta tests/lint/build relevantes.
4. Si falla algo: **detén**, informa y pide decisión. Fail-hard: no hay commit ni PR si los tests no pasan.
5. Realiza code review de los cambios.
6. Aplica fixes necesarios.
7. Crea commit(s) semánticos con ID de Jira.
8. Crea PR al branch correcto:
   - feature/bugfix → `develop`
   - release/hotfix → `main`/`master`
9. Opcionalmente, captura aprendizaje con `/telos:retro` si el cambio fue significativo.

## Si falta información

Pregunta por tipo de trabajo, branch destino y convención de commits.
