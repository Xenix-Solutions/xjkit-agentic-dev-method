# XJKit — método de desarrollo con agentes de IA

> **Versión 0.1.3**, 2026-09-10. Este documento describe la versión vigente y solo la vigente. Los cambios
> entre versiones están en `CHANGELOG.md`. El porqué de cada decisión, y lo que se descartó con su razón, en
> el registro de decisiones del repositorio de evolución del método. Aquí no hay historia: si algo se
> explica, es porque hoy funciona así.

XJKit es una forma de desarrollar software en pareja con un agente de IA: el humano decide lo que puede
juzgar, el agente construye, un segundo agente revisa, y cada paso deja un documento corto que permite
continuar sin la conversación. Está pensado para trabajar en sesiones cortas, cortar cuando el humano
quiera y no perder nada en el corte. Deriva de la metodología JCC y conserva de ella lo que demostró valer.

El agente de referencia es Claude Code. El método en sí no depende de él: las fases, los documentos y las
puertas son texto. Lo específico de Claude Code está en una sección propia al final.

---

## Vocabulario

Las palabras que este documento usa con un sentido preciso. Están aquí para que el resto se lea de corrido,
también dentro de unas semanas.

| Palabra | Qué significa aquí |
|---|---|
| **Método** | XJKit: las fases, los documentos, las reglas y las plantillas descritas en este documento |
| **Operador** | La persona que trabaja con el agente y toma las decisiones que le corresponden. Si hay varias, la dueña de cada trabajo |
| **Agente** | La sesión de IA con la que se trabaja. Hoy, Claude Code |
| **Repo** | El repositorio git del proyecto que se está construyendo. Todo lo que el método necesita vive dentro de él |
| **Kit** | Con minúscula: los ficheros del método que se copian dentro de cada repo para que el agente los use, es decir, los commands y la definición del revisor. XJKit, con mayúsculas, es el nombre del método entero |
| **Command** | Una orden que el operador teclea al agente con una barra al inicio, como `/xjkit-design`. Carga las instrucciones de una fase. Sin command, la sesión es de consulta |
| **Fase** | Cada uno de los cuatro pasos del trabajo: Design, Spec, Implementation, Review |
| **Artefacto** | El documento que produce una fase: `DESIGN.md`, `SPEC.md`, `REVIEW.md`, `HANDOFF`. Es lo que permite continuar en otra sesión sin la conversación |
| **Work item** | Una unidad de trabajo con carpeta propia: un Feature, un Epic o un Analysis |
| **Camino** | El nivel de ceremonia de un trabajo: consulta, acotado o estructural |
| **Puerta** | Un punto donde el trabajo se detiene hasta que el operador dice que sí |
| **Mesa común** | El momento en que el agente pide al operador una decisión, en un formato fijo |
| **Hogar** | El único sitio donde vive cada tipo de información: el estado en un fichero, el mapa en otro, la historia en otro |
| **Sesión** | Una conversación con el agente, de principio a fin. El operador decide cuándo empieza y cuándo termina |

---

## Cinco principios

1. **El humano decide lo que puede juzgar. Otro agente revisa lo que no.** El operador aprueba el diseño y
   el plan, dispone sobre los hallazgos de la revisión y decide cuándo cerrar. Las decisiones técnicas las toma el
   agente y las deja escritas con la alternativa que descartó. Un revisor en contexto fresco cubre el eje
   técnico que el operador no puede juzgar.
2. **El artefacto es el puente.** Cada fase termina con un documento que basta para abrir la siguiente en
   una sesión nueva, sin la conversación. Nada importante vive solo en el chat.
3. **El estado vive en un fichero pequeño, escrito para humanos.** `xdocs/STATUS.md` dice qué está
   activo, dónde quedó y cuál es el siguiente paso. El fichero de instrucciones del agente solo apunta a él.
4. **La ceremonia se clasifica al principio, en voz alta, y solo sube.** Consulta, acotado o estructural.
   El agente propone, el operador corrige, y si un trabajo resulta mayor de lo clasificado, sube de camino.
   La aprobación humana no escala; el tamaño del artefacto sí.
