# دليل البدء السريع لـ OLAMIP

*حقوق النشر © 2025 رالف غونزاليس – https://olamip.org*

يوضّح هذا الدليل السريع كيفية تنفيذ OLAMIP على موقعك الإلكتروني بأبسط طريقة ممكنة.

---

## 1. إنشاء ملف `olamip.json`

ضع ملفًا صالحًا باسم `olamip.json` في جذر موقعك:

`https://yourdomain.com/olamip.json`

ابدأ بملف صغير ونظيف يحتوي على أهم صفحاتك.

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
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

---

## 2. إضافة وسوم الاكتشاف إلى HTML

أضف الوسوم التالية داخل قسم `<head>` في صفحتك الرئيسية والصفحات المهمة:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

---

## 3. اجعل الملخصات قصيرة وواضحة

اكتب ملخصات قصيرة وواقعية تشرح بوضوح موضوع الصفحة ولماذا هي مهمة. تجنّب اللغة التسويقية، النصوص المكررة، والوصف الغامض.

أمثلة جيدة:

- "دليل للمبتدئين حول التصوير بالتعريض الطويل."
- "تفاصيل المنتج والتسعير لخطة المؤسسات."

أمثلة يجب تجنبها:

- "أفضل صفحة على الإنترنت."
- "محتوى مذهل ستحبه بالتأكيد."

---

## 4. استخدم أنواع المحتوى الصحيحة

اختر أدق `content_type` ممكن:

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

---

## 5. نظّم المحتوى باستخدام الأقسام

استخدم `sections` لتجميع المحتوى، و`entries` للصفحات الفردية. إذا لزم الأمر، أضف `subsections` لإنشاء بنية أعمق.

مثال:

- قسم: المدونة  
- قسم فرعي: الدروس  
- عنصر: كيفية استخدام OLAMIP  

---

## 6. حدّد الأولوية بشكل مقصود

استخدم:

- `high` للصفحات الأكثر أهمية  
- `medium` للصفحات العادية  
- `low` للصفحات القديمة أو المتخصصة  

لا تجعل كل شيء `high`.

---

## 7. استخدم policy للتحكم في المحتوى الذي يمكن للذكاء الاصطناعي قراءته

حقل `policy` يخبر أنظمة الذكاء الاصطناعي ما إذا كان مسموحًا لها بقراءة قسم أو قسم فرعي أو عنصر.

- `"allow"` → السماح بالقراءة  
- `"forbid"` → منع القراءة  

إذا لم تحدد `policy`، فسيتم وراثتها من العنصر الأعلى.  
إذا لم يحدد أي عنصر أعلى سياسة، فالقيمة الافتراضية هي `"allow"`.

أمثلة:

**تخطي قسم كامل:**

```json
{
  "title": "Deprecated Guides",
  "summary": "Outdated content, not for AI.",
  "url": "https://yourdomain.com/old-guides/",
  "section_type": "doc_category",
  "policy": "forbid"
}
```

**تخطي صفحة واحدة:**

```json
{
  "title": "Internal Test Page",
  "summary": "Page used for staging only.",
  "url": "https://yourdomain.com/test/",
  "content_type": "page",
  "policy": "forbid"
}
```

---

## 8. إضافة بيانات اللغة

استخدم رموز اللغة BCP‑47 مثل:

- `ar`
- `en`
- `fr`
- `de`
- `es`
- `pt-BR`
- `zh-CN`

يمكنك تحديد اللغة على مستوى الملف أو القسم أو العنصر.

---

## 9. تحديث الملف بانتظام

كلما أضفت أو عدّلت أو حذفت صفحات مهمة، قم بتحديث `olamip.json`.

إذا كان موقعك يتغير كثيرًا، يمكنك أيضًا إضافة `olamip-delta.json` في نفس المجلد ليبقى الذكاء الاصطناعي محدثًا بين عمليات التحديث الكاملة.

---

## 10. التحقق قبل النشر

تأكد من:

- أن JSON صالح  
- أن الروابط كاملة (absolute)  
- أن الحقول المطلوبة موجودة  
- أن الملخصات واضحة وقصيرة  
- أن الوسوم موحدة ومتناسقة  
- أن الملف يُقدّم من جذر الموقع  

---

## قائمة التحقق للتنفيذ الأساسي

- [ ] إنشاء `olamip.json`  
- [ ] وضعه في جذر الموقع  
- [ ] إضافة وسوم الاكتشاف في `<head>`  
- [ ] تضمين الأقسام والعناصر للمحتوى الأساسي  
- [ ] إضافة metadata واللغة  
- [ ] كتابة ملخصات قصيرة ودقيقة  
- [ ] تحديث الملف عند تغيّر الموقع  
- [ ] إضافة `olamip-delta.json` إذا رغبت في تحديثات تدريجية  

---

## نقطة بداية مقترحة

إذا لم تكن متأكدًا من أين تبدأ، فابدأ بتطبيق OLAMIP على أهم صفحاتك فقط:

- الصفحة الرئيسية  
- صفحة فهرس المدونة  
- أفضل المقالات  
- صفحات المنتجات أو الخدمات الأساسية  
- صفحات التوثيق المهمة  

ثم قم بالتوسّع تدريجيًا.
