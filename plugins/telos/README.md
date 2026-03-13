# telos — Ingeniería del Propósito

Plugin para asistentes de código que aplica **Ingeniería del Propósito**: trabaja con LLMs desde propósito explícito en lugar de partir de contexto abundante.

## Qué incluye

### Metodología de propósito

| Comando | Descripción |
|---------|-------------|
| `/telos:brief` | Convierte una petición en una ficha de propósito con los 4 horizontes |
| `/telos:review` | Revisa una propuesta contra la ficha de propósito |
| `/telos:retro` | Captura aprendizaje reutilizable tras cerrar un cambio |

### Ciclo de vida de proyecto

| Comando | Descripción |
|---------|-------------|
| `/telos:init` | Iniciar un proyecto nuevo |
| `/telos:resume` | Retomar un proyecto existente |
| `/telos:plan` | Planificar: diseño, requisitos, tareas, roadmap |
| `/telos:exec` | Ejecutar el plan guiado por el propósito |
| `/telos:check` | Validar contra propósito + tests + commit + PR |
| `/telos:sync` | Sincronizar cambios remotos |

### Skills de conocimiento (no invocables directamente)

| Skill | Descripción |
|-------|-------------|
| `purpose-core` | Principios, horizontes, anti-patrones y flujo de la Ingeniería del Propósito |
| `dev-core` | Reglas del ciclo de vida por fases (init → sync) |
| `git-core` | Flujo corporativo de Git: Git Flow, branch naming, PRs, SemVer |

## Los cuatro horizontes

Todo cambio debe articular:

- **Funcional**: qué problema desaparece o qué capacidad nueva aparece.
- **Arquitectónico**: qué principio de diseño debe protegerse.
- **Restricción**: qué no puede ocurrir bajo ninguna circunstancia.
- **Autoría**: cómo debe leerse, mantenerse o evolucionar el resultado.

## Flujo típico

```
/telos:brief  →  (implementación)  →  /telos:review  →  /telos:retro
```

Con ciclo de proyecto completo:

```
/telos:init o /telos:resume
    → /telos:plan (incluye brief)
    → /telos:exec
    → /telos:check (incluye review + commit + PR)
    → /telos:sync
```

## Instalación en Claude Code

```bash
# Añadir el marketplace
/plugin marketplace add owner/ingenieria-del-proposito

# Instalar el plugin
/plugin install telos
```

## Estructura del plugin

```
plugins/telos/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── purpose-core/          # Conocimiento base de la metodología
│   │   ├── SKILL.md
│   │   ├── assets/            # Plantillas de ficha, review y retro
│   │   └── references/        # Modelo operativo, anti-patrones
│   ├── dev-core/              # Ciclo de vida de proyecto
│   │   └── SKILL.md
│   ├── git-core/              # Flujo corporativo de Git
│   │   ├── SKILL.md
│   │   ├── references/        # Branching, commits, PRs, releases
│   │   └── scripts/           # Validaciones automáticas
│   ├── brief/                 # /telos:brief
│   │   └── SKILL.md
│   ├── review/                # /telos:review
│   │   └── SKILL.md
│   ├── retro/                 # /telos:retro
│   │   └── SKILL.md
│   ├── init/                  # /telos:init
│   │   └── SKILL.md
│   ├── resume/                # /telos:resume
│   │   └── SKILL.md
│   ├── plan/                  # /telos:plan
│   │   └── SKILL.md
│   ├── exec/                  # /telos:exec
│   │   └── SKILL.md
│   ├── check/                 # /telos:check
│   │   └── SKILL.md
│   └── sync/                  # /telos:sync
│       └── SKILL.md
└── README.md
```

## Distribución multiplataforma

Este plugin es el contenido canónico. Para otras plataformas, se generan adaptadores en `../../dist/`:

- `dist/codex/` — adaptación para OpenAI Codex
- `dist/opencode/` — adaptación para OpenCode

## Autor

Joan Llopis

## Licencia

MIT
