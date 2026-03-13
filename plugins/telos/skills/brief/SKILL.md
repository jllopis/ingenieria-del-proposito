---
name: brief
description: "Convierte una petición o tarea en una ficha de propósito breve y accionable con los cuatro horizontes (funcional, arquitectónico, restricción, autoría)."
user-invocable: true
---

# /telos:brief

Convierte una petición o tarea en una ficha de propósito breve y accionable.

## Contexto

Este comando forma parte del plugin `telos`. La ficha de propósito es el artefacto central de la Ingeniería del Propósito: define el para qué de un cambio antes de aportar contexto o pedir código. La base teórica y los principios están en la skill `purpose-core`.

## Comportamiento

### Si el usuario proporciona una descripción del cambio

1. Resume el cambio en una frase clara.
2. Redacta los cuatro horizontes (funcional, arquitectónico, restricción, autoría) a partir de lo que el usuario ha descrito.
3. Identifica el contexto mínimo necesario para ejecutar.
4. Si algún horizonte queda ambiguo o incompleto, señálalo con una pregunta concreta al final.
5. Devuelve la ficha completa.

### Si el usuario no proporciona descripción

Pregunta brevemente:

> ¿Qué cambio quieres definir? Describe en una o dos frases qué necesitas hacer.

No pidas más de lo necesario para arrancar.

### Si el usuario proporciona algo parcial

Completa lo que puedas inferir razonablemente y marca con `[pendiente]` lo que necesite confirmación del usuario.

## Formato de salida

Usa siempre esta estructura:

```md
## Ficha de propósito

**Cambio:** [una frase]

**Funcional:** [qué capacidad nueva aparece o qué problema desaparece]

**Arquitectónico:** [qué principio de diseño debe sobrevivir]

**Restricción:** [qué no puede ocurrir]

**Autoría:** [cómo debe leerse o mantenerse el resultado]

**Contexto mínimo:**
- [artefacto o dato imprescindible]
- [restricción técnica relevante]
```

Si quedan dudas, añade al final:

```md
**Pendiente de aclarar:**
- [pregunta concreta 1]
- [pregunta concreta 2]
```

## Reglas

- No pidas archivos ni contexto antes de formular el propósito.
- No escribas propósitos vagos. Si no puedes ser concreto, pregunta.
- Prioriza brevedad: la ficha debe completarse en menos de dos minutos.
- Adapta el idioma al del usuario.

## Ejemplo

Entrada del usuario:
> Necesitamos añadir notificaciones push al módulo de pedidos

Salida esperada:

```md
## Ficha de propósito

**Cambio:** Añadir notificaciones push al módulo de pedidos.

**Funcional:** El usuario debe recibir una notificación push en tiempo real cuando el estado de su pedido cambie, sin necesidad de consultar la app manualmente.

**Arquitectónico:** El envío de notificaciones debe estar desacoplado del flujo de cambio de estado del pedido, para que un fallo en el servicio de push no bloquee ni retrase la transición del pedido.

**Restricción:** No enviar notificaciones duplicadas por el mismo evento. No exponer datos del pedido (dirección, importe) en el payload de la notificación visible.

**Autoría:** El punto de emisión del evento debe ser explícito y fácil de localizar. El servicio de notificaciones debe poder sustituirse sin tocar la lógica de pedidos.

**Contexto mínimo:**
- Interfaz actual del servicio de pedidos (cambios de estado)
- Proveedor de push actual o previsto
- Restricciones de frecuencia o throttling existentes

**Pendiente de aclarar:**
- ¿Se notifican todos los cambios de estado o solo algunos (ej. enviado, entregado)?
- ¿Hay requisito de persistencia para notificaciones no entregadas?
```

## Persistencia

Una vez validada la ficha, **sugiere al usuario guardarla en `docs/REQUIREMENTS.md`** dentro de la sección de fichas de propósito. La ficha actúa como user story + criterios de aceptación, y debe sobrevivir a la conversación para que `/telos:exec` y `/telos:check` puedan consultarla.

Si el usuario prefiere no persistirla (cambio pequeño, exploración), respeta su decisión.

## Siguiente paso

Una vez guardada la ficha, el usuario puede trabajar con ella como base para implementación. Cuando haya una propuesta o código, usar `/telos:review` para evaluarla contra los horizontes.
