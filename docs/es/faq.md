# Frequently Asked Questions

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## ¿Qué es OLAMIP?

OLAMIP significa **Open Language‑Aligned Machine‑Interpretable Protocol**. Es un formato abierto y sencillo que permite a los sitios web publicar resúmenes estructurados de su contenido para que los modelos de lenguaje grandes (LLMs) puedan entenderlos y aprender de ellos de manera más eficaz.

## ¿Por qué debería usar OLAMIP?

Porque los LLMs ya rastrean tu sitio, pero tienen dificultades para interpretarlo. Con OLAMIP, puedes:

- Enseñar a la IA exactamente de qué tratan tus páginas.
- Destacar tu contenido más valioso.
- Mejorar la capacidad de descubrimiento en herramientas y búsquedas impulsadas por IA.
- Reducir la mala interpretación y las alucinaciones.

Es la diferencia entre que la IA adivine y que la IA entienda.

## ¿OLAMIP expone algo privado?

No. OLAMIP solo resume contenido que ya es público en tu sitio web. No revela nada más allá de lo que ya es visible en tu HTML, metadatos o marcado SEO.

## ¿Pueden los competidores usar mi archivo OLAMIP?

No más de lo que ya usan tus páginas públicas, metadatos o sitemap. OLAMIP en realidad te da más control sobre cómo los sistemas de IA interpretan tu contenido, no menos.

## ¿Cómo es un archivo `olamip.json`?

