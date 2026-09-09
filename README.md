# XJKit — método de desarrollo con agentes de IA

XJKit es una forma de desarrollar software en pareja con un agente de IA: el humano decide lo que puede
juzgar, el agente construye, un segundo agente revisa, y cada paso deja un documento corto que permite
continuar en otra sesión sin la conversación. Sesiones cortas, cortes cuando el humano quiera, y nada se
pierde en el corte. Deriva de la metodología JCC y conserva de ella lo que demostró valer.

El agente de referencia es Claude Code. El método es texto; lo específico de la herramienta está en una
sección propia del documento.

## Empieza aquí

- **El método:** [`docs/XJKIT.md`](docs/XJKIT.md). Describe solo la versión vigente. El nombre del fichero es
  estable para que los enlaces no caduquen.
- **El kit:** [`kit/.claude/`](kit/.claude/), cinco commands y la definición del revisor. Se copia dentro de
  cada proyecto; no hay instalador ni copia global. Desde la raíz del proyecto, en PowerShell:

```powershell
git clone --depth 1 https://github.com/Xenix-Solutions/xjkit-agentic-dev-method $env:TEMP\xjkit; New-Item -ItemType Directory -Force .\.claude | Out-Null; Copy-Item -Recurse -Force $env:TEMP\xjkit\kit\.claude\* .\.claude\; Remove-Item -Recurse -Force $env:TEMP\xjkit
```

  Después, abre Claude Code en el proyecto y teclea `/xjkit-init`.

- **Cambios por versión:** [`CHANGELOG.md`](CHANGELOG.md).

## URL estable del documento

```
https://raw.githubusercontent.com/Xenix-Solutions/xjkit-agentic-dev-method/main/docs/XJKIT.md
```

Es la que llevan las skills del kit para que cualquier sesión pueda consultar el método.

## Qué hay aquí y qué no

Este repositorio publica cada versión liberada: el documento, el kit y el changelog. La evolución del método
(decisiones, pruebas, debates) vive en un repositorio privado de Xenix Solutions.

## Licencia

[MIT](LICENSE).
