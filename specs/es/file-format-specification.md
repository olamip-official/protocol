# Especificación del Formato de Archivo OLAMIP

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Descripción General

El archivo OLAMIP (`/olamip.json`) es un documento JSON estructurado que proporciona resúmenes curados de las páginas más importantes de su sitio web. Está diseñado para ser fácilmente interpretado por modelos de lenguaje de gran escala (LLMs), permitiéndoles comprender, priorizar y utilizar su contenido con claridad, precisión e intención.

## Ubicación del Archivo

El archivo OLAMIP debe alojarse en la raíz de su dominio:

`https://yourdomain.com/olamip.json`

## Declaración de la Ubicación del Archivo OLAMIP

Para maximizar la adopción y garantizar que los sistemas puedan localizar de forma fiable su archivo OLAMIP, publique tanto una etiqueta `<link>` como una etiqueta `<meta>` en la sección `<head>` de su sitio.

### Descubrimiento Principal: `<link rel="olamip">`

- Práctica estandarizada: los rastreadores ya analizan etiquetas `<link>` para recursos como canonical, sitemap y alternate.
- Amigable para máquinas: declara una relación formal entre la página y el archivo OLAMIP.
- Interoperabilidad: se integra con estándares web existentes, facilitando la adopción por sistemas de IA sin manejo especial.

### Descubrimiento Secundario: `<meta name="olamip-location">`

- Legible para humanos: fácil de añadir y entender.
- Compatibilidad: algunos analizadores prefieren etiquetas `<meta>`.
- Redundancia: actúa como respaldo si un rastreador no soporta `rel="olamip"`.

### Por Qué Usar Ambos

- Preparado para el futuro: distintos sistemas pueden implementar el descubrimiento de manera diferente.
- Resiliencia: si un método falla, el otro funciona como respaldo.
- Facilidad de integración: los desarrolladores pueden elegir el método más adecuado.
- Claridad: reduce ambigüedad sobre la ubicación del archivo.

### Implementación Recomendada

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## Estructura del Archivo

El archivo OLAMIP debe ser un documento JSON válido codificado en UTF‑8 que contenga:

- `protocol`
- `version`
- `identity`
- `content`
- `metadata`

### Estructura General

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": { "...": "..." },
  "content": { "...": "..." },
  "metadata": { "...": "..." }
}
```

## Objeto Identity

| Campo | Tipo | Requerido | Descripción |
|---|---|---:|---|
| `name` | `string` | Sí | Nombre del sitio web u organización. |
| `type` | `string` | Sí | Tipo de entidad, como `company`, `blog` o `ecommerce`. |
| `canonical_description` | `string` | Sí | Descripción legible del sitio. |
| `tags` | `array<string>` | No | Palabras clave opcionales. |

## Objeto Content

El objeto `content` contiene:

- Un `overview`
- Una lista de `sections`
- Cada sección puede contener `subsections`
- Cada sección o subsección puede contener `entries`

### Objeto Overview

| Campo | Tipo | Requerido | Descripción |
|---|---|---:|---|
| `summary` | `string` | Sí | Explicación concisa del propósito del sitio. |

## Especificación de Section

Una `Section` representa una categoría o agrupación de contenido.

### Campos de Section

| Campo | Tipo | Requerido | Descripción |
|---|---|---:|---|
| `title` | `string` | Sí | Nombre de la sección. |
| `summary` | `string` | Sí | Descripción de la sección. |
| `url` | `string` | Sí | URL canónica. |
| `section_type` | `string` | Sí | Clasificación semántica. |
| `policy` | `string` | No | `"allow"` o `"forbid"`. |
| `tags` | `array<string>` | No | Palabras clave. |
| `priority` | `string` | No | `"high"`, `"medium"`, `"low"`. |
| `published` | `string` | No | Fecha ISO 8601. |
| `language` | `string` | No | Código BCP‑47. |
| `entries` | `array<Entry>` | Sí | Lista de entradas. |
| `subsections` | `array<Section>` | No | Sub-secciones. |

### Valores Permitidos de `section_type`

- `blog_category`
- `news_section`
- `product_collection`
- `doc_category`
- `research_category`
- `project_group`
- `content_section`

## Política y Herencia

El campo `policy` controla si los sistemas de IA pueden ingerir contenido.

Valores válidos:

- `"allow"`
- `"forbid"`

### Comportamiento por Defecto

Si no se define, el valor efectivo es `"allow"`.

### Reglas de Herencia

Orden de evaluación:

1. Nivel Entry
2. Nivel Subsection
3. Nivel Section
4. Valor por defecto (`"allow"`)

### Requisitos para IA

- Respetar la herencia.
- Tratar `"forbid"` como prohibición estricta.
- Tratar `"allow"` como permiso.

## Jerarquía

```text
content
└── sections[]
    ├── Section
    │     ├── entries[]
    │     └── subsections[]
```

## Objeto Entry

Unidad mínima de contenido.

### Campos

| Campo | Tipo | Requerido | Descripción |
|---|---|---:|---|
| `title` | `string` | Sí | Título. |
| `summary` | `string` | Sí | Descripción breve. |
| `url` | `string` | Sí | URL absoluta. |
| `content_type` | `string` | Sí | Clasificación. |
| `policy` | `string` | No | `"allow"` o `"forbid"`. |
| `tags` | `array<string>` | No | Palabras clave. |
| `priority` | `string` | No | `"high"`, `"medium"`, `"low"`. |
| `published` | `string` | No | Fecha ISO. |
| `language` | `string` | No | Código BCP‑47. |
| `metadata` | `object` | No | Datos adicionales. |

## Tipos de Contenido

- `page`
- `landing_page`
- `legal_page`
- `blog_article`
- `news_article`
- `product`
- `service`
- `doc_page`
- `research_paper`
- `dataset`
- `project`
- `media_item`
- `resource`

## Arrays

Campos como `tags` deben ser arrays JSON:

```json
"tags": ["ai", "search", "llm"]
```

## Metadata

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

## Prioridad

Valores:

- `"high"`
- `"medium"`
- `"low"`

Uso:

- `"high"`: contenido crítico.
- `"medium"`: valor por defecto.
- `"low"`: contenido secundario.

## Soporte Multilingüe

Definir `language` en:

- Archivo (`metadata`)
- Sección
- Entrada

Ejemplos:

- `en`
- `es`
- `fr`
- `de`
- `pt-BR`
- `zh-CN`
- `ar`

## Reglas de Validación

- JSON válido.
- URLs absolutas.
- Resúmenes < 500 caracteres.
- Tags en minúsculas.
- Sin comas finales.

## Tags

Reglas:

- Minúsculas
- Una palabra
- ASCII
- Sin espacios
- Guiones permitidos

Ejemplo:

- `machine-learning`
- `data-science`

## Consideraciones Multilingües para Tags

- Usar el idioma del contenido.
- No mezclar idiomas.
- Mantener consistencia.

## Versionado

- Ignorar campos desconocidos.
- Validar contra el esquema más reciente.

## Alineación Semántica

OLAMIP complementa estándares como schema.org, sitemaps y sistemas de rastreo, proporcionando una capa estructurada optimizada para IA que mejora la interpretación, recuperación y priorización del contenido.
