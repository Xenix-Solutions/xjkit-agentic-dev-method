---
name: xjkit-init
description: "XJKit, día 0: crea la estructura del método en un proyecto (xdocs/ con STATUS.md y README.md, bloque XJKit y reglas operativas en CLAUDE.md, anexos/, .gitignore, git init), con el visto bueno del operador. Se invoca con la barra al inicio."
argument-hint: "[opcional: qué es el proyecto, en una frase]"
disable-model-invocation: true
---

Instrucciones para el día 0 de XJKit. El operador quiere iniciar este proyecto con el método. El método
completo:
https://raw.githubusercontent.com/Xenix-Solutions/xjkit-agentic-dev-method/main/docs/XJKIT.md

RESULTADO: el proyecto con la estructura del método creada y un primer commit, sin que se haya escrito
nada antes del visto bueno del operador sobre el plan completo. No diseñas ni implementas nada.

Lo que el operador trae, si lo da:

$ARGUMENTS

PASO 1. Detecta el estado, solo lectura.
- Si existe `xdocs/STATUS.md`, el proyecto ya está iniciado: dilo y para. Este command no migra ni repara.
- Si hay código pero no hay `CLAUDE.md`, recomienda crearlo primero con `/init` y curarlo: el bloque XJKit
  no sustituye a las instrucciones del proyecto. Si el operador prefiere seguir, creas `CLAUDE.md` solo con
  el bloque.
- Si hay `CLAUDE.md` sin bloque XJKit, le añadirás la sección sin tocar el resto.
- Mira si hay repo git y si hay historia.

PASO 2. Pregunta al operador, en una sola tanda, solo lo que no puedas deducir del repo ni de lo que trajo, y
di qué has dado por defecto para que lo corrija. Las siete cosas que hay que saber:
(a) ¿Qué es el proyecto, en una frase? (b) ¿Es de un cliente? (c) ¿Qué conectores externos usa? "Ninguno" es
respuesta válida; lo no declarado no entra. (d) ¿Stack previsto, para el `.gitignore`? (e) ¿Política de push:
sin push ni PR desde la sesión, push a una rama con PR bajo visto bueno, o libre? ¿Despliega en push?
(f) ¿Lee rutas fuera de este repo? (g) ¿Quién es el operador que firma el trabajo? Nombre, sin correo.

PASO 3. Propón el plan, fichero a fichero, y espera el visto bueno. Aprobado, ejecuta en este orden:
1. `.gitignore` antes del primer commit: lo del stack, basura de sistema y editor, secretos (`.env`,
   `*.local.json`), medios pesados (`*.mp4 *.mov *.mp3 *.wav`). Comprimidos solo si el operador confirma
   que no son entregables.
2. `CLAUDE.md` con la sección `## Metodología XJKit` de abajo, tal cual, y la sección `## Reglas operativas`
   con las respuestas de (c), (e) y (f); la de confidencialidad solo si (b) es sí, nombrando el remoto
   privado. Si el fichero existe, añade las secciones sin reescribir el resto.
3. `xdocs/README.md` con la cabecera de abajo y la tabla vacía. `xdocs/STATUS.md` con la plantilla de abajo.
4. `anexos/` en la raíz, con un `README.md` de una línea: "Material aportado por el operador: transcripciones, hojas de
   cálculo, imágenes, ficheros de terceros. Los documentos del método van en xdocs/."
5. `README.md` en la raíz como portada mínima, qué es el proyecto y enlaces a `xdocs/README.md`,
   `xdocs/STATUS.md` y `CLAUDE.md`. Si ya existe, añade solo los enlaces. La portada no es el índice.
6. `git init` si no hay repo, y el primer commit con el visto bueno del operador: "Inicio del proyecto con
   XJKit". Sin push. Si el repo despliega en push, propón además reglas `permissions.ask` para `git push` y
   `gh pr create` en `.claude/settings.json`, y escríbelas con su visto bueno.

PASO 4. Da al operador el siguiente paso listo para pegar: `/xjkit-design <lo que quiere hacer>`.

---

Bloque para `CLAUDE.md` (copia exacta de la plantilla del método; si difieren, manda el método):

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

Cabecera de `xdocs/README.md`:

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

`xdocs/STATUS.md` inicial:

```markdown
# Estado del proyecto <nombre>

Sin trabajo activo. El siguiente empieza con `/xjkit-design <encargo>`.

## Pendientes durables
```