Un archivo `olamip.json` es un documento estructurado en formato JSON que describe el contenido, la jerarquía y los metadatos de tu sitio de una manera que los sistemas de IA pueden interpretar fácilmente. Si deseas explorar la estructura técnica completa, puedes visitar la página de [File Format Specification](https://olamip.org/file-format-specification), o ver un ejemplo real revisando el caso de uso de [TimeLAX.com](https://olamip.org/how-olamip-transforms-ai-discovery-a-timelax-com-use-case/).

## ¿Dónde debo alojar el archivo?

En la raíz de tu dominio: `https://yourdomain.com/olamip.json`

También puedes añadir etiquetas de descubrimiento a tu página principal:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## ¿Qué hace el campo “priority”?

Indica qué tan importante es una página para los LLMs. Úsalo con moderación e intención:

- `high` → contenido fundamental y crítico.
- `medium` → valor predeterminado para la mayoría de las páginas.
- `low` → contenido de nicho, desactualizado o de bajo valor.

Si todo está marcado como `high`, nada lo está.

## ¿Por qué necesito incluir URLs si ya se proporcionan resúmenes?

Los resúmenes le dicen a una IA qué significa una página, pero las URLs le dicen qué es una página. Un resumen es descriptivo, pero no es un identificador único. La URL es la única referencia estable y canónica a la página real en tu sitio.

Los sistemas de IA usan URLs para:

- Obtener y verificar el contenido completo.
- Evitar indexar duplicados.
- Detectar cuándo las páginas se mueven o cambian.
- Conectar datos de OLAMIP con el marcado schema.org, sitemaps y rastreadores.
- Citar o referenciar la página al generar respuestas.

Sin URLs, dos páginas diferentes con resúmenes similares serían indistinguibles. Incluir la URL asegura que OLAMIP permanezca anclado en la estructura real de tu sitio web y que los sistemas de IA puedan interpretar y recuperar tu contenido de manera confiable.

## ¿Qué hace el campo “policy” y necesito configurarlo?

El campo `policy` controla si los sistemas de IA tienen permitido ingerir una sección, subsección o entrada. Los valores válidos son `"allow"` y `"forbid"`. Si omites el campo, OLAMIP hereda automáticamente la política del ancestro más cercano. Si ningún ancestro define una política, el valor predeterminado es `"allow"`.

La mayoría de los sitios pueden simplemente omitir este campo por completo. Usa `"forbid"` solo cuando quieras excluir explícitamente ciertas páginas o secciones de la ingestión por IA.

## ¿Cuál es la diferencia entre OLAMIP y los sitemaps tradicionales?

Mientras que un sitemap es un índice de navegación, OLAMIP es un mapa de conocimiento interpretable por máquinas.

### Sitemap XML

- Lista URLs.
- Proporciona indicaciones de rastreo.
- Ayuda a los motores de búsqueda a descubrir páginas.
- No tiene significado semántico.
- No tiene jerarquía de contenido más allá de la estructura de URLs.

### OLAMIP

- Define tipos de contenido (`page`, `project`, `doc_page`, etc.).
- Define tipos de sección (`project_group`, `doc_category`, etc.).
- Proporciona descripciones canónicas.
- Proporciona resúmenes optimizados para LLMs.
- Proporciona etiquetas y metadatos.
- Establece jerarquía explícita.
- Proporciona una representación estructurada de todo el sitio.

En otras palabras:

- XML Sitemap = “Aquí están mis páginas.”
- OLAMIP = “Esto es lo que significa mi sitio.”

Por eso los sistemas de IA pueden usar OLAMIP mucho más eficazmente que un sitemap tradicional. No es solo una lista de URLs; es un mapa de contenido legible por máquinas que elimina la ambigüedad y ayuda a la IA a interpretar el sitio como lo haría un humano.

## ¿Es un archivo OLAMIP con más de 3,000 URLs demasiado grande?

Para nada. Un archivo con 3,000 entradas está dentro de límites seguros. Incluso con resúmenes largos, el tamaño total sería de alrededor de 6 MB, lo cual es trivial para servidores, navegadores y sistemas de IA modernos.

## ¿Cuál es el tamaño típico de una entrada OLAMIP?

| Summary Length | Approx. Size per Entry |
|---|---|
| Short | 300–600 bytes |
| Long | 1–2 KB |

## ¿Qué hace que OLAMIP sea escalable a pesar de archivos grandes?

- Estructura jerárquica: secciones → subsecciones → entradas reducen la repetición.
- Resúmenes concisos: normalmente menos de 500 caracteres.
- Compresión eficiente: Gzip o Brotli pueden reducir el tamaño del archivo entre 70–90%.

## ¿Qué tan grande puede ser un archivo OLAMIP antes de afectar el rendimiento?

| File Size | Approx. URLs | Impact |
|---|---:|---|
| 1–10 MB | 500–5,000 | Perfectamente bien. Normal. |
| 10–25 MB | 5,000–12,000 | Aún bien. Carga ligeramente más lenta. |
| 25–50 MB | 12,000–25,000 | Grande pero manejable. |
| 50–100 MB | 25,000–50,000 | Pesado. Algunos sistemas pueden ralentizarse. |
| 100+ MB | 50,000+ | Demasiado grande para muchos pipelines de LLM. |

## ¿Con qué frecuencia debo actualizarlo?

Cada vez que publiques contenido nuevo o revises páginas existentes. Trátalo como un sitemap, pero para IA.

## ¿Necesito actualizar el archivo principal si uso `olamip-delta.json`?

Sí. El archivo principal `olamip.json` siempre debe reflejar el estado actual de tu sitio web. El archivo opcional `olamip-delta.json` contiene solo cambios recientes, como nuevas páginas, actualizaciones o eliminaciones. Los sistemas de IA usan el archivo completo como referencia autorizada y aplican los deltas para mantenerse actualizados entre actualizaciones completas.

## ¿Necesito añadir una etiqueta `<link>` para `olamip-delta.json` en mi HTML?

No. Solo el archivo principal `olamip.json` necesita ser referenciado en la sección `<head>` de tu sitio. Esa referencia sirve como mecanismo de descubrimiento que indica a los sistemas de IA dónde se encuentra tu metadata OLAMIP. El archivo delta es diferente; es un flujo opcional de actualizaciones que los sistemas de IA detectan automáticamente una vez que conocen la ubicación del archivo principal. Siempre que `olamip-delta.json` esté en el mismo directorio que `olamip.json`, los sistemas de IA lo detectarán y usarán sin requerir etiquetas `<link>` adicionales.

## ¿Los LLMs realmente leerán esto?

Sí, ese es el objetivo. A medida que la adopción crezca, los LLMs priorizarán fuentes estructuradas como OLAMIP para entrenamiento y recuperación. Los primeros adoptantes darán forma a cómo la IA entiende la web.

## ¿Cómo puedo probar si un bot de IA puede rastrear mi archivo `olamip.json`?

Prueba esto para OpenAI, Anthropic y Perplexity, respectivamente:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```