5. **Una regla entra por un fallo observado y sale cuando deja de cambiar la conducta.** Es la regla que
   protege al método de crecer por acumulación. Se aplica en cada versión y la vigila el operador.

---

## Los tres caminos

Al empezar Design, el agente dice en una frase qué camino le parece y por qué. El operador puede
corregirlo. Un trabajo puede subir de camino, nunca bajar: si un acotado no cabe en una sesión, pasa a
estructural y sigue desde donde estaba.

| Camino | Cuándo | Qué produce | Puertas humanas |
|---|---|---|---|
| **Consulta** | Se quiere una respuesta, no código ni cambios. Es la sesión sin command. | Nada, o un `ANALYSIS` si merece rastro. | Ninguna |
| **Acotado** | Cambio a un flujo que ya existe. Se describe en pocas frases, toca ficheros conocidos, no crea interfaces que otros usen ni cambia el modelo de datos. Cabe en una sesión. | `DESIGN.md` corto con la especificación dentro, `REVIEW.md`, commit. Sin `SPEC` ni `HANDOFF`. | Aprobar el diseño en chat. Aprobar el plan. Disponer sobre la revisión. |
| **Estructural** | Subsistema nuevo, interfaz nueva, modelo de datos, proyecto nuevo, o duda. | `DESIGN.md`, `SPEC.md`, código, `REVIEW.md`, `HANDOFF` cuando haya cortes. | Aprobar el diseño. Aprobar el plan. Disponer sobre la revisión. Disparar el cierre. |

Cuando dudes, el camino más pesado.

---

## Puertas y mesa común

Una **puerta** es un punto donde el trabajo se detiene hasta que el operador dice que sí. En estructural hay
cuatro: aprobar el diseño, aprobar el plan de implementación, disponer sobre los hallazgos de la revisión y
disparar el cierre. En acotado, las tres primeras. La aprobación de la especificación no es puerta: el
agente la cierra con un veredicto y el operador lee sus decisiones sin firmarlas, porque ese eje lo cubre
el revisor. La del plan se conserva aunque el operador suela firmarla sin más: le da ocasión de opinar
antes de que se toque código, y acostumbra a quien empieza a una posición de control.

**La mesa común** es el momento en que el agente pide una decisión al operador en vez de tomarla él. Van a
la mesa las decisiones con **consecuencias que el operador puede juzgar**: coste, servicios de pago, qué ve
el usuario final, datos que se borran o migran, alcance, plazo, reversibilidad de negocio. Las decisiones
técnicas las toma el agente y las deja escritas con la alternativa que descartó; el revisor las cuestiona
después.

Toda decisión que llega a la mesa tiene este formato, sin excepción, para que el operador pueda juzgar sin
leer argumentación larga:

> **Decisión.** La pregunta en una línea.
> Opciones: (a) … · (b) … · como mucho (c).
> Recomendación: una opción y **una** razón.
> Qué cuesta revertirla.
> Qué pasa si el operador no decide.

Nunca más de dos o tres decisiones en un mismo mensaje. Si el operador dice "no lo entiendo", el agente
reescribe más corto, nunca más largo.

---

## El workflow

Cuatro fases y un cierre. Las fases son estados de un **work item**; lo único cíclico es el bucle entre
Implementation y Review.

| Fase | Pregunta que responde | Artefacto | Puerta humana |
|---|---|---|---|
| **1. Design** | ¿Qué hacemos y cómo lo abordamos? | `DESIGN.md` | El operador aprueba el diseño y confirma el camino |
| **2. Spec** (solo estructural) | ¿Qué hay que construir exactamente? | `SPEC.md` con veredicto | Ninguna. El agente cierra con LISTO, CON RESERVAS o NO |
| **3. Implementation** | Construirlo y demostrar que funciona | Código, commit, `STATUS.md` al día | El operador aprueba el plan antes de codificar. La evidencia de verificación se muestra siempre |
| **4. Review** | ¿Cumple el SPEC y no rompe nada? | `REVIEW.md` con veredicto | El operador dispone sobre cada hallazgo |
| **Cierre** | ¿Qué necesita la siguiente sesión? | `HANDOFF_yyyymmdd_<slug>.md` y `STATUS.md` | El operador lo dispara |

