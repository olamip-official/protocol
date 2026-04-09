# OLAMIP Delta File Format Specification

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## 1. OLAMIP Delta Updates

### Keeping AI Systems Informed as Your Website Evolves

Modern websites evolve constantly: new products, updated articles, retired pages, seasonal campaigns, and documentation changes. If AI systems only see a static snapshot of your site, they quickly fall out of sync with reality. That is exactly why `olamip-delta.json` exists.

OLAMIP’s delta mechanism ensures that large language models (LLMs) stay aligned with your latest content without requiring full reprocessing of your entire metadata file every time something changes.

### What Is `olamip-delta.json`?

`olamip-delta.json` is an optional companion file to `olamip.json`. While the main OLAMIP file is your authoritative, complete snapshot, the delta file contains only the changes since the last update.

This includes:

- Newly added pages.  
- Updated summaries or metadata.  
- Removed URLs.  

Together, `olamip.json` + `olamip-delta.json` turn your website into a stream of structured changes, not a static dataset.

### Why Delta Updates Matter for AI

Incremental updates give AI systems several advantages:

- **Efficiency** — No need to re‑ingest your entire OLAMIP file.  
- **Freshness** — New content is reflected quickly.  
- **Scalability** — Large catalogs remain manageable.  
- **Precision** — Only meaningful changes are processed.  

For ecommerce stores, news sites, documentation hubs, and blogs, this means AI systems stay synchronized with your content lifecycle.

### How `olamip-delta.json` Works

The basic workflow is:

1. Publish `olamip.json`. This is your full, canonical snapshot.  
2. Generate `olamip-delta.json` whenever content changes. The delta file contains structured lists of `added`, `updated`, and `removed` entries.  
3. AI systems apply the delta:  
   - `added` → appended to the index  
   - `updated` → overwrite existing entries  
   - `removed` → deleted from the index  
4. Repeat as your site evolves. You can generate deltas daily, weekly, or tied to your publishing workflow.  

### The Missed‑Update Problem

A common concern is:

> What if AI systems do not crawl my delta file every day?

Example:

- Day 1: You publish `olamip.json`.  
- Day 30: AI crawlers read it.  
- Day 31: You add 5 new products.  
- Day 32: You add 10 more.  
- Day 33: AI crawlers return.  

If your delta file only contains the most recent changes, the Day 31 updates would be lost.

## 2. Rolling Delta Window

Instead of storing only the latest changes, OLAMIP recommends keeping all deltas from the last 7–30 days inside the delta file.

This ensures:

- AI systems can crawl weekly or monthly.  
- No updates are lost.  
- Webmasters do not need to worry about crawler timing.  

### 2.1 Recommended Delta Structure

A delta file should be a valid UTF‑8‑encoded JSON document containing:

- `protocol`  
- `version`  
- `base_version`  
- `generated_at`  
- `window`  
- `changes`  
- `metadata`  

#### High‑Level Structure

```json
{
  "protocol": "OLAMIP",
  "version": "1.0",
  "base_version": "1.0",
  "generated_at": "2026-01-21T12:00:00Z",
  "window": {
    "start": "2026-01-14T00:00:00Z",
    "end": "2026-01-21T12:00:00Z"
  },
  "changes": {
    "added": [],
    "updated": [],
    "removed": []
  },
  "metadata": {}
}
```

### 2.2 Core Fields

#### `protocol`

- Type: `string`  
- Required: Yes  
- Value: Must be `"OLAMIP"`  

#### `version`

- Type: `string`  
- Required: Yes  
- Value: Delta file format version, such as `"1.0"`  

#### `base_version`

- Type: `string`  
- Required: Yes  
- Value: The version of the main `olamip.json` snapshot that this delta is based on  

#### `generated_at`

- Type: `string`  
- Required: Yes  
- Value: ISO 8601 timestamp indicating when the delta file was produced  

#### `window`

- Type: `object`  
- Required: Yes  
- Value: Defines the time range covered by the delta file  

#### `changes`

- Type: `object`  
- Required: Yes  
- Value: Contains `added`, `updated`, and `removed` change lists  

#### `metadata`

- Type: `object`  
- Required: No  
- Value: Optional file‑level metadata  

### 2.3 Window Object

