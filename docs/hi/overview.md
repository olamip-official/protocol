# OLAMIP अवलोकन

*कॉपीराइट © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP एक खुला मानक है, जो वेबसाइटों को AI प्रणालियों के साथ स्पष्ट और उद्देश्यपूर्ण तरीके से संवाद करने में सक्षम बनाता है। अव्यवस्थित HTML, CSS, और JavaScript से अर्थ निकालने के लिए बड़े भाषा मॉडल्स (LLMs) को मजबूर करने के बजाय, OLAMIP आपकी साइट की सबसे महत्वपूर्ण सामग्री का एक साफ़, संरचित, machine‑interpretable प्रतिनिधित्व प्रदान करता है।

यह दस्तावेज़ OLAMIP के उद्देश्य, दर्शन, फ़ाइल फ़ॉर्मेट, और अपडेट मॉडल का एक उच्च-स्तरीय परिचय देता है।

---

## 1. OLAMIP क्यों बनाया गया

आधुनिक वेबसाइटें ब्राउज़र के लिए बनाई जाती हैं, AI के लिए नहीं। इसलिए LLMs अक्सर इन समस्याओं से जूझते हैं:

- Layout-heavy HTML  
- JavaScript-rendered content  
- Navigation menus, ads, और boilerplate  
- Duplicate या irrelevant pages  

LLMs *पहले से ही* आपकी साइट crawl करते हैं, लेकिन वे अक्सर उसे गलत समझते हैं। OLAMIP AI प्रणालियों को वही देता है जिसकी उन्हें सच में ज़रूरत होती है:

- एक हल्की JSON फ़ाइल (`/olamip.json`)  
- साफ़, मानव‑क्यूरेटेड summaries  
- संरचित metadata  
- Canonical URLs  
- Sections, subsections, और entries की स्पष्ट hierarchy  

OLAMIP अपनाकर आप तय करते हैं कि AI आपकी सामग्री को कैसे समझे और प्रस्तुत करे।

---

## 2. मूल दर्शन: Human-Friendly + Machine-Friendly

OLAMIP को मनुष्यों और AI के बीच एक संचार पुल के रूप में डिज़ाइन किया गया है। प्रोटोकॉल का हर हिस्सा होना चाहिए:

| सिद्धांत | Human-Friendly | Machine-Friendly |
|-----------|----------------|------------------|
| File Format | लिखना और validate करना आसान | कठोर, पूर्वानुमेय JSON |
| Field Names | सहज और स्वव्याख्यात्मक | lowercase, schema-compliant |
| Summaries | स्पष्ट, ब्रांड-संगत भाषा | संक्षिप्त, तथ्यात्मक, अर्थपूर्ण |
| Priority System | सरल labels (`"high"`, `"medium"`, `"low"`) | ranking weights से मैप करने योग्य |
| Documentation | सरल भाषा के गाइड | JSON schemas, validator rules |
| Tooling | Generators, CMS plugins | CLI tools, parsers, test suites |

- **मनुष्यों के लिए:** OLAMIP को गहरी तकनीकी विशेषज्ञता के बिना अपनाना आसान होना चाहिए।  
- **मशीनों के लिए:** OLAMIP को संरचित, पूर्वानुमेय, और असंदिग्ध होना चाहिए।

---

## 3. OLAMIP फ़ाइल (`/olamip.json`)

एक OLAMIP फ़ाइल एक संरचित JSON दस्तावेज़ है, जो यहाँ होस्ट की जाती है:

`https://yourdomain.com/olamip.json`

इसमें शामिल हैं:

- `protocol` — अनिवार्य रूप से `"OLAMIP"`  
- `version` — प्रोटोकॉल संस्करण  
- `identity` — आप कौन हैं  
- `content` — आपकी साइट की संरचित hierarchy  
- `metadata` — भाषा, last updated, और अन्य global fields  

### 3.1 Discovery Tags

