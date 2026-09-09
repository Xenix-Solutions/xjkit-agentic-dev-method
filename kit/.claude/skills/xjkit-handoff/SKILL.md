---
name: xjkit-handoff
description: "XJKit, cierre de sesión: escribe el HANDOFF para la siguiente sesión, reescribe STATUS.md y deja los índices al día. Se invoca con la barra al inicio, o cuando el operador dice 'prepara el cierre'."
argument-hint: "[opcional: tema de la sesión para el nombre del handoff]"
disable-model-invocation: true
---

Instrucciones para el cierre de sesión de XJKit. El operador va a cerrar esta sesión. El método completo:
https://raw.githubusercontent.com/Xenix-Solutions/xjkit-agentic-dev-method/main/docs/XJKIT.md

RESULTADO: un `HANDOFF` con el que la siguiente sesión retoma el trabajo sin perder ninguna decisión ni
ninguna evidencia; `xdocs/STATUS.md` reescrito; índices al día; el commit hecho. El cierre no avanza
fases ni reedita documentos anteriores.

Tema de la sesión, si el operador lo da:

$ARGUMENTS

PASO 0. Antes de escribir, di en una línea en qué punto está el trabajo, contrastando `xdocs/STATUS.md`
con los ficheros del repo y con `git log`. Si lo declarado no cuadra con la realidad, dilo; el operador
confirma.

PASO 1. Escribe el handoff. Va en la carpeta del work item: `HANDOFF_yyyymmdd_<slug>.md`; en un Epic, en su
`handoffs/`. El slug es el tema de la sesión, no el nombre de la carpeta; dos cierres el mismo día llevan
slugs distintos. Primera línea: `> Escrito el <fecha de hoy> · modelo <tu modelo> · effort ${CLAUDE_EFFORT}`.

El lector es la siguiente sesión del agente, no el operador: estructura fija, denso, rutas y commits
exactos. Secciones:
1. **Estado al cerrar.** Qué está hecho y verificado, qué está a medias, qué hay sin commitear. Describe lo
   que hay ahora, no lo que ocurrirá después.
2. **Qué se hizo.** Por pasos, con los commits.
3. **Qué se verificó.** Con la salida real de los comandos, no con "pasa". La evidencia vive aquí una sola
   vez; si otro documento la necesita, que enlace.
4. **Qué pasó en qué orden.** Hipótesis que se corrigieron, caminos que se descartaron y por qué, y los
   fallos de herramientas o de lanzamiento de agentes tal como ocurrieron, sin atribuirlos a nadie. Es el
   único sitio donde queda el hilo temporal.
5. **Recursos que se nombran.** Bases de datos, entornos, despliegues, credenciales por nombre: de dónde
   salió cada uno, de qué commit o copia.
6. **Cómo retomar.** Qué leer primero y el command exacto con su ruta, listo para pegar.
7. **Consumo.** Tu modelo y, si el operador pega la salida de `/usage`, una línea con los números. Si no la
   pega, omite.

PASO 2. Backport. Si en esta sesión se descubrió o corrigió algo que contradice un documento vigente,
enmiéndalo antes de cerrar: ADDENDUM fechado en el DESIGN o el SPEC afectado; corrección directa en
`CLAUDE.md`. Todos los documentos vigentes que contradiga, no solo el primero. Los documentos fechados
anteriores, handoffs y reviews, no se tocan: son fotos.

PASO 3. Reescribe la sección de este work item en `xdocs/STATUS.md`, solo esa: camino, fase, dueño, fecha
de hoy, la lista de pasos con sus marcas, la decisión abierta si la hay, el siguiente command con su ruta y
el enlace a este handoff. Si el cierre termina el work item, marca su fila de `xdocs/README.md` como cerrada
y retira su sección. Pendientes durables: añade lo decidido "no ahora" en esta sesión y poda lo hecho o
caducado. Lo que es simple continuidad va al handoff, no a pendientes.

PASO 4. Índices. Si esta sesión creó documentos que aún no están registrados, regístralos: en la tabla del
README del Epic si es un Epic; en `xdocs/README.md` si es un work item nuevo.

PASO 5. Haz el commit del handoff y de todo lo que hayas tocado, con mensaje descriptivo. El operador ya
pidió el cierre; no le pidas permiso para el commit. Sin push, salvo que la política de push de `CLAUDE.md`
lo permita. Sin commit, quien clone no verá el cierre.

Al terminar, reporta al operador en pocas líneas: dónde quedó el handoff, qué dice ahora la sección de
STATUS y qué documentos vigentes enmendaste. No generes un prompt de arranque aparte: el siguiente command
con su ruta es el arranque, y la siguiente sesión lee el resto.

Si después de este cierre el trabajo sigue en esta misma sesión, lo nuevo va como sección fechada al
final de este handoff; en otra sesión, handoff nuevo.
