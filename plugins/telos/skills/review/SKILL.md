---
name: review
description: "Revisa una propuesta, diseño, diff o PR contra una ficha de propósito existente. Evalúa horizonte por horizonte."
user-invocable: true
---

# /telos:review

Revisa una propuesta, diseño, diff o PR contra una ficha de propósito existente.

## Contexto

Este comando forma parte del plugin `telos`. Se usa después de `/telos:brief`, cuando ya existe una ficha de propósito y hay una solución que evaluar. La base teórica y los criterios de rechazo están en la skill `purpose-core`.

## Comportamiento

### Si existe ficha de propósito y propuesta/código

1. Evalúa la solución horizonte por horizonte.
2. Para cada horizonte, indica si cumple o no cumple con una observación concreta.
3. Señala riesgos, trade-offs o violaciones explícitas.
4. Propón correcciones concretas si algún horizonte no se cumple.
5. Emite una decisión recomendada.

### Si existe propuesta pero no ficha de propósito

Pregunta al usuario:

> No he encontrado una ficha de propósito para este cambio. ¿Quieres que la construya primero con `/telos:brief`, o prefieres indicarme brevemente el propósito funcional, arquitectónico, de restricción y de autoría?

No revises en el vacío. Sin propósito explícito la revisión se reduce a impresiones.

### Si existe ficha pero no propuesta

Indica al usuario que necesitas algo concreto que revisar: código, diff, diseño, pseudocódigo o descripción de la solución propuesta.

## Formato de salida

Usa siempre esta estructura:

```md
## Revisión contra propósito

**Funcional:**
- Cumple / No cumple
- Observación: [detalle concreto]

**Arquitectónico:**
- Cumple / No cumple
- Observación: [detalle concreto]

**Restricción:**
- Cumple / No cumple
- Observación: [detalle concreto]

**Autoría:**
- Cumple / No cumple
- Observación: [detalle concreto]

**Riesgos o trade-offs:**
- [riesgo 1]
- [riesgo 2]

**Decisión recomendada:** Aprobar / Aprobar con observaciones / Pedir cambios

**Correcciones sugeridas:**
- [corrección 1]
- [corrección 2]
```

Si todos los horizontes se cumplen y no hay riesgos significativos, omite las secciones de riesgos y correcciones.

## Reglas

- No evalúes solo si el código "funciona". Evalúa contra los cuatro horizontes.
- Sé específico en las observaciones: señala líneas, decisiones de diseño o patrones concretos.
- Si la propuesta cumple la función pero rompe arquitectura o restricción, la decisión debe ser "Pedir cambios".
- No suavices violaciones. Un incumplimiento es un incumplimiento.
- Adapta el idioma al del usuario.

## Ejemplo

Ficha de propósito del cambio:
> - Funcional: el usuario personaliza su interfaz sin recargar.
> - Arquitectónico: preferencias extensibles sin alterar esquema principal.
> - Restricción: preferencia corrupta no bloquea login; degradación a valores por defecto.
> - Autoría: servicio expone con claridad qué preferencias existen.

Propuesta: un nuevo campo JSON en la tabla `users` con getter/setter directo.

Salida esperada:

```md
## Revisión contra propósito

**Funcional:**
- Cumple
- Observación: el campo JSON permite lectura y escritura sin recarga.

**Arquitectónico:**
- No cumple
- Observación: el campo vive directamente en la tabla `users`. Cada nueva preferencia requiere migración o cambios en la lógica de serialización. No hay separación entre almacenamiento y resolución.

**Restricción:**
- No cumple
- Observación: no hay validación del contenido JSON ni mecanismo de fallback. Un valor corrupto puede provocar un error no controlado en el flujo de login.

**Autoría:**
- No cumple
- Observación: las preferencias disponibles solo se descubren leyendo el código de serialización. No hay interfaz, enum o contrato que las haga visibles.

**Riesgos o trade-offs:**
- Acopla el ciclo de vida de las preferencias al de la tabla de usuarios.
- Ausencia de validación abre la puerta a estados inconsistentes.

**Decisión recomendada:** Pedir cambios

**Correcciones sugeridas:**
- Extraer preferencias a tabla o almacén separado con contrato explícito.
- Añadir validación con schema y fallback a valores por defecto.
- Exponer las preferencias disponibles mediante una interfaz o enum.
```

## Siguiente paso

Si la revisión detecta incumplimientos, el usuario corrige la propuesta y puede volver a ejecutar `/telos:review`. Cuando el cambio se cierre, usar `/telos:retro` para capturar aprendizaje.
