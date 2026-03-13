---
name: retro
description: "Convierte el resultado de un cambio completado en aprendizaje reutilizable (lección de propósito)."
user-invocable: true
---

# /telos:retro

Convierte el resultado de un cambio completado en aprendizaje reutilizable para el equipo.

## Contexto

Este comando forma parte del plugin `telos`. Se usa después de cerrar un cambio, cuando el equipo quiere capturar qué funcionó, qué falló y qué podría mejorar en la ficha de propósito para trabajos futuros. La base teórica está en la skill `purpose-core`.

## Comportamiento

### Si el usuario describe qué pasó con el cambio

1. Resume el cambio en una frase.
2. Identifica qué funcionó bien gracias a la ficha de propósito.
3. Identifica qué falló o no salió como se esperaba.
4. Detecta qué faltó en la ficha original (horizontes vagos, restricciones omitidas, contexto insuficiente).
5. Extrae un patrón reutilizable: una regla, plantilla parcial o criterio que pueda aplicarse a cambios similares en el futuro.

### Si el usuario no da suficiente contexto sobre el resultado

Pregunta brevemente:

> ¿Cómo fue el resultado del cambio? Cuéntame qué salió bien, qué dio problemas y si la ficha de propósito ayudó o se quedó corta.

### Si no hubo ficha de propósito previa

Igualmente es útil hacer la retro. Indica que la lección más inmediata puede ser precisamente que habría convenido tener una ficha, y ayuda al usuario a formular el patrón retroactivamente.

## Formato de salida

Usa siempre esta estructura:

```md
## Lección de propósito

**Cambio:** [una frase]

**Qué funcionó:**
- [aspecto positivo concreto vinculado a la ficha o al proceso]

**Qué falló:**
- [problema concreto y su causa probable]

**Qué faltó en la ficha:**
- [horizonte que estaba vago, restricción omitida, contexto que habría ayudado]

**Patrón reutilizable:**
- [regla o criterio extraído que pueda aplicarse a cambios similares]
```

## Reglas

- No hagas retros genéricas. Vincula cada punto a algo concreto del cambio.
- El patrón reutilizable debe ser específico y accionable, no una reflexión abstracta.
- Si no hay aprendizaje significativo, di que el cambio fue limpio y no fuerces hallazgos.
- Adapta el idioma al del usuario.

## Ejemplo

Entrada del usuario:
> Añadimos las notificaciones push. La ficha decía desacoplar del flujo de pedidos, y lo hicimos con eventos. Pero nos olvidamos de poner throttling y un usuario recibió 14 notificaciones en un minuto cuando su pedido pasó por varios estados rápido. Lo arreglamos con un debounce pero fue un hotfix.

Salida esperada:

```md
## Lección de propósito

**Cambio:** Añadir notificaciones push al módulo de pedidos.

**Qué funcionó:**
- El desacoplamiento por eventos permitió que el fallo de push no afectara al flujo de pedidos.
- La arquitectura basada en eventos facilitó el hotfix posterior sin tocar lógica de negocio.

**Qué falló:**
- Un usuario recibió 14 notificaciones en un minuto por transiciones rápidas de estado. Causa: ausencia de throttling o debounce en el consumidor de eventos.

**Qué faltó en la ficha:**
- El horizonte de restricción no contemplaba límites de frecuencia de envío. La restricción "no duplicar notificaciones por el mismo evento" no cubría ráfagas de eventos distintos en ventana corta.

**Patrón reutilizable:**
- En sistemas basados en eventos orientados al usuario, incluir siempre en el horizonte de restricción un límite de frecuencia por destinatario (ej: máximo N notificaciones por usuario en ventana de T minutos). Aplica a notificaciones, emails, webhooks y cualquier canal de salida con impacto directo en UX.
```

## Siguiente paso

Los patrones reutilizables extraídos pueden incorporarse a las fichas de propósito de futuros cambios similares, enriqueciendo progresivamente los criterios del equipo.
