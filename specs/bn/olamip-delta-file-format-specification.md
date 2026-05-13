# OLAMIP‑DELTA ফাইল ফরম্যাট স্পেসিফিকেশন  
*কপিরাইট © ২০২৫ রালফ গনজালেজ – https://olamip.org*

OLAMIP‑DELTA প্রোটোকল `olamip.json`‑এ ক্রমবর্ধমান (incremental) আপডেটের জন্য একটি JSON‑ভিত্তিক ডেল্টা ফাইল ফরম্যাট নির্ধারণ করে। এই স্পেসিফিকেশনটি সেই ওয়েবমাস্টার এবং ইমপ্লিমেন্টারদের জন্য তৈরি, যারা structured data, HTML এবং sitemap‑ধরনের প্রকাশনার সাথে পরিচিত।

---

## 1. ওভারভিউ

`olamip.json` আপনার সাইটের কাঠামো এবং কনটেন্টের পূর্ণ, authoritative স্ন্যাপশট।  
`olamip-delta.json` একটি ঐচ্ছিক সহকারী ফাইল, যা শেষ আপডেটের পর থেকে হওয়া পরিবর্তনগুলো বর্ণনা করে।

দুটি ফাইল মিলে একটি মেশিন‑ব্যাখ্যাযোগ্য পরিবর্তন‑ধারা তৈরি করে:

- `olamip.json`: পূর্ণ, কাঠামোবদ্ধ স্ন্যাপশট  
- `olamip-delta.json`: ক্রমবর্ধমান পরিবর্তন‑লগ  

---

## 2. মূল ধারণা

- **Entry** — পাতা‑স্তরের কনটেন্ট (যেমন ব্লগ আর্টিকেল, পণ্য পৃষ্ঠা, ডক পৃষ্ঠা)।  
- **Section / Subsection** — শ্রেণিবদ্ধ গ্রুপিং (যেমন ব্লগ ক্যাটাগরি, ডক ক্যাটাগরি, পণ্য সংগ্রহ)।  
- **Operation** — `added`, `updated`, `removed` এর যেকোনো একটি।  
- **Delta** — একটি দিনের পরিবর্তন‑সেট, যাতে `added`, `updated`, `removed` থাকে।

এই ফরম্যাট:

- **মেশিন‑ব্যাখ্যাযোগ্য‑প্রথম**, তবে মানব‑পাঠযোগ্যও।  
- **ক্রমবর্ধমান**, যাতে AI সিস্টেম পুরো সাইট পুনরায় প্রক্রিয়া না করেও আপ‑টু‑ডেট থাকতে পারে।  

---

## 3. ফাইল অবস্থান ও ডিসকভারি

### 3.1 ফাইল অবস্থান

`olamip.json` রুট ডিরেক্টরিতে থাকতে হবে:

```text
https://yourdomain.com/olamip.json
```

`olamip-delta.json` একই ডিরেক্টরিতে থাকতে হবে:

```text
https://yourdomain.com/olamip-delta.json
```

ভার্সনড ডেল্টা (ঐচ্ছিক):

```text
/olamip.json
/olamip-delta-YYYY-MM-DD.json
```

### 3.2 ডিসকভারি মেকানিজম

ওয়েবমাস্টারদের `<head>`‑এ `olamip.json` ঘোষণা করতে হবে:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

AI সিস্টেম স্বয়ংক্রিয়ভাবে একই ডিরেক্টরিতে `olamip-delta.json` খুঁজে নেয়।  
`olamip-delta.json`‑এর জন্য আলাদা `<link>` বা `<meta>` প্রয়োজন নেই।

---

