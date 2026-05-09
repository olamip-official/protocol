# OLAMIP File Format Specification

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Overview

The OLAMIP file (`/olamip.json`) is a structured JSON document that provides curated summaries of your website’s most important pages. It is designed to be easily parsed by large language models (LLMs), enabling them to understand, prioritize, and use your content with clarity, precision, and intent.

## File Location

The OLAMIP file must be hosted at the root of your domain:

`https://yourdomain.com/olamap.json`

## Declaring the OLAMIP File Location

To maximize adoption and ensure that systems can reliably locate your OLAMIP file, publish both a `<link>` tag and a `<meta>` tag in your site’s `<head>` section.

### Primary Discovery: `<link rel="olamip">`

- Standardized practice: Crawlers and parsers already scan `<link>` tags for resources like canonical, sitemap, and alternate.
- Machine‑friendly: Declares a formal relationship between the page and the OLAMIP file.
- Interoperability: Fits neatly into existing web standards, making it easier for AI systems to adopt without special handling.

### Fallback Discovery: `<meta name="olamip-location">`

- Human‑readable: Simple for webmasters to add and understand.
- Compatibility: Some parsers and tools prefer scanning `<meta>` tags for metadata.
- Redundancy: Acts as a backup if a crawler does not yet support `rel="olamip"`.

### Why Both Together Are Stronger

- Future‑proofing: As OLAMIP adoption grows, different systems may implement discovery differently. Including both ensures no system is left behind.
- Resilience: If one method fails, the other provides a fallback.
- Ease of integration: Developers can choose whichever method fits their pipeline best, without forcing webmasters to guess.
- Trust and clarity: Dual signals reduce ambiguity and make it explicit where the OLAMIP file lives.

### Best Practice Implementation

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

By including both tags, you make your OLAMIP file discoverable by the widest range of crawlers, validators, and AI systems.

## File Structure

The OLAMIP file must be a valid UTF‑8‑encoded JSON document containing:

- `protocol`  
- `version`  
- `identity`  
- `content`  
- `metadata`  