### Design

Entrevista entre pares. El agente empieza clasificando el camino y, si hay código que respetar, cuenta
cómo funciona hoy y qué podría romperse. Después pregunta por tandas cortas: supuestos, casos límite,
contradicciones, huecos. Las preguntas cerradas solo para cerrar una bifurcación ya acotada. Cuando cree
que lo tiene, lo dice y pide el visto bueno antes de escribir.

Toda afirmación sobre el estado actual del código, los datos o la interfaz debe estar comprobada en la
sesión, con su evidencia, o escrita como supuesto.

Si el encargo resulta ser una pregunta sobre qué hacer, y no sobre cómo, el agente lo dice: eso es una
consulta que puede terminar en un `ANALYSIS`, no un diseño. Si resulta ser varios trabajos con diseño
propio, propone un **Epic** antes de escribir.

Con el visto bueno escribe `DESIGN.md`, lo registra, actualiza `STATUS.md` y lo commitea. En acotado, el diseño lleva
dentro la especificación y la verificación, y la sesión sigue a Implementation. En estructural, cierra
ofreciendo el command de Spec, que puede abrirse en sesión nueva.

### Spec

Solo en estructural. Convierte el diseño en una especificación que una sesión nueva pueda implementar sin
leer la conversación. El agente toma las decisiones técnicas, las escribe con la alternativa razonable
que descartó, y lleva a la mesa solo las que tienen consecuencias que el operador puede juzgar.

Si el diseño dejó preguntas abiertas que afectan al SPEC, las cierra contra la fuente primaria o declara
el SPEC bloqueado por ellas; no especifica sobre una incógnita.

Cierra con un veredicto a la pregunta *¿se puede implementar esto sin inventar decisiones que nada
registra?*: **LISTO**, **CON RESERVAS** con la lista, o **NO**. Con NO no se abre Implementation. El SPEC
se commitea al cerrar.

### Implementation

Lee `SPEC.md` (o el `DESIGN.md` en acotado) y el fichero de instrucciones del proyecto. Explora,
planifica y escribe la lista de pasos del plan en la sección del work item de `STATUS.md`; el operador
aprueba el plan antes de que se toque código. Codifica imitando los patrones que ya hay. Verifica con la salida real de los comandos y marca cada paso en
`STATUS.md` al verificarlo. Commit con mensaje descriptivo; push según la política del proyecto.

Nada fuera del SPEC: lo que aparezca de paso se anota como pendiente, no se arregla. Tests nuevos solo
donde el SPEC los pide. Si la realidad obliga a cambiar el diseño, el cambio se registra como **ADDENDUM**
fechado en `DESIGN.md` y se enmienda el SPEC; si tiene consecuencias que el operador puede juzgar, antes
pasa por la mesa.

Al terminar lanza al revisor con exactamente cuatro datos: la ruta del SPEC o del DESIGN, la carpeta del
work item, si había código que preservar y dónde, y qué modelo implementó. Ni su evidencia ni su opinión.
Recibido el REVIEW, presenta los hallazgos al operador para disponer y corrige los que toquen; cada
corrección vuelve a un revisor nuevo, en contexto fresco, acotado al fix. Si a la tercera pasada siguen saliendo hallazgos no triviales,
el problema está en el contrato, no en el código: no hay cuarta pasada, se vuelve al SPEC o al DESIGN.

En acotado, con el veredicto limpio, cierra el work item: marca la fila del índice como cerrada, retira su
sección de `STATUS.md` y commitea el REVIEW junto con ellos.

### Review

La hace quien no escribió el código, en contexto fresco, con el encargo de **refutar**. Busca, en orden:
regresión sobre lo que debía preservarse, cumplimiento del SPEC, correctitud y casos límite, que la
verificación pasa de verdad ejecutándola él, y fuera de alcance tocado. No revisa estilo ni propone
refactors que el SPEC no exija.

