# Frequently Asked Questions

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## What is OLAMIP?

OLAMIP stands for **Open Language‑Aligned Machine‑Interpretable Protocol**. It is a simple, open format that lets websites publish structured summaries of their content so large language models (LLMs) can understand and learn from them more effectively.

## Why should I use OLAMIP?

Because LLMs already crawl your site, but they struggle to interpret it. With OLAMIP, you can:

- Teach AI exactly what your pages are about.
- Highlight your most valuable content.
- Improve discoverability in AI‑powered tools and search.
- Reduce misinterpretation and hallucination.

It is the difference between AI guessing and AI understanding.

## Does OLAMIP expose anything private?

No. OLAMIP only summarizes content that is already public on your website. It does not reveal anything beyond what is already visible in your HTML, metadata, or SEO markup.

## Can competitors use my OLAMIP file?

No more than they already use your public pages, metadata, or sitemap. OLAMIP actually gives you more control over how AI systems interpret your content, not less.

## What does an `olamip.json` file look like?

An `olamip.json` file is a structured, JSON‑formatted document that describes your site’s content, hierarchy, and metadata in a way AI systems can easily interpret. If you would like to explore the full technical structure, you can visit the [File Format Specification](https://olamip.org/file-format-specification) page, or see a real‑world example by reviewing the [TimeLAX.com use case](https://olamip.org/how-olamip-transforms-ai-discovery-a-timelax-com-use-case/).

## Where should I host the file?

At the root of your domain: `https://yourdomain.com/olamip.json`

You can also add discovery tags to your homepage:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## What does the “priority” field do?

It signals how important a page is for LLMs. Use it sparingly and intentionally:

- `high` → cornerstone, mission‑critical content.
- `medium` → default for most pages.
- `low` → niche, outdated, or low‑value content.

If everything is marked `high`, nothing is.

## Why do I need to include URLs if summaries are already provided?

Summaries tell an AI what a page means, but URLs tell an AI what a page is. A summary is descriptive, but it is not a unique identifier. The URL is the only stable, canonical reference to the actual page on your site.

AI systems use URLs to:

- Fetch and verify the full content.
- Avoid indexing duplicates.
- Detect when pages move or change.
- Connect OLAMIP data to schema.org markup, sitemaps, and crawlers.
- Cite or reference the page when generating responses.

Without URLs, two different pages with similar summaries would be indistinguishable. Including the URL ensures that OLAMIP remains grounded in the real structure of your website and that AI systems can reliably interpret and retrieve your content.

## What does the “policy” field do, and do I need to set it?

The `policy` field controls whether AI systems are allowed to ingest a section, subsection, or entry. Valid values are `"allow"` and `"forbid"`. If you omit the field, OLAMIP automatically inherits the policy from the nearest ancestor. If no ancestor defines a policy, the default is `"allow"`.

Most sites can simply omit the field entirely. Use `"forbid"` only when you want to explicitly exclude certain pages or sections from AI ingestion.

## What is the difference between OLAMIP and traditional sitemaps?

While a sitemap is a navigation index, OLAMIP is a machine‑interpretable knowledge map.

### XML Sitemap

- Lists URLs.
- Provides crawl hints.
- Helps search engines discover pages.
- Has no semantic meaning.
- Has no content hierarchy beyond URL structure.

### OLAMIP

- Defines content types (`page`, `project`, `doc_page`, etc.).
- Defines section types (`project_group`, `doc_category`, etc.).
- Provides canonical descriptions.
- Provides summaries optimized for LLMs.
- Provides tags and metadata.
- Establishes explicit hierarchy.
- Gives AI systems a structured representation of the entire site.

In other words:

- XML Sitemap = “Here are my pages.”
- OLAMIP = “Here is what my site means.”

That is why AI systems can use OLAMIP far more effectively than a traditional sitemap. It is not just a list of URLs; it is a machine‑readable content map that removes ambiguity and helps AI interpret the site the way a human would.

## Is an OLAMIP file with 3,000+ URLs too large?

Not at all. A file with 3,000 entries is well within safe limits. Even with long summaries, the total size would be around 6 MB, which is trivial for modern servers, browsers, and AI systems.

## What is the typical size of an OLAMIP entry?

| Summary Length | Approx. Size per Entry |
|---|---|
| Short | 300–600 bytes |
| Long | 1–2 KB |

## What makes OLAMIP scalable despite large file sizes?

- Hierarchical structure: Sections → subsections → entries reduce repetition.
- Concise summaries: Typically under 500 characters.
- Efficient compression: Gzip or Brotli can reduce file size by 70–90%.

## How large can an OLAMIP file get before performance issues arise?

| File Size | Approx. URLs | Impact |
|---|---:|---|
| 1–10 MB | 500–5,000 | Perfectly fine. Normal. |
| 10–25 MB | 5,000–12,000 | Still fine. Slightly slower load. |
| 25–50 MB | 12,000–25,000 | Large but manageable. |
| 50–100 MB | 25,000–50,000 | Heavy. Some systems may slow. |
| 100+ MB | 50,000+ | Too large for many LLM pipelines. |

For most webmasters, OLAMIP files with tens of thousands of entries remain well within safe and scalable limits.

## How often should I update it?

Whenever you publish new content or revise existing pages. Treat it like a sitemap, but for AI.

## Do I need to update the main file if I use `olamip-delta.json`?

Yes. The main `olamip.json` must always reflect the current state of your website. The optional `olamip-delta.json` file contains only recent changes, such as new pages, updates, or removals. AI systems use the full file as the authoritative snapshot and apply deltas to stay up‑to‑date between full refreshes.

## Do I need to add a `<link>` tag for `olamip-delta.json` in my HTML?

No. Only the main `olamip.json` file needs to be referenced in your site’s `<head>` section. That reference serves as the discovery mechanism that tells AI systems where your OLAMIP metadata lives. The delta file is different; it is an optional, supplemental update stream that AI systems look for automatically once they know the location of your main file. As long as `olamip-delta.json` is placed in the same directory as `olamip.json`, AI systems will detect and use it without requiring any additional `<link>` tags.

## Will LLMs actually read this?

Yes, that is the goal. As adoption grows, LLMs will prioritize structured sources like OLAMIP for training and retrieval. Early adopters will shape how AI understands the web.

## How can I test whether an AI bot can crawl my `olamip.json` file?

Try this for OpenAI, Anthropic, and Perplexity, respectively:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```
