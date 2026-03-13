---
created: 2026-03-12
updated: 2026-03-12
area:
  - "[[Computación]]"
type: nota
tags:
  - ia
  - llm
  - programming
  - ingenieria-del-proposito
aliases:
  - Ingeniería del Propósito
  - Ingenieria del Proposito
  - Ingeniería del propósito
  - Ingenieria del proposito
  - ingeniería del propósito
  - ingenieria del proposito
source:
  - "[La Tiranía del Contexto](https://disidencia.incontrolada.com/2026/02/28/la-tirania-del-contexto/)"
---

# Ingeniería del Propósito

La **Ingeniería del Propósito** es un modelo de interacción con LLMs que nace del artículo [La Tiranía del Contexto](https://disidencia.incontrolada.com/2026/02/28/la-tirania-del-contexto/). Propone un concepto de trabajo en el que el programador define primero el **para qué** de un cambio y solo después aporta el **contexto mínimo necesario** para ejecutarlo. No parte de la idea de que más contexto produce automáticamente mejores respuestas, sino de que la calidad del trabajo mejora cuando la IA opera bajo una dirección explícita, unos límites claros y unos criterios de evaluación estables.

No sustituye al contexto. Lo subordina.

## Definición breve

La Ingeniería del Propósito propone que el centro del trabajo con IA no sea la acumulación de información, sino la formulación explícita del fin que debe gobernar la solución.

Su tesis central puede resumirse así:

- El contexto sin propósito tiende al ruido.
- El propósito sin algo de contexto puede quedar abstracto.
- La combinación correcta es **propósito primero, contexto después**.

## Qué problema resuelve

Este enfoque nace como respuesta a varios problemas frecuentes en el uso de LLMs para programar:

- La **fatiga de contexto**: el desarrollador dedica demasiada energía a copiar, pegar, resumir y curar información para el modelo.
- La **falacia de la exhaustividad**: se asume que si la IA recibe todo el historial del proyecto, la respuesta será correcta.
- La **pérdida de agencia**: el programador pasa de arquitecto a operador de chat.
- La **evaluación débil**: si solo se pide "que funcione", cualquier solución aparentemente válida compite con otra, aunque sea mala a nivel arquitectónico.

La Ingeniería del Propósito devuelve un criterio rector: la solución no se evalúa solo por si compila o pasa una prueba puntual, sino por si **cumple el propósito que justifica el cambio**.

## En qué se basa

### 1. En un giro teleológico

La pregunta principal deja de ser "¿por qué existe este código?" para pasar a ser "¿para qué debe existir el siguiente cambio?".

Ese desplazamiento del pasado al futuro cambia el rol del programador:

- Antes: curador de contexto.
- Ahora: diseñador de fines, restricciones y criterios.

### 2. En una crítica a la Ingeniería de Contexto

La Ingeniería de Contexto se centra en optimizar la causa material del trabajo: archivos, especificaciones, logs, historial, convenciones y ejemplos previos. Ese material sigue siendo útil, pero no basta para gobernar la interpretación del modelo.

Cuando el contexto crece sin dirección:

- se diluye la señal,
- aparecen respuestas genéricas,
- se heredan errores del código previo,
- y el coste cognitivo humano aumenta.

### 3. En una lectura aristotélica del desarrollo con IA

La propuesta se apoya en las cuatro causas de Aristóteles como marco de comprensión del trabajo técnico:

| Causa | En desarrollo con IA | Papel en la Ingeniería del Propósito |
| --- | --- | --- |
| Material | Código existente, documentación, tickets, logs, esquema de datos | Es útil, pero no gobierna por sí solo |
| Formal | Arquitectura, patrones, principios, interfaces, estructura | Traduce el propósito a forma estable |
| Eficiente | La colaboración programador-IA | El humano aporta juicio; la IA acelera la ejecución |
| Final | El para qué del cambio | Es la causa rectora del proceso |

La tesis diferencial del enfoque es esta: la **causa final** debe gobernar a las demás.

### 4. En una visión realista de los LLMs

Los LLMs no son motores deductivos perfectos. Son sistemas probabilísticos sensibles a la señal, al ruido y a la forma de la tarea. Por eso:

- más información no implica mejor decisión,
- totalidad informativa no implica control total,
- y contexto abundante sin jerarquía no garantiza calidad.

### 5. En la recuperación de la agencia profesional

El propósito no mejora solo la salida de la IA; mejora la posición del programador frente al sistema. Le devuelve:

- criterio para aceptar o rechazar soluciones,
- continuidad entre iteraciones,
- y autoría sobre el resultado final.

## Qué la distingue de otros enfoques

| Enfoque | Centro | Pregunta dominante | Riesgo principal |
| --- | --- | --- | --- |
| Ingeniería de Contexto | Información | "¿Qué datos le doy?" | Saturación y ruido |
| Ingeniería de Intención | Instrucción o estado deseado | "¿Qué debe hacer?" | Optimizar ejecución sin aclarar fines de fondo |
| Ingeniería del Propósito | Fin rector del cambio | "¿Para qué, bajo qué principios y con qué límites?" | Exige más claridad y juicio humano |

La diferencia no es cosmética. La Ingeniería del Propósito no busca solo especificar mejor una tarea, sino establecer el **criterio de legitimidad** de la solución.

## Fundamentos

Los fundamentos prácticos de esta ingeniería son:

1. **El propósito precede al contexto.** Primero se define la dirección; luego se decide qué información hace falta.
2. **El contexto debe ser mínimo y funcional.** Solo se aporta lo necesario para ejecutar, no para compensar una idea mal pensada.
3. **El propósito debe ser explícito.** Si no puede formularse con claridad, tampoco puede evaluarse.
4. **Toda solución debe poder rechazarse por violar el propósito.** Esto convierte el propósito en criterio operativo, no en eslogan.
5. **La arquitectura forma parte del propósito.** No basta con que el sistema haga algo; importa cómo sobrevivirá en el tiempo.
6. **Las restricciones son tan importantes como la funcionalidad.** Un cambio correcto a nivel funcional puede ser inaceptable a nivel de seguridad, mantenibilidad o ética.
7. **La legibilidad también es parte del diseño.** La forma en que el código se leerá y mantendrá pertenece al propósito.

## Taxonomía del Propósito

La propuesta operativa se articula en cuatro horizontes. Juntos forman un marco breve pero suficiente para dirigir a la IA.

### I. Propósito funcional

Define la utilidad real del cambio.

- Pregunta clave: ¿Qué capacidad nueva adquiere el usuario o el sistema?
- Enfoque correcto: describir qué problema desaparece, no solo qué función se implementa.
- Ejemplo: "El usuario debe poder guardar sin percibir latencia, moviendo la persistencia a segundo plano."

### II. Propósito arquitectónico

Define cómo debe sobrevivir la solución.

- Pregunta clave: ¿Qué principio de diseño debe proteger este cambio?
- Enfoque correcto: priorizar desacoplamiento, sustitución, extensibilidad, observabilidad o simplicidad, según el caso.
- Ejemplo: "Las preferencias deben ser extensibles sin modificar la tabla `User` cada vez."

### III. Propósito de restricción

Define qué resultados son inaceptables aunque la función parezca correcta.

- Pregunta clave: ¿Qué no debe pasar bajo ninguna circunstancia?
- Enfoque correcto: fijar límites de seguridad, compatibilidad, complejidad o comportamiento.
- Ejemplo: "Ningún dato personal puede aparecer en logs, ni siquiera en flujos de error."

### IV. Propósito de experiencia o autoría

Define cómo debe sentirse y leerse el código resultante.

- Pregunta clave: ¿Cómo debe entenderlo otro humano?
- Enfoque correcto: hacer explícita la preferencia por legibilidad, declaratividad, claridad operativa o rendimiento extremo cuando proceda.
- Ejemplo: "El flujo debe leerse de forma casi narrativa para que un junior entienda la intención sin recorrer cinco capas."

## Modelo operativo

La Ingeniería del Propósito puede aplicarse como un ciclo corto de cinco pasos:

1. **Nombrar el cambio.**
   Formular en una línea qué se quiere introducir o resolver.
2. **Declarar los cuatro horizontes.**
   Funcional, arquitectónico, restricción y autoría.
3. **Aportar contexto mínimo.**
   Archivos, interfaces, contratos, tablas o fragmentos estrictamente necesarios.
4. **Pedir propuesta y justificar decisiones.**
   La IA debe explicar cómo su solución satisface cada horizonte.
5. **Evaluar contra el propósito.**
   Si la solución viola uno de los horizontes, se rechaza o se itera.

## Cómo se aplica en la práctica

El modelo operativo anterior puede desplegarse así en el día a día:

1. Define el cambio en una frase.
2. Redacta los cuatro horizontes en menos de dos minutos.
3. Adjunta solo los artefactos indispensables.
4. Pide una propuesta con explicación de trade-offs.
5. Revisa el resultado preguntando:
   "¿cumple la función?",
   "¿protege la arquitectura?",
   "¿respeta las restricciones?",
   "¿se lee como quería?".
6. Si falla en alguno de esos puntos, itera corrigiendo el propósito o el diseño, no añadiendo contexto a ciegas.

## Plantillas

### Plantilla rápida de propósito

```md
**Propósito del cambio**

- Funcional: El fin de este cambio es que [usuario/sistema] pueda [capacidad nueva] sin que [efecto indeseado].
- Arquitectónico: La solución debe seguir [principio o patrón] para que en el futuro sea sencillo [cambio esperado].
- Restricción: Bajo ninguna circunstancia debe [estado inaceptable].
- Autoría: Quiero que el resultado sea [legible / declarativo / simple / eficiente], priorizando [criterio principal].

**Contexto mínimo**

- Archivos o interfaces imprescindibles:
- Restricciones técnicas existentes:
- Contratos o dependencias relevantes:
```

### Plantilla de encargo a un LLM

```md
Actúa como colaborador técnico. No optimices solo para que "funcione".
Diseña la solución para cumplir este propósito:

Cambio:
[nombre breve del cambio]

Horizonte funcional:
[para qué existe]

Horizonte arquitectónico:
[cómo debe sobrevivir]

Horizonte de restricción:
[qué no debe pasar]

Horizonte de autoría:
[cómo debe leerse o mantenerse]

Contexto mínimo:
[archivos, interfaces, contratos]

Entrega:
1. Propuesta de solución.
2. Justificación explícita de cómo cumple cada horizonte.
3. Riesgos o trade-offs.
4. Código o pseudocódigo.
```

### Plantilla de revisión

```md
Revisa esta propuesta únicamente contra el propósito declarado.

Comprueba:
1. Si cumple el objetivo funcional.
2. Si preserva el principio arquitectónico.
3. Si viola alguna restricción.
4. Si la legibilidad y la estructura reflejan la autoría deseada.

Si no cumple alguno de esos puntos, indica exactamente dónde y propón una corrección.
```

## Modelos de aplicación

Las plantillas anteriores pueden combinarse con distintos modelos de uso según el momento del trabajo.

### Modelo 1. Propósito primero, contexto después

Plantilla asociada: **plantilla rápida de propósito** + **plantilla de encargo a un LLM**.

Útil para cambios normales de producto o mantenimiento.

- Primero se fija el propósito.
- Después se incorpora el contexto mínimo.
- La IA propone.
- El humano valida contra el propósito.

### Modelo 2. Propósito como filtro de exploración

Plantilla asociada: **plantilla rápida de propósito** (solo los horizontes, sin contexto previo).

Útil cuando el sistema es desconocido o muy grande.

- El propósito se formula antes de explorar.
- La IA o el programador buscan solo el contexto relevante para ese fin.
- Se evita abrir todo el sistema sin una hipótesis de trabajo.

### Modelo 3. Propósito como criterio de revisión

Plantilla asociada: **plantilla de revisión**.

Útil cuando ya existe una solución y hay que decidir si aceptarla.

- No se pregunta "¿está bien?" en abstracto.
- Se pregunta "¿cumple este propósito?".
- La discusión deja de ser subjetiva y se vuelve evaluable.

## Ejemplos

### Ejemplo 1. Preferencias de usuario

**Cambio:** añadir preferencias de usuario.

- Funcional: el usuario debe personalizar su interfaz sin recargar la página.
- Arquitectónico: las preferencias deben ser extensibles sin alterar el esquema principal en cada nueva opción.
- Restricción: una preferencia corrupta no puede bloquear el login; debe haber degradación segura a valores por defecto.
- Autoría: el servicio debe exponer de forma obvia qué preferencias existen y cómo se resuelven.

**Resultado esperado del LLM:**

- una propuesta que desacople almacenamiento y resolución,
- validación robusta,
- y una interfaz legible.

### Ejemplo 2. Refactor de integración de pagos

**Cambio:** sustituir un proveedor de pagos sin tocar la UI.

- Funcional: cambiar de proveedor sin reescribir flujos de compra.
- Arquitectónico: mantener acoplamiento nulo entre UI y motor de pagos.
- Restricción: no romper compatibilidad con órdenes en curso.
- Autoría: el punto de extensión debe ser evidente y pequeño.

**Resultado esperado del LLM:**

- abstracción del proveedor,
- adaptación por interfaz,
- migración gradual,
- y tests centrados en contrato.

### Ejemplo 3. Hotfix de observabilidad

**Cambio:** mejorar trazabilidad de errores en autenticación.

- Funcional: identificar por qué falla el acceso en producción.
- Arquitectónico: no contaminar la capa de dominio con detalles de logging.
- Restricción: jamás registrar credenciales, tokens ni datos personales.
- Autoría: los puntos de observación deben ser explícitos y fáciles de auditar.

**Resultado esperado del LLM:**

- eventos o hooks de observabilidad claros,
- centralización del logging,
- redacción segura de metadatos,
- y ausencia total de PII en registros.

## Ventajas

Las ventajas principales de esta ingeniería son:

- **Reduce la carga cognitiva.** Obliga a aclarar la dirección antes de abrir decenas de archivos.
- **Mejora la calidad de la delegación.** La IA recibe criterios, no solo materiales.
- **Protege la arquitectura.** Evita aceptar soluciones funcionales pero destructivas a medio plazo.
- **Facilita la revisión.** Se puede rechazar una propuesta por incumplir el propósito, aunque "funcione".
- **Da continuidad entre iteraciones.** El propósito suele permanecer más estable que el contexto detallado.
- **Recupera la autoría.** El programador vuelve a decidir qué sistema quiere construir.

## Justificación

La justificación de la Ingeniería del Propósito puede formularse en tres niveles:

### Justificación técnica

Los LLMs pierden eficacia cuando la señal relevante queda diluida entre material irrelevante o poco jerarquizado. El propósito actúa como filtro y criterio de selección.

### Justificación metodológica

El desarrollo no consiste solo en reproducir el pasado del sistema, sino en orientar su siguiente estado. El contexto explica de dónde venimos; el propósito decide hacia dónde vamos.

### Justificación profesional

Sin propósito explícito, el desarrollador queda reducido a mediador de datos. Con propósito explícito, recupera juicio, criterio y capacidad de diseño.

## Límites y matices

La Ingeniería del Propósito no debe presentarse como una solución mágica.

- No elimina la necesidad de contexto en auditorías, análisis forense o sistemas legacy complejos.
- No reemplaza el conocimiento técnico ni la disciplina de diseño.
- Un propósito vago produce resultados vagos.
- A veces el problema real está en las herramientas de contexto y no solo en la formulación del trabajo.

Por eso conviene entenderla como un principio rector: **el contexto sigue siendo necesario, pero ya no debe mandar**.

## Fórmula resumida

La fórmula operativa del enfoque puede expresarse así:

> **Propósito claro + contexto mínimo + evaluación por horizontes = colaboración útil con LLMs**

## Cierre

La Ingeniería del Propósito propone pasar de la obsesión por alimentar modelos a la disciplina de dirigirlos. Su aporte central no es solo mejorar prompts, sino reinstalar una idea fuerte: en el trabajo con IA, el valor diferencial del programador no reside en transportar contexto, sino en definir con precisión el fin, los principios y los límites del sistema que quiere construir.