Reporta **todo** lo que encuentra, también lo dudoso y lo menor: el filtro es del operador. Por hallazgo:
fichero, tipo, gravedad, confianza y cláusula del SPEC. Termina con un veredicto. Escribe `REVIEW.md` en
la carpeta del work item; una re-review de un fix es una sección fechada al final del mismo fichero, sin
reescribir la revisión original. El REVIEW declara en su cabecera qué modelo revisó y cuál implementó.

El revisor corre en su propio contexto, así que no consume el de la sesión que lo lanza: en la sesión de
implementación solo entra su informe. Si aun así el operador cierra la sesión antes de la revisión, el
cierre deja en `STATUS.md` todos los pasos marcados y la Review pendiente; la sesión siguiente abre con el
command de Implementation, lee ese estado y va directamente a lanzar al revisor, sin reimplementar nada.

### Cierre

Lo dispara el operador, con el command o con una frase natural como "prepara el cierre" o "prepáralo para
la siguiente sesión"; en el segundo caso el agente lee la skill de cierre del kit y la sigue, porque las
skills no se cargan solas. Escribe el `HANDOFF` para la siguiente sesión y reescribe la sección del work item en
`STATUS.md`. Si el cierre termina el work item, marca la fila del índice como cerrada y retira su sección.
Hace el commit del cierre, sin push salvo que la política del proyecto lo permita. El detalle del HANDOFF
está en *Sesiones y traspaso*.

---

## Dónde vive cada cosa

Todo lo que el método necesita vive en el repositorio. Ninguna máquina ni ninguna memoria personal guarda
estado del proyecto. Así el trabajo sobrevive a cambiar de equipo y a que lo toque otra persona.

```
<repo>/
├── CLAUDE.md            instrucciones del agente: bloque XJKit de ~12 líneas + reglas operativas del proyecto
├── xdocs/               toda la documentación del método
│   ├── STATUS.md        estado vivo, escrito para humanos; una sección por work item activo
│   ├── README.md        índice de work items + cabecera con las convenciones de naming de este repo
│   ├── yyyymmdd_feature_<slug>/     un Feature: carpeta plana con DESIGN, SPEC, REVIEW, HANDOFF
│   ├── yyyymmdd_epic_<slug>/        un Epic: DESIGN transversal, README, handoffs/, feature-NN_<slug>/
│   └── yyyymmdd_analysis_<slug>/    un Analysis previo a todo trabajo
├── anexos/              material que aporta el operador: transcripciones, hojas, imágenes, ficheros de terceros
└── .claude/             el kit: skills y agente del método (ver *Con Claude Code*)
```

Cada tipo de información tiene un **hogar**: un único sitio donde vive, con un comportamiento propio. Cuando
no lo hay, todo acaba apilado en el fichero que el agente lee siempre, y el estado se convierte en un
diario. Hay tres hogares, y no se mezclan:

| Hogar | Fichero | Comportamiento |
|---|---|---|
| **Estado**: qué está activo y dónde quedó | `xdocs/STATUS.md` | Se **reescribe** en cada cierre de fase y en cada handoff. Nunca acumula historia |
| **Mapa**: qué existe | `xdocs/README.md` y, en un Epic, su `README.md` | **Crece** una fila por work item o documento. En un Feature, el listado de la carpeta es el mapa |
| **Historia**: qué pasó, con evidencia | `HANDOFF_`, `REVIEW`, `ANALYSIS_`, `ADDENDUM` | **Fotos fechadas**: no se reeditan. Lo que las supera se escribe en un documento nuevo o en una sección fechada al final |

**Los work items.** Un **Feature** es un trabajo que recorre el workflow entero, incluido el puramente
técnico. Un **Epic** agrupa varios Features bajo un diseño transversal; nace cuando Design descubre que
son varios trabajos, o cuando un encargo nuevo es un capítulo de un Feature existente. Un **Analysis** es
una deliberación con rastro sobre qué hacer, previa a cualquier trabajo. Un proyecto que es un solo Epic
también va en carpeta de Epic.

