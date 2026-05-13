# OLAMIP‑DELTA फ़ाइल फ़ॉर्मेट विनिर्देश  
*कॉपीराइट © 2025 राल्फ गोंजालेज़ – https://olamip.org*

OLAMIP‑DELTA प्रोटोकॉल `olamip.json` में क्रमिक (incremental) अपडेट के लिए JSON‑आधारित डेल्टा फ़ाइल फ़ॉर्मेट को परिभाषित करता है। यह विनिर्देश उन वेबमास्टर्स और इम्प्लीमेंटर्स के लिए बनाया गया है जो structured data, HTML और sitemap‑शैली प्रकाशन से परिचित हैं।

---

## 1. अवलोकन

`olamip.json` आपकी साइट की संरचना और सामग्री का पूर्ण, authoritative स्नैपशॉट है।  
`olamip-delta.json` एक वैकल्पिक सहायक फ़ाइल है जो पिछले अपडेट के बाद से हुए परिवर्तनों का वर्णन करती है।

दोनों मिलकर मशीन‑व्याख्यायोग्य परिवर्तन‑धारा बनाते हैं:

- `olamip.json`: पूर्ण, संरचित स्नैपशॉट  
- `olamip-delta.json`: क्रमिक परिवर्तन‑लॉग  

---

## 2. मुख्य अवधारणाएँ

- **Entry** — पत्ती‑स्तर की सामग्री (ब्लॉग लेख, उत्पाद पृष्ठ, डॉक पृष्ठ आदि)।  
- **Section / Subsection** — श्रेणीबद्ध समूह (ब्लॉग श्रेणी, डॉक श्रेणी, उत्पाद संग्रह आदि)।  
- **Operation** — `added`, `updated`, `removed` में से एक।  
- **Delta** — एक दिन‑आधारित परिवर्तन‑सेट जिसमें `added`, `updated`, `removed` शामिल होते हैं।

यह फ़ॉर्मेट:

- **मशीन‑व्याख्यायोग्य‑प्रथम** है, लेकिन मानव‑पठनीय भी।  
- **क्रमिक** है, ताकि AI सिस्टम बिना पूरी साइट को पुनः‑प्रोसेस किए सिंक रह सकें।  

---

## 3. फ़ाइल स्थान और खोज

### 3.1 फ़ाइल स्थान

`olamip.json` डोमेन की रूट पर होना चाहिए:

```text
https://yourdomain.com/olamip.json
```

`olamip-delta.json` उसी डायरेक्टरी में होना चाहिए:

```text
https://yourdomain.com/olamip-delta.json
```

वर्ज़न‑आधारित डेल्टा (वैकल्पिक):

```text
/olamip.json
/olamip-delta-YYYY-MM-DD.json
```

### 3.2 खोज तंत्र

वेबमास्टर्स को `<head>` में `olamip.json` का स्थान घोषित करना चाहिए:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

AI सिस्टम जो `olamip.json` जानते हैं, वे स्वतः ही उसी डायरेक्टरी में `olamip-delta.json` खोजते हैं।  
`olamip-delta.json` के लिए अलग `<link>` या `<meta>` की आवश्यकता नहीं है।

---

## 4. शीर्ष‑स्तरीय संरचना

