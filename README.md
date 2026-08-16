# ◈ Coty

Comparador, visor y editor de archivos de texto — todo en un solo archivo HTML, sin servidor, sin build, sin dependencias que instalar. Ábrelo en el navegador y ya.

> Nacido para comparar `.md`, terminó soportando 9 formatos, edición en caliente al disco real, historial de versiones, y exportación a documentos HTML standalone con su propio diseño.

---

## Por qué existe

La mayoría de comparadores de texto son o bien herramientas de terminal (`diff`, `git diff`) sin interfaz visual amigable, o servicios web que requieren subir tus archivos a un servidor de terceros. Cotejo es lo contrario: un solo archivo `.html` que puedes abrir localmente, sin conexión, sin cuenta, sin que tu contenido salga de tu navegador — hasta que tú decidas exportarlo o guardarlo.

Pensado mobile-first: la mayor parte de su diseño se probó y ajustó trabajando desde el teléfono, no desde una laptop.

## Uso

1. Descarga `coty.html`.
2. Ábrelo en cualquier navegador moderno (Chrome, Edge, Firefox, Safari).
3. Arrastra archivos o usa el botón **Abrir**.

No hay instalación, no hay `npm install`, no hay servidor que levantar. Todas las librerías (marked, highlight.js, KaTeX, jsdiff, Twemoji) cargan vía CDN la primera vez que abres el archivo; si trabajas sin conexión después de la primera carga, algunas dependen de que el navegador las tenga cacheadas.

## Formatos soportados

| Extensión | Vista | Edición | Exportar (▶) |
|---|---|---|---|
| `.md` `.markdown` `.txt` | Markdown renderizado (tablas, LaTeX, código) | En modo fuente (`</>`) | HTML standalone con diseño propio |
| `.py` `.js` `.c` `.h` `.cpp` `.cc` `.hpp` | Coloreado (highlight.js) | Sí, siempre | Modo *literate*: comentarios como prosa, código como fragmentos |
| `.html` `.htm` | Coloreado | — | Preview real ejecutando el archivo en iframe sandboxeado |
| `.yaml` `.yml` `.glsl` `.vert` `.frag` | Coloreado | Sí, siempre | — |
| `.csv` | Tabla | Sí (edita el crudo) | HTML standalone con la tabla |
| `.json` | Árbol plegable | Sí (edita el crudo, valida al guardar) | HTML standalone con el árbol |

## Funciones principales

**Comparar y fusionar** — diff línea por línea (vía [jsdiff](https://github.com/kpdecker/jsdiff)) entre dos archivos cualesquiera, con contador de cambios y selección de hunks para merge manual.

**Editor in-panel** — edición directa sobre el contenido ya coloreado (`contenteditable`), con números de línea que respetan el ajuste visual del texto (wrap), búsqueda con resaltado, y guardado con dos rutas:
- **Guardar en disco**: escribe al archivo real usando la [File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API) — soportado en Chrome/Edge/Android, no en Firefox/Safari.
- **Descargar**: genera una descarga normal, funciona en cualquier navegador.

Un indicador visual (punto de color) marca los archivos con cambios sin persistir, y la app avisa antes de cerrarlos por accidente.

**Historial de versiones** — snapshots automáticos (al guardar) o manuales, guardados en una carpeta `.history/` centralizada junto a metadata (fecha, tamaño, trigger). Permite ver el diff contra cualquier versión anterior o restaurarla. Funciona tanto con escritura directa (File System API) como en modo solo-lectura vía selección de carpeta (`webkitdirectory`) para navegadores sin esa API.

**Exportación a HTML standalone** — el botón ▶ convierte un Markdown en un documento HTML independiente con:
- Hero animado, secciones con scroll-reveal (detecta automáticamente el nivel de heading que estructura el documento)
- Soporte LaTeX vía [KaTeX](https://katex.org/)
- Bloques de código resaltados con botón de copiar
- Tema claro/oscuro togglable, sincronizado con el tema activo de Cotejo
- Emojis renderizados con [Twemoji](https://github.com/jdecked/twemoji) para verse igual en cualquier plataforma

**Modo literate** — para `.py`/`.js`/`.c`/`.cpp`, el botón ▶ separa comentarios de código y los presenta como un documento narrativo intercalado, en el mismo orden en que aparecen en el archivo — útil para convertir un script comentado en documentación legible sin mantener dos archivos separados.

**Preview real de HTML** — abre `.html`/`.htm` ejecutándose de verdad en un iframe con `sandbox`. Incluye un candado 🔓/🔒 en el menú de ajustes para bloquear la ejecución de JavaScript en archivos de origen no confiable — con `allow-scripts` nunca combinado con `allow-same-origin`, así el script corre aislado sin acceso al resto de la página.

**Crear archivo** — desde el menú de ajustes, crea un archivo vacío del formato que elijas y entra directo en modo edición.

## Privacidad y seguridad

- Todo el procesamiento ocurre en el navegador. Ningún archivo se sube a ningún servidor.
- El acceso a disco (guardado directo, historial) requiere permiso explícito del usuario vía los diálogos nativos del navegador — Cotejo no puede leer ni escribir nada sin que tú lo autorices carpeta por carpeta.
- El preview de HTML corre en un iframe sandboxeado; con el candado activo el script del archivo no tiene forma de acceder a cookies, `localStorage` o el DOM de Cotejo.

## Limitaciones conocidas

- El guardado directo a disco y el historial con escritura requieren la File System Access API — no disponible en Firefox ni Safari. En esos navegadores, Cotejo cae automáticamente a descarga de archivos sueltos.
- El permiso de carpeta (tanto para edición como para historial) no persiste entre sesiones — es una restricción de seguridad del navegador, no de la app.
- El modo literate cubre lenguajes con comentarios estilo `#` o `/* */`/`//`; no está pensado para todos los lenguajes de programación existentes.

## Stack

Un solo archivo HTML. Sin build, sin bundler, sin `package.json`. Librerías cargadas vía CDN:

- [marked](https://github.com/markedjs/marked) — parser de Markdown
- [highlight.js](https://highlightjs.org/) — resaltado de sintaxis
- [KaTeX](https://katex.org/) — renderizado de LaTeX
- [jsdiff](https://github.com/kpdecker/jsdiff) — motor de diff
- [Twemoji](https://github.com/jdecked/twemoji) — emojis consistentes entre plataformas

## Filosofía

Sin servidor, sin cuenta, sin telemetría, sin monetización. Auditable de punta a punta abriendo "Ver código fuente" en el navegador — todo el comportamiento de la app está en ese único archivo.

---

*Desarrollado de forma colaborativa, iterando directo sobre uso real en dispositivos móviles.*
