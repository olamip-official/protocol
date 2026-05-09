# Inicio rápido de OLAMIP

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Esta guía rápida muestra cómo implementar OLAMIP en un sitio web de la forma más sencilla posible.

## 1. Crea `olamip.json`

Coloca un archivo `olamip.json` válido en la raíz de tu sitio:

`https://yourdomain.com/olamip.json`

Empieza con una instantánea pequeña y limpia de tus páginas más importantes.

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": {
    "name": "Ejemplo de Sitio Web",
    "type": "blog",
    "canonical_description": "Un sitio web sobre tecnología y novedades de productos.",
    "tags": ["tecnología", "blog"]
  },
  "content": {
    "overview": {
      "summary": "Un blog de tecnología que cubre noticias de productos, tutoriales y artículos de opinión."
    },
    "sections": [
      {
        "title": "Blog",
        "summary": "Artículos y guías.",
        "url": "https://sudominio.com/blog/",
        "section_type": "blog_category",
        "entries": [
          {
            "title": "Bienvenido al blog",
            "summary": "Una introducción al contenido y propósito del sitio.",
            "url": "https://yourdomain.com/blog/bienvenido/",
            "content_type": "blog_article",
            "tags": ["intro", "blog"],
            "priority": "high",
            "language": "es"
          }
        ]
      }
    ]
  },
  "metadata": {
    "last_updated": "2026-04-06",
    "language": "es",
    "source_url": "https://sudominio.com/",
    "copyright": "© 2026 Example Site"
  }
}
```

## 2. Añade etiquetas de descubrimiento a tu HTML

Añade esto a la sección `<head>` de tu página de inicio y de las páginas clave:

```html
<link rel="olamip" href="https://sudominio.com/olamip.json">
<meta name="olamip-location" content="https://sudominio.com/olamip.json">
```

## 3. Mantén los resúmenes concisos

Escribe resúmenes breves y factuales que expliquen de qué trata la página y por qué importa. Evita el lenguaje promocional, el texto duplicado y las descripciones vagas.

Bien:

- “Una guía para principiantes sobre fotografía de larga exposición.”
- “Detalles del producto y precios del plan empresarial.”

Evita:

- “La mejor página de internet.”
- “Contenido increíble que te encantará.”

## 4. Usa los tipos de contenido correctos

Elige el `content_type` más específico que puedas:

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

## 5. Organiza con secciones

Usa `sections` para grupos de contenido y `entries` para páginas individuales. Si hace falta, añade `subsections` para crear una estructura más profunda.

Ejemplo:

- Sección: Blog
- Subsección: Tutoriales
- Entrada: Cómo usar OLAMIP

## 6. Define la prioridad de forma intencional

Usa:

- `high` para tus páginas más importantes.
- `medium` para páginas normales.
- `low` para páginas de nicho o antiguas.

No marques todo como `high`.

## 7. Usa `policy` para controlar la ingestión por IA

El campo `policy` indica a los sistemas de IA si pueden ingerir una sección, subsección o entrada.

- Usa `"allow"` para permitir explícitamente la ingestión.
- Usa `"forbid"` para indicar a los sistemas de IA que eviten una sección, subsección o entrada concreta.

Si omites el campo `policy`, OLAMIP hereda la política del ancestro más cercano. Si ningún ancestro define una política, la política efectiva es `"allow"`. En la mayoría de los sitios, puedes omitir `policy` y usar solo `"forbid"` donde quieras que los sistemas de IA omitan contenido.

Ejemplos:

- Omitir una sección completa:

  ```json
  {
    "title": "Guías obsoletas",
    "summary": "Contenido desactualizado, no apto para IA.",
    "url": "https://sudominio.com/old-guides/",
    "section_type": "doc_category",
    "policy": "forbid"
  }
  ```

- Omitir una sola página:

  ```json
  {
    "title": "Página interna de prueba",
    "summary": "Página usada solo para pruebas.",
    "url": "https://sudominio.com/test/",
    "content_type": "page",
    "policy": "forbid"
  }
  ```

## 8. Añade metadatos de idioma

Usa códigos de idioma BCP-47 como:

- `en`
- `es`
- `fr`
- `de`
- `pt-BR`
- `zh-CN`

Define el idioma a nivel de archivo, de sección o de entrada según sea necesario.

## 9. Actualiza el archivo regularmente

Cada vez que añadas, cambies o elimines páginas importantes, actualiza `olamip.json`.

Si tu sitio cambia con frecuencia, mantén también `olamip-delta.json` en el mismo directorio para que los sistemas de IA puedan mantenerse actualizados entre refrescos completos.

## 10. Valida antes de publicar

Antes de ponerlo en producción, asegúrate de que:

- El JSON sea válido.
- Las URLs sean absolutas.
- Estén presentes los campos obligatorios.
- Los resúmenes sean claros y estén por debajo de la longitud recomendada.
- Las etiquetas estén normalizadas y sean coherentes.
- El archivo se sirva desde la ruta raíz.

## Lista mínima de implementación

- [ ] Crear `olamip.json`.
- [ ] Alojarlo en la raíz del sitio.
- [ ] Añadir etiquetas de descubrimiento al `<head>` del HTML.
- [ ] Incluir secciones y entradas para el contenido clave.
- [ ] Añadir metadatos y campos de idioma.
- [ ] Mantener los resúmenes breves y precisos.
- [ ] Actualizarlo cada vez que cambie el sitio.
- [ ] Añadir `olamip-delta.json` si quieres actualizaciones incrementales.

## Punto de partida sugerido

Si no estás seguro de por dónde empezar, implementa OLAMIP primero solo para tus páginas principales:

- Página de inicio.
- Índice del blog.
- Los mejores artículos.
- Páginas clave de productos o servicios.
- Páginas importantes de documentación.

Después amplíalo desde ahí.
