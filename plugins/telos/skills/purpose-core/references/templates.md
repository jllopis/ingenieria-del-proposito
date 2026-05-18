# Templates

Índice de plantillas disponibles. El contenido canónico vive en `assets/`; este fichero sirve como referencia rápida.

## Plantillas disponibles

| Plantilla | Fichero | Cuándo usarla |
|-----------|---------|---------------|
| Ficha de propósito | `assets/purpose-brief-template.md` | Al arrancar un cambio o convertir una petición en ficha clara |
| Revisión contra propósito | `assets/purpose-review-template.md` | Al evaluar una propuesta, diff, PR o documento contra la ficha |
| Lección de propósito | `assets/purpose-retro-template.md` | Al cerrar un cambio y capturar aprendizaje reutilizable |
| Cuerpo de PR | `assets/pr-template.md` | Como descripción de PR en modo `dev-team` (ficha + revisión + cambios + validación) |
| Mensaje de commit | `assets/commit-message-template.md` | Como cuerpo del commit en modo `dev-solo` (ficha + revisión embebidas en el commit) |
| ADR | `assets/adr-template.md` | Para fichas de sub-decisión técnica (qué LLM, qué retry, qué shape de prompt) — alcance ADR detectado por `/telos:brief` |

## Correspondencia con comandos y modos

| Plantilla | Comando / fase | Modos donde aplica |
|-----------|----------------|---------------------|
| Ficha de propósito | `/telos:brief` (paraguas, capacidad) | todos |
| Revisión contra propósito | embebida en `/telos:check` paso 2 | todos |
| Lección de propósito | embebida en `/telos:check` paso 7 | todos |
| Cuerpo de PR | `/telos:check` paso 6 | `dev-team` |
| Mensaje de commit | `/telos:check` paso 6 | `dev-solo` (y `documental` si el almacén canónico es git) |
| ADR | `/telos:brief` con alcance sub-decisión | todos |
