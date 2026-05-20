# Integración con LLMs: el plugin telos

## Objetivo

Este documento traduce la `Ingeniería del Propósito` a artefactos directamente utilizables con un LLM. El objetivo no es volver a explicar la teoría, sino reducir fricción de adopción: que un equipo pueda activar la metodología con comandos simples y con skills que aporten contexto operativo estable.

La integración se distribuye como el plugin **telos** para Claude Code, con adaptadores para OpenAI Codex y OpenCode.

**Estado actual:** v2.3.0 (mayo 2026).

## Decisión de diseño

La integración es **modular y por capas**:

- tres **skills de conocimiento** para el contexto operativo persistente (metodología, ciclo de vida, Git),
- cuatro **slash commands** para los momentos concretos del flujo,
- **plantillas** reutilizables como salida estándar,
- tres **modos de proyecto** que adaptan el cierre y la validación al contexto: `dev-team` (equipo + PRs), `dev-solo` (un dev + agente IA, sin PRs), `documental` (entregables no-código).

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
│       │   └── plugin.json            # Manifiesto: name "telos", v2.3.0
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

### Modos de proyecto

El modo se declara al inicio del proyecto en `docs/REQUIREMENTS.md` como cabecera obligatoria, junto con el almacén canónico y la política de push automático:

```md
**Modo del proyecto:** dev-team | dev-solo | documental
**Almacén canónico:** [git remote URL | Drive folder | Confluence space]
**Push automático:** yes | no
```

Cada skill lee el modo y adapta su comportamiento. El cierre de `/telos:check` es el caso más visible:

| Modo | `/telos:check` cierra con… | Validación |
|------|----------------------------|------------|
| `dev-team` | commit + push + PR con ficha embebida (plantilla `pr-template.md`) | tests / lint / build (fail-hard) |
| `dev-solo` | commit + push con ficha y revisión en el mensaje del commit (plantilla `commit-message-template.md`) | tests / lint / build (fail-hard) |
| `documental` | publicación al almacén canónico + registro de versión + notificación a stakeholders | plantilla aplicada + referencias íntegras + aprobaciones presentes |

En `dev-solo`, la disciplina del propósito sustituye al reviewer humano: el agente revisa contra la ficha antes del commit y la traza queda en el mensaje. En `documental`, el "deliverable" son documentos (en Drive, Confluence, local, etc.); las skills de Git pasan a opcionales.

### Plantillas

Las plantillas existen como activos copiables en `purpose-core/assets/`:

- `purpose-brief-template.md` — ficha de propósito
- `purpose-review-template.md` — revisión contra propósito
- `purpose-retro-template.md` — lección de propósito
- `pr-template.md` — cuerpo de PR en modo `dev-team` (ficha + revisión + cambios + validación)
- `commit-message-template.md` — cuerpo del commit en modo `dev-solo` (ficha + revisión embebidas)
- `adr-template.md` — Architecture Decision Record para fichas de sub-decisión técnica (alcance detectado automáticamente por `/telos:brief`)

Son utilizables en tickets, PRs, notas de diseño, retrospectivas y catálogos de ADRs.

## Flujo recomendado de uso

> Los **modos de proyecto** (dev-team / dev-solo / documental) y los **ritos de uso** descritos aquí son ortogonales. Un mismo proyecto en modo `dev-solo` puede usar el rito ligero para un bugfix puntual y el rito proyecto para una feature de varios días.

### Rito ligero (cambio puntual)

```
/telos:brief  →  (implementas)  →  /telos:check
```

### Rito proyecto (feature de varios días)

```
/telos:plan (incluye brief)  →  /telos:exec  →  /telos:check
```

`/telos:check` ofrece al final capturar una lección de propósito si el cambio dejó aprendizaje. Si la lección refina una ficha (capacidad o paraguas), produce un diff sobre `REQUIREMENTS.md` que pide aprobación antes de aplicarse. Si la tarea cerrada es la última de una fase del ROADMAP, dispara una **retro de fase** más profunda con diffs sobre el paraguas y la fase siguiente.

No es obligatorio usar todos los comandos. En cambios pequeños basta con `/telos:brief` y trabajar contra la ficha. En cambios ya hechos, se puede entrar directamente por `/telos:check`. Las operaciones Git (init/resume/sync) se piden en lenguaje natural a `telos-git-core`.

## Refuerzos sobre la metodología base

El plugin no es estático. Cada iteración añade refuerzos derivados de uso real. Los actuales (v2.3.0):

| Refuerzo | Qué hace | Skill afectada |
|---|---|---|
| **R1** — Promoción de lecciones | `/telos:check` clasifica obligatoriamente cada lección (local / capacidad / paraguas) y produce diff sobre `REQUIREMENTS.md` cuando corresponde | check |
| **R2** — Plantilla PR | El cuerpo del PR en `dev-team` lleva ficha + revisión por horizontes embebidas, visibles a reviewers humanos | check + asset `pr-template.md` |
| **R3** — ADRs sub-decisión | `/telos:brief` detecta alcance (paraguas / capacidad / sub-decisión / exploración) y propone `docs/adr/NNNN-*.md` para sub-decisiones técnicas | brief + asset `adr-template.md` |
| **R4** — Retro de fase | `/telos:check` detecta cierre de fase del ROADMAP y dispara retro más profunda con diffs sobre paraguas y siguiente fase | check |
| **R5** — Recitar horizontes | `/telos:exec` lista explícitamente qué horizontes toca cada tarea antes de empezar; sustituye la auto-revisión vaga | exec |
| **R6** — Validación horizonte por horizonte | `/telos:brief` valida horizonte por horizonte en alcances paraguas y capacidad, no en bloque | brief |
| **R7** — Origen temporal de docs | `/telos:plan` paso 0.5: cuando hay docs preexistentes, pregunta si reflejan el ahora, una visión futura o un mix, antes de destilar | plan |
| **R8** — Flujo no lineal | Las lecciones pueden refinar fichas durante `plan`, no solo durante `check`. Cada horizonte formulado es una micro-lección | purpose-core |
| **R9** — Push automático | `/telos:check` integra `git push` al cierre en modos `dev-*` con remote configurado. Override con `**Push automático:** no` en `REQUIREMENTS.md` | check |

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
