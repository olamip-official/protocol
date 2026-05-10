# Especificación del Formato de Archivo OLAMIP-DELTA

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

El protocolo OLAMIP-DELTA define un formato de archivo delta basado en JSON para actualizaciones incrementales de `olamip.json`. Esta especificación está dirigida a administradores web e implementadores que ya están familiarizados con datos estructurados, HTML y publicación tipo sitemap.

## 1. Descripción General

`olamip.json` es la representación autoritativa y completa de la estructura y el contenido de su sitio. `olamip-delta.json` es un archivo complementario opcional que describe los cambios realizados desde la última actualización.

Juntos forman un flujo de cambios interpretable por máquinas:

- `olamip.json`: instantánea completa y estructurada.
- `olamip-delta.json`: registro incremental de cambios.

---

## 2. Conceptos Principales

- **Entry** — un elemento de contenido de nivel hoja, por ejemplo un artículo de blog, una página de producto o una página de documentación.
- **Section / Subsection** — una agrupación jerárquica, por ejemplo una categoría de blog, una categoría de documentación, una colección de productos o un grupo de proyectos.
- **Operation** — una de `added`, `updated`, `removed`.
- **Delta** — un conjunto de cambios delimitado por fecha que contiene `added`, `updated` y `removed`.

Este formato está pensado para ser:

- **Primero interpretable por máquinas**, pero lo bastante legible para revisarlo manualmente.
- **Incremental**, para que los sistemas de IA puedan mantenerse sincronizados sin reprocesar todo el sitio.

---

## 3. Ubicación del Archivo y Descubrimiento

### 3.1 Ubicación del Archivo

`olamip.json` debe alojarse en la raíz de su dominio:

```text
https://yourdomain.com/olamip.json
```

`olamip-delta.json` debe alojarse en el mismo directorio:

```text
https://yourdomain.com/olamip-delta.json
```

Los deltas versionados (opcional) viven en el mismo directorio:

```text
/olamip.json
/olamip-delta-YYYY-MM-DD.json
```

### 3.2 Mecanismo de Descubrimiento

Los administradores web deben declarar la ubicación de `olamip.json` usando ambas etiquetas en el `<head>` del sitio:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Los sistemas de IA que conocen `olamip.json` buscarán automáticamente `olamip-delta.json` en el mismo directorio y aplicarán los deltas según esta especificación. No hace falta añadir una etiqueta `<link>` o `<meta>` separada para `olamip-delta.json`.

---

## 4. Estructura de Nivel Superior

En el nivel superior, `olamip-delta.json` es un objeto JSON con los siguientes campos:

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "window_days": 7,
  "last_updated": "2026-04-28",
  "deltas": [
    {
      "date": "2026-04-27",
      "added": [
        {
          "title": "Ejemplo de entrada",
          "url": "https://example.com/page/",
          "summary": "...",
          "content_type": "blog_article"
        }
      ],
      "updated": [
        {
          "url": "https://example.com/page/",
          "summary": "Resumen actualizado"
        }
      ],
      "removed": [
        {
          "url": "https://example.com/removed-page/"
        }
      ]
    }
  ]
}
```

### 4.1 Campos Obligatorias de Nivel Superior

- `protocol` (string, requerido)  
  Debe ser `"OLAMIP-DELTA"`.  
  Esto distingue el archivo delta del archivo principal OLAMIP y de otros formatos delta.

- `version` (string, requerido)  
  Debe coincidir con el esquema de versión documentado, por ejemplo `"1.0.0"`.  
  Esto permite a los implementadores detectar cambios de esquema.

- `window_days` (integer, opcional)  
  Número de días de la ventana móvil; debe ser positivo (normalmente 7–30).  
  Solo es obligatorio si se usa un archivo delta móvil.

- `last_updated` (string, requerido)  
  Fecha en que este archivo delta fue modificado por última vez, en formato ISO‑8601 `YYYY-MM-DD`.  
  Esto ayuda a los rastreadores a decidir cuándo volver a leer el archivo.

- `deltas` (array, requerido)  
  Matriz no vacía de conjuntos de cambios por fecha.  
  Cada elemento es un **objeto delta** con `date`, `added`, `updated` y `removed`.

### 4.2 Matriz `deltas` y Ordenación por Fecha

- `deltas` debe estar ordenada por `"date"` en orden ascendente (más antigua primero, más reciente al final).
- Cada objeto delta debe tener un campo `"date"` en formato ISO‑8601 `YYYY-MM-DD`.
- `deltas` no puede estar vacía.

Este orden no es opcional para cumplir con la especificación: los sistemas de IA asumen que los deltas están ordenados cronológicamente.

---

## 5. Estructura del Objeto Delta

Cada elemento de `deltas` es un objeto delta con esta forma:

```json
{
  "date": "2026-04-27",
  "added": [ ... ],
  "updated": [ ... ],
  "removed": [ ... ]
}
```

Para claridad e interpretación consistente por máquinas, los administradores web **DEBEN** incluir los tres campos de operación en cada objeto delta, incluso cuando algunos estén vacíos.

Campos:

- `date` (string) — fecha ISO‑8601 `YYYY-MM-DD`.
- `added` (array) — cero o más elementos añadidos.
- `updated` (array) — cero o más elementos modificados.
- `removed` (array) — cero o más elementos eliminados.

Ejemplo de un delta solo de adiciones:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Nueva página",
      "summary": "Una nueva página añadida al sitio.",
      "url": "https://example.com/new-page/"
    }
  ],
  "updated": [],
  "removed": []
}
```

