# Distribuciones multiplataforma

El contenido canónico vive en `plugins/telos/`. La distribución para otras plataformas usa dos mecanismos:

## Agent Skills (estándar abierto)

El directorio `.agents/skills/` en la raíz del repo contiene symlinks a las skills del plugin. Cualquier herramienta compatible con el estándar Agent Skills (agentskills.io) puede usarlas directamente:

- **OpenAI Codex**: `$skill-installer` con URL del repo, o clonar y las detecta automáticamente
- **Cursor, VS Code Copilot, Gemini CLI, Goose, Roo Code**, etc.: clonar el repo o copiar `.agents/skills/`

No hay ficheros duplicados: los symlinks apuntan a `plugins/telos/skills/`.

## Adaptadores específicos

| Directorio | Plataforma | Estado |
|------------|------------|--------|
| `opencode/` | OpenCode | Pendiente de generar |

## Diferencias entre plataformas

### Claude Code (canónico)
- Plugin con `plugin.json` + `skills/*/SKILL.md`
- Comandos: `/telos:brief`, `/telos:review`, etc.
- Skills de conocimiento cargadas automáticamente

### Agent Skills (Codex, Cursor, etc.)
- Skills en `.agents/skills/` con el mismo formato SKILL.md
- Invocación explícita: `$skill-name` (Codex) o equivalente
- Invocación implícita: automática por descripción del skill

### OpenCode
- Slash commands en `command/` como archivos `.md` con frontmatter `description:`
- Sin namespace; comandos como `/telos-plan`, `/telos-exec`, etc.
- Las skills se referencian como `skills/<nombre>/SKILL.md`
