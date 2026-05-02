# OLAMIP-DELTA File Format Specification

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

The OLAMIP-DELTA protocol defines a JSON‑based delta file format for incremental updates to `olamip.json`. This specification is aimed at webmasters and implementers who are familiar with structured data, HTML, and sitemap‑style publishing.

## 1. Overview

`olamip.json` is the authoritative, full‑snapshot representation of your site’s structure and content. `olamip-delta.json` is an optional companion file that describes the changes to that snapshot since the last update.

Together, they form a machine‑interpretable stream of changes:

- `olamip.json`: full, structured snapshot.
- `olamip-delta.json`: incremental change log.

---

## 2. Core Concepts

- **Entry** — a leaf‑level content item (e.g., blog article, product page, doc page).
- **Section / Subsection** — a hierarchical grouping (e.g., blog category, doc category, product collection, project group).
- **Operation** — one of `added`, `updated`, `removed`.
- **Delta** — a date‑scoped change set containing `added`, `updated`, and `removed`.

This format is intended to be:
- **Machine‑interpretable first**, but human‑readable enough to inspect manually.
- **Incremental**, so AI systems can stay synchronized without re‑processing your entire site.

---

## 3. File Location and Discovery

### 3.1 File Location

`olamip.json` must be hosted at the root of your domain:

```text
https://yourdomain.com/olamip.json
```

`olamip-delta.json` must be hosted in the same directory:

```text
https://yourdomain.com/olamip-delta.json
```

Versioned deltas (optional) live in the same directory:

```text
/olamip.json
/olamip-delta-YYYY-MM-DD.json
```

### 3.2 Discovery Mechanism

Webmasters must declare the location of `olamip.json` using both tags in the site’s `<head>`:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

AI systems that know about `olamip.json` automatically look for `olamip-delta.json` in the same directory and apply deltas as described in this specification. There is no need to add a separate `<link>` or `<meta>` tag for `olamip-delta.json`.

---

## 4. Top‑Level Structure