यह सुनिश्चित करने के लिए कि AI प्रणालियाँ आपकी OLAMIP फ़ाइल को भरोसेमंद तरीके से ढूँढ सकें, दोनों शामिल करें:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

दोनों का उपयोग redundancy, compatibility, और future-proofing प्रदान करता है।

---

## 4. फ़ाइल संरचना

### 4.1 Identity Object

वेबसाइट या संगठन का वर्णन करता है:

- `name` — आवश्यक  
- `type` — आवश्यक  
- `canonical_description` — आवश्यक  
- `tags` — वैकल्पिक  

### 4.2 Content Object

इसमें शामिल है:

- एक `overview`  
- `sections`  
- वैकल्पिक `subsections`  
- `entries` (सबसे सूक्ष्म content units)  

यह unlimited nesting depth का समर्थन करता है।

### 4.3 Sections

एक Section संबंधित सामग्री को समूहित करता है। आवश्यक फ़ील्ड हैं:

- `title`  
- `summary`  
- `url`  
- `section_type`  

वैकल्पिक फ़ील्ड हैं:

- `policy` (`"allow"` या `"forbid"`)  
- `tags`  
- `priority`  
- `published`  
- `language`  
- `subsections`  
- `entries`  

**Policy inheritance:**  
यदि इसे छोड़ा जाए, तो policy पूर्वजों (ancestors) से inherit होती है। डिफ़ॉल्ट `"allow"` है।

### 4.4 Entries

Entries व्यक्तिगत content items का प्रतिनिधित्व करती हैं, जैसे:

- Blog articles  
- News stories  
- Products  
- Documentation pages  
- Research papers  
- Media items  

आवश्यक फ़ील्ड:

- `title`  
- `summary`  
- `url`  
- `content_type`  

वैकल्पिक फ़ील्ड:

- `policy`, `tags`, `priority`, `published`, `language`, `metadata`  

URLs आवश्यक हैं क्योंकि वे सामग्री की विशिष्ट पहचान करते हैं और AI प्रणालियों को pages को verify, deduplicate, और cross-reference करने में मदद करते हैं।

---

## 5. Metadata और भाषा समर्थन

metadata object में शामिल हो सकता है:

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### बहुभाषी समर्थन

भाषा निम्न स्तरों पर परिभाषित की जा सकती है:

- File level  
- Section level  
- Entry level  

BCP‑47 language codes का उपयोग करें (जैसे, `en`, `es`, `pt-BR`, `zh-CN`)।

यह AI प्रणालियों को मदद करता है:

- सही tokenizers चुनने में  
- भाषाओं को मिलाने से बचाने में  
- retrieval accuracy सुधारने में  
- hallucinations कम करने में  

---

## 6. Tags और Priority

### 6.1 Tags

Tags हल्के semantic cues प्रदान करते हैं। वे होने चाहिए:

- Lowercase  
- Single-word  
- ASCII  
- Multi-word concepts के लिए hyphenated  
- Entries भर में consistent  

उदाहरण:

| अवधारणा | वैध Tag |
|---------|-----------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

Tags AI प्रणालियों को content cluster करने, disambiguate करने, और अधिक सटीक रूप से retrieve करने में मदद करते हैं।

### 6.2 Priority

मान:

- `high` — flagship content  
- `medium` — default  
- `low` — niche या outdated  

`high` का उपयोग बहुत सीमित रूप से करें (entries का 5–10%)।

---

## 7. Delta Updates (`olamip-delta.json`)

वेबसाइटें लगातार बदलती रहती हैं। OLAMIP एक वैकल्पिक companion file के माध्यम से incremental updates का समर्थन करता है:

`/olamip-delta.json`

इस फ़ाइल में केवल यह शामिल होता है:

- जोड़ी गई entries  
- अपडेट की गई entries  
- हटाए गए URLs  

### 7.1 Delta Updates क्यों महत्वपूर्ण हैं

