# OLAMIP Quick Start

*कॉपीराइट © 2025 Ralph Gonzalez – https://olamip.org*

यह क्विक स्टार्ट गाइड दिखाता है कि OLAMIP को वेबसाइट पर सबसे सरल तरीके से कैसे लागू किया जाए।

## 1. `olamip.json` बनाएँ

अपनी साइट के रूट पर एक वैध `olamip.json` फ़ाइल रखें:

`https://yourdomain.com/olamip.json`

अपने सबसे महत्वपूर्ण पृष्ठों का छोटा और साफ़ स्नैपशॉट से शुरुआत करें।

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": {
    "name": "Example Site",
    "type": "blog",
    "canonical_description": "प्रौद्योगिकी और उत्पाद अपडेट्स के बारे में एक वेबसाइट।",
    "tags": ["तकनीकी", "blog"]
  },
  "content": {
    "overview": {
      "summary": "एक टेक्नोलॉजी ब्लॉग जो उत्पाद समाचार, ट्यूटोरियल और विचार लेखों को कवर करता है।"
    },
    "sections": [
      {
        "title": "ब्लॉग",
        "summary": "लेख और गाइड।",
        "url": "https://yourdomain.com/blog/",
        "section_type": "blog_category",
        "entries": [
          {
            "title": "ब्लॉग में आपका स्वागत है",
            "summary": "साइट की सामग्री और उद्देश्य का परिचय।",
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

## 2. HTML में discovery tags जोड़ें

अपने होमपेज और प्रमुख पृष्ठों के `<head>` सेक्शन में यह जोड़ें:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## 3. संक्षेप (summaries) को छोटा रखें

छोटे, तथ्यात्मक सार लिखें जो बताते हों कि पेज किस बारे में है और क्यों महत्वपूर्ण है। मार्केटिंग भाषा, दोहराव, और अस्पष्ट विवरण से बचें।

अच्छा:

- “लॉन्ग एक्सपोज़र फोटोग्राफी के लिए शुरुआती गाइड।”
- “एंटरप्राइज़ प्लान के लिए उत्पाद विवरण और मूल्य निर्धारण।”

बचें:

- “इंटरनेट का सबसे बेहतरीन पेज।”
- “अद्भुत सामग्री जो आपको पसंद आएगी।”

## 4. सही content types का उपयोग करें

जितना संभव हो उतना विशिष्ट `content_type` चुनें:

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

## 5. sections के साथ व्यवस्थित करें

कंटेंट समूहों के लिए `sections` और व्यक्तिगत पेजों के लिए `entries` का उपयोग करें। आवश्यकता होने पर गहरी संरचना के लिए `subsections` जोड़ें।

उदाहरण:

- Section: Blog
- Subsection: Tutorials
- Entry: How to Use OLAMIP

## 6. प्राथमिकता (priority) सोच-समझकर सेट करें

उपयोग करें:

- `high` सबसे महत्वपूर्ण पृष्ठों के लिए।
- `medium` सामान्य पृष्ठों के लिए।
- `low` विशेष या पुराने पृष्ठों के लिए।

सभी को `high` मार्क न करें।

## 7. AI ingestion को policy से नियंत्रित करें

`policy` फ़ील्ड AI सिस्टम को बताता है कि वे किसी section, subsection या entry को ingest कर सकते हैं या नहीं।

- `"allow"` उपयोग करें ingestion की अनुमति देने के लिए।
- `"forbid"` उपयोग करें ताकि AI सिस्टम उस सामग्री से बचें।

यदि `policy` फ़ील्ड नहीं दिया गया है, तो OLAMIP निकटतम ancestor से policy inherit करता है। यदि कोई ancestor policy परिभाषित नहीं करता, तो डिफ़ॉल्ट `"allow"` होता है। अधिकांश साइटों के लिए, आप `policy` छोड़ सकते हैं और केवल `"forbid"` का उपयोग कर सकते हैं जहाँ ज़रूरी हो।

उदाहरण:

- पूरी section को छोड़ें:

```json
{
  "title": "Deprecated Guides",
  "summary": "पुरानी सामग्री, AI के लिए नहीं।",
  "url": "https://yourdomain.com/old-guides/",
  "section_type": "doc_category",
  "policy": "forbid"
}
```

- एक पेज को छोड़ें:

```json
{
  "title": "Internal Test Page",
  "summary": "केवल staging के लिए उपयोग किया गया पेज।",
  "url": "https://yourdomain.com/test/",
  "content_type": "page",
  "policy": "forbid"
}
```

## 8. भाषा metadata जोड़ें

BCP-47 भाषा कोड का उपयोग करें, जैसे:

- `en`
- `es`
- `fr`
- `de`
- `pt-BR`
- `zh-CN`

ज़रूरत के अनुसार फ़ाइल, section, या entry स्तर पर भाषा सेट करें।

## 9. फ़ाइल को नियमित रूप से अपडेट करें

जब भी आप महत्वपूर्ण पेज जोड़ते, बदलते या हटाते हैं, `olamip.json` अपडेट करें।

यदि आपकी साइट अक्सर बदलती है, तो उसी डायरेक्टरी में `olamip-delta.json` भी बनाए रखें ताकि AI सिस्टम पूर्ण रिफ्रेश के बीच अपडेटेड रहें।

## 10. प्रकाशित करने से पहले validate करें

लाइव जाने से पहले सुनिश्चित करें:

- JSON वैध है।
- URLs पूर्ण (absolute) हैं।
- आवश्यक फ़ील्ड मौजूद हैं।
- सार स्पष्ट और अनुशंसित लंबाई के भीतर हैं।
- Tags सामान्यीकृत और सुसंगत हैं।
- फ़ाइल रूट पाथ से सर्व हो रही है।

## न्यूनतम कार्यान्वयन चेकलिस्ट

- [ ] `olamip.json` बनाएँ।
- [ ] इसे साइट रूट पर होस्ट करें।
- [ ] HTML `<head>` में discovery tags जोड़ें।
- [ ] प्रमुख कंटेंट के लिए sections और entries शामिल करें।
- [ ] metadata और भाषा फ़ील्ड जोड़ें।
- [ ] सार को छोटा और सटीक रखें।
- [ ] साइट बदलने पर इसे अपडेट करें।
- [ ] incremental updates के लिए `olamip-delta.json` जोड़ें।

## सुझाया गया प्रारंभिक बिंदु

यदि आप निश्चित नहीं हैं कि कहाँ से शुरू करें, तो पहले केवल अपने शीर्ष पृष्ठों के लिए OLAMIP लागू करें:

- होमपेज।
- ब्लॉग इंडेक्स।
- सर्वश्रेष्ठ लेख।
- प्रमुख उत्पाद या सेवा पृष्ठ।
- महत्वपूर्ण दस्तावेज़ पृष्ठ।

फिर धीरे-धीरे विस्तार करें।
