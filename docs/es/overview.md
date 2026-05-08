# Descripción general de OLAMIP

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP es un estándar abierto que permite a los sitios web comunicarse de forma clara e intencional con los sistemas de IA. En lugar de obligar a los modelos de lenguaje grandes (LLMs) a adivinar el significado a partir de HTML, CSS y JavaScript ruidosos, OLAMIP ofrece una representación limpia, estructurada e interpretable por máquinas del contenido más importante de tu sitio.

Este documento ofrece una introducción general al propósito, la filosofía, el formato del archivo y el modelo de actualización de OLAMIP.

---

## 1. Por qué se creó OLAMIP

Los sitios web modernos están diseñados para navegadores, no para IA. Como resultado, los LLM tienen dificultades con:

- HTML con mucho diseño y estructura.
- Contenido renderizado con JavaScript.
- Menús de navegación, anuncios y texto repetitivo.
- Páginas duplicadas o irrelevantes.

Los LLM *ya* rastrean tu sitio, pero a menudo lo interpretan mal. OLAMIP resuelve esto proporcionando a los sistemas de IA exactamente lo que necesitan:

- Un archivo JSON ligero (`/olamip.json`).
- Resúmenes limpios, curados por humanos.
- Metadatos estructurados.
- URL canónicas.
- Una jerarquía clara de secciones, subsecciones y entradas.

Al adoptar OLAMIP, tomas el control de cómo la IA entiende y representa tu contenido.

---

## 2. Filosofía central: amigable para humanos + amigable para máquinas

OLAMIP está diseñado como un puente de comunicación entre humanos y IA. Cada parte del protocolo debe ser:

| Principio | Amigable para humanos | Amigable para máquinas |
|-----------|------------------------|------------------------|
| Formato de archivo | Fácil de escribir y validar | JSON estricto y predecible |
| Nombres de campos | Intuitivos y autoexplicativos | En minúsculas y compatibles con schema |
| Resúmenes | Lenguaje claro y alineado con la marca | Conciso, factual y semánticamente rico |
| Sistema de prioridad | Etiquetas simples (`"high"`, `"medium"`, `"low"`) | Se puede mapear a pesos de clasificación |
| Documentación | Guías en lenguaje sencillo | JSON schemas, reglas de validación |
| Herramientas | Generadores, plugins CMS | Herramientas CLI, analizadores, suites de prueba |

- **Para humanos:** OLAMIP debe ser fácil de adoptar sin necesidad de conocimientos técnicos profundos.  
- **Para máquinas:** OLAMIP debe ser estructurado, predecible y sin ambigüedad.

---

## 3. El archivo OLAMIP (`/olamip.json`)

Un archivo OLAMIP es un documento JSON estructurado alojado en:

`https://yourdomain.com/olamip.json`

Contiene:

- `protocol` — debe ser `"OLAMIP"`.
- `version` — versión del protocolo.
- `identity` — quién eres.
- `content` — la jerarquía estructurada de tu sitio.
- `metadata` — idioma, fecha de última actualización y otros campos globales.

### 3.1 Etiquetas de descubrimiento

Para garantizar que los sistemas de IA puedan localizar de forma fiable tu archivo OLAMIP, incluye ambas:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Usar ambas proporciona redundancia, compatibilidad y preparación para el futuro.

---

## 4. Estructura del archivo

### 4.1 Objeto Identity

Describe el sitio web o la organización:

- `name` — obligatorio.
- `type` — obligatorio.
- `canonical_description` — obligatorio.
- `tags` — opcional.

### 4.2 Objeto Content

Contiene:

- Un `overview`.
- `sections`.
- `subsections` opcionales.
- `entries` (las unidades de contenido más granulares).

Esto admite una profundidad de anidación ilimitada.

### 4.3 Secciones

Una Section agrupa contenido relacionado. Los campos obligatorios incluyen:

- `title`.
- `summary`.
- `url`.
- `section_type`.

Los campos opcionales incluyen:

- `policy` (`"allow"` o `"forbid"`).
- `tags`.
- `priority`.
- `published`.
- `language`.
- `subsections`.
- `entries`.

**Herencia de política:**  
Si se omite, la política se hereda de los ancestros. El valor predeterminado es `"allow"`.

### 4.4 Entradas

Las entradas representan elementos individuales de contenido, como:

- Artículos de blog.
- Noticias.
- Productos.
- Páginas de documentación.
- Trabajos de investigación.
- Elementos multimedia.

Campos obligatorios:

- `title`.
- `summary`.
- `url`.
- `content_type`.

Campos opcionales:

- `policy`, `tags`, `priority`, `published`, `language`, `metadata`.