### High‑level structure

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": { "...": "..." },
  "content": { "...": "..." },
  "metadata": { "...": "..." }
}
```

## Identity Object

| Field | Type | Required | Description |
|---|---|---:|---|
| `name` | `string` | Yes | Name of the website or organization. |
| `type` | `string` | Yes | Entity type, such as `company`, `blog`, or `ecommerce`. |
| `canonical_description` | `string` | Yes | Human‑readable description of the site. |
| `tags` | `array<string>` | No | Optional keywords describing the domain or industry. |

## Content Object

The `content` object contains:

- An `overview`  
- A list of `sections`  
- Each section may contain `subsections`  
- Each section or subsection may contain `entries`  

This supports multi‑level hierarchies.

### Overview Object

| Field | Type | Required | Description |
|---|---|---:|---|
| `summary` | `string` | Yes | A concise explanation of the website’s purpose. |

## Section Object Specification

A `Section` represents a category, collection, or grouping of content. Sections may contain:

- `entries` (content items)  
- `subsections` (nested Section objects)  

This allows unlimited nesting depth.

### Section Fields

| Field | Type | Required | Description |
|---|---|---:|---|
| `title` | `string` | Yes | Human‑readable name of the section. |
| `summary` | `string` | Yes | Description of what the section contains. |
| `url` | `string` | Yes | Canonical URL of the section. |
| `section_type` | `string` | Yes | Semantic classification from the taxonomy. |
| `policy` | `string` | No | `"allow"` or `"forbid"`. See inheritance rules below. |
| `tags` | `array<string>` | No | Optional keywords. |
| `priority` | `string` | No | `"high"`, `"medium"`, or `"low"`. |
| `published` | `string` | No | ISO 8601 date. |
| `language` | `string` | No | Use BCP‑47 language codes. |
| `entries` | `array<Entry>` | Yes | Array of Entry objects. |
| `subsections` | `array<Section>` | No | Array of nested Section objects. |

### Allowed `section_type` Values

| `section_type` | Meaning |
|---|---|
| `blog_category` | Groups blog articles. |
| `news_section` | Groups news articles. |
| `product_collection` | Groups products or services. |
| `doc_category` | Groups documentation pages. |
| `research_category` | Groups research papers or datasets. |
| `project_group` | Groups portfolio projects. |
| `content_section` | Generic fallback. |

## Policy Behavior and Inheritance

The `policy` field controls whether AI systems are permitted to ingest the content represented by a Section, Subsection, or Entry. Valid values are `"allow"` and `"forbid"`. This field is optional at all levels of the OLAMIP structure.

### Default Behavior

If the `policy` field is omitted at a given level, the effective policy is determined through inheritance. If no ancestor defines a policy, the effective policy defaults to `"allow"`.

### Inheritance Rules

AI systems must determine the effective policy for each Entry using the following lookup order:

1. **Entry‑level policy**  
   If the Entry defines a `policy`, that value is authoritative.

2. **Subsection‑level policy**  
   If the Entry omits `policy`, AI systems must check the nearest Subsection that contains it.

3. **Section‑level policy**  
   If neither the Entry nor its Subsection defines `policy`, AI systems must use the policy defined at the Section level.

4. **Default policy**  
   If no ancestor defines a `policy`, the effective policy is `"allow"`.

### Intended Webmaster Usage

- To make the entire website ingestible by AI systems, omit the `policy` field everywhere.
- To control ingestion, use `"allow"` and `"forbid"` selectively at any level of the hierarchy.
- A policy applied at a Section or Subsection automatically applies to all descendants unless overridden.

### AI System Requirements

AI systems must:

- Apply the default `"allow"` only when no explicit policy exists in the ancestor chain.  
- Respect the effective policy determined through inheritance.  
- Treat `"forbid"` as a strict prohibition on ingestion.  
- Treat `"allow"` as permission to ingest the content represented by that node.  

## Multi‑Level Hierarchy Diagram

```text
content
└── sections[]
    ├── Section (Level 1)
    │     ├── entries[]
    │     └── subsections[]
    │           ├── Section (Level 2)
    │           │     ├── entries[]
    │           │     └── subsections[]
    │           │           └── Section (Level 3)
    │           │                 └── entries[]
    │           └── ...
    └── ...