Este patrón es obligatorio incluso cuando solo aplica una operación en un día dado.

---

## 6. Operaciones

### 6.1 `added`

Un elemento `added` es un **objeto completo** del tipo aplicable (entrada, sección o subsección). Los campos deben coincidir con la especificación del formato OLAMIP para ese tipo.

Para una **entrada**:

- `title` (string, requerido) — título legible por humanos.
- `summary` (string, requerido) — descripción concisa.
- `url` (string, requerido) — URL canónica absoluta.
- `content_type` (string, requerido) — tomado de la taxonomía `content_type` de OLAMIP.
- `tags` (array, opcional) — lista de etiquetas en minúsculas y de un solo token.
- `priority` (string, opcional) — `"high"`, `"medium"` o `"low"`.
- `policy` (string, opcional) — `"allow"` o `"forbid"`.
- `language` (string, opcional) — código de idioma BCP‑47.
- `metadata` (object, opcional) — datos estructurados específicos del dominio.

Para una **section o subsection**, los objetos `added` siguen la especificación a nivel de sección de OLAMIP, con `title`, `summary`, `url`, `section_type`, `policy`, `tags`, `priority`, etc.

#### Reglas para `added`

- Los elementos `added` deben incluir `url`.
- Los elementos `added` deben ser objetos completos, como mínimo con todos los campos obligatorios de la especificación principal OLAMIP.
- Ninguna `url` puede aparecer más de una vez dentro del mismo objeto delta.

Ejemplo:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Nueva entrada del blog",
      "summary": "Una breve introducción a la nueva funcionalidad.",
      "url": "https://example.com/blog/new-feature/",
      "content_type": "blog_article",
      "tags": ["feature", "new"],
      "priority": "medium"
    }
  ],
  "updated": [],
  "removed": []
}
```

### 6.2 `updated`

Un elemento `updated` debe incluir `url` y puede ser cualquiera de los dos tipos siguientes:

- Un **objeto parcial** que contenga solo los campos modificados, o
- Un **objeto completo** que contenga todos los campos.

En cualquier elemento `updated`, los campos ausentes se interpretan como **sin cambios**.

#### Ejemplo de actualización parcial

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "url": "https://example.com/post/to-update/",
      "summary": "Resumen actualizado con nueva información.",
      "tags": ["feature", "update", "docs"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Ejemplo de sustitución completa

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "title": "Título actualizado de la entrada",
      "summary": "Resumen actualizado con nueva información.",
      "url": "https://example.com/post/to-update/",
      "content_type": "blog_article",
      "tags": ["feature", "update", "docs"],
      "priority": "medium"
    }
  ],
  "added": [],
  "removed": []
}
```

#### Reglas para `updated`

