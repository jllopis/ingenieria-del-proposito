# Ingeniería del Propósito

Trabaja con LLMs desde **propósito explícito** en lugar de partir de contexto abundante.

La Ingeniería del Propósito propone que el centro del trabajo con IA no sea la acumulación de información, sino la formulación explícita del fin que debe gobernar la solución. Todo cambio se articula en cuatro horizontes:

- **Funcional**: qué problema desaparece o qué capacidad nueva aparece.
- **Arquitectónico**: qué principio de diseño debe protegerse.
- **Restricción**: qué no puede ocurrir bajo ninguna circunstancia.
- **Autoría**: cómo debe leerse, mantenerse o evolucionar el resultado.

> Propósito claro + contexto mínimo + evaluación por horizontes = colaboración útil con LLMs

## Plugin telos

Este repositorio es un **marketplace** de Claude Code que distribuye el plugin **telos**: skills y comandos para aplicar la metodología directamente en tu flujo de trabajo.

### Comandos disponibles

Cuatro comandos. Regla mental: `brief → exec → check`, y si el cambio es grande lo envuelves en `plan` al principio.

| Comando | Descripción |
|---------|-------------|
| `/telos:brief` | Convierte una petición en una ficha de propósito (átomo de la metodología) |
| `/telos:plan` | Modo proyecto: brief + diseño + requisitos + tareas + roadmap |
| `/telos:exec` | Implementar guiado por la ficha, con auto-revisión por horizontes |
| `/telos:check` | Revisión por horizontes + tests + commit + PR + lección opcional |

Las operaciones Git puras (inicializar repo, retomar uno existente, sincronizar con remoto, branching, PRs, releases) las aplica la skill `telos-git-core` en lenguaje natural ("inicializa el proyecto", "sincroniza con develop"). No tienen comando propio.

> **Nota:** El autocompletado del CLI muestra la forma corta (`/brief`, `/plan`, `/exec`, `/check`). Ambas formas son válidas.

### Instalación

```bash
# Añadir el marketplace
/plugin marketplace add jllopis/ingenieria-del-proposito

# Instalar el plugin
/plugin install telos

# Recargar en la sesión actual
/reload-plugins
```

### Actualización

```bash
# Refrescar el marketplace
/plugin marketplace update ingenieria-del-proposito

# Actualizar el plugin
/plugin update telos@ingenieria-del-proposito

# Recargar en la sesión actual
/reload-plugins
```

Para actualizaciones automáticas: `/plugin` → pestaña **Marketplaces** → seleccionar `ingenieria-del-proposito` → **Enable auto-update**.

### Flujo típico

Modo ligero (cambio puntual):

```
/telos:brief  →  (implementas)  →  /telos:check
```

Modo proyecto (feature de varios días):

```
/telos:plan (incluye brief)  →  /telos:exec  →  /telos:check
```

`/telos:check` ofrece al final capturar una lección de propósito si el cambio dejó aprendizaje. Para operaciones Git (init repo, sync, branching) pide a `telos-git-core` en lenguaje natural.

## Documentación

- [Ingeniería del Propósito](docs/Ingenieria-del-Proposito.md) — documento conceptual completo
- [Manifiesto](docs/Manifiesto-Ingenieria-del-Proposito.md) — tesis central en formato breve
- [Guía práctica para equipos](docs/Guia-Practica-Equipos-Ingenieria-del-Proposito.md) — procesos, plantillas y criterios
- [Integración con LLMs](docs/Integracion-LLMs-Ingenieria-del-Proposito.md) — arquitectura del plugin
- [Infografía](docs/Infografia-Ingenieria-del-Proposito.md) — resumen visual

## Origen

Este enfoque nace del artículo [La Tiranía del Contexto](https://disidencia.incontrolada.com/2026/02/28/la-tirania-del-contexto/).

## Otras plataformas

El repositorio incluye `.agents/skills/` compatible con el estándar abierto [Agent Skills](https://agentskills.io). Funciona con:

- **OpenAI Codex** — instalar vía `$skill-installer` con URL del repo
- **Cursor, VS Code Copilot, Gemini CLI**, y cualquier herramienta compatible — clonar el repo
- **OpenCode** — adaptador en `dist/opencode/`

## Autor

Joan Llopis

## Licencia

MIT