```

This structure supports:

- News → Politics → Opinion → Articles  
- Docs → API → Authentication → Pages  
- Store → Clothing → Men → Jackets → Products  
- Research → Physics → Quantum → Papers  

## Entry Object Specification

An `Entry` is the most granular content unit. Examples include:

- Blog article  
- News article  
- Product page  
- Documentation page  
- Research paper  
- Portfolio project  
- Legal page  
- Downloadable resource  

### Entry Fields

| Field | Type | Required | Description |
|---|---|---:|---|
| `title` | `string` | Yes | Human‑readable title. |
| `summary` | `string` | Yes | Concise description of the content. |
| `url` | `string` | Yes | Canonical, absolute URL. |
| `content_type` | `string` | Yes | Semantic classification from the taxonomy. |
| `policy` | `string` | No | `"allow"` or `"forbid"`. Same as in Sections/Subsections. |
| `tags` | `array<string>` | No | Optional keywords. |
| `priority` | `string` | No | `"high"`, `"medium"`, or `"low"`. |
| `published` | `string` | No | ISO 8601 publication date. |
| `language` | `string` | No | Use BCP‑47 language codes. |
| `metadata` | `object` | No | Domain‑ or page‑specific structured information. |

### Why URLs Are Required

The `url` field is essential because it serves as the canonical identifier for the content. While summaries convey the meaning of the content, URLs link that meaning to a specific and verifiable location on the web. AI systems utilize URLs for deduplication, retrieval, validation, and cross‑referencing with schema.org, sitemaps, and crawlers.

### Allowed `content_type` Values

#### General Pages

| `content_type` | Meaning |
|---|---|
| `page` | Standard content page. |
| `landing_page` | Marketing or campaign page. |
| `legal_page` | Terms, privacy, disclaimers. |

#### Blog

| `content_type` | Meaning |
|---|---|
| `blog_article` | A blog post. |

#### News

| `content_type` | Meaning |
|---|---|
| `news_article` | A news story. |

#### E‑commerce

| `content_type` | Meaning |
|---|---|
| `product` | A product page. |
| `service` | A service offering. |

#### Documentation

| `content_type` | Meaning |
|---|---|
| `doc_page` | A documentation or help page. |

#### Research

| `content_type` | Meaning |
|---|---|
| `research_paper` | Academic or scientific paper. |
| `dataset` | Research dataset. |

#### Portfolio

| `content_type` | Meaning |
|---|---|
| `project` | Portfolio project or case study. |

#### Media / Resources

| `content_type` | Meaning |
|---|---|
| `media_item` | Video, audio, gallery, etc. |
| `resource` | Downloadable or reference material. |

## Arrays and Multi‑Value Fields

Some OLAMIP fields are designed to hold more than one value. Whenever a field contains multiple elements, such as `tags`, or any custom list defined within the optional `metadata` field of an Entry object, it must be expressed as a JSON array.

Arrays are enclosed in square brackets (`[ ]`) and contain a comma‑separated sequence of individual string values. Using arrays ensures that AI systems can reliably interpret multi‑value data without ambiguity. Each element inside the array must be a standalone string, and the order of elements should remain consistent whenever it carries semantic meaning.

The `metadata` field is used to store domain‑specific or page‑specific structured information that extends beyond the core OLAMIP fields. This field allows publishers to include additional machine‑interpretable details relevant to their industry or content type, providing AI systems with richer contextual signals without altering the core protocol.

## Metadata Object

A typical metadata object may look like this:

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

## Why Categorical Priority Works Best

| Benefit | Explanation |
|---|---|
| Clarity & consistency | `"high"/"medium"/"low"` is universally interpretable. |
| Simpler for publishers | No numeric scoring required. |
| Easier to validate | Tools can detect misuse. |
| Flexible for LLM pipelines | Models can internally map categories to weights. |

### Priority Field Guidelines

| Value | Meaning |
|---|---|
| `"high"` | Flagship, mission‑critical content. Use sparingly. |
| `"medium"` | Default for most content. |
| `"low"` | Niche, outdated, or low‑value content. |

### Best Practices

| Recommendation | Reason |
|---|---|
| Limit `"high"` to 5–10% | Preserves meaning of the signal. |
| Default to `"medium"` | Ensures consistency. |
| Use `"low"` for niche/legacy content | Reduces noise. |
| Review priorities regularly | Keeps the file accurate. |

### Why It Matters

LLMs may use `priority` to:

- Allocate more attention during training.  
- Rank pages for retrieval tasks.  
- Filter out less relevant content.  

If every page is marked `"high"`, the signal becomes meaningless, and your most valuable content gets lost in the noise.

## Multilingual Support

To fully support multilingual websites, define `language` at:

- File level, as the global default, inside `metadata`.  
- Section level, as an optional override.  
- Entry level, as an optional override.  

This is essential for:

- Multilingual blogs.  
- International news outlets.  
- Research sites with papers in multiple languages.  
- E‑commerce stores with localized product pages.  

## Language Format

Use BCP‑47 language codes, the global standard used by:

- schema.org.  
- The HTML `lang` attribute.  
- W3C.  
- Search engines.  
- Major LLM pipelines.  

### Examples

| Language | Code |
|---|---|
| English | `en` |
| Spanish | `es` |
| French | `fr` |
| German | `de` |
| Portuguese (Brazil) | `pt-BR` |
| Chinese (Simplified) | `zh-CN` |
| Arabic | `ar` |

## Why This Matters for AI Systems

LLMs use language metadata to:

- Choose the correct tokenizer.  
- Apply the right summarization model.  
- Avoid mixing languages in embeddings.  
- Improve retrieval accuracy.  
- Reduce hallucinations in multilingual contexts.  
- Support cross‑language search and translation.  

Without explicit language fields, AI systems must guess, and they often guess wrong.

## General Validation Rules

| Rule | Requirement |
|---|---|
| Valid JSON | No trailing commas or malformed structures. |
| Required fields | Sections and entries must include required fields. |
| Canonical URLs | Must be absolute and stable. |
| Summary length | Under 500 characters. |
| Tags | Lowercase, single‑word strings. |
| Subsections | Must follow the Section schema. |

## Purpose of Tags

Tags provide compact, machine‑interpretable signals that help AI systems understand topical relationships across your site. They support clustering, retrieval, and semantic alignment, but only when they follow a predictable, uniform format.

Tags give AI systems lightweight semantic cues about the themes, topics, or categories associated with an entry. They are not replacements for summaries or section hierarchy; instead, they act as supplemental signals that help models:

- Group related content.  
- Infer topical domains.  
- Improve retrieval accuracy.  
- Reduce ambiguity between similarly titled pages.  
- Support cross‑page reasoning.  

Tags are optional but strongly recommended for sites with large content inventories or overlapping topics.

## Required Format for Tags

Tags must always follow a strict normalization pattern to ensure consistency across publishers and predictable interpretation by AI systems.

- Lowercase.  
- Single‑word strings.  
- ASCII characters only.  
- No spaces.  
- No punctuation except hyphens.  
- No camelCase or snake_case.  

This normalization ensures tags remain unambiguous tokens that can be reliably indexed, embedded, and compared.

## Multi‑Word Tags

Some concepts naturally contain multiple words. To preserve meaning while maintaining the single‑token requirement, multi‑word tags must be converted into hyphenated forms.

### Formatting rules

- Convert all letters to lowercase.  
- Replace spaces with hyphens.  
- Remove punctuation unless it is part of a standard term.  
- Avoid underscores, camelCase, or concatenation.  
- Keep tags concise and semantically meaningful.  

### Examples

| Concept | Valid Tag | Invalid Tags |
|---|---|---|
| Los Angeles | `los-angeles` | `Los Angeles`, `los_angeles`, `losangeles` |
| Machine Learning | `machine-learning` | `MachineLearning`, `machine_learning` |
| Data Science | `data-science` | `data science`, `datascience` |
| Customer Support | `customer-support` | `customerSupport`, `customer support` |

Hyphenation preserves semantic boundaries while keeping the tag machine‑friendly and human‑readable.

## Multilingual Considerations for Tags

Language metadata determines how tags should be interpreted. Tags themselves do not require translation, but they must remain consistent with the language context of the entry.

### Rules for multilingual sites

- Tags should be written in the same language as the entry unless the concept is globally standardized (e.g., `javascript`).  
- If the same concept appears in multiple languages, each language version should use its own localized tag.  
- Hyphenation rules remain the same across languages.  
- Avoid mixing languages within a single tag.  
- Use BCP‑47 language codes at the file, section, or entry level to clarify interpretation.  

### Examples

| Entry Language | Concept | Valid Tag |
|---|---|---|
| `en` | climate change | `climate-change` |
| `es` | cambio climático | `cambio-climatico` |
| `de` | künstliche intelligenz | `kunstliche-intelligenz` |

Tags remain normalized tokens, but their linguistic form should match the content they describe.

## Tags Interaction With AI Systems

Tags help AI systems:

- Cluster related entries.  
- Disambiguate similar topics.  
- Improve retrieval precision.  
- Support cross‑language mapping.  
- Reduce hallucination risk by reinforcing context.  

Tags are not ranking signals, but they improve interpretability and reduce ambiguity, especially in large or multilingual sites.

## OLAMIP File Versioning

| Guideline | Purpose |
|---|---|
| Parsers ignore unknown fields. | Ensures forward compatibility. |
| Publishers validate against latest schema. | Ensures correctness. |

## Semantic Alignment

OLAMIP complements
