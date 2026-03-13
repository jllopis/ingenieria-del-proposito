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

| Comando | Descripción |
|---------|-------------|
| `/telos:brief` | Convierte una petición en una ficha de propósito |
| `/telos:review` | Revisa una propuesta contra la ficha de propósito |
| `/telos:retro` | Captura aprendizaje reutilizable tras cerrar un cambio |
| `/telos:init` | Iniciar un proyecto nuevo |
| `/telos:resume` | Retomar un proyecto existente |
| `/telos:plan` | Planificar: diseño, requisitos, tareas, roadmap |
| `/telos:exec` | Ejecutar el plan guiado por el propósito |
| `/telos:check` | Validar contra propósito + tests + commit + PR |
| `/telos:sync` | Sincronizar cambios remotos |

### Instalación

```bash
# Añadir el marketplace
/plugin marketplace add owner/ingenieria-del-proposito

# Instalar el plugin
/plugin install telos
```

### Flujo típico

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
