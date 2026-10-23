# Humanitas

Revisiones bibliográficas para **ciencias sociales, humanidades y derecho** con citas que se pueden comprobar. Aplicación web de un solo fichero (`index.html`), sin servidor.

**Usar la app:** https://fborrasumh.github.io/humanitas/

**Idiomas de la interfaz:** español (por defecto), inglés y portugués; selector en la barra superior (o `?lang=en` / `?lang=pt` en la URL). El idioma de la revisión (español, inglés, portugués, francés o alemán) es independiente.

## Qué hace

- Eliges **disciplina** (ciencias sociales, humanidades o derecho) y **tipo de trabajo** (diez por disciplina: revisión doctrinal, análisis jurisprudencial, revisión historiográfica, estado de la cuestión, análisis de políticas públicas…). La disciplina fija el ámbito, la voz de la redacción, el tipo de lagunas que se buscan y la estructura por defecto.
- Busca en seis bases académicas en paralelo (OpenAlex, Semantic Scholar, CrossRef, arXiv —a través de DataCite—, Europe PMC y PubMed), reconoce el **tipo documental** (monografía, capítulo, tesis, informe, artículo), fusiona duplicados por DOI y, si no lo hay, por título con similitud alta y año compatible (para no fundir ediciones distintas), y excluye las obras retractadas que marca OpenAlex.
- **Periodo flexible por defecto:** el rango de años orienta la búsqueda pero no descarta obras anteriores, para no perder el canon. En modo estricto se filtra por año, pero se conservan monografías, capítulos y obras sin año.
- Ordena el corpus con un índice pensado para estas disciplinas: pesa menos el recuento de citas (que mide sobre todo qué está indexado) y suma la condición de monografía o capítulo.
- Permite sembrar la revisión con tu propia bibliografía (BibTeX, RIS o lista de DOI), reconociendo `@book`, `@incollection`, `TY - BOOK`, `CHAP`… Es la forma más fiable de incluir la bibliografía nuclear.
- Fija un **corpus numerado** [1]–[N] y le pide al redactor que cite solo por índice. **El código**, no el modelo, extrae todos los marcadores, elimina los fuera de rango y las citas autor-año inventadas, los convierte al estilo elegido —**Chicago autor-fecha** (por defecto), APA 7, Harvard, MLA 9, Vancouver o IEEE— y deja en la bibliografía solo las obras realmente citadas, **en orden alfabético** en los estilos autor-fecha y con formato propio para libro, capítulo y tesis.
- Muestra una auditoría con métricas calculadas en JavaScript (citas resueltas, densidad, proporción de monografías y capítulos, amplitud temporal, concentración de revistas y editoriales, retractaciones), los hallazgos de los agentes validadores, el diagrama de flujo de selección con los recuentos reales y el consumo real de tokens.
- Antes de gastar en la redacción enseña las obras recuperadas (marcando monografías y capítulos) y deja decidir si continuar.
- Exporta a Word, Markdown, LaTeX, BibTeX (con el tipo de entrada correcto), texto y una **traza JSON** (consultas, prompts, recuentos, corpus, consumo). Las secciones se pueden editar y reescribir una a una.
- Hay un **ejemplo que funciona sin clave ni red**, en los tres idiomas. Todas sus obras son ficticias y están marcadas como `[DEMO]`; incluye a propósito una cita fuera de rango y una autor-año inventada para que se vea cómo las elimina el código.

## Cómo se usa la IA

Con tu propia clave de **OpenAI, Google Gemini o Anthropic Claude**. La clave se guarda solo en el navegador y va solo a tu proveedor. No hay IA local. El modelo se elige en un desplegable con los modelos de tu clave.

El coste lo calculas tú: escribes el precio por millón de tokens de tu proveedor y la app suma los tokens reales que devuelve la API.

## Privacidad

- En tu navegador: revisiones, historial (máximo 20), prompts propios y claves.
- Hacia las bases bibliográficas salen las consultas de búsqueda derivadas del tema (y tu correo o tus claves de esas bases, solo si los escribes).
- Hacia tu proveedor de IA salen el tema, las instrucciones (con correo, DNI/NIE y teléfono enmascarados) y los títulos y resúmenes de las obras recuperadas. Antes del primer envío ves una muestra y confirmas.

## Límites

- Una cita verificada demuestra que la obra está en el corpus, **no que diga lo que la frase afirma**. El agente verificador es un modelo y puede equivocarse.
- Las bases abiertas cubren mal la producción en español y portugués, la doctrina jurídica y muchas monografías; la legislación y la jurisprudencia no se buscan en bases oficiales. Completa siempre con tu propia bibliografía.
- Con nombres de OpenAlex o Semantic Scholar («Nombre Apellido») la app toma la última palabra como apellido, así que un segundo apellido español puede quedar mal en la cita autor-fecha. CrossRef, BibTeX y RIS sí conservan los apellidos compuestos.
- Las puntuaciones del agente `referee` son autoevaluaciones del modelo, no calibradas.
- Si editas o reescribes una sección, las métricas siguen siendo las de la versión generada.
- Probada con IA y bases simuladas; **no** se ha probado en esta versión con claves ni bases reales.

## Autoría

Fernando Borrás Rocher (Universidad Miguel Hernández de Elche). ORCID: [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573)

## Cómo citar

Borrás Rocher, F. (2026). *Humanitas* (v3.0.0) [Software]. https://doi.org/10.5281/zenodo.21423295

## Licencia

MIT. Véase [LICENSE](LICENSE).