- Los elementos `updated` deben incluir `url`.
- `updated` puede ser un objeto parcial (solo los campos cambiados) o un objeto completo (sustitución total).
- Los elementos `updated` no deben cambiar la identidad de una entrada mediante reasignación de `url`; eso es una decisión de configuración, no del archivo delta.

### 6.3 `removed`

Un elemento `removed` es **mínimo** y solo de identidad.

```json
{
  "url": "https://example.com/page/to-remove/"
}
```

#### Reglas para `removed`

- Los elementos `removed` deben incluir `url` y **no deben** incluir ningún otro campo.
- `url` debe ser una URL absoluta y canónica.
- `removed` se aplica a la URL dada y a sus descendientes estructurales (subsecciones y entradas), si existen.
- Ninguna `url` puede aparecer más de una vez dentro del mismo objeto delta.

Ejemplo:

```json
{
  "date": "2026-04-27",
  "removed": [
    {
      "url": "https://example.com/old-page/"
    }
  ],
  "added": [],
  "updated": []
}
```

---

## 7. URL e Identidad del Objeto

OLAMIP‑DELTA usa URLs canónicas como identidad principal para entradas, secciones y subsecciones.

### Reglas de identidad

- `url` debe ser una URL absoluta, incluyendo esquema y dominio.
- Ninguna `url` puede aparecer más de una vez dentro del mismo objeto delta.
- Cuando se elimina una sección, todos sus descendientes (subsecciones y entradas) se consideran eliminados lógicamente.
- Cambiar una URL requiere un **remove** seguido de un **add**, no una actualización in situ de `url`.

### Buenas prácticas para URLs

- Use URLs canónicas estables que no cambien con frecuencia.
- Evite parámetros de consulta o fragmentos específicos de sesión.
- Si una página se mueve, actualice las redirecciones del sitio y trate el cambio como una cuestión de autoridad de contenido a nivel de URL, no como una responsabilidad del archivo delta.

---

## 8. Ventana Delta Móvil (Recomendada)

La **ventana delta móvil** es el modo recomendado para `olamip-delta.json`.

En este modelo:

- El archivo delta contiene cambios de los últimos `window_days`.
- Los sistemas de IA leen todos los deltas del archivo y los aplican en orden cronológico.
- Esto evita pérdidas de actualizaciones cuando los rastreadores se retrasan.

### Ejemplo de ventana móvil (7 días)

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "window_days": 7,
  "last_updated": "2026-04-28",
  "deltas": [
    {
      "date": "2026-04-22",
      "added": [
        {
          "title": "Nuevo producto A",
          "url": "https://example.com/product/a",
          "summary": "Una nueva incorporación al catálogo."
        }
      ],
      "updated": [],
      "removed": []
    },
    {
      "date": "2026-04-23",
      "added": [],
      "updated": [
        {
          "url": "https://example.com/product/b",
          "summary": "Descripción actualizada con nuevas funciones."
        }
      ],
      "removed": []
    },
    {
      "date": "2026-04-24",
      "added": [],
      "updated": [],
      "removed": [
        {
          "url": "https://example.com/product/c"
        }
      ]
    }
  ]
}
```

Los sistemas de IA:

- Leen todos los deltas del archivo.
- Los aplican en orden cronológico.
- Terminan sincronizados con el estado más reciente.

---

## 9. Deltas Versionados (Alternativa)

Algunos administradores web pueden preferir **deltas versionados**, donde cada archivo contiene cambios de un solo día.

### Ejemplo de organización

```text
/olamip.json
/olamip-delta-2026-03-01.json
/olamip-delta-2026-03-02.json
/olamip-delta-2026-03-03.json
```

### Forma del archivo

Cada archivo versionado tiene la misma estructura lógica que un archivo móvil, pero representa solo una fecha de cambio:

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "date": "2026-03-02",
  "added": [
    {
      "title": "Nueva entrada del blog",
      "url": "https://example.com/blog/new-post/",
      "summary": "Breve panorama de la actualización."
    }
  ],
  "updated": [],
  "removed": []
}
```

### Reglas de ingestión

Los sistemas de IA deberían:

- Detectar todos los archivos `olamip-delta-*.json` en el mismo directorio que `olamip.json`.
- Ordenarlos por `date` en orden ascendente.
- Aplicar solo los que estén dentro de la ventana configurada (por ejemplo, los últimos 7–30 días).

