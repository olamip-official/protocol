# OLAMIP Overview

*কপিরাইট © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP একটি উন্মুক্ত মান, যা ওয়েবসাইটগুলোকে AI সিস্টেমের সঙ্গে স্পষ্ট ও উদ্দেশ্যমূলকভাবে যোগাযোগ করতে সক্ষম করে। বিশৃঙ্খল HTML, CSS, এবং JavaScript থেকে অর্থ অনুমান করতে বড় ভাষা মডেলগুলোকে (LLMs) বাধ্য করার বদলে, OLAMIP আপনার সাইটের সবচেয়ে গুরুত্বপূর্ণ কনটেন্টের একটি পরিষ্কার, কাঠামোবদ্ধ, machine‑interpretable উপস্থাপন দেয়।

এই নথিটি OLAMIP-এর উদ্দেশ্য, দর্শন, ফাইল ফরম্যাট, এবং আপডেট মডেলের একটি উচ্চ‑স্তরের পরিচয় প্রদান করে।

---

## 1. OLAMIP কেন তৈরি করা হয়েছিল

আধুনিক ওয়েবসাইটগুলো ব্রাউজারের জন্য তৈরি, AI-এর জন্য নয়। ফলে LLM-গুলো সাধারণত সমস্যায় পড়ে:

- Layout-heavy HTML  
- JavaScript-rendered content  
- Navigation menus, ads, এবং boilerplate  
- Duplicate বা irrelevant pages  

LLM-গুলো *ইতিমধ্যেই* আপনার সাইট crawl করে, কিন্তু তারা প্রায়ই সেটিকে ভুলভাবে ব্যাখ্যা করে। OLAMIP এই সমস্যা সমাধান করে AI সিস্টেমকে ঠিক যা দরকার তা দিয়ে:

- একটি হালকা JSON ফাইল (`/olamip.json`)  
- পরিষ্কার, মানব-নির্বাচিত summaries  
- Structured metadata  
- Canonical URLs  
- Sections, subsections, এবং entries-এর একটি স্পষ্ট hierarchy  

OLAMIP গ্রহণ করে আপনি AI কীভাবে আপনার কনটেন্ট বোঝে ও উপস্থাপন করে, তার নিয়ন্ত্রণ নিজের হাতে নেন।

---

## 2. মূল দর্শন: Human-Friendly + Machine-Friendly

OLAMIP মানুষ এবং AI-এর মধ্যে একটি যোগাযোগ সেতু হিসেবে নকশা করা হয়েছে। প্রোটোকলের প্রতিটি অংশকে হতে হবে:

| নীতি | Human-Friendly | Machine-Friendly |
|-----------|----------------|------------------|
| File Format | লেখা ও যাচাই করা সহজ | কঠোর, পূর্বানুমেয় JSON |
| Field Names | স্বজ্ঞাত ও স্বব্যাখ্যামূলক | lowercase, schema-compliant |
| Summaries | পরিষ্কার, ব্র্যান্ড-সামঞ্জস্যপূর্ণ ভাষা | সংক্ষিপ্ত, তথ্যভিত্তিক, অর্থবহ |
| Priority System | সহজ লেবেল (`"high"`, `"medium"`, `"low"`) | ranking weights-এ ম্যাপযোগ্য |
| Documentation | সরল ভাষার গাইড | JSON schemas, validator rules |
| Tooling | Generators, CMS plugins | CLI tools, parsers, test suites |

- **মানুষের জন্য:** গভীর প্রযুক্তিগত দক্ষতা ছাড়াই OLAMIP ব্যবহার করা সহজ হতে হবে।  
- **মেশিনের জন্য:** OLAMIP-কে কাঠামোবদ্ধ, পূর্বানুমেয়, এবং দ্ব্যর্থতাহীন হতে হবে।

---

## 3. OLAMIP ফাইল (`/olamip.json`)

একটি OLAMIP ফাইল হলো একটি কাঠামোবদ্ধ JSON ডকুমেন্ট, যা এখানে হোস্ট করা হয়:

`https://yourdomain.com/olamip.json`

এতে থাকে:

- `protocol` — অবশ্যই `"OLAMIP"`  
- `version` — প্রোটোকল সংস্করণ  
- `identity` — আপনি কে  
- `content` — আপনার সাইটের কাঠামোবদ্ধ hierarchy  
- `metadata` — ভাষা, last updated, এবং অন্যান্য global fields  