At the top level, `olamip-delta.json` is a JSON object with the following fields:

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
          "title": "Example entry",
          "url": "https://example.com/page/",
          "summary": "...",
          "content_type": "blog_article"
        }
      ],
      "updated": [
        {
          "url": "https://example.com/page/",
          "summary": "Updated summary"
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

### 4.1 Required Top‑Level Fields

- `protocol` (string, required)  
  Must be `"OLAMIP-DELTA"`.  
  This distinguishes the delta file from the main OLAMIP file and from other delta formats.

- `version` (string, required)  
  Must match the documented version scheme (e.g., `"1.0.0"`).  
  This allows implementers to detect schema changes.

- `window_days` (integer, optional)  
  Number of days in the rolling window; must be positive (typically 7–30).  
  Only required if using a rolling delta file.

- `last_updated` (string, required)  
  Date this delta file was last modified, in ISO‑8601 `YYYY-MM-DD` format.  
  This helps crawlers decide when to re‑fetch.

- `deltas` (array, required)  
  Non‑empty array of date‑scoped change sets.  
  Each item is a **delta object** with `date`, `added`, `updated`, and `removed`.

### 4.2 `deltas` Array and Date Ordering

- `deltas` must be sorted by `"date"` in ascending order (oldest first, newest last).
- Each delta object must have a `"date"` field in ISO‑8601 `YYYY-MM-DD` format.
- `deltas` must be non‑empty.

This ordering is not optional for conformance: AI systems assume deltas are chronologically sorted.

---

## 5. Delta Object Structure

Each item in `deltas` is a delta object with the following shape:

```json
{
  "date": "2026-04-27",
  "added": [ ... ],
  "updated": [ ... ],
  "removed": [ ... ]
}
```

For machine‑interpretable clarity and consistency, webmasters **MUST** include all three operation fields in every delta object, even when some are empty.

Fields:

- `date` (string) — ISO‑8601 `YYYY-MM-DD` date.
- `added` (array) — zero or more newly added items.
- `updated` (array) — zero or more modified items.
- `removed` (array) — zero or more deleted items.

Example of an additions‑only delta object:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "New Page",
      "summary": "A new page added to the site.",
      "url": "https://example.com/new-page/"
    }
  ],
  "updated": [],
  "removed": []
}
```

This pattern is required even when only one operation applies on a given day.

---

## 6. Operations

### 6.1 `added`

An `added` item is a **full object** of the applicable type (entry, section, or subsection). The fields must match the OLAMIP file‑format specification for that type.

For an **entry**:

- `title` (string, required) — human‑readable title.
- `summary` (string, required) — concise description.
- `url` (string, required) — canonical absolute URL.
- `content_type` (string, required) — from the OLAMIP `content_type` taxonomy.
- `tags` (array, optional) — list of lowercase, single‑token tags.
- `priority` (string, optional) — `"high"`, `"medium"`, or `"low"`.
- `policy` (string, optional) — `"allow"` or `"forbid"`.
- `language` (string, optional) — BCP‑47 language code.
- `metadata` (object, optional) — domain‑specific structured data.

For a **section or subsection**, `added` objects follow the OLAMIP section‑level spec (with `title`, `summary`, `url`, `section_type`, `policy`, `tags`, `priority`, etc.).

#### Rules for `added`

- `added` items must include `url`.
- `added` items must be full objects (at minimum, all required fields from the main OLAMIP spec).
- No `url` may appear more than once in the same delta object.

Example:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "New Blog Post",
      "summary": "A brief introduction to the new feature.",
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

An `updated` item must include `url` and may be either:

- A **partial object** containing only changed fields, or
- A **full object** containing all fields.

For any `updated` item, missing fields are treated as **unchanged**.

#### Partial update example

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "url": "https://example.com/post/to-update/",
      "summary": "Updated summary with new information.",
      "tags": ["feature", "update", "docs"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Full replacement example

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "title": "Updated Post Title",
      "summary": "Updated summary with new information.",
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

#### Rules for `updated`

- `updated` items must include `url`.
- `updated` may be a partial object (patching only changed fields) or a full object (full replacement).
- `updated` items must not change the identity of an entry via `url` remapping; that is a configuration decision, not a delta‑file concern.

### 6.3 `removed`

A `removed` item is **minimal** and identity‑only.

```json
{
  "url": "https://example.com/page/to-remove/"
}
```

#### Rules for `removed`

- `removed` items must include `url` and **must not** include any other fields.
- `url` must be an absolute, canonical URL.
- `removed` applies to the given URL and its structural descendants (subsections and entries), if any.
- No `url` may appear more than once in the same delta object.

Example:

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

## 7. URL and Object Identity

OLAMIP‑DELTA uses canonical URLs as the primary identity for entries, sections, and subsections.

### Identity rules

- `url` must be an absolute URL (including scheme and domain).
- No `url` may appear more than once within the same delta object (no duplicate adds/updates/removes for the same `url` on the same day).
- When a section is removed, all of its descendants (subsections and entries) are logically removed.
- Changing a URL requires a **remove** followed by an **add**, not an in‑place `url` update.

### Best practices for URLs

- Use stable, canonical URLs that are unlikely to change frequently.
- Avoid query parameters or session‑specific fragments.
- If a page moves, update site‑level redirects and treat the change as a URL‑level content‑authority question, not a delta‑file responsibility.

---

## 8. Rolling Delta Window (Recommended)

The **rolling delta window** is the recommended mode for `olamip-delta.json`.

In this model:

- The delta file contains changes from the last `window_days`.
- AI systems read all deltas in the file and apply them in chronological order.
- This prevents missed updates when crawlers are delayed.

### Rolling Delta Example (7‑Day Window)

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
          "title": "New Product A",
          "url": "https://example.com/product/a",
          "summary": "A new addition to the catalog."
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
          "summary": "Updated description reflecting new features."
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

AI systems:

- Read all deltas in the file.
- Apply them in chronological order.
- End up synchronized with the latest state.

---

## 9. Versioned Deltas (Alternative)

Some webmasters may prefer **versioned deltas**, where each file contains changes for one date.

### Example Layout

```text
/olamip.json
/olamip-delta-2026-03-01.json
/olamip-delta-2026-03-02.json
/olamip-delta-2026-03-03.json
```

### File Shape

Each versioned file has the same logical structure as a rolling file, but represents only one change date:

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "date": "2026-03-02",
  "added": [
    {
      "title": "New Blog Post",
      "url": "https://example.com/blog/new-post/",
      "summary": "Short overview of the update."
    }
  ],
  "updated": [],
  "removed": []
}
```

### Ingestion Rules

AI systems should:

- Discover all `olamip-delta-*.json` files in the same directory as `olamip.json`.
- Sort them by `date` in ascending order.
- Apply only those within the configured window (e.g., last 7–30 days).

This approach is particularly useful for:

- News sites with frequent daily updates.
- High‑volume e‑commerce catalogs.
- Documentation platforms with rapid release cycles.

---

## 10. Structural Changes: Sections and Subsections

OLAMIP‑DELTA applies the same delta pattern to **sections** and **subsections** as to **entries**. This ensures that AI systems understand not only which pages exist, but also how they are organized.

### 10.1 Adding a Section or Subsection

When adding a section or subsection, include a **full section** object in `added`.

Example:

```json
{
  "date": "2026-05-01",
  "added": [
    {
      "title": "Tutorials",
      "summary": "Step‑by‑step guides for beginners and advanced users.",
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

#### Rules

- Section `added` objects must include `title`, `summary`, `url`, and `section_type`.
- Use `section_type` to reflect the semantic role (e.g., `blog_category`, `doc_category`, `product_collection`, `research_category`).
- `priority` and `policy` help AI systems understand importance and ingestion rules for the entire subtree.

### 10.2 Updating a Section or Subsection

Include `url` plus any fields that have changed.

Example:

```json
{
  "date": "2026-05-02",
  "updated": [
    {
      "url": "https://example.com/blog/tutorials/",
      "title": "Step‑by‑Step Tutorials",
      "summary": "Guides for beginners and advanced users, with code examples and best practices.",
      "tags": ["tutorials", "beginner", "advanced", "code-examples"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Rules

- `updated` sections must include `url`.
- `updated` may include any subset of the other fields.
- Missing fields are treated as unchanged.

### 10.3 Removing a Section or Subsection

Include only `url` in `removed`.

Example:

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

#### Rules

- Section `removed` items must include `url` and must not include any other fields.
- The URL must be the canonical URL of the section or subsection being removed.
- When a section is removed, all of its descendants are logically removed.

---

## 11. Conformance

Implementers and webmasters should treat the following as conformance rules.

### 11.1 MUST Rules

A conforming OLAMIP‑DELTA file **MUST**:

- Declare `protocol: "OLAMIP-DELTA"`.
- Set `version` to a value that matches the documented version scheme.
- Include a non‑empty `deltas` array.
- Have each delta object include a `date` in ISO‑8601 `YYYY-MM-DD` format.
- Sort `deltas` by `date` in ascending order.
- Include all three operation fields (`added`, `updated`, `removed`) in every delta object, even when they are empty arrays.
- Ensure `url` in all three operations is an absolute, canonical URL.
- Ensure no `url` appears more than once within the same delta object (no duplicate adds/updates/removes for the same URL on the same day).
- Have `added` and `updated` items include `url`.
- Have `removed` items include `url` and no other fields.
- Treat missing fields in `updated` items as unchanged.
- Treat `removed` items as removing the given URL and its descendants (e.g., subsections and entries) from the index.

### 11.2 SHOULD Rules

A conforming implementation **SHOULD**:

- Adopt the **rolling delta window** pattern by default, unless the site’s workflow strongly favors versioned deltas.
- Keep `last_updated` accurate to help crawlers and validators know when a new version of the file is available.
- Use stable, canonical URLs that are unlikely to change over time, to avoid ambiguity during delta application.
- Keep `window_days` within the typical range of 7–30 days, adjusting based on how frequently the site publishes new content.
- For versioned deltas, keep enough files within the configured window (e.g., last 7–30 days) to avoid missed updates when crawlers are delayed.

### 11.3 MAY Rules

A conforming implementation **MAY**:

- Use the versioned delta format (`olamip-delta-YYYY-MM-DD.json`) instead of a single rolling file, if that fits the deployment pipeline better.
- Emit `olamip-delta.json` more or less frequently than `olamip.json`, as long as the relationship between snapshots and deltas is clear and consistent.
- Add diagnostic or logging metadata under a separate key outside the protocol fields (e.g., a `diagnostics` object), as long as it does not conflict with required fields or change the meaning of the delta.

---

## 12. Best Practices

To ensure robust, long‑term compatibility with AI systems, webmasters should follow these best practices when generating `olamip-delta.json`.

### 12.1 Automate delta generation

- Tie delta generation to your CMS or deployment pipeline.
- Every time content is published, updated, or removed, update `olamip-delta.json` accordingly.
- Validate the file’s schema before deployment, to avoid syntax errors or invalid fields.

### 12.2 Keep `olamip.json` authoritative

- Retain a valid, up‑to‑date `olamip.json`.
- New AI systems can bootstrap from the full file, then apply deltas going forward.
- Do not treat `olamip-delta.json` as a replacement for the main snapshot.

### 12.3 Use stable, canonical URLs

- Use absolute URLs with scheme and domain.
- Avoid URL patterns that change frequently unless the site mechanism (e.g., redirects) is robust enough to maintain canonical meaning.
- When a page’s URL truly changes, treat it as a “remove‑then‑add”, not an in‑place URL update in the delta.

### 12.4 Keep summaries and metadata consistent

- Summaries in `added` and `updated` items should be consistent with the actual content.
- Minor wording tweaks that do not change the meaning may still be submitted as deltas, but they should be minimized to avoid unnecessary churn.
- Tags should be normalized (lowercase, single‑word, hyphenated multi‑word) and reflect stable concepts.

---

## 13. Related Resources

For a complete picture of the OLAMIP ecosystem, webmasters should also consult:

- **[OLAMIP File Format Specification](https://olamip.org/file-format-specification/)**  
  Describes the structure of `olamip.json` and defines the fields for entries, sections, and subsections that underlie the delta format.

- **[OLAMIP Frequently Asked Questions](https://olamip.org/frequently-asked-questions/)**  
  Explains deployment, discovery, and operational questions for webmasters.

- **[OLAMIP‑DELTA File Format Specification](https://olamip.org/delta-updates/)**  
  Formal JSON Schema for `olamip-delta.json`, which can be used to validate and lint delta files automatically.

- **[Why OLAMIP Is a Superior Standard to LLMs.txt](https://olamip.org/why-olamip-is-a-superior-standard-to-llms-txt/)**  
  Explains the advantages of OLAMIP and OLAMIP‑DELTA over simpler, non‑structured metadata formats.

---

This completes the OLAMIP‑DELTA file format specification. Webmasters and implementers can use this document as a reference for generating, validating, and consuming `olamip-delta.json` files in production.