Este enfoque es especialmente útil para:

- Sitios de noticias con actualizaciones diarias frecuentes.
- Catálogos de comercio electrónico de alto volumen.
- Plataformas de documentación con ciclos de lanzamiento rápidos.

---

## 10. Cambios Estructurales: Secciones y Subsecciones

OLAMIP‑DELTA aplica el mismo patrón delta a **secciones** y **subsecciones** que a las **entradas**. Esto garantiza que los sistemas de IA entiendan no solo qué páginas existen, sino también cómo están organizadas.

### 10.1 Añadir una Section o Subsection

Al añadir una sección o subsección, incluya un objeto **section completo** en `added`.

Ejemplo:

```json
{
  "date": "2026-05-01",
  "added": [
    {
      "title": "Tutoriales",
      "summary": "Guías paso a paso para principiantes y usuarios avanzados.",
      "url": "https://example.com/tutorials/",
      "section_type": "blog_category",
      "policy": "allow",
      "tags": ["tutorials", "beginner", "advanced"],
      "priority": "medium"
    }
  ],
  "updated": [],
  "removed": []
}
```

#### Reglas

- Los objetos `added` de sección deben incluir `title`, `summary`, `url` y `section_type`.
- Use `section_type` para reflejar el rol semántico, por ejemplo `blog_category`, `doc_category`, `product_collection`, `research_category`.
- `priority` y `policy` ayudan a los sistemas de IA a entender la importancia y las reglas de ingestión para todo el subárbol.

### 10.2 Actualizar una Section o Subsection

Incluya `url` más cualquier campo que haya cambiado.

Ejemplo:

