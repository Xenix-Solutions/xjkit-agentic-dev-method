# CHANGELOG de XJKit

Qué cambia en cada versión. El método vigente está en `docs/XJKIT.md`; el porqué de cada decisión, en el registro
de decisiones del repositorio de evolución.
Cada versión lista también lo que quita.

## 0.1.3 — 2026-09-10

El plan de un SPEC cerrado no se queda en `STATUS.md`.

**Entra**
- En el cierre, si el work item continúa con otro SPEC, el plan del SPEC cerrado se colapsa en una línea con enlace a su REVIEW y a su HANDOFF. En el doc y en la skill de handoff. Fallo observado en el primer ciclo completo real: `STATUS.md` conservó los nueve pasos del plan cerrado y un resumen de la Review, duplicando la línea del bloque.

**Sale**
- Nada.

## 0.1.2 — 2026-09-10

Criterio de admisión de los pendientes durables de `STATUS.md`.

**Entra**
- Qué es un pendiente durable: una línea con enlace, algo que este repo tendrá que hacer y se ha decidido no hacer ahora. No entra lo que ya es objeto de un DESIGN o SPEC en curso, lo de otro repo ni una nota de vigilancia sin decisión. La poda del cierre incluye lo que haya pasado a un work item. En el doc y en las skills de handoff e implement. Fallo observado en el primer proyecto real: el Backlog heredado de JCC entró entero, 16 entradas de varias líneas, seis de ellas fuera de ese criterio.

**Sale**
- Nada.

## 0.1.1 — 2026-09-09

Corrección al comando de copia del kit, sin cambios en el método ni en el kit.

**Entra**
- El comando de copia crea `.claude/` si no existe y copia dentro el contenido del kit, así que funciona también en un proyecto que ya tiene `.claude/` (por ejemplo, con `settings.json`). Fallo observado al adaptar el primer proyecto real: el comando anterior daba error con `.claude/` previo.

**Sale**
- Nada.

## 0.1 — 2026-09-09

Primera versión. Nace de JCC v1.5.6 con el objetivo de volver a lo simple sin perder el humano en el bucle.

**Entra**
- Método en un documento que describe solo la versión vigente, con vocabulario al principio.
- Tres caminos clasificados al abrir Design: consulta, acotado, estructural. Un trabajo sube, nunca baja.
- Cuatro puertas humanas: aprobar el diseño, aprobar el plan, disponer sobre la revisión, disparar el cierre.
- Regla de la tercera pasada de revisión: si siguen saliendo hallazgos no triviales, se vuelve al contrato.
- Mesa común reservada a consecuencias que el operador puede juzgar, con formato de ficha fijo.
- Estado en `xdocs/STATUS.md`, escrito para humanos, una sección por work item con lista de pasos.
- Kit dentro de cada repo: cinco commands (`init`, `design`, `spec`, `implement`, `handoff`) y el agente `xjkit-review`.
- Convenciones de nombres en la cabecera de `xdocs/README.md` de cada repo, no en el kit.
- Línea informativa con fecha, modelo y effort en cada artefacto.
- Regla de admisión de reglas y prueba en repo de juguete antes de publicar.

**Sale, respecto a JCC v1.5.6**
- Estado y backlog dentro de `CLAUDE.md`; el bloque baja de 77 líneas a unas doce.
- Línea de versión del bloque y detección de desfase.
- Commands `upgrade`, `query`, `analysis`, `audit`, `start` como vestíbulo. ANALYSIS sigue como tipo de documento.
- Perfil de modelo por fase, copias etiquetadas en las skills, verificación de modelo al arrancar y calibraciones por modelo.
- Sesión fresca obligada entre Spec e Implementation.
- Aprobación del SPEC como puerta.
- Skill separada del revisor: el contrato vive en la definición del agente.
- README obligatorio por Feature; columna Merge/PR del índice; dossier BRIEF para sucesor; receta de arranque en memoria; ritual `/usage` obligatorio.
- Historia y perfil de modelo dentro del documento del método.

**Estado**: kit probado en repo de juguete con siete escenarios (`pruebas/ESCENARIOS.md`); siete correcciones entraron por fallo observado durante la ronda. Publicado en `xjkit-agentic-dev-method`. Sin uso en proyecto real todavía.
