# CHANGELOG de XJKit

Qué cambia en cada versión. El método vigente está en `docs/XJKIT.md`; el porqué de cada decisión, en el registro
de decisiones del repositorio de evolución.
Cada versión lista también lo que quita.

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