```json
{
  "date": "2026-05-02",
  "updated": [
    {
      "url": "https://example.com/blog/tutorials/",
      "title": "Tutoriales paso a paso",
      "summary": "Guías para principiantes y usuarios avanzados, con ejemplos de código y buenas prácticas.",
      "tags": ["tutorials", "beginner", "advanced", "code-examples"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Reglas

- Las secciones `updated` deben incluir `url`.
- `updated` puede incluir cualquier subconjunto de los otros campos.
- Los campos ausentes se consideran sin cambios.

### 10.3 Eliminar una Section o Subsection

Incluya solo `url` en `removed`.

Ejemplo:

```json
{
  "date": "2026-05-03",
  "removed": [
    {
      "url": "https://example.com/blog/retired/"
    }
  ],
  "added": [],
  "updated": []
}
```

#### Reglas

- Los elementos `removed` de sección deben incluir `url` y no deben incluir ningún otro campo.
- La URL debe ser la URL canónica de la sección o subsección eliminada.
- Cuando se elimina una sección, todos sus descendientes se consideran eliminados.

---

## 11. Conformidad

Los implementadores y administradores web deben tratar lo siguiente como reglas de conformidad.

### 11.1 Reglas MUST

Un archivo OLAMIP‑DELTA conforme **DEBE**:

- Declarar `protocol: "OLAMIP-DELTA"`.
- Establecer `version` con un valor que coincida con el esquema de versión documentado.
- Incluir una matriz `deltas` no vacía.
- Hacer que cada objeto delta incluya `date` en formato ISO‑8601 `YYYY-MM-DD`.
- Ordenar `deltas` por `date` en orden ascendente.
- Incluir los tres campos de operación (`added`, `updated`, `removed`) en cada objeto delta, incluso si están vacíos.
- Asegurar que `url` en las tres operaciones sea una URL absoluta y canónica.
- Asegurar que ninguna `url` aparezca más de una vez dentro del mismo objeto delta.
- Hacer que los elementos `added` y `updated` incluyan `url`.
- Hacer que los elementos `removed` incluyan `url` y ningún otro campo.
- Tratar los campos ausentes en `updated` como sin cambios.
- Tratar los elementos `removed` como eliminación de la URL dada y de sus descendientes del índice.

### 11.2 Reglas SHOULD

Una implementación conforme **DEBERÍA**:

- Adoptar por defecto el patrón de **ventana delta móvil**, salvo que el flujo de trabajo del sitio favorezca claramente los deltas versionados.
- Mantener `last_updated` precisa para ayudar a rastreadores y validadores a saber cuándo hay una nueva versión del archivo.
- Usar URLs canónicas estables que no cambien con el tiempo, para evitar ambigüedad al aplicar los deltas.
- Mantener `window_days` dentro del rango típico de 7–30 días, ajustándolo según la frecuencia de publicación del sitio.
- En deltas versionados, conservar suficientes archivos dentro de la ventana configurada (por ejemplo, los últimos 7–30 días) para evitar actualizaciones perdidas cuando los rastreadores se retrasan.

### 11.3 Reglas MAY

Una implementación conforme **PUEDE**:

- Usar el formato de delta versionado (`olamip-delta-YYYY-MM-DD.json`) en lugar de un único archivo móvil, si eso encaja mejor con el flujo de despliegue.
- Emitir `olamip-delta.json` con más o menos frecuencia que `olamip.json`, siempre que la relación entre instantáneas y deltas sea clara y coherente.
- Añadir metadatos de diagnóstico o registro bajo una clave separada fuera de los campos del protocolo, por ejemplo un objeto `diagnostics`, siempre que no entren en conflicto con los campos obligatorios ni cambien el significado del delta.

---

## 12. Buenas Prácticas

Para garantizar compatibilidad sólida y a largo plazo con los sistemas de IA, los administradores web deberían seguir estas buenas prácticas al generar `olamip-delta.json`.

### 12.1 Automatizar la generación de deltas

- Vincule la generación de deltas a su CMS o canal de despliegue.
- Cada vez que se publique, actualice o elimine contenido, actualice `olamip-delta.json` en consecuencia.
- Valide el esquema del archivo antes del despliegue para evitar errores de sintaxis o campos inválidos.

### 12.2 Mantener `olamip.json` como fuente autoritativa

- Conserve un `olamip.json` válido y actualizado.
- Los nuevos sistemas de IA pueden arrancar desde el archivo completo y luego aplicar los deltas.
- No trate `olamip-delta.json` como reemplazo de la instantánea principal.

### 12.3 Usar URLs canónicas estables

- Use URLs absolutas con esquema y dominio.
- Evite patrones de URL que cambien con frecuencia, salvo que el sistema del sitio, por ejemplo redirecciones, mantenga con robustez el significado canónico.
- Cuando una página cambie realmente de URL, trátelo como una operación “remove-then-add”, no como una actualización in situ de la URL en el delta.

### 12.4 Mantener coherencia en resúmenes y metadatos

- Los resúmenes en los elementos `added` y `updated` deben ser coherentes con el contenido real.
- Pequeños cambios de redacción que no alteren el significado pueden enviarse como deltas, pero deberían minimizarse para evitar ruido innecesario.
- Las etiquetas deben estar normalizadas (minúsculas, una sola palabra, guiones para palabras múltiples) y reflejar conceptos estables.

---

## 13. Recursos Relacionados

Para obtener una visión completa del ecosistema OLAMIP, los administradores web también deberían consultar:

- **[Especificación del Formato de Archivo OLAMIP](https://olamip.org/file-format-specification/)**  
  Describe la estructura de `olamip.json` y define los campos para entradas, secciones y subsecciones que sustentan el formato delta.

- **[Preguntas Frecuentes de OLAMIP](https://olamip.org/frequently-asked-questions/)**  
  Explica cuestiones de despliegue, descubrimiento y operación para administradores web.

- **[Especificación del Formato de Archivo OLAMIP‑DELTA](https://olamip.org/delta-updates/)**  
  Esquema JSON formal para `olamip-delta.json`, que puede utilizarse para validar y revisar automáticamente los archivos delta.

- **[Por qué OLAMIP es un estándar superior a LLMs.txt](https://olamip.org/why-olamip-is-a-superior-standard-to-llms-txt/)**  
  Explica las ventajas de OLAMIP y OLAMIP‑DELTA frente a formatos de metadatos más simples y no estructurados.

---

Con esto concluye la especificación del formato de archivo OLAMIP‑DELTA. Los administradores web e implementadores pueden usar este documento como referencia para generar, validar y consumir archivos `olamip-delta.json` en producción.
