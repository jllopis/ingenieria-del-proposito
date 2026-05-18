# telos — Ingeniería del Propósito

Plugin para asistentes de código que aplica **Ingeniería del Propósito**: trabaja con LLMs desde propósito explícito en lugar de partir de contexto abundante.

## Comandos

Cuatro comandos. La regla mental: `brief → exec → check`, y si el cambio es grande, lo envuelves en `plan` al principio.

| Comando | Cuándo usarlo |
|---------|---------------|
| `/telos:brief` | Atómico: convertir una petición en ficha de propósito con los 4 horizontes. Detecta alcance (paraguas / capacidad / sub-decisión ADR / exploración) y propone destino de persistencia. |
| `/telos:plan` | Modo proyecto: pregunta por modo, genera ficha-paraguas + fichas de capacidad + `docs/DESIGN.md`, `REQUIREMENTS.md`, `TASKS.md`, `ROADMAP.md` adaptados al modo |
| `/telos:exec` | Implementar guiado por la ficha. Recita explícitamente los horizontes que toca cada tarea antes de empezar, y auto-revisa al cerrar. |
| `/telos:check` | Cierre: revisión por horizontes + validación (técnica o documental) + cierre adaptado al modo (PR / commit con ficha / publicación) + lección clasificada (local / capacidad / paraguas) con diff propuesto a las fichas. Detecta cierre de fase y lanza retro de fase. |

Las operaciones Git puras (inicializar repo, retomar un repo existente, sincronizar con remoto, branching, PRs, releases) las cubre la skill `telos-git-core` cuando se las pides en lenguaje natural ("inicializa el proyecto", "retoma este repo", "sincroniza con develop"). No tienen comando propio.

## Modos de proyecto

| Modo | Para qué | Cierre |
|------|----------|--------|
| `dev-team` | Equipo de desarrollo con Git Flow y reviewers | commit + PR con ficha + revisión embebidas |
| `dev-solo` | Un desarrollador + agente IA, sin reviewers humanos | commit semántico con ficha + revisión **en el mensaje** (sustituye al PR) |
| `documental` | Entregables no-código: políticas, procedimientos, registros (almacén local, Drive, Confluence...) | publicación al almacén canónico + aprobaciones + notificación a stakeholders |

El modo se declara al inicio del proyecto en `docs/REQUIREMENTS.md` y las skills lo leen para adaptar su comportamiento.

## Skills de conocimiento (no invocables directamente)

Todas las skills están prefijadas con `telos-` para evitar colisión con skills built-in u otros plugins.

| Skill | Descripción |
|-------|-------------|
| `telos-purpose-core` | Principios, horizontes, anti-patrones y flujo de la Ingeniería del Propósito. Plantillas en `assets/`. |
| `telos-dev-core` | Reglas del ciclo de vida en 3 fases (plan → exec → check) |
| `telos-git-core` | Flujo corporativo de Git: Git Flow, branch naming, PRs, SemVer + recetas de init/resume/sync |

## Los cuatro horizontes

Todo cambio debe articular:

- **Funcional**: qué problema desaparece o qué capacidad nueva aparece.
- **Arquitectónico**: qué principio de diseño debe protegerse.
- **Restricción**: qué no puede ocurrir bajo ninguna circunstancia.
- **Autoría**: cómo debe leerse, mantenerse o evolucionar el resultado.

## Flujo típico

Modo ligero (un cambio puntual):

```
/telos:brief  →  (implementas)  →  /telos:check
```

Modo proyecto (feature de varios días):

```
/telos:plan (incluye brief)  →  /telos:exec  →  /telos:check
```

`/telos:check` ofrece al final capturar una lección de propósito si el cambio dejó aprendizaje.

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
│   ├── purpose-core/            # Conocimiento base (no invocable)
│   │   ├── SKILL.md
│   │   ├── assets/              # Plantillas de ficha, review y retro
│   │   └── references/          # Modelo operativo, anti-patrones
│   ├── dev-core/                # Ciclo de vida en 3 fases (no invocable)
│   │   └── SKILL.md
│   ├── git-core/                # Flujo Git + recetas init/resume/sync (no invocable)
│   │   ├── SKILL.md
│   │   ├── references/          # Branching, commits, PRs, releases
│   │   └── scripts/             # Validaciones automáticas
│   ├── brief/                   # /telos:brief
│   ├── plan/                    # /telos:plan
│   ├── exec/                    # /telos:exec
│   └── check/                   # /telos:check
└── README.md
```

> Los nombres de skill en el frontmatter están prefijados (`telos-brief`, `telos-plan`, etc.). Las carpetas conservan el nombre corto porque es lo que define el comando `/telos:<carpeta>`.

## Distribución multiplataforma

Este plugin es el contenido canónico. Para otras plataformas, se generan adaptadores en `../../dist/`:

- `dist/opencode/` — adaptación para OpenCode

## Autor

Joan Llopis

## Licencia

MIT
