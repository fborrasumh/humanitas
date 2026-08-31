# Changelog — Humanitas

Todas las versiones siguen [Keep a Changelog](https://keepachangelog.com/es/1.1.0/)
y [Versionado Semántico](https://semver.org/lang/es/).

---

## [2.0.0] — 2026-08-31

Versión centrada en la **integridad de las citas** y en adecuar la herramienta a cómo se
cita realmente en humanidades, ciencias sociales y derecho. En la v1 el redactor escribía
a partir de un texto generado por otro agente, citaba con marcadores de tipo
`[Autor, Año]` que nada garantizaba que existieran, y la lista de referencias usaba
numeración `[1] [2] [3]`: dos sistemas de citación incompatibles conviviendo en el mismo
documento. En la v2 las citas se anclan a un corpus numerado y se verifican en el
navegador con código determinista.

### Añadido

**Citación anclada y verificable**
- El corpus recuperado se congela como lista numerada `[1]…[N]` y se entrega íntegro al
  redactor, que solo puede citar por índice.
- Validación determinista en JavaScript: se extraen todos los marcadores, se eliminan los
  que caen fuera de rango y las citas de tipo `[Autor, Año]`. La comprobación no se delega
  al modelo de lenguaje.
- Si una sección se genera sin ninguna cita, se reescribe automáticamente exigiéndolas.
- **Conversión real al estilo elegido.** Chicago, APA y Harvard producen citas
  autor-fecha; MLA produce autor sin fecha; IEEE y Vancouver quedan numéricas. En los
  estilos autor-fecha la bibliografía se alfabetiza; en los numéricos se renumera por
  orden de aparición.
- Las referencias finales son **solo las efectivamente citadas**.
- `narrator` y `polish` se descartan automáticamente si su reescritura destruye citas.

**Tratamiento de monografías y capítulos**
- Nuevo campo de tipo documental propagado desde OpenAlex y CrossRef, con formato de
  referencia diferenciado para libro, capítulo, tesis y artículo.
- El índice de relevancia premia la condición de monografía en lugar de penalizarla, y
  reduce el peso del recuento bruto de citas (del 45 % al 28 %), que en estas disciplinas
  mide sobre todo qué está indexado.
- Métrica de proporción de monografías y capítulos, con aviso cuando la bibliografía es
  exclusivamente de artículos.
- Aviso en el checkpoint cuando no se ha recuperado ninguna monografía, sugiriendo
  importar la bibliografía nuclear.

**Panel de Auditoría (nuevo)**
- Métricas verificables: citas resueltas, densidad, cobertura del corpus, monografías,
  amplitud temporal, concentración de editoriales y revistas (HHI), acceso abierto y
  retractaciones.
- Tabla de citación por sección.
- Hallazgos de `skeptic`, `verifier` y `thread`, que en la v1 se ejecutaban y se
  descartaban sin usarse.
- Desglose del consumo real de tokens y coste por agente.

**Fuentes y corpus**
- Checkpoint de fuentes antes de redactar, que no existía: permite revisar lo recuperado
  y cancelar antes de gastar tokens en la redacción.
- Comprobación de retractaciones en OpenAlex, sin coste de tokens.
- Deduplicación por DOI normalizado con fusión campo a campo; similitud de Jaccard
  ≥ 0,88 con coincidencia de año como criterio subsidiario, más estricto que en apps
  biomédicas por la abundancia de ediciones y series con títulos casi idénticos.
- Búsquedas en paralelo con concurrencia configurable.
- Importación de bibliografía propia en BibTeX, RIS o listas de DOI, con reconocimiento
  del tipo documental.

**Trazabilidad y exportación**
- Diagrama de flujo de selección en SVG descargable, con los recuentos reales.
- Sección metodológica redactada sobre cifras reales, no inventadas por el modelo.
- Exportación a `.docx` OOXML real, generada sin dependencias externas.
- Exportación de las obras citadas en BibTeX, con el tipo de entrada correcto
  (`@book`, `@incollection`, `@article`).
- Exportación de `humanitas_run.json` con consultas, corpus, métricas y consumo.

**Interfaz**
- Contador de coste real en vivo y vista previa en streaming.
- Nuevos conmutadores: auditoría de contenido, checkpoint de fuentes, exclusión de
  retractados, pulido de estilo, streaming y periodo flexible.

### Cambiado

- **El filtro temporal ahora funciona.** En la v1 el deslizador de años era decorativo:
  su valor solo aparecía en el texto de la metodología y no filtraba ni acotaba ninguna
  búsqueda. Al implementarlo se comprobó que una ventana de treinta años elimina el canon
  disciplinar, así que el periodo es **flexible por defecto** —orienta las consultas sin
  descartar obras anteriores— y, en modo estricto, las monografías y capítulos se
  conservan igualmente. El rango sube hasta 80 años.
- **PubMed** pasa de `esummary` a `efetch` en XML: los resúmenes ahora se recuperan de
  verdad. En la v1 el campo llegaba siempre vacío.
- **OpenAlex, CrossRef, Europe PMC y Semantic Scholar** reciben el rango temporal y
  devuelven tipo documental y editorial.
- El límite de resumen sube de 500 a 900–1.200 caracteres.
- El historial y el borrador migran de `localStorage` a **IndexedDB**, con 20 entradas y
  su traza. Migración automática.
- La exportación a Word deja de ser HTML renombrado a `.doc`.
- El `narrator`, que en la v1 se ejecutaba siempre sin posibilidad de desactivarlo,
  queda **desactivado por defecto**: era una tercera reescritura del mismo texto.
- El «Quality Score /100» se sustituye por «citas resueltas». El juicio del `referee` se
  mantiene, etiquetado como autoevaluación no calibrada.
- arXiv usa una cadena de reintentos en lugar de depender de un único proxy.
- Prompts reescritos en español, con la regla de citación por índice como restricción
  explícita, y estructuras por defecto en el idioma de salida.
- **La presentación ya no anuncia fuentes que no se consultan.** El onboarding afirmaba
  interrogar JSTOR, SSRN, PhilPapers, HeinOnline y Dialnet; ninguna de ellas aparecía en
  el código. Ahora se nombran las seis bases realmente consultadas.

### Corregido

- **Clave de API de Semantic Scholar expuesta** en el código fuente servido públicamente.
  Eliminada; se sustituye por un campo opcional para la clave propia del usuario.
  *Quien haya desplegado la v1 debe revocar esa clave.*
- **Inyección de HTML** a través de títulos de obras, texto de lagunas y tema del usuario.
  Todo el contenido de terceros se escapa antes de insertarse en el DOM.
- Los agentes `skeptic`, `verifier` y `thread` se invocaban y su respuesta se descartaba
  sin asignar, consumiendo tokens sin efecto alguno.
- Incoherencia entre el sistema de citación del texto y el de la lista de referencias.
- Deduplicación por los primeros 50 caracteres del título, que fusionaba obras distintas.
- Fallos silenciosos al guardar en `localStorage` por exceso de cuota.
- La estimación de coste era una constante sin relación con el consumo real.

### Seguridad

- Revocar la clave de Semantic Scholar de la v1 es **obligatorio** al actualizar. La misma
  credencial estaba embebida en más de una aplicación del catálogo.

---

## [1.x] — anterior

Pipeline de 20 agentes en 5 fases, selector de disciplina (ciencias sociales, humanidades,
derecho) y tipo de trabajo, 6 bases bibliográficas, asistente de 3 pasos, detección de
lagunas doctrinales, editor de tokens por agente y exportación a Markdown, `.doc`, LaTeX
y texto plano.