## 4. টপ‑লেভেল স্ট্রাকচার

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "window_days": 7,
  "last_updated": "2026-04-28",
  "deltas": [ ... ]
}
```

### 4.1 আবশ্যিক টপ‑লেভেল ফিল্ড

- `protocol`: অবশ্যই `"OLAMIP-DELTA"`  
- `version`: ডকুমেন্টেড ভার্সন স্কিম অনুসরণ করতে হবে  
- `window_days`: ঐচ্ছিক; rolling window‑এর দিন সংখ্যা  
- `last_updated`: ISO‑8601 তারিখ  
- `deltas`: non‑empty array  

### 4.2 `deltas` array‑এর নিয়ম

- তারিখ অনুযায়ী ascending ক্রম  
- প্রতিটি delta‑তে ISO‑8601 `date`  
- array খালি নয়  

---

## 5. Delta অবজেক্ট স্ট্রাকচার

```json
{
  "date": "2026-04-27",
  "added": [ ... ],
  "updated": [ ... ],
  "removed": [ ... ]
}
```

সব delta‑তেই তিনটি অপারেশন ফিল্ড থাকতে হবে, এমনকি খালি হলেও।

---

## 6. অপারেশনসমূহ

### 6.1 `added`

`added` আইটেমগুলো **পূর্ণ অবজেক্ট**।

Entry‑র জন্য:

- `title`, `summary`, `url`, `content_type`  
- ঐচ্ছিক: `tags`, `priority`, `policy`, `language`, `metadata`

নিয়ম:

- `url` আবশ্যিক  
- পূর্ণ অবজেক্ট হতে হবে  
- একই delta‑তে URL পুনরাবৃত্তি নয়  

---

### 6.2 `updated`

`updated` হতে পারে:

- **partial object** (শুধু পরিবর্তিত ফিল্ড)  
- **full object**  

নিয়ম:

- `url` আবশ্যিক  
- অনুপস্থিত ফিল্ড = অপরিবর্তিত  
- URL পরিবর্তন allowed নয় (remove + add করতে হবে)  

---

### 6.3 `removed`

`removed` আইটেম **শুধু URL** ধারণ করে:

```json
{ "url": "https://example.com/page/" }
```

নিয়ম:

- শুধুই `url`  
- canonical URL  
- সেকশন মুছলে তার সব সন্তানও মুছে যায়  

---

## 7. URL ও পরিচয়

- URL হলো প্রাথমিক পরিচয়  
- একই delta‑তে URL পুনরাবৃত্তি নয়  
- URL পরিবর্তন = remove + add  
- স্থিতিশীল canonical URL ব্যবহার করুন  

---

## 8. Rolling Delta Window (প্রস্তাবিত)

- delta ফাইল শুধুমাত্র শেষ `window_days`‑এর পরিবর্তন রাখে  
- AI সিস্টেম সব delta ক্রমানুসারে প্রয়োগ করে  

---

## 9. Versioned Deltas (বিকল্প)

প্রতিদিনের জন্য আলাদা delta ফাইল:

```text
/olamip-delta-2026-03-01.json
/olamip-delta-2026-03-02.json
```

AI সিস্টেম:

- সব delta ফাইল খুঁজে  
- তারিখ অনুযায়ী sort করে  
- window‑এর মধ্যে থাকা ফাইল প্রয়োগ করে  

---

## 10. কাঠামোগত পরিবর্তন: Sections ও Subsections

### 10.1 যোগ করা

`added`‑এ পূর্ণ section অবজেক্ট।

### 10.2 আপডেট করা

`updated`‑এ `url` + পরিবর্তিত ফিল্ড।

### 10.3 মুছে ফেলা

`removed`‑এ শুধুই `url`।

---

## 11. Conformance

### 11.1 MUST

- `"OLAMIP-DELTA"`  
- বৈধ `version`  
- non‑empty `deltas`  
- ascending তারিখ  
- প্রতিটি delta‑তে `added`, `updated`, `removed`  
- canonical URLs  
- URL পুনরাবৃত্তি নয়  
- `removed`‑এ শুধুই `url`  

### 11.2 SHOULD

- rolling window ব্যবহার  
- `last_updated` সঠিক রাখা  
- স্থিতিশীল canonical URLs  

### 11.3 MAY

- versioned deltas ব্যবহার  
- diagnostics metadata যোগ করা  

---

## 12. বেস্ট প্র্যাকটিস

- delta generation স্বয়ংক্রিয় করুন  
- `olamip.json` authoritative রাখুন  
- canonical URLs ব্যবহার করুন  
- summaries ও metadata সঙ্গতিপূর্ণ রাখুন  

---

## 13. সম্পর্কিত রিসোর্স

- **OLAMIP File Format Specification**  
- **OLAMIP FAQ**  
- **OLAMIP‑DELTA JSON Schema**  
- **Why OLAMIP Is Superior to LLMs.txt**  

---

এতেই OLAMIP‑DELTA ফাইল ফরম্যাট স্পেসিফিকেশন সম্পূর্ণ হলো।  
ওয়েবমাস্টার ও ইমপ্লিমেন্টাররা এটি ব্যবহার করে production‑grade `olamip-delta.json` তৈরি, যাচাই ও প্রয়োগ করতে পারবেন।