**Las convenciones de nombres pertenecen a cada proyecto, no al método.** Viven en la cabecera de
`xdocs/README.md` del propio repo, y el fichero de instrucciones del agente apunta ahí. Así, si el método
cambia una convención en una versión futura, ningún proyecto existente tiene que renombrar nada: los nuevos
adoptan la nueva y los viejos siguen con la suya. La convención que el kit propone al crear un proyecto:

- Carpetas de work item: `yyyymmdd_<epic|feature|analysis>_<slug>/`. Fecha de apertura, guion bajo entre
  campos, guion entre palabras. Dentro de un Epic: `feature-NN_<slug>/`, con NN el orden del roadmap.
- Ficheros vivos: `DESIGN.md`, `SPEC.md` (o `SPEC-NN_<slug>.md` si el diseño se descompuso), `README.md`.
- Ficheros fechados: `TIPO_yyyymmdd_<slug>.md` con TIPO en mayúsculas: `HANDOFF_`, `ANALYSIS_`, `BRIEF_`.
  `REVIEW.md` es la primera revisión; las pasadas siguientes, `REVIEW-NN_<slug>.md`.
- Solo lo activo se renombra. Lo cerrado conserva nombre y enlaces.

**`STATUS.md`** tiene una sección por work item activo. Cada sección lleva el camino, la fase, el dueño,
su propia fecha de actualización, la lista de pasos con marcas, la decisión abierta si la hay, el siguiente
command listo para pegar y el enlace al último HANDOFF. La lista de pasos es la del trabajo abierto: el plan
de un SPEC ya cerrado se colapsa en una línea con enlace a su REVIEW y su HANDOFF cuando el work item
continúa con otro SPEC. Al pie, los **pendientes durables**: una línea por
cosa, con enlace a su detalle. Entra lo que este repo tendrá que hacer y se ha decidido no hacer ahora. No
entra lo que ya es objeto de un DESIGN o SPEC en curso, lo que pertenece a otro repo, ni una nota de
vigilancia sin decisión. Se poda en cada cierre: lo hecho, lo caducado y lo que haya pasado a un work item.
Cada sesión edita solo la sección de su work
item. Plantilla al final.

---

## Los artefactos

Lo que cada uno contiene como mínimo para que la fase siguiente abra en sesión nueva. Las secciones que
no aplican se omiten; ningún documento mejora por ser más largo. Todos empiezan con una línea informativa:

```markdown
> Escrito el 2026-09-09 · modelo `<id>` · effort `<nivel>`
```

| Artefacto | Contiene | Lector principal |
|---|---|---|
| **`DESIGN.md`** | Camino y por qué. Objetivo y problema. Alcance y fuera de alcance. Decisiones tomadas, cada una con las alternativas descartadas en una línea. Qué se preserva si hay código. Supuestos y preguntas abiertas con dueño. En acotado, además: delta de ficheros, interfaces tocadas y cómo se verifica. Los cambios posteriores, como `ADDENDUM` fechado al final | El operador; la sesión de Spec |
| **`SPEC.md`** | Resumen en dos frases. Stack y arquitectura, con la alternativa descartada. Delta: añadido, modificado, eliminado. Interfaces y contratos sin ambigüedad. Qué se preserva. Migración de datos si aplica, idempotente. Fuera de alcance. Verificación de extremo a extremo. Veredicto: LISTO, CON RESERVAS con lista, o NO | La sesión de Implementation |
| **`REVIEW.md`** | Cabecera con modelo revisor, modelo implementador y si coinciden. Hallazgos, cada uno con fichero, tipo, gravedad, confianza, cláusula del SPEC y disposición del operador: corregir, diferir, decisión del operador, descartado con motivo. Veredicto. Re-reviews como secciones fechadas al final | El operador, para disponer; la sesión que corrige |
| **`HANDOFF_yyyymmdd_<slug>.md`** | Qué se hizo. Qué se verificó, con la salida real. Qué pasó en qué orden: hipótesis corregidas, caminos descartados. Linaje de los recursos que nombra. Cómo retomar, con el command exacto. Modelo que corrió y, si el operador la pega, la línea de consumo | La siguiente sesión |
| **`ANALYSIS_yyyymmdd_<slug>.md`** | La pregunta como quedó formulada. Contexto comprobado, con evidencia. Opciones con pros y contras. Recomendación. Lo decidido, o las preguntas abiertas con dueño. Qué sigue | El operador; Design, si nace trabajo |

