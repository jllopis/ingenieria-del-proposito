# Guía práctica para equipos: aplicar Ingeniería del Propósito con LLMs

## Objetivo

Esta guía traduce la `Ingeniería del Propósito` a una práctica de equipo. No está pensada para defender la tesis, sino para usarla en el trabajo diario con LLMs: diseño de cambios, implementación, revisión, coordinación y aprendizaje continuo.

La idea central es simple:

- el equipo no debe empezar por reunir todo el contexto posible,
- debe empezar por declarar el propósito del cambio,
- y solo después aportar el contexto mínimo necesario para ejecutarlo.

## Cuándo usar esta guía

Esta guía es especialmente útil cuando el equipo:

- usa LLMs para diseñar o implementar cambios,
- sufre fatiga por exceso de contexto,
- detecta respuestas útiles a nivel local pero pobres a nivel arquitectónico,
- quiere estandarizar cómo delega trabajo a IA,
- necesita criterios objetivos para revisar salidas generadas por modelos.

No pretende sustituir análisis profundos de sistemas legacy, auditorías o investigación forense. En esos casos el contexto amplio sigue siendo necesario, pero conviene que también esté gobernado por un propósito explícito.

## Principios operativos

Todo equipo que adopte esta práctica debería comprometerse, como mínimo, con estos principios:

1. **Propósito antes que contexto.** Ningún cambio relevante se delega a un LLM sin propósito explícito.
2. **Contexto mínimo útil.** Solo se comparte la información que el modelo necesita para ejecutar o proponer.
3. **Arquitectura visible.** El propósito debe incluir no solo utilidad, sino también criterios de diseño y supervivencia.
4. **Restricciones explícitas.** Lo inaceptable debe estar escrito.
5. **Revisión contra propósito.** La salida del LLM no se valida solo porque compile o parezca razonable.
6. **Autoría humana.** El equipo conserva el juicio final sobre diseño, legibilidad y trade-offs.

## Artefacto base: la ficha de propósito

La unidad mínima de trabajo con esta metodología es la **ficha de propósito**. Puede vivir en un ticket, una descripción de PR, una nota de diseño o un comando interno, pero debe existir.

Toda ficha debe responder a estos cuatro horizontes:

- **Funcional**: qué capacidad nueva aparece o qué problema desaparece.
- **Arquitectónico**: qué principio de diseño debe protegerse.
- **De restricción**: qué no puede ocurrir bajo ninguna circunstancia.
- **De autoría**: cómo debe leerse o mantenerse el resultado.

### Plantilla mínima

```md
## Ficha de propósito

Cambio:
[describir el cambio en una frase]

Funcional:
[qué debe poder hacer ahora el usuario o el sistema]

Arquitectónico:
[qué principio debe proteger esta solución]

Restricción:
[qué resultado es inaceptable]

Autoría:
[cómo debe leerse, mantenerse o evolucionar el resultado]

Contexto mínimo:
- [archivo, interfaz, contrato o fragmento imprescindible]
- [restricción técnica relevante]
- [dependencia o límite operativo]
```

## Flujo de trabajo recomendado

El flujo mínimo de equipo puede organizarse en cinco pasos.

### 1. Aclarar el cambio

Antes de abrir archivos o empezar a pegar contexto en un chat, el responsable del trabajo debe poder formular:

- qué se quiere cambiar,
- para qué existe ese cambio,
- y qué riesgos o límites lo rodean.

Si esto no puede expresarse con claridad, el equipo aún no está listo para delegar nada al modelo.

### 2. Redactar la ficha de propósito

La ficha debe completarse en pocos minutos. Si cuesta demasiado, el problema no es de plantilla, sino de comprensión del cambio.

La regla práctica es esta:

- si el cambio es pequeño, la ficha puede ocupar 6 o 8 líneas;
- si el cambio es grande, puede acompañarse de una nota de diseño;
- en ambos casos, el propósito debe seguir siendo breve, explícito y verificable.

### 3. Aportar contexto mínimo

Solo después de fijar el propósito se añade contexto. El equipo debe resistir la tendencia a volcar repositorios enteros.

Preguntas útiles para filtrar contexto:

- ¿Qué necesita ver realmente el modelo para proponer algo correcto?
- ¿Qué artefactos solo añaden ruido histórico?
- ¿Qué parte del problema puede resolverse sin mostrar todo el sistema?

