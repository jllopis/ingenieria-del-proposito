---
name: purpose-core
description: Conocimiento base de Ingeniería del Propósito. Define los siete principios, los cuatro horizontes (funcional, arquitectónico, restricción, autoría), el flujo operativo y los anti-patrones. Gobierna el criterio de los comandos brief, review y retro.
user-invocable: false
---

# Ingeniería del Propósito

Trabaja con LLMs desde propósito explícito en lugar de partir de contexto abundante.

## Regla central

No empieces por pedir archivos o contexto. Empieza por formular el propósito.

## Principios

1. **El propósito precede al contexto.** Primero se define la dirección; luego se decide qué información hace falta.
2. **El contexto debe ser mínimo y funcional.** Solo se aporta lo necesario para ejecutar, no para compensar una idea mal pensada.
3. **El propósito debe ser explícito.** Si no puede formularse con claridad, tampoco puede evaluarse.
4. **Toda solución debe poder rechazarse por violar el propósito.** El propósito es criterio operativo, no eslogan.
5. **La arquitectura forma parte del propósito.** No basta con que el sistema haga algo; importa cómo sobrevivirá en el tiempo.
6. **Las restricciones son tan importantes como la funcionalidad.** Un cambio correcto a nivel funcional puede ser inaceptable a nivel de seguridad, mantenibilidad o ética.
7. **La legibilidad también es parte del diseño.** La forma en que el código se leerá y mantendrá pertenece al propósito.

## Los cuatro horizontes

Todo cambio debe articular, al menos, estos cuatro horizontes:

- **Funcional**: qué problema desaparece o qué capacidad nueva aparece.
- **Arquitectónico**: qué principio de diseño debe protegerse o respetarse.
- **Restricción**: qué no puede ocurrir bajo ninguna circunstancia.
- **Autoría**: cómo debe leerse, mantenerse o evolucionar el resultado.

## Cuándo activar esta skill

Úsala cuando el usuario pida cualquiera de estas cosas o algo equivalente:

- definir mejor una tarea antes de delegarla a un LLM,
- convertir una petición vaga en una ficha clara,
- revisar si una propuesta o PR cumple un propósito,
- reducir prompts basados en contexto excesivo,
- traducir una necesidad técnica a los cuatro horizontes,
- capturar una lección aprendida tras un cambio.

## Flujo de trabajo

### Orden básico

1. Nombrar el cambio en una frase.
2. Declarar los cuatro horizontes.
3. Aportar solo el contexto mínimo necesario.
4. Pedir propuesta con justificación por horizonte.
5. Evaluar la propuesta contra el propósito declarado.

### Encadenamiento de comandos

Los tres comandos cubren los momentos clave del ciclo:

```
/telos:brief  →  (trabajo / implementación)  →  /telos:review  →  /telos:retro
```

- `/telos:brief`: al arrancar un cambio. Produce la ficha de propósito.
- `/telos:review`: al revisar una propuesta, diff o PR. Evalúa contra la ficha.
- `/telos:retro`: al cerrar un cambio. Captura aprendizaje reutilizable.

No es obligatorio usar los tres siempre. En cambios pequeños basta con el brief. En cambios ya hechos, se puede entrar directamente por el review.

## Comportamiento esperado del LLM

- Si la petición del usuario es ambigua, formula primero una ficha de propósito antes de producir código.
- Si faltan datos esenciales para cerrar la ficha, pregunta solo lo imprescindible. No pidas contexto por completismo.
- Si el usuario ya tiene una propuesta o código, revísalo contra los cuatro horizontes antes de sugerir cambios.
- Si el trabajo terminó y hubo aprendizaje útil, ofrece capturarlo como patrón reutilizable.
- Adapta el idioma de la respuesta al idioma del usuario.

## Criterios de rechazo

Una propuesta debe cuestionarse o rechazarse si:

- cumple la función pero rompe la arquitectura,
- cumple la función pero viola una restricción,
- cumple la función pero produce un resultado opaco o difícil de mantener,
- o depende de demasiado contexto innecesario para justificarse.

## Anti-patrones

Evita estos errores frecuentes:

| Anti-patrón | En su lugar |
|-------------|-------------|
| Empezar pegando archivos sin propósito explícito | Formular primero el cambio y los cuatro horizontes |
| Escribir propósitos vagos ("mejorar esto", "hacerlo más limpio") | Especificar qué mejora concreta, bajo qué criterio y con qué límite |
| Omitir restricciones y confiar en que el modelo las infiera | Escribir explícitamente lo inaceptable, aunque parezca obvio |
| Aceptar soluciones porque funcionan aunque violen el propósito | Evaluar horizonte por horizonte antes de aprobar |
| Tratar la salida del LLM como decisión final | Tratarla como material de trabajo; la decisión es del humano |
| Compensar falta de claridad añadiendo más contexto | Reescribir el propósito con más precisión |

## Salidas preferidas

Según la necesidad del usuario, devuelve una de estas formas:

- **Ficha de propósito**: plantilla en `assets/purpose-brief-template.md`
- **Revisión contra propósito**: plantilla en `assets/purpose-review-template.md`
- **Lección aprendida**: plantilla en `assets/purpose-retro-template.md`

## Referencias

- Flujo operativo detallado: `references/operating-model.md`
- Índice de plantillas: `references/templates.md`
- Anti-patrones ampliados: `references/anti-patterns.md`