Otros documentos que el trabajo real produce y se reconocen sin ser obligatorios: `BRIEF_` para material
de partida o una vista para otra audiencia, `RUNBOOK_<slug>.md` para pasos de operación. Se registran donde
viven.

---

## Sesiones y traspaso

**El operador decide cuándo cortar una sesión.** Por contexto, por cambio de modelo, por cansancio. El
método no impone fronteras de sesión; garantiza que ningún corte pierde nada.

- **Frontera de fase.** El artefacto basta. Design, Spec y Review cierran de forma que la fase siguiente
  abra desde el documento, sin la conversación.
- **Corte a mitad de fase.** `STATUS.md` dice dónde quedó, con la lista de pasos marcados. El `HANDOFF`
  dice qué pasó, qué se verificó y cómo retomar. Los dos se escriben en el cierre.
- **Apertura.** Cualquier sesión que toque trabajo en curso lee `STATUS.md`, sigue sus enlaces hasta el
  último `HANDOFF` y contrasta con `git log` antes de afirmar nada. Si el estado declarado no cuadra con
  los ficheros o con git, lo dice antes de seguir.

El `HANDOFF` se escribe **para la siguiente sesión**, no para el operador: estructurado, denso, con rutas y
commits exactos. El operador lo lee de vez en cuando; `STATUS.md` es lo que se escribe para él. La
evidencia de ejecución vive una sola vez, en el `HANDOFF`; el resto enlaza.

Trabajo posterior al propio handoff en la misma sesión: sección fechada al final del mismo handoff. En otra
sesión: handoff nuevo.

---

## Sesiones sin command

Una sesión sin command es la que se abre sin teclear ningún `/xjkit-*`: se le pregunta o se le pide algo
en lenguaje normal. Es el camino de consulta y es la sesión por defecto. El fichero de instrucciones le da
lo que necesita:
leer `STATUS.md` antes de opinar, no avanzar fases ni tocar el estado, y las convenciones de ubicación por
si escribe un documento. Una sesión de consulta puede terminar en un `ANALYSIS_` registrado en el índice,
y ese es todo su rastro. Si empieza a fijar alcance, modelo de datos o contratos, se ha convertido en
diseño: el agente lo dice y ofrece el command de Design con el análisis como material.

---

## Varias personas, varias máquinas

El kit y toda la documentación viajan con el repositorio, así que dos máquinas o dos personas tienen el
método al clonar. `STATUS.md` viaja con la rama: dos personas en work items distintos no chocan porque
editan secciones distintas; dos personas en el mismo work item chocan al mergear, y ese conflicto es la
señal correcta. El **dueño** de cada work item, nombrado en su sección, es quien firma sus puertas.

Mínimo exigible a cualquiera que toque el repo: no avanzar fases sin command y no dejar trabajo sin
reflejar en `STATUS.md` o en un `HANDOFF`.

La memoria personal del agente, si la herramienta la tiene, guarda solo preferencias del operador. Nunca
estado del proyecto.

---

## Cómo evoluciona XJKit

- **Regla de admisión.** Una frase entra en el método, en una skill o en el bloque de instrucciones solo
  si nace de un fallo observado, si quitarla cambiaría la conducta en el caso común, y si no puede
  sustituirla una comprobación mecánica ni la clasificación de caminos. Sale cuando deja de cumplirlo.
  Cada versión lista lo que quita. El guardián es el operador, en mesa, con el formato de decisión de arriba.
- **Tres documentos.** Este describe la versión vigente. `CHANGELOG.md` lleva los cambios por versión.
  El registro de decisiones del repositorio de evolución lleva el porqué y lo rechazado con razón. Ninguna
  frase de este documento cita una versión.
