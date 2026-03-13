# Integración con LLMs: el plugin telos

## Objetivo

Este documento traduce la `Ingeniería del Propósito` a artefactos directamente utilizables con un LLM. El objetivo no es volver a explicar la teoría, sino reducir fricción de adopción: que un equipo pueda activar la metodología con comandos simples y con skills que aporten contexto operativo estable.

La integración se distribuye como el plugin **telos** para Claude Code, con adaptadores para OpenAI Codex y OpenCode.

## Decisión de diseño

La integración es **modular y por capas**:

- tres **skills de conocimiento** para el contexto operativo persistente (metodología, ciclo de vida, Git),
- nueve **slash commands** para los momentos concretos del flujo,
- y **plantillas** reutilizables como salida estándar.

Esto evita dos errores comunes:

- convertir una skill en un prompt gigante y rígido,
- o depender solo de comandos sueltos sin una filosofía operativa consistente.

## Arquitectura del repositorio

```text
ingenieria_proposito/                  # Raíz del repo (marketplace)
├── .claude-plugin/
│   └── marketplace.json               # Catálogo de plugins
├── plugins/
│   └── telos/                         # Plugin telos
│       ├── .claude-plugin/
│       │   └── plugin.json            # Manifiesto: name "telos", v1.0.0
│       ├── skills/
│   ├── purpose-core/            # Metodología de propósito (no invocable)
│   │   ├── SKILL.md
│   │   ├── assets/              # Plantillas de ficha, review y retro
│   │   └── references/          # Modelo operativo, anti-patrones
│   ├── dev-core/                # Ciclo de vida de proyecto (no invocable)
│   │   └── SKILL.md
│   ├── git-core/                # Flujo corporativo de Git (no invocable)
│   │   ├── SKILL.md
│   │   ├── references/          # Branching, commits, PRs, releases
│   │   └── scripts/             # Validaciones automáticas
│       ├── brief/                     # /telos:brief
│       ├── review/                    # /telos:review
│       ├── retro/                     # /telos:retro
│       ├── init/                      # /telos:init
│       ├── resume/                    # /telos:resume
│       ├── plan/                      # /telos:plan
│       ├── exec/                      # /telos:exec
│       ├── check/                     # /telos:check
│       └── sync/                      # /telos:sync
├── dist/                              # Adaptadores multiplataforma
│   ├── codex/
│   └── opencode/
├── docs/                              # Documentación del proyecto
└── README.md
```

## Qué vive en cada capa

### Skills de conocimiento (no invocables)

| Skill | Contenido |
|-------|-----------|
| `purpose-core` | Principios, horizontes, flujo, criterios de rechazo, anti-patrones |
| `dev-core` | Fases del ciclo de vida (init → sync), reglas globales, integración con propósito |
| `git-core` | Git Flow, branch naming con Jira, PRs, SemVer, seguridad |

Estas skills se cargan automáticamente cuando un comando las referencia. No aparecen como comandos para el usuario.

### Slash commands

#### Metodología de propósito

| Comando | Cuándo usarlo |
|---------|---------------|
| `/telos:brief` | Al arrancar una tarea o convertir una petición difusa en ficha clara |
| `/telos:review` | Al revisar una propuesta, diff o PR contra el propósito |
| `/telos:retro` | Al cerrar un cambio y capturar aprendizaje reutilizable |

#### Ciclo de vida de proyecto

| Comando | Cuándo usarlo |
|---------|---------------|
| `/telos:init` | Al iniciar un proyecto desde cero |
| `/telos:resume` | Al retomar un proyecto existente |
| `/telos:plan` | Al planificar: diseño, requisitos, tareas, roadmap |
| `/telos:exec` | Al implementar el plan aprobado |
| `/telos:check` | Al validar + commit + PR |
| `/telos:sync` | Al sincronizar cambios remotos |

### Plantillas

Las plantillas existen como activos copiables en `purpose-core/assets/`:

- `purpose-brief-template.md` — ficha de propósito
- `purpose-review-template.md` — revisión contra propósito
- `purpose-retro-template.md` — lección de propósito

Son utilizables en tickets, PRs, notas de diseño y retrospectivas.

## Flujo recomendado de uso

### Flujo de propósito (independiente)

```
/telos:brief  →  (implementación)  →  /telos:review  →  /telos:retro
```

### Flujo de proyecto completo

```
/telos:init o /telos:resume
    → /telos:plan (incluye brief automáticamente)
    → /telos:exec
    → /telos:check (incluye review + commit + PR)
    → /telos:sync
```

No es obligatorio usar todos los comandos. En cambios pequeños basta con `/telos:brief`. En cambios ya hechos, se puede entrar directamente por `/telos:review`.

## Distribución multiplataforma

El repositorio es un **marketplace** de Claude Code. El contenido canónico del plugin vive en `plugins/telos/`. Para otras plataformas se generan adaptadores en `dist/`.

| Plataforma | Directorio | Formato |
|------------|------------|---------|
| Claude Code | `plugins/telos/` | Plugin nativo con `plugin.json` + `skills/` |
| OpenAI Codex | `dist/codex/` | System prompts combinados como `.md` |
| OpenCode | `dist/opencode/` | Skills + slash commands en estructura OpenCode |

### Instalación en Claude Code

```bash
# Añadir el marketplace
/plugin marketplace add owner/ingenieria-del-proposito

# Instalar el plugin
/plugin install telos
```

## Beneficios de este modelo

- Las skills aportan consistencia entre tareas y personas.
- Los comandos reducen fricción de entrada.
- Las plantillas hacen que el resultado sea copiable a tickets, PRs y documentos.
- La distribución multiplataforma permite que todo el equipo use la misma metodología independientemente de su herramienta.

## Límites

- Las skills no reemplazan criterio humano.
- Los slash commands no corrigen un cambio mal entendido.
- Si el equipo no usa la ficha de propósito en reviews, la integración se vuelve cosmética.

## Siguiente evolución

1. Publicar en el marketplace oficial de Anthropic cuando el plugin sea estable.
2. Generar distribuciones completas para Codex y OpenCode.
3. Insertar automáticamente la ficha en PRs o tickets.
4. Medir adopción, retrabajo y claridad de revisión.
5. Usar esos datos para mejorar las skills y los comandos.
