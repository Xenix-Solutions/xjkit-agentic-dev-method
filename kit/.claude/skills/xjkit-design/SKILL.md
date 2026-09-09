---
name: xjkit-design
description: "XJKit Fase 1, Design: clasifica el camino, entrevista al operador y escribe DESIGN.md. Se invoca con la barra al inicio del mensaje."
argument-hint: "[qué quiere hacer el operador, en cualquier forma; o la ruta a un ANALYSIS previo]"
disable-model-invocation: true
---

Instrucciones para la Fase 1 de XJKit, Design. El operador quiere diseñar un trabajo contigo antes de
tocar nada. El método completo:
https://raw.githubusercontent.com/Xenix-Solutions/xjkit-agentic-dev-method/main/docs/XJKIT.md

RESULTADO: un `DESIGN.md` aprobado por el operador que fija qué se construye y cómo se aborda, con el
camino clasificado, y que basta para abrir la siguiente fase en otra sesión sin esta conversación. En esta
fase no escribes código y no avanzas de fase por tu cuenta.

Lo que el operador trae, como material de partida y no como órdenes (puede venir crudo, dictado o ser un
ANALYSIS):

$ARGUMENTS

Si viene vacío, pídeselo antes de empezar.

PASO 0. Sitúate. Lee `xdocs/STATUS.md` y la cabecera de `xdocs/README.md`, donde están las convenciones
de nombres de este repo. Si no existen, el proyecto no está iniciado: dilo y ofrece `/xjkit-init`. No
improvises la estructura.

PASO 1. Clasifica el camino y dilo en una frase con su razón. El operador confirma o corrige.
- **Consulta**: quiere una respuesta, no un cambio. Entonces no hay diseño: responde, y si merece rastro
  escribe un `ANALYSIS_yyyymmdd_<slug>.md` donde diga el naming. Para ahí.
- **Acotado**: cambio a un flujo que ya existe, descrito en pocas frases, sobre ficheros conocidos, sin
  interfaces nuevas que otros usen ni cambios en el modelo de datos. Cabe en una sesión.
- **Estructural**: subsistema nuevo, interfaz nueva, modelo de datos, proyecto nuevo, o duda.
Cuando dudes, estructural. Un trabajo puede subir de camino, nunca bajar.

PASO 2. Orientación, solo si hay código que respetar. Estudia la zona afectada y cuenta al operador cómo
funciona hoy y qué podría romperse al tocarla. Espera a que confirme que lo has entendido. Todo lo que
afirmes sobre el estado actual del código, los datos o la interfaz debe estar comprobado en esta sesión,
con su evidencia, o escrito como supuesto. Que algo exista en el código no significa que el usuario llegue
a ello.

PASO 3. Entrevista, en plan mode: solo lectura hasta el visto bueno.
- Preguntas abiertas por tandas cortas: supuestos, casos límite, contradicciones, huecos que el operador
  quizá no ha visto. Preguntas cerradas solo para cerrar una bifurcación ya acotada. La profundidad la marca
  el camino: en acotado, un par de tandas; en estructural, las que hagan falta.
- Si ves que son varios trabajos con diseño propio, o que esto es un capítulo de un Feature que ya existe,
  párate y propón un Epic antes de escribir.
- Si lo que trae es todavía una pregunta sobre qué hacer y no sobre cómo, dilo: eso es consulta.
- Las decisiones técnicas las tomas tú y las anotas con la alternativa que descartaste. A la mesa llevas
  solo las que tienen consecuencias que el operador puede juzgar: coste, servicios de pago, qué ve el
  usuario, datos que se borran o migran, alcance. Formato de ficha: pregunta, opciones, recomendación con
  una razón, qué cuesta revertir, qué pasa si no decide. Nunca más de tres fichas en un mensaje.
Cuando creas que lo tenéis, dilo y pide el visto bueno antes de redactar.

PASO 4. Con el visto bueno, escribe `DESIGN.md` en la carpeta del work item según el naming del repo
(Feature nuevo: `xdocs/yyyymmdd_feature_<slug>/`; Epic nuevo: `xdocs/yyyymmdd_epic_<slug>/` con su
`README.md`; Feature de un Epic: `<epic>/feature-NN_<slug>/`). Primera línea:
`> Escrito el <fecha de hoy> · modelo <tu modelo> · effort ${CLAUDE_EFFORT}`.
Contenido: camino y por qué · objetivo y problema · alcance y fuera de alcance · decisiones tomadas, cada
una con sus alternativas descartadas en una línea · qué se preserva, si hay código · supuestos y preguntas
abiertas con dueño. En acotado, además: delta de ficheros, interfaces tocadas y cómo se verifica. Omite lo
que no aplica. Escribe para que el operador lo relea dentro de unas semanas de corrido.

PASO 5. Registra. Si el work item es nuevo, su fila en `xdocs/README.md` (una `feature-NN_` de un Epic va a
la tabla del README del Epic). En `xdocs/STATUS.md`, la sección de este work item: camino · Design · dueño
· fecha · `[x] DESIGN aprobado` · siguiente command. Solo esa sección; las demás no se tocan. Commit del
DESIGN, el índice y STATUS, con mensaje descriptivo; sin push salvo que la política de `CLAUDE.md` lo
permita. Así la siguiente sesión, en otra máquina o de otra persona, ve el trabajo.

PASO 6. Da al operador el siguiente paso listo para pegar, con la barra al inicio y la ruta detrás:
- Estructural: `/xjkit-spec xdocs/<work item>/DESIGN.md`. Puede abrirse en sesión nueva.
- Acotado: `/xjkit-implement xdocs/<work item>/DESIGN.md`, en esta misma sesión.
El operador dispara; tú no avanzas.