- **Prueba antes de publicar.** Cada versión del kit pasa por un repositorio de juguete con escenarios
  fijos antes de usarse en un proyecto real. El primero: un corte en cualquier punto del workflow,
  retomado en sesión fresca, no pierde ninguna decisión ni ninguna evidencia.
- **Adaptar un proyecto** que venga de otro método es un procedimiento escrito una vez, no un command.

---

## Con Claude Code

Todo lo anterior es texto. Esta sección es lo único que depende de la herramienta.

**El kit vive en el repo del proyecto**, en `.claude/skills/xjkit-<nombre>/SKILL.md` y
`.claude/agents/xjkit-review.md`. Se copia desde la carpeta `kit/` del repositorio público del método al
crear el proyecto, desde la raíz del proyecto:

```powershell
git clone --depth 1 https://github.com/Xenix-Solutions/xjkit-agentic-dev-method $env:TEMP\xjkit; New-Item -ItemType Directory -Force .\.claude | Out-Null; Copy-Item -Recurse -Force $env:TEMP\xjkit\kit\.claude\* .\.claude\; Remove-Item -Recurse -Force $env:TEMP\xjkit
```

Vale también para un proyecto que ya tiene `.claude/`: añade el kit y no toca lo que hubiera.
A partir de ahí, `/xjkit-init` hace el resto. No hay instalador ni copia global, y cada proyecto lleva su
versión del método.

**Cinco commands y un agente:**

| Pieza | Qué hace |
|---|---|
| `/xjkit-init` | Bootstrap del día 0: `xdocs/` con `STATUS.md` y `README.md`, bloque XJKit y reglas operativas en `CLAUDE.md`, `anexos/`, `.gitignore`, `git init`. Con el visto bueno del operador antes de escribir |
| `/xjkit-design <encargo>` | Fase 1, incluida la clasificación del camino |
| `/xjkit-spec <ruta al DESIGN>` | Fase 2, solo en estructural |
| `/xjkit-implement <ruta al SPEC o al DESIGN>` | Fase 3, y lanza al revisor |
| `/xjkit-handoff` | Cierre. También se dispara con la frase natural del operador |
| agente `xjkit-review` | Fase 4. Lo lanza `implement` como subagente; su contrato completo está en la definición |

- Los commands se invocan con la barra **al inicio** del mensaje y el contexto detrás. Todos llevan
  `disable-model-invocation: true`: nunca se autodisparan. Si aparece un `/xjkit-*` en mitad de un mensaje,
  el agente avisa y no lo invoca.
- Design y Spec entrevistan en **plan mode**: solo lectura hasta el visto bueno.
- El revisor corre como **subagente con definición de agente**: contexto propio, sin la conversación del
  implementador, con `model` y `effort` fijados en su frontmatter para que no herede los de la sesión. Nace
  con `model: claude-fable-5-1` y `effort: high`; cambiarlo es editar esa línea en el repo. La sesión que lo
  lanza no le pasa modelo ni effort: un parámetro de invocación tiene precedencia sobre el frontmatter y
  anularía la decisión del operador. La caché de la sesión implementadora no se ve afectada.
- **Modelo y effort los elige el operador** al abrir cada sesión. Se fijan antes de empezar y no se tocan
  a mitad: cambiar de modelo invalida la caché siempre; cambiar el effort la invalida en todos los modelos
  salvo Fable 5.1 con suscripción. El método no recomienda ningún modelo; la línea informativa de cada
  artefacto registra el que se usó, con el effort que Claude Code inyecta en la skill.
- Las **reglas operativas** del proyecto (conectores permitidos, rutas fuera del repo, política de push,
  confidencialidad) viven en `CLAUDE.md` para el agente y, las que deban hacerse cumplir, también en
  `permissions` de `.claude/settings.json` para la herramienta. Nunca solo en la conversación.
- La memoria automática de Claude Code guarda solo preferencias del operador.

---

## Plantillas

