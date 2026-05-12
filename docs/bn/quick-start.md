# OLAMIP Quick Start

*কপিরাইট © 2025 Ralph Gonzalez – https://olamip.org*

এই কুইক স্টার্ট গাইডটি দেখায় কীভাবে সবচেয়ে সহজ উপায়ে একটি ওয়েবসাইটে OLAMIP বাস্তবায়ন করতে হয়।

## 1. `olamip.json` তৈরি করুন

আপনার সাইটের রুটে একটি বৈধ `olamip.json` ফাইল রাখুন:

`https://yourdomain.com/olamip.json`

আপনার সবচেয়ে গুরুত্বপূর্ণ পৃষ্ঠাগুলোর একটি ছোট ও পরিষ্কার স্ন্যাপশট দিয়ে শুরু করুন।

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": {
    "name": "Example Site",
    "type": "blog",
    "canonical_description": "প্রযুক্তি এবং পণ্য আপডেট সম্পর্কে একটি ওয়েবসাইট।",
    "tags": ["প্রযুক্তি", "blog"]
  },
  "content": {
    "overview": {
      "summary": "একটি প্রযুক্তি ব্লগ, যেখানে পণ্য সংবাদ, টিউটোরিয়াল, এবং মতামতভিত্তিক লেখা অন্তর্ভুক্ত।"
    },
    "sections": [
      {
        "title": "ব্লগ",
        "summary": "প্রবন্ধ এবং গাইড।",
        "url": "https://yourdomain.com/blog/",
        "section_type": "blog_category",
        "entries": [
          {
            "title": "ব্লগে স্বাগতম",
            "summary": "সাইটের বিষয়বস্তু এবং উদ্দেশ্যের একটি পরিচয়।",
            "url": "https://yourdomain.com/blog/welcome/",
            "content_type": "blog_article",
            "tags": ["intro", "blog"],
            "priority": "high",
            "language": "bn"
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

## 2. HTML-এ discovery tag যোগ করুন

আপনার হোমপেজ এবং গুরুত্বপূর্ণ পৃষ্ঠাগুলোর `<head>` অংশে এগুলো যোগ করুন:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## 3. সংক্ষিপ্ত summary ব্যবহার করুন

ছোট, তথ্যভিত্তিক সারাংশ লিখুন যা বোঝায় পৃষ্ঠাটি কী সম্পর্কে এবং কেন এটি গুরুত্বপূর্ণ। মার্কেটিং ভাষা, পুনরাবৃত্তি, এবং অস্পষ্ট বর্ণনা এড়িয়ে চলুন।

ভালো:

- “দীর্ঘ এক্সপোজার ফটোগ্রাফির জন্য একটি beginner guide.”
- “এন্টারপ্রাইজ প্ল্যানের জন্য পণ্যের বিবরণ এবং মূল্য নির্ধারণ।”

এড়িয়ে চলুন:

- “ইন্টারনেটের সেরা পৃষ্ঠা।”
- “অসাধারণ কন্টেন্ট যা আপনি ভালোবাসবেন।”

## 4. সঠিক content type ব্যবহার করুন

যতটা সম্ভব নির্দিষ্ট `content_type` বেছে নিন:

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

## 5. sections দিয়ে সংগঠিত করুন

কন্টেন্টের গ্রুপের জন্য `sections` এবং আলাদা পৃষ্ঠার জন্য `entries` ব্যবহার করুন। প্রয়োজনে আরও গভীর কাঠামোর জন্য `subsections` যোগ করতে পারেন।

উদাহরণ:

- Section: Blog
- Subsection: Tutorials
- Entry: How to Use OLAMIP

## 6. priority সচেতনভাবে সেট করুন

ব্যবহার করুন:

- `high` আপনার সবচেয়ে গুরুত্বপূর্ণ পৃষ্ঠাগুলোর জন্য।
- `medium` সাধারণ পৃষ্ঠাগুলোর জন্য।
- `low` বিশেষায়িত বা পুরোনো পৃষ্ঠাগুলোর জন্য।

সবকিছুকে `high` চিহ্নিত করবেন না।

## 7. AI ingestion নিয়ন্ত্রণে policy ব্যবহার করুন

`policy` ফিল্ড AI সিস্টেমকে জানায় তারা কোনো section, subsection, বা entry ingest করতে পারবে কি না।

- `"allow"` ব্যবহার করুন ingest করার অনুমতি দিতে।
- `"forbid"` ব্যবহার করুন AI সিস্টেমকে নির্দিষ্ট অংশ এড়াতে বলতে।

যদি `policy` ফিল্ড না দেন, OLAMIP নিকটতম ancestor থেকে policy inherit করে। যদি কোনো ancestor policy নির্ধারণ না করে, তাহলে কার্যকর policy হবে `"allow"`। বেশিরভাগ সাইটের জন্য, আপনি `policy` বাদ দিতে পারেন এবং যেখানে দরকার শুধু `"forbid"` ব্যবহার করতে পারেন।

উদাহরণ:

- পুরো একটি section বাদ দিন:

```json
{
  "title": "Deprecated Guides",
  "summary": "পুরোনো কন্টেন্ট, AI-এর জন্য নয়।",
  "url": "https://yourdomain.com/old-guides/",
  "section_type": "doc_category",
  "policy": "forbid"
}
```

- একটি single page বাদ দিন:

```json
{
  "title": "Internal Test Page",
  "summary": "শুধু staging-এর জন্য ব্যবহৃত পৃষ্ঠা।",
  "url": "https://yourdomain.com/test/",
  "content_type": "page",
  "policy": "forbid"
}
```

## 8. ভাষার metadata যোগ করুন

BCP-47 language code ব্যবহার করুন, যেমন:

- `en`
- `es`
- `fr`
- `de`
- `pt-BR`
- `zh-CN`

প্রয়োজনে ফাইল, section, বা entry স্তরে ভাষা সেট করুন।

## 9. ফাইলটি নিয়মিত আপডেট করুন

যখনই আপনি গুরুত্বপূর্ণ পৃষ্ঠা যোগ, পরিবর্তন, বা অপসারণ করেন, `olamip.json` আপডেট করুন।

আপনার সাইট যদি প্রায়ই বদলায়, তাহলে একই ডিরেক্টরিতে `olamip-delta.json`-ও রাখুন, যাতে full refresh-এর মাঝেও AI সিস্টেম আপডেটেড থাকতে পারে।

## 10. প্রকাশের আগে validate করুন

লাইভ করার আগে নিশ্চিত করুন:

- JSON বৈধ।
- URLs absolute।
- প্রয়োজনীয় ফিল্ড উপস্থিত।
- summaries পরিষ্কার এবং প্রস্তাবিত দৈর্ঘ্যের মধ্যে।
- tags normalized এবং consistent।
- ফাইলটি root path থেকে serve হচ্ছে।

## ন্যূনতম implementation checklist

- [ ] `olamip.json` তৈরি করুন।
- [ ] এটি সাইটের root-এ host করুন।
- [ ] HTML `<head>`-এ discovery tags যোগ করুন।
- [ ] প্রধান কন্টেন্টের জন্য sections এবং entries অন্তর্ভুক্ত করুন।
- [ ] metadata এবং language field যোগ করুন।
- [ ] summary সংক্ষিপ্ত এবং সঠিক রাখুন।
- [ ] সাইট পরিবর্তন হলে এটি আপডেট করুন।
- [ ] incremental updates-এর জন্য `olamip-delta.json` যোগ করুন।

## প্রস্তাবিত শুরু

আপনি কোথা থেকে শুরু করবেন তা নিয়ে নিশ্চিত না হলে, প্রথমে শুধু আপনার শীর্ষ পৃষ্ঠাগুলোর জন্য OLAMIP বাস্তবায়ন করুন:

- হোমপেজ।
- ব্লগ ইনডেক্স।
- সেরা প্রবন্ধ।
- প্রধান পণ্য বা পরিষেবা পৃষ্ঠা।
- গুরুত্বপূর্ণ ডকুমেন্টেশন পৃষ্ঠা।

তারপর ধীরে ধীরে এটি প্রসারিত করুন।
