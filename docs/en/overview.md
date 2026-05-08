# OLAMIP Overview

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP is an open standard that enables websites to communicate clearly and intentionally with AI systems. Instead of forcing large language models (LLMs) to guess meaning from noisy HTML, CSS, and JavaScript, OLAMIP provides a clean, structured, machine‑interpretable representation of your site’s most important content.

This document provides a high‑level introduction to OLAMIP’s purpose, philosophy, file format, and update model.

---

## 1. Why OLAMIP Was Created

Modern websites are built for browsers, not for AI. As a result, LLMs struggle with:

- Layout‑heavy HTML  
- JavaScript‑rendered content  
- Navigation menus, ads, and boilerplate  
- Duplicate or irrelevant pages  

LLMs *already* crawl your site, but they often misinterpret it. OLAMIP solves this by giving AI systems exactly what they need:

- A lightweight JSON file (`/olamip.json`)  
- Clean, human‑curated summaries  
- Structured metadata  
- Canonical URLs  
- A clear hierarchy of sections, subsections, and entries  

By adopting OLAMIP, you take control of how AI understands and represents your content.

---

## 2. Core Philosophy: Human‑Friendly + Machine‑Friendly

OLAMIP is designed as a communication bridge between humans and AI. Every part of the protocol must be:

| Principle | Human‑Friendly | Machine‑Friendly |
|-----------|----------------|------------------|
| File Format | Easy to write and validate | Strict, predictable JSON |
| Field Names | Intuitive and self‑explanatory | Lowercase, schema‑compliant |
| Summaries | Clear, brand‑aligned language | Concise, factual, semantically rich |
| Priority System | Simple labels (`"high"`, `"medium"`, `"low"`) | Mappable to ranking weights |
| Documentation | Plain‑language guides | JSON schemas, validator rules |
| Tooling | Generators, CMS plugins | CLI tools, parsers, test suites |

- **For humans:** OLAMIP must be easy to adopt without deep technical expertise.  
- **For machines:** OLAMIP must be structured, predictable, and unambiguous.

---

## 3. The OLAMIP File (`/olamip.json`)

An OLAMIP file is a structured JSON document hosted at:

`https://yourdomain.com/olamip.json`

It contains:

- `protocol` — must be `"OLAMIP"`  
- `version` — protocol version  
- `identity` — who you are  
- `content` — your site’s structured hierarchy  
- `metadata` — language, last updated, and other global fields  

### 3.1 Discovery Tags

To ensure AI systems can reliably locate your OLAMIP file, include both:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Using both provides redundancy, compatibility, and future‑proofing.

---

## 4. File Structure

### 4.1 Identity Object

Describes the website or organization:

- `name` — required  
- `type` — required  
- `canonical_description` — required  
- `tags` — optional  

### 4.2 Content Object

Contains:

- An `overview`  
- `sections`  
- Optional `subsections`  
- `entries` (the most granular content units)  

This supports unlimited nesting depth.

### 4.3 Sections

A Section groups related content. Required fields include:

- `title`  
- `summary`  
- `url`  
- `section_type`  

Optional fields include:

- `policy` (`"allow"` or `"forbid"`)  
- `tags`  
- `priority`  
- `published`  
- `language`  
- `subsections`  
- `entries`  

**Policy inheritance:**  
If omitted, policy is inherited from ancestors. Default is `"allow"`.

### 4.4 Entries

Entries represent individual content items such as:

- Blog articles  
- News stories  
- Products  
- Documentation pages  
- Research papers  
- Media items  

Required fields:

- `title`  
- `summary`  
- `url`  
- `content_type`  

Optional fields:

- `policy`, `tags`, `priority`, `published`, `language`, `metadata`  

URLs are required because they uniquely identify content and allow AI systems to verify, deduplicate, and cross‑reference pages.

---

## 5. Metadata and Language Support

The metadata object may include:

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### Multilingual Support

Language can be defined at:

- File level  
- Section level  
- Entry level  

Use BCP‑47 language codes (e.g., `en`, `es`, `pt-BR`, `zh-CN`).

This helps AI systems:

- Choose correct tokenizers  
- Avoid mixing languages  
- Improve retrieval accuracy  
- Reduce hallucinations  

---

## 6. Tags and Priority

### 6.1 Tags

Tags provide lightweight semantic cues. They must be:

- Lowercase  
- Single‑word  
- ASCII  
- Hyphenated for multi‑word concepts  
- Consistent across entries  

Examples:

| Concept | Valid Tag |
|---------|-----------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

Tags help AI systems cluster, disambiguate, and retrieve content more accurately.

### 6.2 Priority

Values:

- `high` — flagship content  
- `medium` — default  
- `low` — niche or outdated  

Use `high` sparingly (5–10% of entries).

---

## 7. Delta Updates (`olamip-delta.json`)

Websites evolve constantly. OLAMIP supports incremental updates through an optional companion file:

`/olamip-delta.json`

This file contains only:

- Added entries  
- Updated entries  
- Removed URLs  

### 7.1 Why Delta Updates Matter

- AI systems stay fresh without reprocessing the full file.  
- Large catalogs remain manageable.  
- Updates propagate quickly.  
- No missed changes.  

### 7.2 Rolling Delta Window (Recommended)

Keep all deltas from the last 7–30 days in a single file.  
AI systems apply them chronologically to stay synchronized.

### 7.3 Versioned Delta Files (Alternative)

Publish separate files:

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`  
...

Ideal for high‑volume sites (news, ecommerce, documentation).

### 7.4 Discovery

Only the main OLAMIP file needs `<link>` and `<meta>` tags.  
AI systems automatically look for delta files in the same directory.

---

## 8. OLAMIP vs Traditional Sitemaps

- XML Sitemap: “Here are my pages.”  
- OLAMIP: “Here is what my site means.”  

Sitemaps list URLs.  
OLAMIP provides:

- Summaries  
- Content types  
- Section hierarchy  
- Tags  
- Metadata  
- Priorities  
- Ingestion policies  

Together with schema.org, OLAMIP gives AI systems a complete, human‑curated understanding of your site.

---

## 9. Frequently Asked Questions (Summary)

### Does OLAMIP expose private information?

No. It only summarizes publicly available content.

### Can competitors use my OLAMIP file?

No more than they already use your public pages.

### How large can an OLAMIP file be?

1–10 MB is typical.  
Even 3,000+ entries is normal and safe.

### How often should I update it?

Whenever content changes — treat it like a sitemap for AI.

### Do I need to update the main file if I use deltas?

Yes. `olamip.json` is always the authoritative snapshot.

### Will LLMs actually read this?

Yes — that is the purpose of the protocol.  
You can test with:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```

---

## 10. Summary

OLAMIP transforms your website into a machine‑interpretable knowledge map:

- `olamip.json` — your full, structured snapshot  
- `olamip-delta.json` — your incremental change log  
- Sections, subsections, entries — a clear hierarchy  
- Summaries, tags, priorities — human‑curated meaning  
- Discovery tags — reliable ingestion  
- Multilingual support — global readiness  

By adopting OLAMIP, you ensure that AI systems understand your content the way you intend — accurately, consistently, and at scale.
