# Distribuciones multiplataforma

Este directorio contiene adaptadores generados a partir del contenido canónico en `plugins/telos/`.

## Plataformas

| Directorio | Plataforma | Formato |
|------------|------------|---------|
| `codex/` | OpenAI Codex | System prompts como archivos `.md` en directorio de instrucciones |
| `opencode/` | OpenCode | Slash commands como archivos `.md` en `command/` |

## Generación

Los adaptadores se generan manualmente a partir de los SKILL.md del plugin. El contenido canónico siempre es `plugins/telos/skills/*/SKILL.md`.

**No editar directamente los ficheros de `dist/`.** Si hay que hacer cambios, hazlos en `plugins/telos/` y regenera.

## Diferencias entre plataformas

### Claude Code (canónico)
- Plugin con `plugin.json` + `skills/*/SKILL.md`
- Comandos: `/telos:brief`, `/telos:review`, etc.
- Skills de conocimiento cargadas automáticamente

### OpenAI Codex
- System prompts como archivos Markdown
- Sin namespace de plugin; los prompts se inyectan directamente
- Las skills de conocimiento se incluyen como contexto en cada prompt

### OpenCode
- Slash commands en `command/` como archivos `.md` con frontmatter `description:`
- Sin namespace; comandos como `/dev-plan`, `/dev-exec`, etc.
- Las skills se referencian como `skills/<nombre>/SKILL.md`