- AI प्रणालियाँ पूरी फ़ाइल फिर से प्रोसेस किए बिना अद्यतित रहती हैं।  
- बड़े catalogs प्रबंधनीय बने रहते हैं।  
- अपडेट तेज़ी से लागू होते हैं।  
- कोई बदलाव छूटता नहीं।  

### 7.2 Rolling Delta Window (अनुशंसित)

पिछले 7–30 दिनों के सभी delta एक ही फ़ाइल में रखें।  
AI प्रणालियाँ उन्हें कालानुक्रमिक रूप से लागू करती हैं ताकि वे synchronized रहें।

### 7.3 Versioned Delta Files (वैकल्पिक)

अलग-अलग फ़ाइलें प्रकाशित करें:

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`  
...  

यह उच्च-परिमाण वाली साइटों (news, ecommerce, documentation) के लिए आदर्श है।

### 7.4 Discovery

केवल main OLAMIP फ़ाइल को `<link>` और `<meta>` tags की आवश्यकता होती है।  
AI प्रणालियाँ उसी directory में delta files को स्वतः खोजती हैं।

---

## 8. OLAMIP बनाम पारंपरिक Sitemaps

- XML Sitemap: “यहाँ मेरे pages हैं।”  
- OLAMIP: “यह मेरी साइट का अर्थ है।”  

Sitemaps URLs सूचीबद्ध करते हैं।  
OLAMIP प्रदान करता है:

- Summaries  
- Content types  
- Section hierarchy  
- Tags  
- Metadata  
- Priorities  
- Ingestion policies  

Schema.org के साथ मिलकर OLAMIP AI प्रणालियों को आपकी साइट की एक पूर्ण, मानव‑क्यूरेटेड समझ देता है।

---

## 9. अक्सर पूछे जाने वाले प्रश्न (सारांश)

### क्या OLAMIP निजी जानकारी उजागर करता है?

नहीं। यह केवल सार्वजनिक रूप से उपलब्ध सामग्री का सार प्रस्तुत करता है।

### क्या प्रतिस्पर्धी मेरी OLAMIP फ़ाइल का उपयोग कर सकते हैं?

जितना वे पहले से आपकी सार्वजनिक pages का उपयोग कर सकते हैं, उससे अधिक नहीं।

### एक OLAMIP फ़ाइल कितनी बड़ी हो सकती है?

आमतौर पर 1–10 MB सामान्य है।  
3,000+ entries भी सामान्य और सुरक्षित हैं।

### इसे कितनी बार अपडेट करना चाहिए?

जब भी सामग्री बदलती है — AI के लिए इसे sitemap की तरह समझें।

### यदि मैं deltas का उपयोग करूँ, तो क्या main file को भी अपडेट करना होगा?

हाँ। `olamip.json` हमेशा authoritative snapshot है।

### क्या LLMs वास्तव में इसे पढ़ेंगे?

हाँ — यही इस protocol का उद्देश्य है।  
आप इसे इस तरह test कर सकते हैं:

```bash
curl -A "GPTBot" [https://yourdomain.com/olamip.json](https://yourdomain.com/olamip.json)
curl -A "ClaudeBot" [https://yourdomain.com/olamip.json](https://yourdomain.com/olamip.json)
curl -A "PerplexityBot" [https://yourdomain.com/olamip.json](https://yourdomain.com/olamip.json)
```

---

## 10. सारांश

OLAMIP आपकी वेबसाइट को एक machine-interpretable knowledge map में बदल देता है:

- `olamip.json` — आपका पूर्ण, संरचित snapshot  
- `olamip-delta.json` — आपका incremental change log  
- Sections, subsections, entries — एक स्पष्ट hierarchy  
- Summaries, tags, priorities — मानव‑क्यूरेटेड अर्थ  
- Discovery tags — भरोसेमंद ingestion  
- Multilingual support — वैश्विक तैयारी  

OLAMIP अपनाकर आप सुनिश्चित करते हैं कि AI प्रणालियाँ आपकी सामग्री को उसी तरह समझें जैसा आप चाहते हैं — सटीक, सुसंगत, और बड़े पैमाने पर।
