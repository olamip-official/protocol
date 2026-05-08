# OLAMIP Quick Start

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

This quick start shows how to implement OLAMIP on a website in the simplest possible way.

## 1. Create `olamip.json`

Place a valid `olamip.json` file at the root of your site:

`https://yourdomain.com/olamip.json`

Start with a small, clean snapshot of your most important pages.

```json
{
  "protocol": "OLAMIP",
  "version": "1.0",
  "identity": {
    "name": "Example Site",
    "type": "blog",
    "canonical_description": "A website about technology and product updates.",
    "tags": ["technology", "blog"]
  },
  "content": {
    "overview": {
      "summary": "A technology blog covering product news, tutorials, and opinion pieces."
    },
    "sections": [
      {
        "title": "Blog",
        "summary": "Articles and guides.",
        "url": "https://yourdomain.com/blog/",
        "section_type": "blog_category",
        "entries": [
          {
            "title": "Welcome to the Blog",
            "summary": "An introduction to the site’s content and purpose.",
            "url": "https://yourdomain.com/blog/welcome/",
            "content_type": "blog_article",
            "tags": ["intro", "blog"],
            "priority": "high",
            "language": "en"
          }
        ]
      }
    ]
  },
  "metadata": {
    "last_updated": "2026-04-06",
    "language": "en",
    "source_url": "https://yourdomain.com/",
    "copyright": "© 2026 Example Site"
  }
}
```

## 2. Add discovery tags to your HTML

Add these to the `<head>` section of your homepage and key pages:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## 3. Keep summaries concise

Write short, factual summaries that explain what the page is about and why it matters. Avoid marketing language, duplicate text, and vague descriptions.

Good:

- “A beginner guide to long exposure photography.”
- “Product details and pricing for the enterprise plan.”

Avoid:

- “The best page on the internet.”
- “Amazing content you will love.”

## 4. Use the correct content types

Choose the most specific `content_type` you can:

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

## 5. Organize with sections

Use `sections` for content groups and `entries` for individual pages. If needed, add `subsections` to create deeper structure.

Example:

- Section: Blog
- Subsection: Tutorials
- Entry: How to Use OLAMIP

## 6. Set priority intentionally

Use:

- `high` for your most important pages.
- `medium` for normal pages.
- `low` for niche or old pages.

Do not mark everything as `high`.

## 7. Use policy to control AI ingestion

The `policy` field tells AI systems whether they are allowed to ingest a section, subsection, or entry.

- Use `"allow"` to explicitly allow ingestion.
- Use `"forbid"` to tell AI systems to avoid a specific section, subsection, or entry.

If you omit the `policy` field, OLAMIP inherits the policy from the nearest ancestor. If no ancestor defines a policy, the effective policy is `"allow"`. For most sites, you can leave `policy` out and only use `"forbid"` where you want AI systems to skip content.

Examples:

- Skip an entire section:

  ```json
  {
    "title": "Deprecated Guides",
    "summary": "Outdated content, not for AI.",
    "url": "https://yourdomain.com/old-guides/",
    "section_type": "doc_category",
    "policy": "forbid"
  }
  ```

- Skip a single page:

  ```json
  {
    "title": "Internal Test Page",
    "summary": "Page used for staging only.",
    "url": "https://yourdomain.com/test/",
    "content_type": "page",
    "policy": "forbid"
  }
  ```

## 8. Add language metadata

Use BCP-47 language codes such as:

- `en`
- `es`
- `fr`
- `de`
- `pt-BR`
- `zh-CN`

Set language at the file level, section level, or entry level as needed.

## 9. Update the file regularly

Whenever you add, change, or remove important pages, update `olamip.json`.

If your site changes often, also maintain `olamip-delta.json` in the same directory so AI systems can stay current between full refreshes.

## 10. Validate before publishing

Before going live, make sure:

- The JSON is valid.
- URLs are absolute.
- Required fields are present.
- Summaries are clear and under the recommended length.
- Tags are normalized and consistent.
- The file is served from the root path.

## Minimal implementation checklist

- [ ] Create `olamip.json`.
- [ ] Host it at the site root.
- [ ] Add discovery tags to the HTML `<head>`.
- [ ] Include sections and entries for key content.
- [ ] Add metadata and language fields.
- [ ] Keep summaries short and accurate.
- [ ] Update it whenever the site changes.
- [ ] Add `olamip-delta.json` if you want incremental updates.

## Suggested starting point

If you are unsure where to begin, implement OLAMIP for only your top pages first:

- Homepage.
- Blog index.
- Best articles.
- Key product or service pages.
- Important documentation pages.

Then expand from there.