Las URL son obligatorias porque identifican de forma única el contenido y permiten a los sistemas de IA verificar, deduplicar y cruzar referencias entre páginas.

---

## 5. Metadatos y soporte de idioma

El objeto metadata puede incluir:

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### Soporte multilingüe

El idioma puede definirse en:

- Nivel de archivo.
- Nivel de sección.
- Nivel de entrada.

Usa códigos de idioma BCP‑47 (por ejemplo, `en`, `es`, `pt-BR`, `zh-CN`).

Esto ayuda a los sistemas de IA a:

- Elegir los tokenizadores correctos.
- Evitar mezclar idiomas.
- Mejorar la precisión de recuperación.
- Reducir las alucinaciones.

---

## 6. Etiquetas y prioridad

### 6.1 Etiquetas

Las etiquetas proporcionan señales semánticas ligeras. Deben ser:

- En minúsculas.
- De una sola palabra.
- ASCII.
- Con guiones para conceptos de varias palabras.
- Consistentes en todas las entradas.

Ejemplos:

| Concepto | Etiqueta válida |
|---------|------------------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

Las etiquetas ayudan a los sistemas de IA a agrupar, desambiguar y recuperar contenido con mayor precisión.

### 6.2 Prioridad

Valores:

- `high` — contenido insignia.
- `medium` — valor predeterminado.
- `low` — contenido de nicho o desactualizado.

Usa `high` con moderación (5–10% de las entradas).

---

## 7. Actualizaciones delta (`olamip-delta.json`)

Los sitios evolucionan constantemente. OLAMIP admite actualizaciones incrementales mediante un archivo complementario opcional:

`/olamip-delta.json`

Este archivo contiene solo:

- Entradas añadidas.
- Entradas actualizadas.
- URL eliminadas.

### 7.1 Por qué importan las actualizaciones delta

- Los sistemas de IA se mantienen actualizados sin reprocesar el archivo completo.
- Los catálogos grandes siguen siendo manejables.
- Las actualizaciones se propagan rápidamente.
- No se pierden cambios.

### 7.2 Ventana delta continua (recomendada)

Mantén todos los deltas de los últimos 7 a 30 días en un solo archivo.  
Los sistemas de IA los aplican cronológicamente para mantenerse sincronizados.

### 7.3 Archivos delta versionados (alternativa)

Publica archivos separados:

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`  
...

Ideal para sitios de alto volumen (noticias, comercio electrónico, documentación).

### 7.4 Descubrimiento

Solo el archivo principal de OLAMIP necesita etiquetas `<link>` y `<meta>`.  
Los sistemas de IA buscan automáticamente archivos delta en el mismo directorio.

---

## 8. OLAMIP frente a los sitemaps tradicionales

- Sitemap XML: “Aquí están mis páginas.”
- OLAMIP: “Aquí está lo que significa mi sitio.”

Los sitemaps enumeran URL.  
OLAMIP proporciona:

- Resúmenes.
- Tipos de contenido.
- Jerarquía de secciones.
- Etiquetas.
- Metadatos.
- Prioridades.
- Políticas de ingestión.

Junto con schema.org, OLAMIP ofrece a los sistemas de IA una comprensión completa y curada por humanos de tu sitio.

---

## 9. Preguntas frecuentes (resumen)

### ¿OLAMIP expone información privada?

No. Solo resume contenido disponible públicamente.

### ¿Pueden los competidores usar mi archivo OLAMIP?

No más de lo que ya usan tus páginas públicas.

### ¿Qué tamaño puede tener un archivo OLAMIP?

1–10 MB es lo habitual.  
Incluso 3,000+ entradas es normal y seguro.

### ¿Con qué frecuencia debo actualizarlo?

Cada vez que cambie el contenido — trátalo como un sitemap para IA.

### ¿Debo actualizar el archivo principal si uso deltas?

Sí. `olamip.json` es siempre la instantánea autorizada.

### ¿Los LLM realmente lo leerán?

Sí — ese es el propósito del protocolo.  
Puedes probarlo con:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```

---

## 10. Resumen

OLAMIP transforma tu sitio web en un mapa de conocimiento interpretable por máquinas:

- `olamip.json` — tu instantánea completa y estructurada.
- `olamip-delta.json` — tu registro incremental de cambios.
- Secciones, subsecciones, entradas — una jerarquía clara.
- Resúmenes, etiquetas, prioridades — significado curado por humanos.
- Etiquetas de descubrimiento — ingestión fiable.
- Soporte multilingüe — preparación global.

Al adoptar OLAMIP, aseguras que los sistemas de IA comprendan tu contenido como tú pretendes: con precisión, de forma consistente y a escala.
