# Anti-patrones

Errores frecuentes al aplicar Ingeniería del Propósito y qué hacer en su lugar.

## 1. Empezar pegando archivos sin propósito explícito

**Síntoma:** el primer mensaje al LLM es un volcado de código o documentación.

**En su lugar:** formular primero el cambio en una frase y redactar los cuatro horizontes. Solo después aportar el contexto mínimo necesario.

## 2. Escribir propósitos vagos

**Síntoma:** frases como "mejorar rendimiento", "hacerlo más limpio" o "refactorizar esto".

**En su lugar:** especificar qué mejora concreta se busca, bajo qué criterio se medirá y qué límite no debe cruzarse. Un propósito útil tiene dirección y criterio verificable.

## 3. Omitir restricciones y confiar en que el modelo las infiera

**Síntoma:** la ficha tiene buen propósito funcional y arquitectónico, pero el horizonte de restricción está vacío o es genérico.

**En su lugar:** escribir explícitamente lo inaceptable, aunque parezca obvio. Lo que no se escribe no se evalúa. Especialmente relevante en seguridad, privacidad, compatibilidad y límites operativos.

## 4. Aceptar soluciones porque funcionan aunque violen el propósito

**Síntoma:** la propuesta compila, pasa tests y parece razonable, pero rompe el principio arquitectónico o introduce deuda técnica.

**En su lugar:** evaluar horizonte por horizonte antes de aprobar. "Funciona" no es criterio suficiente. Si viola arquitectura o restricción, pedir cambios.

## 5. Tratar la salida del LLM como decisión final

**Síntoma:** se acepta la propuesta del modelo sin revisión humana, especialmente bajo presión de tiempo.

**En su lugar:** tratar la salida como material de trabajo. La decisión de diseño, los trade-offs y la aceptación final corresponden al equipo, no al modelo.

## 6. Compensar falta de claridad añadiendo más contexto

**Síntoma:** cuando el LLM no produce buenas respuestas, se le envían más archivos, más historial, más ejemplos.

**En su lugar:** revisar si el problema está en la formulación del propósito. Un propósito impreciso no se arregla con más contexto; se arregla reescribiendo el propósito con más precisión.

## 7. Usar la ficha como trámite burocrático

**Síntoma:** el equipo rellena la plantilla por obligación pero no la usa para revisar ni para rechazar propuestas.

**En su lugar:** integrar la ficha como criterio de aceptación real en PRs y reviews. Si la ficha no influye en las decisiones, se ha convertido en ceremonia vacía.