### 3.1 Discovery Tags

AI সিস্টেম যেন নির্ভরযোগ্যভাবে আপনার OLAMIP ফাইল খুঁজে পায়, সেজন্য উভয়টি অন্তর্ভুক্ত করুন:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

দুটিই ব্যবহার করলে redundancy, compatibility, এবং future-proofing নিশ্চিত হয়।

---

## 4. ফাইলের গঠন

### 4.1 Identity Object

ওয়েবসাইট বা সংস্থাকে বর্ণনা করে:

- `name` — আবশ্যক  
- `type` — আবশ্যক  
- `canonical_description` — আবশ্যক  
- `tags` — ঐচ্ছিক  

### 4.2 Content Object

এতে থাকে:

- একটি `overview`  
- `sections`  
- ঐচ্ছিক `subsections`  
- `entries` (সবচেয়ে সূক্ষ্ম content units)  

এটি unlimited nesting depth সমর্থন করে।

### 4.3 Sections

একটি Section সম্পর্কিত কনটেন্টকে গোষ্ঠীবদ্ধ করে। আবশ্যক ক্ষেত্রগুলো:

- `title`  
- `summary`  
- `url`  
- `section_type`  

ঐচ্ছিক ক্ষেত্রগুলো:

- `policy` (`"allow"` বা `"forbid"`)  
- `tags`  
- `priority`  
- `published`  
- `language`  
- `subsections`  
- `entries`  

**Policy inheritance:**  
যদি না দেওয়া হয়, policy পূর্বপুরুষদের কাছ থেকে inherit হয়। ডিফল্ট হলো `"allow"`।

### 4.4 Entries

Entries পৃথক content item নির্দেশ করে, যেমন:

- Blog articles  
- News stories  
- Products  
- Documentation pages  
- Research papers  
- Media items  

আবশ্যক ক্ষেত্রগুলো:

- `title`  
- `summary`  
- `url`  
- `content_type`  

ঐচ্ছিক ক্ষেত্রগুলো:

- `policy`, `tags`, `priority`, `published`, `language`, `metadata`  

URL আবশ্যক, কারণ এগুলো কনটেন্টকে এককভাবে শনাক্ত করে এবং AI সিস্টেমকে পৃষ্ঠা verify, deduplicate, এবং cross-reference করতে সহায়তা করে।

---

## 5. Metadata এবং ভাষা সমর্থন

metadata object-এ থাকতে পারে:

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### বহুভাষিক সমর্থন

ভাষা নির্ধারণ করা যেতে পারে:

- File level  
- Section level  
- Entry level  

BCP‑47 language code ব্যবহার করুন (যেমন, `en`, `es`, `pt-BR`, `zh-CN`)।

এটি AI সিস্টেমকে সাহায্য করে:

- সঠিক tokenizer বেছে নিতে  
- ভাষা মিশে যাওয়া এড়াতে  
- retrieval accuracy উন্নত করতে  
- hallucinations কমাতে  

---

## 6. Tags এবং Priority

### 6.1 Tags

Tags হালকা semantic cue প্রদান করে। এগুলো হতে হবে:

- Lowercase  
- Single-word  
- ASCII  
- Multi-word ধারণার জন্য hyphenated  
- Entries জুড়ে consistent  

উদাহরণ:

| ধারণা | বৈধ Tag |
|---------|-----------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

Tags AI সিস্টেমকে content cluster, disambiguate, এবং আরও নির্ভুলভাবে retrieve করতে সাহায্য করে।

### 6.2 Priority

মানসমূহ:

- `high` — flagship content  
- `medium` — default  
- `low` — niche বা outdated  

`high` খুব সীমিতভাবে ব্যবহার করুন (entries-এর 5–10%)।

---

## 7. Delta Updates (`olamip-delta.json`)

ওয়েবসাইটগুলো নিয়মিত পরিবর্তিত হয়। OLAMIP একটি ঐচ্ছিক companion file-এর মাধ্যমে incremental updates সমর্থন করে:

`/olamip-delta.json`

এই ফাইলে শুধু থাকে:

- যোগ করা entries  
- আপডেট করা entries  
- সরানো URLs  

### 7.1 Delta Updates কেন গুরুত্বপূর্ণ

