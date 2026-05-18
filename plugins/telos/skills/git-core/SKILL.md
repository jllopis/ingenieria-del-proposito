---
name: telos-git-core
description: "Flujo corporativo de Git del plugin telos: ramas, commits, PRs, merges y releases. Git Flow, Bitbucket por defecto, branch naming con Jira IDs, PR reviews y tagging SemVer. Delegado desde telos-dev-core para todas las operaciones Git del ciclo de vida de proyecto."
user-invocable: false
---

# Git Corporate Workflow

## Purpose

Aplicar el modelo corporativo de Git con reglas claras de ramas, PRs, revisiones, commits, tags y seguridad.

## Rol dentro del sistema de skills

Esta skill es el **especialista de Git** del plugin telos. Opera como delegado desde `telos-dev-core`, que es el orquestador del ciclo de vida del proyecto. Cuando `telos-dev-core` necesita operaciones Git (crear ramas, commits, PRs, releases, sync), delega a esta skill.

Si el usuario invoca directamente una operación Git sin pasar por `telos-dev-core`, esta skill aplica igualmente sus reglas de forma autónoma.

## Inputs

- Contexto del repo (cliente/proyecto, hosting actual si no es Bitbucket).
- Identificador de tarea (Jira o equivalente).
- Tipo de trabajo (feature, bugfix, release, hotfix).
- Estado de ramas existentes (develop, stage, main/master).
- Política de releases y versionado si existe.

## Outputs

- Recomendación explícita de ramas y comandos a usar.
- Checklist de PR y validaciones previas a merge.
- Mensajes de commit propuestos y tagging si aplica.
- Avisos de seguridad o incumplimientos detectados.

## Steps

1. Confirmar hosting: usar Bitbucket salvo que el cliente indique otro servidor.
2. Determinar tipo de trabajo y rama base:
   - feature/bugfix desde `develop`
   - release desde `develop`
   - hotfix desde `main/master`
3. Crear rama con ID de Jira cuando sea posible. Formato recomendado:
   - `feature/<jira-id>-<resumen-kebab>`
   - `bugfix/<jira-id>-<resumen-kebab>`
   - `hotfix/<jira-id>-<resumen-kebab>`
   - `release/<version>`
4. Mantener `develop`, `stage` y `main/master` protegidas: nunca hacer commits directos, solo vía PR.
5. Preparar cambios con commits pequeños, frecuentes y semánticos; evitar trabajo a medias.
6. Asegurar calidad antes de commit:
   - ejecutar pruebas relevantes
   - revisar que no haya secretos
   - no incluir dependencias en el repo salvo requerimiento explícito
7. Abrir PR y aplicar el flujo de revisión:
   - auto-revisión antes de solicitar review
   - al menos un revisor obligatorio antes de merge
   - revisar PRs de compañeros lo antes posible
8. Integración:
   - usar `rebase` solo en ramas locales privadas
   - evitar rebase de ramas compartidas; usar merge en ramas públicas
9. Releases:
   - fusionar release en `main/master` y `develop`
   - etiquetar cada release con SemVer

## Checks

- ¿La rama tiene ID de Jira?
- ¿El PR apunta a la rama correcta?
- ¿Se ejecutaron tests?
- ¿Hay secretos o credenciales en los cambios?
- ¿El commit es pequeño, único y con mensaje imperativo?
- ¿Hay tag SemVer si se trata de un release?

## Errors to avoid

- Hacer commit directo a `develop`, `stage` o `main/master`.
- Rebasear ramas ya compartidas.
- Subir tokens/passwords.
- Mezclar múltiples cambios no relacionados en un mismo commit.
- Etiquetar releases sin SemVer.

## Lifecycle operations

Estas recetas las invocaba antes el ciclo de vida como comandos (`/telos:init`, `/telos:resume`, `/telos:sync`). Ahora se aplican cuando el usuario pide la operación equivalente en lenguaje natural ("inicializa el proyecto", "retoma este repo", "sincroniza con remoto") o desde `telos-dev-core`.

### Recipe: init (inicializar proyecto)

1. Confirma stack, nombre del proyecto y repo remoto.
2. Inicializa el repo (si no existe) con estructura mínima.
3. Crea ramas `main` y `develop` si se usa Git Flow.
4. Instala dependencias y bootstrap mínimo.
5. Ejecuta un smoke test o instrucción de arranque.
6. Documenta cómo ejecutar localmente.

Si falta info: pregunta por stack, nombre, repo remoto, branch principal y si se usa Git Flow.

### Recipe: resume (retomar repo existente)

1. Inspecciona ramas y remotos (`git branch -a`, `git remote -v`).
2. Detecta cambios locales sin commit (`git status`).
3. Identifica base branch correcta según tipo de trabajo (feature/bugfix → develop; hotfix → main).
4. Localiza entrypoints relevantes y dependencias clave.
5. Si la rama actual está desactualizada, propone aplicar la receta **sync**.

Si falta info: pregunta por tipo de trabajo, ID de Jira, branch base y objetivo actual.

### Recipe: sync (sincronizar con remoto)

1. `git fetch`.
2. Verifica estado local limpio.
3. Aplica modo:
   - **merge** (default) para ramas compartidas.
   - **rebase** solo en ramas locales privadas.
   - **ff-only** si no hay cambios locales.
4. Resuelve conflictos y valida estado.

Si falta info: pregunta por el modo deseado (merge/rebase/ff-only).

## Examples

### Example 1: Nueva feature

Input: "Voy a implementar login social. Tarea Jira ABC-123."
Output:
- Crear rama `feature/ABC-123-login-social` desde `develop`.
- Commits pequeños con mensajes imperativos.
- PR hacia `develop`, con auto-revisión y al menos 1 revisor.

### Example 2: Hotfix producción

Input: "Bug crítico en producción."
Output:
- Crear rama `hotfix/ABC-999-fix-crash` desde `main`.
- PR hacia `main`, luego merge a `develop`.
- Tag SemVer al release.

### Example 3: Preparar release

Input: "Vamos a release 1.4.0."
Output:
- Crear rama `release/1.4.0` desde `develop`.
- Ajustes finales, PR hacia `main` y merge de vuelta a `develop`.
- Tag `v1.4.0`.

## Safety / Constraints

- No introducir secretos en el repo.
- No reescribir historial en ramas compartidas.
- No fusionar sin PR y review aprobada.

## Scripts

- `scripts/validate_git_workflow.sh`: Validaciones automáticas de naming de ramas, secretos y checks de PR.
  - Uso básico: `bash scripts/validate_git_workflow.sh --base develop`
  - Opciones: `--all` para escanear todo el repo, `--no-secret-scan` para omitir escaneo.

## References

- [references/branching-model.md](references/branching-model.md)
- [references/pr-template.md](references/pr-template.md)
- [references/commit-messages.md](references/commit-messages.md)
- [references/release-tagging.md](references/release-tagging.md)
- [references/git-team-workflow.md](references/git-team-workflow.md)
