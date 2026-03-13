# Adaptador OpenCode

Slash commands y skills para OpenCode generados desde `plugins/telos/`.

## Uso

Copia la estructura a tu directorio de configuración de OpenCode:

```
~/.config/opencode/
├── skills/
│   ├── purpose-core/SKILL.md
│   ├── dev-core/SKILL.md
│   └── git-core/SKILL.md
└── command/
    ├── telos-brief.md
    ├── telos-review.md
    ├── telos-retro.md
    ├── telos-init.md
    ├── telos-resume.md
    ├── telos-plan.md
    ├── telos-exec.md
    ├── telos-check.md
    └── telos-sync.md
```

## Nota sobre nombres

OpenCode no soporta namespace con `:`. Los comandos usan guión: `/telos-brief`, `/dev-plan`, etc.

## Nota

Estos ficheros se generan desde el plugin canónico. No editar directamente.