The `window` object defines the time span covered by the delta file.

| Field | Type | Required | Description |
|---|---|---:|---|
| `start` | `string` | Yes | ISO 8601 timestamp for the beginning of the delta window. |
| `end` | `string` | Yes | ISO 8601 timestamp for the end of the delta window. |

### 2.4 Changes Object

The `changes` object contains the actual incremental updates.

| Field | Type | Required | Description |
|---|---|---:|---|
| `added` | `array<AddedItem>` | Yes | New entries introduced since the previous snapshot or delta window. |
| `updated` | `array<UpdatedItem>` | Yes | Existing entries that changed in summary, tags, metadata, policy, priority, or language. |
| `removed` | `array<RemovedItem>` | Yes | URLs that no longer exist and should be deleted from the index. |

### 2.5 Change Item Specification

Each change item should include enough information for an AI system to apply the update reliably.

#### Added Item

An added item should include:

- `url`  
- `title`  
- `summary`  
- `content_type`  
- (optional) `section_url`  
- (optional) `section_title`  
- (optional) `policy`  
- (optional) `tags`  
- (optional) `priority`  
- (optional) `published`  
- (optional) `language`  
- (optional) `metadata`  

#### Updated Item

An updated item should include:

- `url`  
- One or more updated fields (e.g., `summary`, `tags`, `priority`)  
- (optional) `changed_fields`  
- (optional) `updated_at` (ISO 8601 timestamp)  

#### Removed Item

A removed item may be represented as:

- A URL string, or  
- An object containing the URL and optional removal metadata (e.g., `reason`, `deleted_at`)  

### 2.6 Recommended Change Item Shapes

#### Added

```json
{
  "url": "https://example.com/products/new-item/",
  "title": "New Product",
  "summary": "A newly released product.",
  "content_type": "product",
  "section_url": "https://example.com/products/",
  "section_title": "Products",
  "published": "2026-01-21",
  "language": "en"
}
```

#### Updated

```json
{
  "url": "https://example.com/products/existing-item/",
  "changed_fields": ["summary", "tags"],
  "summary": "Updated summary text.",
  "tags": ["feature-updated", "sale"],
  "updated_at": "2026-01-21T12:00:00Z"
}
```

#### Removed

```json
{
  "url": "https://example.com/products/old-item/"
}
```

### 2.7 Delta Processing Rules

AI systems should process delta files in the following way:

1. Validate that the file is proper JSON.  
2. Confirm that `protocol` is `"OLAMIP"`.  
3. Ensure that `base_version` is compatible with the current snapshot.  
4. Apply `added` items first.  
5. Apply `updated` items next.  
6. Apply `removed` items last.  
7. Preserve the most recent known state after all changes are applied.  

### 2.8 Conflict Handling

If the same URL appears in multiple delta categories:

- `removed` takes precedence over `updated`.  
- `updated` takes precedence over `added` only when the URL already exists in the index.  
- If an item is both added and removed in the same window, the removal should win unless the add is clearly newer and explicitly supersedes it.  

### 2.9 Best Practices

- Keep the rolling window between 7 and 30 days.  
- Publish deltas on a predictable cadence.  
- Include all meaningful updates, not only major changes.  
- Keep timestamps consistent and in UTC.  
- Preserve stable URLs whenever possible.  
- Use `changed_fields` for clarity when updating entries.  
- Validate delta files before publishing.  

### 2.10 Why This Matters

The delta file keeps AI systems synchronized with your website without forcing them to reprocess everything from scratch. That makes OLAMIP more scalable, more current, and more practical for real‑world websites that change frequently.

## 3. Rolling Delta Example (7‑Day Window)

```json
{
  "version": "1.0",
  "window_days": 7,
  "last_updated": "2026-03-05",
  "deltas": [
    {
      "date": "2026-03-05",
      "added": [
        {
          "title": "New Product A",
          "url": "https://example.com/product/a",
          "summary": "A new addition to our catalog.",
          "tags": ["product", "new"]
        }
      ],
      "updated": [],
      "removed": []
    },
    {
      "date": "2026-03-04",
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
      "date": "2026-03-03",
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

AI systems should:

- Read all deltas.  
- Apply them in chronological order.  
- End up fully synchronized.  

No missed updates; ever.