`olamip-delta.json` एक JSON ऑब्जेक्ट है:

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "window_days": 7,
  "last_updated": "2026-04-28",
  "deltas": [
    {
      "date": "2026-04-27",
      "added": [ ... ],
      "updated": [ ... ],
      "removed": [ ... ]
    }
  ]
}
```

### 4.1 आवश्यक शीर्ष‑स्तरीय फ़ील्ड

- **`protocol`** — `"OLAMIP-DELTA"` होना अनिवार्य  
- **`version`** — दस्तावेज़ित वर्ज़न स्कीम से मेल खाना चाहिए  
- **`window_days`** — वैकल्पिक; rolling window के दिनों की संख्या  
- **`last_updated`** — ISO‑8601 तिथि  
- **`deltas`** — दिन‑आधारित परिवर्तन‑सेट की non‑empty array  

### 4.2 `deltas` array और तिथि क्रम

- `deltas` को तिथि के अनुसार **ascending** क्रम में होना चाहिए  
- प्रत्येक delta में ISO‑8601 `date` होना चाहिए  
- array खाली नहीं हो सकती  

---

## 5. Delta ऑब्जेक्ट संरचना

प्रत्येक delta ऑब्जेक्ट:

```json
{
  "date": "2026-04-27",
  "added": [ ... ],
  "updated": [ ... ],
  "removed": [ ... ]
}
```

वेबमास्टर्स को **तीनों फ़ील्ड अनिवार्य रूप से शामिल करने चाहिए**, भले ही वे खाली हों।

---

## 6. ऑपरेशन्स

### 6.1 `added`

`added` आइटम **पूर्ण ऑब्जेक्ट** होते हैं (entry, section या subsection)।

Entry के लिए आवश्यक फ़ील्ड:

- `title`  
- `summary`  
- `url`  
- `content_type`  
- वैकल्पिक: `tags`, `priority`, `policy`, `language`, `metadata`

नियम:

- `added` में `url` अनिवार्य  
- पूर्ण ऑब्जेक्ट होना चाहिए  
- एक ही delta में कोई URL दो बार नहीं आ सकता  

---

### 6.2 `updated`

`updated` आइटम:

- **partial object** हो सकते हैं (केवल बदले हुए फ़ील्ड)  
- या **full object**  

नियम:

- `url` अनिवार्य  
- गायब फ़ील्ड = अपरिवर्तित  
- URL बदलना allowed नहीं — इसके लिए remove + add करना होगा  

---

### 6.3 `removed`

`removed` आइटम **न्यूनतम** होते हैं:

```json
{ "url": "https://example.com/page/" }
```

नियम:

- केवल `url`  
- कोई अन्य फ़ील्ड नहीं  
- URL canonical होना चाहिए  
- सेक्शन हटाने पर उसके सभी descendants हटे माने जाते हैं  

---

## 7. URL और पहचान

- URL प्राथमिक पहचान है  
- एक delta में कोई URL दो बार नहीं आ सकता  
- URL बदलने के लिए remove + add आवश्यक  
- स्थिर canonical URLs का उपयोग करें  

---

## 8. Rolling Delta Window (अनुशंसित)

Rolling window में:

- delta फ़ाइल केवल पिछले `window_days` के परिवर्तन रखती है  
- AI सिस्टम सभी deltas को क्रम में लागू करते हैं  

उदाहरण: 7‑day window

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "window_days": 7,
  "last_updated": "2026-04-28",
  "deltas": [ ... ]
}
```

---

## 9. Versioned Deltas (वैकल्पिक)

प्रत्येक दिन के लिए अलग delta फ़ाइल:

```text
/olamip-delta-2026-03-01.json
/olamip-delta-2026-03-02.json
```

AI सिस्टम:

- सभी delta फ़ाइलें खोजते हैं  
- तिथि के अनुसार sort करते हैं  
- window के भीतर आने वाली फ़ाइलें लागू करते हैं  

---

## 10. संरचनात्मक परिवर्तन: Sections और Subsections

### 10.1 जोड़ना

`added` में पूर्ण section ऑब्जेक्ट शामिल करें।

### 10.2 अपडेट करना

`updated` में `url` + बदले हुए फ़ील्ड शामिल करें।

### 10.3 हटाना

`removed` में केवल `url`।

---

## 11. Conformance

### 11.1 MUST

- `protocol: "OLAMIP-DELTA"`  
- वैध `version`  
- non‑empty `deltas`  
- तिथि ascending क्रम में  
- प्रत्येक delta में `added`, `updated`, `removed`  
- सभी URLs canonical  
- एक delta में URL दोहराया नहीं जा सकता  
- `removed` में केवल `url`  

### 11.2 SHOULD

- rolling window अपनाएँ  
- `last_updated` सटीक रखें  
- स्थिर canonical URLs उपयोग करें  

### 11.3 MAY

- versioned deltas उपयोग कर सकते हैं  
- diagnostics metadata जोड़ सकते हैं  

---

## 12. सर्वोत्तम प्रथाएँ

- delta generation को स्वचालित करें  
- `olamip.json` को authoritative रखें  
- canonical URLs का उपयोग करें  
- summaries और metadata को सुसंगत रखें  

---

## 13. संबंधित संसाधन

- **OLAMIP File Format Specification**  
- **OLAMIP FAQ**  
- **OLAMIP‑DELTA JSON Schema**  
- **Why OLAMIP Is Superior to LLMs.txt**  

---

यह OLAMIP‑DELTA फ़ाइल फ़ॉर्मेट विनिर्देश पूर्ण होता है।  
वेबमास्टर्स और इम्प्लीमेंटर्स इसे उत्पादन‑स्तर पर `olamip-delta.json` बनाने, सत्यापित करने और उपयोग करने के लिए संदर्भ के रूप में उपयोग कर सकते हैं।