### 4. Pedir propuesta al LLM

El encargo al modelo debe incluir dos exigencias:

- que proponga una solución,
- y que justifique cómo cumple cada horizonte del propósito.

Esto obliga al modelo a explicitar su razonamiento práctico y facilita la revisión posterior.

### 5. Revisar contra propósito

El revisor no debe preguntar solo si el código es correcto. Debe preguntar:

- ¿cumple el fin funcional?
- ¿protege el criterio arquitectónico?
- ¿respeta las restricciones?
- ¿se entiende como esperábamos?

Si la respuesta es negativa en cualquiera de esos puntos, se corrige la propuesta o se reescribe la ficha. No se compensa automáticamente con más contexto.

## Roles dentro del equipo

La metodología funciona mejor cuando cada rol sabe qué aportar.

### Autor del cambio

Responsabilidades:

- redactar la ficha de propósito,
- seleccionar el contexto mínimo,
- pedir una propuesta inicial al LLM,
- y validar que la salida no distorsiona el cambio.

### Revisor técnico

Responsabilidades:

- revisar la solución contra la ficha,
- detectar violaciones arquitectónicas o de restricción,
- y exigir claridad donde el código o la explicación sean opacos.

### Lead o responsable de equipo

Responsabilidades:

- asegurar consistencia terminológica,
- decidir cuándo una ficha requiere diseño adicional,
- introducir la metodología en tickets, PRs y rituales,
- y medir si mejora o degrada la velocidad y la calidad.

## Definition of Ready y Definition of Done

Una forma simple de institucionalizar la práctica es integrarla en dos momentos.

### Definition of Ready

Un cambio está listo para empezar cuando:

- su propósito está redactado,
- las restricciones críticas están explicitadas,
- el alcance es entendible,
- y el contexto mínimo ya está identificado.

### Definition of Done

Un cambio está terminado cuando:

- la solución implementada cumple la ficha de propósito,
- las restricciones se han verificado,
- el revisor entiende la intención del código,
- y la documentación del cambio deja trazabilidad suficiente.

## Ritual mínimo de equipo

No hace falta rediseñar toda la organización para empezar. Basta con introducir tres rituales ligeros que se integran naturalmente en ceremonias ágiles existentes.

### 1. Kickoff de propósito

Se integra en la **refinement** o **sprint planning** existente. En cambios no triviales, antes de implementar:

- revisar la ficha en 5 minutos,
- corregir ambigüedades,
- y confirmar qué contexto es realmente necesario.

### 2. PR review contra propósito

Se integra en el **code review** habitual. Toda PR que use LLMs debería incluir la ficha o un enlace a ella. El review deja de girar en torno a impresiones generales y se apoya en criterios ya fijados.

### 3. Retro de aprendizaje

Se integra en la **retrospectiva** de sprint o con cadencia periódica:

- revisar ejemplos donde el propósito ayudó,
- detectar casos donde faltó precisión,
- y capturar nuevas plantillas o patrones útiles.

## Plantillas recomendadas

### Plantilla de encargo al LLM

```md
Actúa como colaborador técnico del equipo.
No optimices solo para que "funcione".

Trabaja contra esta ficha de propósito:

Cambio:
[cambio]

Funcional:
[propósito funcional]

Arquitectónico:
[propósito arquitectónico]

Restricción:
[propósito de restricción]

Autoría:
[propósito de autoría]

Contexto mínimo:
- [artefacto 1]
- [artefacto 2]

Entrega:
1. Propuesta.
2. Justificación por horizonte.
3. Riesgos y trade-offs.
4. Código, pseudocódigo o pasos sugeridos.
```

### Plantilla de revisión de PR

```md
## Revisión contra propósito

- Funcional: ¿el cambio resuelve el problema descrito?
- Arquitectónico: ¿preserva el principio declarado?
- Restricción: ¿evita estados inaceptables?
- Autoría: ¿el resultado se entiende y se mantiene bien?

Observaciones:
- [hallazgo 1]
- [hallazgo 2]

Decisión:
- [aprobar | pedir cambios]
```

### Plantilla de nota de aprendizaje

```md
## Lección de propósito

Cambio:
[nombre]

Qué funcionó:
- [...]

Qué falló:
- [...]

Qué faltó en la ficha:
- [...]

Patrón reutilizable:
- [...]
```

## Checklist rápido para el día a día

Antes de usar un LLM:

- ¿he escrito el propósito en una frase comprensible?
- ¿he declarado el criterio arquitectónico?
- ¿he dejado por escrito lo inaceptable?
- ¿sé cómo quiero que se lea o mantenga el resultado?
- ¿estoy aportando contexto útil o contexto por ansiedad?

Antes de aceptar una propuesta:

- ¿cumple el fin o solo parece resolverlo?
- ¿introduce deuda futura?
- ¿rompe una restricción?
- ¿puede otro desarrollador entender la intención sin arqueología?

## Anti-patrones comunes

Los fallos más frecuentes al introducir esta práctica suelen ser estos:

### 1. Usar la ficha como trámite

Si el equipo rellena la plantilla por obligación y no la usa para revisar decisiones, la metodología se vacía.

### 2. Escribir propósitos vagos

Frases como "mejorar rendimiento" o "hacerlo más limpio" no bastan. Un propósito útil tiene dirección y criterio.

### 3. Añadir contexto para compensar falta de claridad

Cuando no se sabe bien qué se quiere, aparece la tentación de pegar más archivos. Eso no corrige la falta de dirección.

### 4. Olvidar las restricciones

Muchos equipos describen bien la funcionalidad y la arquitectura, pero dejan implícito lo prohibido. Ese vacío suele pagarse después.

### 5. Tratar la salida del modelo como si fuese una decisión

La propuesta del LLM es material de trabajo. La decisión sigue siendo del equipo.

## Modelo de adopción por niveles

No hace falta implantar todo de golpe. Una secuencia razonable sería esta.

### Nivel 0. Trabajo guiado por contexto

- No existe ficha de propósito.
- El uso de LLMs depende del estilo individual.
- La revisión se centra en el código final.

### Nivel 1. Propósito explícito en cambios relevantes

- Se introduce la ficha en tareas no triviales.
- El equipo empieza a revisar por horizontes.
- Se detectan vacíos recurrentes.

### Nivel 2. Propósito integrado en tickets y PRs

- La ficha forma parte del flujo habitual.
- Los reviewers la usan como criterio de aceptación.
- Empieza a haber plantillas reutilizables.

### Nivel 3. Propósito operacionalizado

- El equipo dispone de comandos, skills o automatismos internos.
- La ficha se genera, valida y reutiliza.
- Las métricas de calidad y retrabajo ya pueden analizarse.

## Métricas útiles

Si el equipo quiere comprobar si la práctica aporta valor, puede observar:

- número de iteraciones de review por cambio,
- retrabajo por incumplimiento arquitectónico,
- tiempo invertido en preparar contexto,
- incidencias causadas por violación de restricciones,
- percepción de claridad del código en revisiones,
- y ratio entre prompts exploratorios y prompts con propósito explícito.

No todas las métricas deben formalizarse desde el primer día. Lo importante es verificar si el equipo gana claridad, consistencia y capacidad de rechazo razonado.

## Ejemplo breve de uso en equipo

Caso: introducir colas para procesar notificaciones.

Ficha resumida:

- Funcional: enviar notificaciones sin bloquear la acción principal del usuario.
- Arquitectónico: desacoplar la generación del evento de su procesamiento.
- Restricción: una caída del worker no puede provocar pérdida silenciosa de mensajes.
- Autoría: el flujo debe ser evidente desde el punto de emisión hasta el reintento.

Con esta ficha:

- el implementador sabe qué proteger,
- el LLM recibe dirección útil,
- y el revisor dispone de un criterio concreto para aceptar o rechazar.

## Recomendación de implantación

Para introducir esta guía sin fricción excesiva:

1. Empezar por un equipo pequeño o una línea de producto.
2. Exigir ficha solo en cambios medianos o grandes.
3. Revisar durante dos semanas qué partes de la plantilla sobran o faltan.
4. Convertir las mejores fichas en ejemplos canónicos del equipo.
5. Automatizar después, no antes.

## Cierre

La Ingeniería del Propósito aplicada a equipos no consiste en escribir mejores prompts. Consiste en trabajar con más dirección compartida. Cuando un equipo hace explícito el fin, los principios, los límites y la forma deseada de una solución, los LLMs dejan de ser un generador imprevisible de código plausible y pasan a ser una herramienta mejor gobernada.

Ese es el salto importante: menos dependencia de contexto por ansiedad y más coordinación basada en propósito.