### Bloque XJKit en `CLAUDE.md`

```markdown
## Metodología XJKit

- Este proyecto se desarrolla con XJKit. Estado del trabajo: `xdocs/STATUS.md`. Índice de work items y convenciones de naming: `xdocs/README.md`.
- Si tu tarea toca trabajo en curso, antes de afirmar nada lee `xdocs/STATUS.md`, sigue sus enlaces hasta el último HANDOFF y contrasta con `git log`. Cuenta al operador dónde estamos en pocas líneas.
- Sin un command `/xjkit-*` no avanzas fases ni editas `STATUS.md`. Si la conversación empieza a fijar alcance, modelo de datos o contratos, párate y ofrece `/xjkit-design`.
- Si escribes un documento, va en `xdocs/`, en la carpeta de su work item, con el naming de la cabecera de `xdocs/README.md`. Un análisis o una deliberación que merezca rastro es un work item de tipo `analysis`: carpeta propia y fila en el índice. Registrar en el índice no es avanzar fases. `anexos/` es solo para material que aporta el operador.
- "Prepara el cierre" o "prepáralo para la siguiente sesión" equivale a `/xjkit-handoff`: lee `.claude/skills/xjkit-handoff/SKILL.md` y síguelo.
- A la mesa van las decisiones con consecuencias que el operador puede juzgar: coste, servicios de pago, qué ve el usuario, datos que se borran o migran, alcance. Formato: opciones, recomendación con una razón, qué cuesta revertir. Las decisiones técnicas las tomas tú y las registras con la alternativa descartada. El operador es quien firma el work item en `xdocs/STATUS.md`.
- Un `/xjkit-*` solo dispara si el mensaje empieza por él. Si aparece en prosa, avisa; no lo invoques tú.

## Reglas operativas

- **Conectores.** Este proyecto usa solo los conectores declarados aquí: <lista, o "ninguno">. Ningún otro se consulta ni se lista.
- **Lectura acotada.** La sesión no lee rutas fuera de este repo salvo las declaradas aquí: <lista, o "ninguna">. El kit bajo `.claude/` no cuenta como fuera.
- **Push.** <una de: "sin push ni PR desde la sesión; los propone" · "push a <rama>; PR con visto bueno" · "libre">.
- <solo cliente> **Confidencialidad.** El material del cliente no sale de este repo ni de su remoto privado <remoto>.
```

### `xdocs/STATUS.md`

```markdown
# Estado del proyecto <nombre>

## 20260901_feature_<slug>
estructural · Implementation · dueño <nombre> · actualizado 2026-09-09
- [x] DESIGN aprobado
- [x] SPEC cerrado, veredicto LISTO
- [x] Paso 1: <qué>
- [ ] Paso 2: <qué>
- [ ] Review
- Decisión abierta: ninguna
- Siguiente: `/xjkit-implement xdocs/20260901_feature_<slug>/SPEC.md`
- Último handoff: `HANDOFF_20260909_<slug>.md`

## Pendientes durables
- <una línea por cosa decidida "no ahora"; se poda>
```

### Cabecera de `xdocs/README.md`

```markdown
# <proyecto> — índice de work items

Convenciones de este repo. Carpetas: `yyyymmdd_<epic|feature|analysis>_<slug>/` en esta raíz; dentro de
un Epic, `feature-NN_<slug>/`. Ficheros vivos: `DESIGN.md`, `SPEC.md`, `README.md`. Ficheros fechados:
`TIPO_yyyymmdd_<slug>.md`. Solo lo activo se renombra. Todo documento empieza con la línea
`> Escrito el yyyy-mm-dd · modelo <id> · effort <nivel>`. En la tabla: Tipo es `Epic`, `Feature` o
`Analysis`; Camino es `consulta`, `acotado` o `estructural`; Estado es `activo` o `cerrado`. Estado vivo
en `STATUS.md`; historia en los `HANDOFF_` de cada work item.

| Fecha | Tipo | Slug | Qué es | Camino | Estado | Enlaces |
|---|---|---|---|---|---|---|
```
