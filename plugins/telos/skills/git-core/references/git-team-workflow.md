# Git Team Workflow

## Idea principal

Mantra del trabajo colaborativo: "Always be branching, and always be pulling".

- Crear ramas siempre. No trabajar directamente en ramas protegidas.
- Actualizarse desde la rama base antes de integrar cambios.
- Tras enviar un PR, no reutilizar esa rama: considerarla cerrada y crear otra nueva.

## Reglas de equipo

### Continuidad tras una pausa

Al retomar una rama tras una pausa, siempre sincronizar primero con la rama base (`develop` o `main` según el caso) y resolver conflictos antes de seguir trabajando.

### Continuidad tras enviar un PR

No seguir trabajando en la misma rama. Crear una nueva rama desde el punto actual y hacer merge o rebase de la anterior si es necesario.

### Gestión de conflictos

- Resolver conflictos en la rama de trabajo, nunca en la rama base.
- Buscar marcadores de conflicto (`>>>>>`, `<<<<<`, `=====`) y resolverlos manualmente.
- Después de resolver, hacer commit explícito indicando qué conflicto se resolvió.
- Si el conflicto pertenece a otro miembro del equipo, coordinarse antes de resolver.

### Pull Requests

- Actualizar con la rama base antes de abrir PR.
- Incluir checklist de pruebas realizadas.
- Asignar al menos un revisor.
- Documentar conflictos resueltos si los hubo.
- Probar lo indicado antes de aprobar un PR ajeno.
- Eliminar la rama tras merge exitoso.

### Mantenimiento continuo

- Sincronizar con la rama base con frecuencia para evitar conflictos grandes.
- Varias PRs al día es normal en equipos activos.
- Coordinar revisiones para no bloquear el flujo.

### Rama de respaldo

Antes de cambios experimentales o destructivos, crear una rama de respaldo y pushearla.

### Prohibiciones

- No usar `git reset --hard` ni `git push --force` en ramas compartidas. Eliminan historial y pueden borrar trabajo de otros.
- No reutilizar ramas cerradas tras merge.

## Buenas prácticas de commit

- Un commit por cambio lógico.
- Mensajes imperativos siguiendo el formato semántico definido en `commit-messages.md`.
- Para bugs: un commit identificando el problema, commits de investigación si aplica, y un commit final con la solución.

## Buenas prácticas de PR

- Incluir checklist con rutas de archivos afectados.
- Añadir capturas de resultados cuando sea visual.
- Incluir notas de reproducción de pruebas.
