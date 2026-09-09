---
name: xjkit-implement
description: "XJKit Fase 3, Implementation: construye según el SPEC (o el DESIGN en acotado), verifica con evidencia real, commitea, lanza al revisor y cierra el bucle de hallazgos con el operador. Se invoca con la barra al inicio."
argument-hint: "[ruta al SPEC.md; en acotado, ruta al DESIGN.md]"
disable-model-invocation: true
---

Instrucciones para la Fase 3 de XJKit, Implementation, que incluye lanzar la Fase 4, Review. El operador
quiere construir un trabajo ya especificado. El método completo:
https://raw.githubusercontent.com/Xenix-Solutions/xjkit-agentic-dev-method/main/docs/XJKIT.md

RESULTADO: código que cumple el contrato, con la verificación ejecutada y su salida real delante del
operador; commit; `xdocs/STATUS.md` con los pasos marcados; el revisor lanzado y sus hallazgos dispuestos
con el operador. No implementas nada que el contrato no pida.

El contrato de este trabajo:

$ARGUMENTS

Si viene vacío, busca en `xdocs/STATUS.md` el work item con SPEC cerrado o DESIGN acotado aprobado y
pregunta al operador si es ese.

PASO 0. Sitúate. Lee `CLAUDE.md`, `xdocs/STATUS.md` y el contrato: el `SPEC` en estructural, o el `DESIGN`
con la especificación dentro en acotado. El DESIGN sirve para entender el porqué; si SPEC y DESIGN se
contradicen, manda el SPEC. Si `STATUS.md` muestra todos los pasos del plan marcados y solo falta la
Review, esta sesión retoma en el PASO 5: no reimplementes.

PASO 1. Explora y planifica, en plan mode. Escribe el plan como lista de pasos con marcas en la sección de
este work item de `xdocs/STATUS.md`, debajo de lo que ya hay. Si hay código que preservar, el plan dice
explícitamente cómo lo preserva. Presenta el plan al operador y espera su visto bueno antes de codificar.

PASO 2. Codifica siguiendo el plan y el contrato. Con código existente, imita los patrones y convenciones
que ya hay; no introduzcas un estilo nuevo. Migraciones de datos idempotentes y, si se puede, reversibles.
Nada fuera del contrato: lo que veas de paso, un bug ajeno o una mejora, va a "Pendientes durables" de
`STATUS.md`, no se arregla aquí. Tests nuevos solo donde el contrato los pide; no conviertas comprobaciones
de usar y tirar en tests permanentes.

Si la realidad te obliga a cambiar el diseño: ADDENDUM fechado en el DESIGN con la decisión y su porqué, y
enmienda del SPEC. Si el cambio tiene consecuencias que el operador puede juzgar, coste, servicios de pago,
qué ve el usuario, datos, alcance, antes pasa por la mesa con una ficha: pregunta, opciones, recomendación
con una razón, qué cuesta revertir, qué pasa si no decide.

PASO 3. Verifica. Ejecuta la verificación del contrato y, con código existente, la de lo preservado. Marca
cada paso en `STATUS.md` cuando su verificación pase. No declares nada hecho sin enseñar la salida real del
comando; si algo falla o se saltó, dilo tal cual.

PASO 4. Cierra el código. Si el trabajo cambió el contrato del proyecto, dependencias, comandos de build o
test, arquitectura, actualiza `CLAUDE.md` y borra lo que este cambio deja obsoleto: actualizar es también
borrar. Commit con mensaje descriptivo. Push o PR solo si la política de push de `CLAUDE.md` lo permite;
si no hay política escrita, propónlo y no lo ejecutes.

PASO 5. Lanza al revisor como subagente con la definición `xjkit-review` del repo, en su propio contexto,
nunca una copia de esta conversación. No fijes tú su modelo ni su effort al lanzarlo: los fija la
definición del agente, y esa es decisión del operador. Si el lanzamiento falla, dilo tal cual y espera;
no lo relances con otro modelo. El mensaje de lanzamiento lleva exactamente cuatro cosas:
1. La ruta del contrato: el SPEC, o el DESIGN en acotado.
2. La carpeta del work item, donde escribirá `REVIEW.md`.
3. Si había código que preservar, sí o no, y dónde está descrito.
4. Qué modelo implementó: el tuyo, o el que diga el último HANDOFF si retomas de otra sesión.
Ni tu evidencia ni tu opinión: no debe darlas por buenas. Si es una re-review de un fix, añade qué
hallazgos se corrigieron y en qué commit, y que el resultado va como sección fechada al final del
`REVIEW.md` existente.

PASO 6. Recibido el informe, presenta al operador cada hallazgo con una disposición propuesta:
**corregir**, **diferir** a pendientes durables, **decisión del operador** si tiene consecuencias que él
puede juzgar, o **descartar** con el motivo. El operador dispone. Corrige los que toquen, re-verifica,
commit, y lanza un subagente revisor **nuevo**, acotado al fix; nunca continúes el anterior. Repite hasta
veredicto limpio o hasta que el operador pare.
Nunca descartes un hallazgo en silencio. Si a la tercera pasada siguen saliendo hallazgos no triviales, el
problema está arriba, en el contrato: no hagas una cuarta; dilo y propón volver al SPEC o al DESIGN.

PASO 7. Cierra la fase en `STATUS.md`, solo la sección de este work item:
- Estructural: `[x] Review limpio` con la fecha, y siguiente command `/xjkit-handoff`. El cierre lo dispara
  el operador, con el command o diciendo "prepara el cierre".
- Acotado: con veredicto limpio, el work item termina aquí. Marca su fila de `xdocs/README.md` como
  cerrada, retira su sección de `STATUS.md`, y haz un commit de cierre con el REVIEW, el índice y STATUS:
  el REVIEW se escribió después del commit del código y sin ese commit no queda en el rastro. Sin handoff:
  el DESIGN, el REVIEW y los commits son el rastro.
Si el operador corta antes de terminar, no hagas nada especial: `STATUS.md` ya dice dónde está el trabajo,
y el cierre con handoff lo pide él.
