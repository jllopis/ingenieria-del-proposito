# Integración con LLMs: el plugin telos

## Objetivo

Este documento traduce la `Ingeniería del Propósito` a artefactos directamente utilizables con un LLM. El objetivo no es volver a explicar la teoría, sino reducir fricción de adopción: que un equipo pueda activar la metodología con comandos simples y con skills que aporten contexto operativo estable.

La integración se distribuye como el plugin **telos** para Claude Code, con adaptadores para OpenAI Codex y OpenCode.

## Decisión de diseño

La integración es **modular y por capas**:

- tres **skills de conocimiento** para el contexto operativo persistente (metodología, ciclo de vida, Git),
- cuatro **slash commands** para los momentos concretos del flujo,
- y **plantillas** reutilizables como salida estándar.

Esto evita tres errores comunes:

- convertir una skill en un prompt gigante y rígido,
- depender solo de comandos sueltos sin una filosofía operativa consistente,
- inflar la superficie de comandos con envoltorios que duplican la metodología (versiones anteriores tenían 9 comandos con review/retro/init/resume/sync separados; ahora review y retro están embebidos en `check`, y init/resume/sync los aplica `telos-git-core` en lenguaje natural).

## Arquitectura del repositorio

```text
ingenieria_proposito/                  # Raíz del repo (marketplace)
├── .claude-plugin/
│   └── marketplace.json               # Catálogo de plugins
├── plugins/
│   └── telos/                         # Plugin telos
│       ├── .claude-plugin/
│       │   └── plugin.json            # Manifiesto: name "telos", v2.0.0
│       └── skills/
│           ├── purpose-core/          # Metodología (no invocable, name: telos-purpose-core)
│           │   ├── SKILL.md
│           │   ├── assets/            # Plantillas de ficha, review y retro
│           │   └── references/        # Modelo operativo, anti-patrones
│           ├── dev-core/              # Ciclo de vida en 3 fases (no invocable, name: telos-dev-core)
│           │   └── SKILL.md
│           ├── git-core/              # Flujo Git + recetas init/resume/sync (no invocable, name: telos-git-core)
│           │   ├── SKILL.md
│           │   ├── references/        # Branching, commits, PRs, releases
│           │   └── scripts/           # Validaciones automáticas
│           ├── brief/                 # /telos:brief (name: telos-brief)
│           ├── plan/                  # /telos:plan
│           ├── exec/                  # /telos:exec
│           └── check/                 # /telos:check (absorbe review + retro)
├── .agents/skills/                    # Symlinks prefijados para Agent Skills
├── dist/                              # Adaptadores multiplataforma
│   └── opencode/
├── docs/                              # Documentación del proyecto
└── README.md
```

## Qué vive en cada capa

### Skills de conocimiento (no invocables)

| Skill | Contenido |
|-------|-----------|
| `telos-purpose-core` | Principios, horizontes, flujo, criterios de rechazo, anti-patrones |
| `telos-dev-core` | Tres fases del ciclo de vida (plan → exec → check), reglas globales, integración con propósito |
| `telos-git-core` | Git Flow, branch naming con Jira, PRs, SemVer, seguridad + recetas de init/resume/sync |

Estas skills se cargan automáticamente cuando un comando las referencia. No aparecen como comandos para el usuario.

### Slash commands

| Comando | Cuándo usarlo |
|---------|---------------|
| `/telos:brief` | Atómico: arrancar un cambio convirtiendo una petición difusa en ficha clara con los 4 horizontes |
| `/telos:plan` | Modo proyecto: incluye brief + genera DESIGN/REQUIREMENTS/TASKS/ROADMAP |
| `/telos:exec` | Implementar el plan o la ficha, con auto-revisión por horizontes |
| `/telos:check` | Cerrar: revisión por horizontes + tests + commit + PR + lección opcional |

> **Nota:** El autocompletado del CLI muestra la forma corta (`/brief`, `/plan`, `/exec`, `/check`). Ambas formas son válidas.

**Operaciones Git puras** (inicializar repo, retomar uno existente, sincronizar con remoto, branching, PRs, releases) no tienen comando propio: las aplica la skill `telos-git-core` cuando el usuario las pide en lenguaje natural ("inicializa el proyecto en Go", "retoma este repo", "sincroniza con develop").

### Plantillas

Las plantillas existen como activos copiables en `purpose-core/assets/`:

- `purpose-brief-template.md` — ficha de propósito
- `purpose-review-template.md` — revisión contra propósito
- `purpose-retro-template.md` — lección de propósito

Son utilizables en tickets, PRs, notas de diseño y retrospectivas.

## Flujo recomendado de uso

### Modo ligero (cambio puntual)

```
/telos:brief  →  (implementas)  →  /telos:check
```

### Modo proyecto (feature de varios días)

```
/telos:plan (incluye brief)  →  /telos:exec  →  /telos:check
```

`/telos:check` ofrece al final capturar una lección de propósito si el cambio dejó aprendizaje.

No es obligatorio usar todos los comandos. En cambios pequeños basta con `/telos:brief` y trabajar contra la ficha. En cambios ya hechos, se puede entrar directamente por `/telos:check`. Las operaciones Git (init/resume/sync) se piden en lenguaje natural a `telos-git-core`.

## Distribución multiplataforma

El repositorio es un **marketplace** de Claude Code y un repositorio de **Agent Skills** (estándar abierto en agentskills.io). El contenido canónico vive en `plugins/telos/`; el directorio `.agents/skills/` contiene symlinks para compatibilidad con el estándar.

| Plataforma | Mecanismo | Instalación |
|------------|-----------|-------------|
| Claude Code | Marketplace plugin | `/plugin marketplace add jllopis/ingenieria-del-proposito` → `/plugin install telos` |
| OpenAI Codex | Agent Skills | `$skill-installer` con URL del repo |
| Cursor, VS Code Copilot, Gemini CLI, etc. | Agent Skills | Clonar el repo o copiar `.agents/skills/` |
| OpenCode | Adaptador en `dist/opencode/` | Copiar skills y commands al directorio de configuración |

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