- পুরো ফাইল পুনরায় প্রক্রিয়া না করেও AI সিস্টেম আপডেটেড থাকে।  
- বড় catalog পরিচালনাযোগ্য থাকে।  
- আপডেট দ্রুত প্রয়োগ হয়।  
- পরিবর্তন বাদ পড়ে না।  

### 7.2 Rolling Delta Window (প্রস্তাবিত)

শেষ 7–30 দিনের সব delta একটি একক ফাইলে রাখুন।  
AI সিস্টেমগুলো chronological order-এ এগুলো প্রয়োগ করে synchronized থাকে।

### 7.3 Versioned Delta Files (বিকল্প)

আলাদা ফাইল প্রকাশ করুন:

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`  
...  

উচ্চ-ভলিউম সাইটের জন্য উপযুক্ত (news, ecommerce, documentation)।

### 7.4 Discovery

শুধু main OLAMIP ফাইলের জন্যই `<link>` এবং `<meta>` tags প্রয়োজন।  
AI সিস্টেম স্বয়ংক্রিয়ভাবে একই directory-তে delta file খোঁজে।

---

## 8. OLAMIP বনাম Traditional Sitemaps

- XML Sitemap: “এখানে আমার পৃষ্ঠাগুলো আছে।”  
- OLAMIP: “আমার সাইট কী বোঝায়, সেটাই এখানে।”  

Sitemap URL তালিকা করে।  
OLAMIP প্রদান করে:

- Summaries  
- Content types  
- Section hierarchy  
- Tags  
- Metadata  
- Priorities  
- Ingestion policies  

Schema.org-এর সঙ্গে মিলিয়ে OLAMIP AI সিস্টেমকে আপনার সাইটের একটি সম্পূর্ণ, মানব-নির্বাচিত ধারণা দেয়।

---

## 9. প্রায় জিজ্ঞাসিত প্রশ্ন (সারাংশ)

### OLAMIP কি ব্যক্তিগত তথ্য প্রকাশ করে?

না। এটি কেবল publicly available content-এর সারাংশ দেয়।

### প্রতিদ্বন্দ্বীরা কি আমার OLAMIP file ব্যবহার করতে পারে?

আপনার public pages তারা ইতিমধ্যেই যতটা ব্যবহার করতে পারে, তার চেয়ে বেশি নয়।

### একটি OLAMIP file কত বড় হতে পারে?

সাধারণত 1–10 MB হয়।  
3,000+ entries-ও স্বাভাবিক এবং নিরাপদ।

### কত ঘন ঘন এটি আপডেট করা উচিত?

যখনই content পরিবর্তিত হয় — AI-এর জন্য এটিকে sitemap-এর মতো বিবেচনা করুন।

### Delta ব্যবহার করলে কি main file আপডেট করতে হবে?

হ্যাঁ। `olamip.json` সবসময় authoritative snapshot।

### LLM-গুলো কি সত্যিই এটি পড়বে?

হ্যাঁ — এটাই প্রোটোকলের উদ্দেশ্য।  
আপনি পরীক্ষা করতে পারেন:

```bash
curl -A "GPTBot" [https://yourdomain.com/olamip.json](https://yourdomain.com/olamip.json)
curl -A "ClaudeBot" [https://yourdomain.com/olamip.json](https://yourdomain.com/olamip.json)
curl -A "PerplexityBot" [https://yourdomain.com/olamip.json](https://yourdomain.com/olamip.json)
```

---

## 10. সারাংশ

OLAMIP আপনার ওয়েবসাইটকে একটি machine-interpretable knowledge map-এ রূপান্তর করে:

- `olamip.json` — আপনার পূর্ণ, কাঠামোবদ্ধ snapshot  
- `olamip-delta.json` — আপনার incremental change log  
- Sections, subsections, entries — একটি পরিষ্কার hierarchy  
- Summaries, tags, priorities — মানব-নির্বাচিত অর্থ  
- Discovery tags — নির্ভরযোগ্য ingestion  
- Multilingual support — বৈশ্বিক প্রস্তুতি  

OLAMIP গ্রহণ করে আপনি নিশ্চিত করেন যে AI সিস্টেম আপনার কনটেন্টকে আপনার অভিপ্রায় অনুযায়ী বোঝে — সঠিকভাবে, ধারাবাহিকভাবে, এবং ব্যাপক পরিসরে।
