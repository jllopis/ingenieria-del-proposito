---
name: telos-purpose-core
description: Conocimiento base de Ingeniería del Propósito. Define los siete principios, los cuatro horizontes (funcional, arquitectónico, restricción, autoría), el flujo operativo y los anti-patrones. Gobierna el criterio del comando `/telos:brief` y de la revisión por horizontes + lección de propósito embebidas en `/telos:check`.
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

Los cuatro horizontes son **universales**: aplican a código, a interacciones puntuales con un LLM, y a entregables no-código. Su traducción a un proyecto documental:

- **Funcional**: qué decisión habilita el documento, qué proceso desbloquea, qué pregunta cierra.
- **Arquitectónico**: cómo encaja en el cuerpo documental existente (jerarquía, referencias cruzadas, normativa aplicable, sistema documental corporativo).
- **Restricción**: qué no puede contener (datos confidenciales, contradicciones con normativa, ambigüedad en responsables, etc.).
- **Autoría**: quién es responsable, cómo se aprueba, cómo se versiona, cómo se mantiene en el tiempo.

## Modos de proyecto

La metodología funciona en tres modos. El modo se declara al inicio del proyecto en `REQUIREMENTS.md` y las skills lo leen para adaptar su comportamiento:

| Modo | Cuándo | Cierre típico |
|------|--------|---------------|
| `dev-team` | Proyecto de desarrollo con equipo (>1 dev) | commit + PR + reviewers humanos |
| `dev-solo` | Un desarrollador + agente IA, sin reviewer humano | commit (la ficha + revisión van en el mensaje); sin PR |
| `documental` | Producción/revisión de políticas, procedimientos, registros u otros entregables no-código | versión publicada y comunicada a stakeholders; validación documental (plantilla, referencias, aprobaciones) en vez de tests |

En `dev-solo`, la disciplina del propósito sustituye al reviewer humano: el agente revisa contra la ficha antes del commit y la traza queda en el mensaje. En `documental`, el "deliverable" son documentos (en Drive, Confluence, local, etc.); las skills de Git pasan a opcionales.

## Cuándo activar esta skill

Úsala cuando el usuario pida cualquiera de estas cosas o algo equivalente:

- definir mejor una tarea (de código o documental) antes de delegarla a un LLM,
- convertir una petición vaga en una ficha clara,
- revisar si una propuesta, PR o documento cumple un propósito,
- reducir prompts basados en contexto excesivo,
- traducir una necesidad técnica o documental a los cuatro horizontes,
- capturar una lección aprendida tras un cambio,
- arrancar un proyecto (de código, documental, mixto) declarando su modo y propósito-paraguas.

## Flujo de trabajo

### Orden básico

1. Nombrar el cambio en una frase.
2. Declarar los cuatro horizontes.
3. Aportar solo el contexto mínimo necesario.
4. Pedir propuesta con justificación por horizonte.
5. Evaluar la propuesta contra el propósito declarado.

### Encadenamiento de comandos

El comando atómico es `/telos:brief`. La revisión por horizontes y la lección de propósito están embebidas en `/telos:check` (cierre de cambio). El flujo completo:

```
/telos:brief  →  (implementación)  →  /telos:check (revisa + cierra + ofrece lección)
```

En modo proyecto, `/telos:plan` envuelve el brief al principio y `/telos:exec` guía la implementación contra la ficha:

```
/telos:plan (incluye brief)  →  /telos:exec  →  /telos:check
```

No es obligatorio usar todos. En cambios pequeños basta con `/telos:brief` y trabajar contra la ficha. En cambios ya hechos, se puede entrar directamente por `/telos:check`.

### El flujo no es estrictamente lineal: las lecciones pueden refinar fichas en cualquier punto

`/telos:check` formaliza el lazo "lección → refinar ficha" al cerrar un cambio, pero el lazo aplica también **durante `plan`**: el propio acto de formular un horizonte expone hipótesis ("creo que esto es PoC, no producción", "no estoy seguro de si esta restricción aplica al alcance actual") que el humano valida o corrige.

Por eso `/telos:brief` valida horizonte por horizonte para alcances paraguas y capacidad (ver `telos-brief`): cada horizonte es ya una micro-lección que puede modificar el siguiente. Tratar la redacción de la ficha-paraguas como un solo paso de "redacta y aprueba en bloque" pierde estas lecciones tempranas.

Regla: **el humano valida cada horizonte antes de avanzar al siguiente**. El agente no asume; pregunta. Lo que el agente destile de docs preexistentes es hipótesis, no propósito, hasta que el humano lo confirma.

### Origen temporal de los docs preexistentes

Cuando se arranca un proyecto sobre un repositorio que ya tiene documentación, **no asumas que esos docs reflejan el proyecto actual**. Un "Diseño funcional v0.1" puede describir:

- el proyecto comprometido **ahora**,
- una visión futura o aspiracional (PoC ahora, producción después; propuesta dependiente de aprobación; fase posterior),
- o un mix de ambos.

`/telos:plan` y `/telos:brief` deben preguntar el origen temporal antes de destilar la ficha. Sin esa distinción, los horizontes heredan restricciones que pueden no aplicar al alcance real, y el propósito se desalinea desde el primer momento.

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
