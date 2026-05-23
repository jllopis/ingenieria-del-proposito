---
name: telos-brief
description: "Activar SOLO si el usuario escribe `/telos:brief` o pide explícitamente una ficha de propósito de Ingeniería del Propósito con los cuatro horizontes (funcional, arquitectónico, restricción, autoría). Aplica a cambios de código, decisiones de arquitectura (ADRs) o entregables documentales (políticas, procedimientos, registros). Antes de redactar, **cuestiona el input** si hay 1-3 ambigüedades que materialmente cambiarían la ficha (no cuestiona por cuestionar). Detecta alcance (paraguas / capacidad / sub-decisión / exploración) y propone destino de persistencia. NO activar para resúmenes genéricos, briefings de reunión, descripciones de tareas, ni cualquier petición que no busque aplicar la metodología."
user-invocable: true
---

# /telos:brief

Convierte una petición o tarea en una ficha de propósito breve y accionable.

## Contexto

Este comando forma parte del plugin `telos`. La ficha de propósito es el artefacto central de la Ingeniería del Propósito: define el para qué de un cambio antes de aportar contexto o pedir código. La base teórica y los principios están en la skill `telos-purpose-core`.

## Comportamiento

### Cuestionar antes de fichar (R10)

Antes de redactar los horizontes, revisa el input del usuario para identificar **1-3 cuestiones que materialmente cambiarían la ficha resultante** si se respondiesen primero. Si las encuentras, pregúntalas y espera respuesta antes de redactar. Si no, abstente y procede directamente. **No cuestiones por cuestionar**: la abstención también es disciplina.

**Criterios para cuestionar:**

- Ambigüedad estructural que afectaría más de un horizonte (no la de un detalle aislado).
- Supuestos ocultos que conviene explicitar antes de fijarlos en Restricción o Autoría.
- Restricciones probables que el usuario no mencionó pero serían críticas (seguridad, retención, cumplimiento, coste, latencia).
- Alcance que conflua varios cambios distintos en una sola ficha (separar antes de fichar).
- Términos clave subespecificados ("rápido", "seguro", "configurable", "robusto", "mínimo") que necesitan métrica o criterio operacional para que un horizonte se pueda evaluar.
- Pista de modo de proyecto incongruente con el contexto (p. ej. el usuario menciona equipo + reviewers pero el repo es `dev-solo`).

**Criterios para abstenerse:**

- El input ya es suficientemente claro y las preguntas serían cosméticas.
- Los detalles preguntados pertenecen a `exec` (cómo implementar), no a propósito (qué/por qué/restricciones).
- La duda razonable cabe como `[pendiente de aclarar]` al final de la ficha sin bloquear el primer borrador.
- Necesitarías más de 3 preguntas: significa que el input es demasiado ambiguo y conviene pedir al usuario que reformule el alcance, no atomizar.

**Formato sugerido cuando cuestionas:**

```md
Antes de fichar, déjame asegurar el alcance:
- [pregunta concreta 1]
- [pregunta concreta 2]
Cuando confirmes, redacto los 4 horizontes.
```

Regla operativa: **cuestionar mejora la ficha o no se hace**. Preguntar por preguntar añade fricción sin valor.

### Si el usuario proporciona una descripción del cambio

1. Aplica "Cuestionar antes de fichar". Si surgen preguntas materiales, plantéalas y espera. Si no, procede.
2. Resume el cambio en una frase clara.
3. Redacta los cuatro horizontes (funcional, arquitectónico, restricción, autoría) a partir de lo que el usuario ha descrito.
4. Identifica el contexto mínimo necesario para ejecutar.
5. Si algún horizonte queda ambiguo o incompleto pero no merecía cuestionar antes, señálalo con `[pendiente]` al final.
6. Devuelve la ficha completa.

### Si destilas la ficha de docs preexistentes

Si la ficha la estás extrayendo de documentos ya existentes (diseño funcional, especificaciones, propuestas), añade un paso previo:

0. **Pregunta el origen temporal**: ¿estos docs reflejan el proyecto actual, una visión futura/aspiracional, o un mix? Sin esa distinción, los horizontes heredan restricciones que pueden no aplicar al alcance comprometido ahora. Un doc llamado "v0.1" o "propuesta" suele describir un ideal, no el alcance acordado.

Solo después de aclarar el origen, destila los 4 horizontes.

### Validación por horizonte (alcance paraguas o capacidad)

Cuando el alcance detectado es **paraguas** o **capacidad** (ver "Detección de alcance"), **no presentes la ficha completa de una sola vez para validación en bloque**. Valida horizonte por horizonte:

1. Presenta el horizonte **Funcional**. Pregunta: "¿lo confirmas, lo refinas o tienes algo que añadir/quitar?"
2. Aplica la respuesta. Solo entonces presenta el horizonte **Arquitectónico** con la misma pregunta.
3. Repite con **Restricción**.
4. Repite con **Autoría**.
5. Al final, muestra la ficha consolidada con los 4 horizontes ya validados y pregunta una última vez si el conjunto es coherente.

El motivo: un horizonte de Funcional impreciso arrastra el resto. Es más rápido corregir uno a la vez que rehacer la ficha entera tras leerla. Para sub-decisión (ADR) o exploración, la validación bloque sigue siendo aceptable.

### Si el usuario no proporciona descripción

Pregunta brevemente:

> ¿Qué cambio quieres definir? Describe en una o dos frases qué necesitas hacer.

No pidas más de lo necesario para arrancar.

### Si el usuario proporciona algo parcial

Aplica "Cuestionar antes de fichar": si lo que falta materialmente cambia la ficha, pregúntalo. Si solo son detalles menores, completa lo que puedas inferir razonablemente y marca con `[pendiente]` lo que necesite confirmación del usuario.

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

## Detección de alcance

Antes de proponer destino, clasifica la ficha por **alcance**:

| Alcance | Cuándo | Destino propuesto |
|---------|--------|-------------------|
| **Proyecto-paraguas** | Es la ficha que gobierna todo un proyecto nuevo | `docs/REQUIREMENTS.md` — sección "Ficha de propósito-paraguas" |
| **Capacidad** | Es una capacidad relevante del proyecto (Intake, Parser, Notifier, una política de seguridad, etc.) | `docs/REQUIREMENTS.md` — sección "Fichas de capacidad", o `docs/requirements/<capacidad>.md` si el proyecto usa split |
| **Sub-decisión (ADR)** | Decisión técnica o de diseño fina dentro de una capacidad: qué LLM, qué retry policy, qué shape de prompt, qué formato de plantilla, etc. | `docs/adr/NNNN-<slug>.md` siguiendo el patrón Architecture Decision Record |
| **Tarea aislada / exploración** | Cambio pequeño que no merece sobrevivir a la conversación | conversación / PR description; no se persiste |

Si dudas, pregúntalo: "¿Es esta ficha para el proyecto completo, una capacidad, una sub-decisión o un cambio puntual?"

## Persistencia

Según alcance:

- **Paraguas o capacidad** → `docs/REQUIREMENTS.md` (o `docs/requirements/<cap>.md` en split). La ficha actúa como user story + criterios de aceptación, y debe sobrevivir a la conversación para que `/telos:exec` y `/telos:check` puedan consultarla.
- **Sub-decisión (ADR)** → `docs/adr/NNNN-<slug>.md` usando la plantilla `telos-purpose-core/assets/adr-template.md`. Numera correlativamente (lee el directorio existente; si no hay, empieza en `0001`). El ADR usa los 4 horizontes como sus secciones principales, no la estructura clásica de ADR.
- **Exploración** → no persistir, respeta la decisión del usuario.

Si el modo del proyecto es `documental` (ver `telos-dev-core`), las fichas de capacidad describen documentos o conjuntos documentales, no módulos de código. El destino es el mismo (`REQUIREMENTS.md` o split).

## Siguiente paso

Una vez guardada la ficha, el usuario puede trabajar con ella como base. Cuando haya una propuesta, código o documento, usar `/telos:check` para evaluarla contra los horizontes y cerrar el cambio.
