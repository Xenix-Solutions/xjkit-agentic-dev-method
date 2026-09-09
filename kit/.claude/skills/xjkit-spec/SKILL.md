---
name: xjkit-spec
description: "XJKit Fase 2, Spec: convierte DESIGN.md en un SPEC.md que otra sesión pueda implementar sin esta conversación, y cierra con veredicto. Solo en el camino estructural. Se invoca con la barra al inicio."
argument-hint: "[ruta al DESIGN.md del work item]"
disable-model-invocation: true
---

Instrucciones para la Fase 2 de XJKit, Spec. El operador quiere convertir un diseño aprobado en una
especificación. El método completo:
https://raw.githubusercontent.com/Xenix-Solutions/xjkit-agentic-dev-method/main/docs/XJKIT.md

RESULTADO: uno o varios `SPEC` que una sesión nueva pueda implementar sin leer el diseño ni este chat, y un
veredicto claro al cerrar. No implementas, no reabres lo decidido en el diseño salvo evidencia nueva, y no
avanzas de fase por tu cuenta.

El DESIGN de este work item:

$ARGUMENTS

Si viene vacío, busca en `xdocs/STATUS.md` el work item que está en Design y pregunta al operador si es ese.

PASO 0. Sitúate. Lee `xdocs/STATUS.md`, la cabecera de `xdocs/README.md` y el `DESIGN.md`. El diseño es tu
fuente de verdad; si sigues en la misma sesión que lo escribió, la conversación queda subordinada a él.
Trabaja en plan mode hasta que las decisiones estén cerradas.

PASO 1. Preguntas abiertas del diseño. Si el DESIGN deja preguntas que afectan al SPEC, ciérralas primero
contra la fuente primaria: documentación oficial, código, datos. Si no puedes, el SPEC queda **bloqueado**:
no lo escribes; añades al DESIGN un ADDENDUM fechado con la pregunta y su dueño, y la sección del work item
en `STATUS.md` dice "Spec bloqueado por <pregunta>". No especifiques sobre una incógnita.

PASO 2. Decide. Las decisiones técnicas las tomas tú: stack, arquitectura, contratos, modelo de datos.
Cada una con la alternativa razonable que descartaste, en una línea. A la mesa llevas solo las que tienen
consecuencias que el operador puede juzgar: coste, servicios de pago, qué ve el usuario, datos que se
borran o migran, alcance. Formato de ficha: pregunta, opciones, recomendación con una razón, qué cuesta
revertir, qué pasa si no decide. Nunca más de tres fichas en un mensaje.

Todo lo que afirmes sobre el estado actual del código, los datos o la interfaz debe estar comprobado en
esta sesión, con su evidencia, o escrito como supuesto.

Si evidencia nueva te dice que una decisión del DESIGN es errónea, ni la reabras en silencio ni la
obedezcas en silencio: llévala a la mesa con la evidencia, y si cambia, ADDENDUM fechado en el DESIGN.

PASO 3. Escribe el SPEC junto al DESIGN. La granularidad sigue la descomposición que el diseño ya
encontró: indivisible, un `SPEC.md`; descompuesto en bloques, un `SPEC-NN_<slug>.md` por bloque, cada uno
autocontenido. Siempre planos, sin subcarpeta. Primera línea:
`> Escrito el <fecha de hoy> · modelo <tu modelo> · effort ${CLAUDE_EFFORT}`.
Contenido, omitiendo lo que no aplica:
1. Resumen: qué se construye o cambia, en dos frases. El porqué está en el DESIGN.
2. Stack y arquitectura, con la alternativa descartada. Producto nuevo: eliges y especificas la estructura
   completa. Código existente: respetas el stack dado y dices cómo encaja el cambio.
3. Delta: ficheros y módulos añadidos, modificados, eliminados.
4. Interfaces y contratos: funciones, APIs, modelo de datos. Sin ambigüedad.
5. Qué se preserva: solo con código existente. Lo que no debe cambiar.
6. Migración de datos, si aplica: idempotente y, si se puede, reversible.
7. Fuera de alcance.
8. Verificación de extremo a extremo: cómo se comprueba que funciona y, con código existente, que lo
   preservado sigue en verde. Si la zona no tiene tests y este trabajo los merece, dilo aquí, qué tests y
   dónde: es la única licencia que Implementation tendrá para crearlos.
Escribe para que la sesión de implementación no tenga que adivinar nada.

PASO 4. Veredicto, en la última línea del SPEC y en tu mensaje, a la pregunta *¿se puede implementar esto
sin inventar decisiones que nada registra?*: **LISTO** · **CON RESERVAS**, con la lista · **NO**, con la
razón. No pidas aprobación del SPEC: da el veredicto y la lista corta de las decisiones técnicas que
tomaste, con su alternativa, para que el operador las lea.

PASO 5. Registra. En la sección de este work item de `xdocs/STATUS.md`: `[x] SPEC cerrado, veredicto
<cuál>` y la fecha. Solo esa sección. Con veredicto NO, la sección dice qué falta y el siguiente command
vuelve a ser `/xjkit-spec`. Commit del SPEC y de STATUS, con mensaje descriptivo; sin push salvo que la
política de `CLAUDE.md` lo permita.

PASO 6. Con LISTO o CON RESERVAS, da al operador el siguiente paso listo para pegar:
`/xjkit-implement xdocs/<work item>/SPEC.md`. Puede abrirse en sesión nueva: todo lo que necesita está en
el SPEC. El operador dispara; tú no avanzas.
