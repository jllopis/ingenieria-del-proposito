# Adaptador OpenCode

Slash commands y skills para OpenCode generados desde `plugins/telos/`.

## Uso

Copia la estructura a tu directorio de configuración de OpenCode:

```
~/.config/opencode/
├── skills/
│   ├── telos-purpose-core/SKILL.md
│   ├── telos-dev-core/SKILL.md
│   └── telos-git-core/SKILL.md
└── command/
    ├── telos-brief.md
    ├── telos-plan.md
    ├── telos-exec.md
    └── telos-check.md
```

## Nota sobre nombres

OpenCode no soporta namespace con `:`. Los comandos usan guión: `/telos-brief`, `/telos-plan`, etc. Las skills mantienen el prefijo `telos-` por la misma razón: sin namespace, los nombres cortos chocarían con built-ins.

## Nota

Estos ficheros se generan desde el plugin canónico. No editar directamente.
