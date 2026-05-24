<p align="center">
  <img src="assets/banner.jpg" alt="لافتة OLAMIP" width="100%">
</p>

<p align="center">
  <img src="assets/logo.png" alt="شعار OLAMIP" width="140">
</p>

<h1 align="center">بروتوكول OLAMIP</h1>

<p align="center">
  <strong>Open Language‑Aligned Machine‑Interpretable Protocol</strong><br>
  معيار متعدد اللغات، منظم، وقابل للقراءة آليًا لمواقع الويب القابلة للفهم بواسطة الذكاء الاصطناعي.
</p>

<p align="center">
  <a href="https://olamip.org/">الموقع</a> •
  <a href="https://olamip.org/file-format-specification/">مواصفة تنسيق الملف</a> •
  <a href="https://olamip.org/delta-updates/">OLAMIP‑DELTA</a> •
  <a href="https://olamip.org/frequently-asked-questions/">الأسئلة الشائعة</a>
</p>

---

# 📘 نظرة عامة

OLAMIP هو بروتوكول مفتوح ومتعدد اللغات يتيح لمواقع الويب وصف بنيتها ومعناها وهدفها بصيغة **قابلة للفهم الآلي**. ويوفر تمثيل JSON نظيفًا يمكن لنماذج اللغة الكبيرة (LLMs) تحليله بشكل موثوق، دون الحاجة إلى تفسير HTML أو CSS أو JavaScript. [web:50][web:58]

يحتوي هذا المستودع على:

- **المواصفة الرسمية لـ OLAMIP**.
- **بروتوكول التحديثات التدريجية OLAMIP‑DELTA**.
- **الوثائق متعددة اللغات ومواد الحوكمة**.
- **أمثلة وقوالب**.

---

# 🌐 فهرس اللغات

تتوفر وثائق OLAMIP بعدة لغات. يحتوي كل قسم (Specs, Docs, Governance) على مجلدات موازية:

```text
/specs/<lang>
/docs/<lang>
/governance/<lang>
```

### اللغات المدعومة

| اللغة | الرمز | Specs | Docs | Governance |
|---|---|---|---|---|
| الإنجليزية | en | [/specs/en](specs/en/) | [/docs/en](docs/en/) | [/governance/en](governance/en/) |
| الإسبانية | es | [/specs/es](specs/es/) | [/docs/es](docs/es/) | [/governance/es](governance/es/) |
| الروسية | ru | [/specs/ru](specs/ru/) | [/docs/ru](docs/ru/) | [/governance/ru](governance/ru/) |
| اليابانية | ja | [/specs/ja](specs/ja/) | [/docs/ja](docs/ja/) | [/governance/ja](governance/ja/) |
| الصينية (المبسطة) | zh‑CN | [/specs/zh-CN](specs/zh-CN/) | [/docs/zh-CN](docs/zh-CN/) | [/governance/zh-CN](governance/zh-CN/) |
| البرتغالية | pt | [/specs/pt](specs/pt/) | [/docs/pt](docs/pt/) | [/governance/pt](governance/pt/) |
| الهندية | hi | [/specs/hi](specs/hi/) | [/docs/hi](docs/hi/) | [/governance/hi](governance/hi/) |
| البنغالية | bn | [/specs/bn](specs/bn/) | [/docs/bn](docs/bn/) | [/governance/bn](governance/bn/) |

> تُضاف لغات أخرى بشكل منتظم.

---

# 📑 هيكل المستودع

```text
/README.md              → الصفحة الرئيسية للمستودع
/CHANGELOG.md           → سجل التغييرات العام
/LICENSE                → الترخيص
/assets/                → الشعارات، اللافتات، الرسوم

/specs/                 → المواصفات الرسمية لـ OLAMIP وOLAMIP‑DELTA
    /en/ /es/ /ru/ /ja/ /zh-CN/ /pt/ /hi/ /bn/

/docs/                  → الوثائق، الأدلة، الشروحات
    /en/ /es/ /ru/ /ja/ /zh-CN/ /pt/ /hi/ /bn/

/governance/            → الحوكمة، الإصدارات، العمليات
    /en/ /es/ /ru/ /ja/ /zh-CN/ /pt/ /hi/ /bn/

/examples/              → أمثلة على ملفات OLAMIP والقوالب
```

---

# 📜 المواصفات

### **المواصفة الأساسية**
- الإنجليزية: [/specs/en](specs/en/).
- نسخة الموقع: [https://olamip.org/file-format-specification/](https://olamip.org/file-format-specification/).

### **OLAMIP‑DELTA**
- الإنجليزية: [/specs/en](specs/en/).
- نسخة الموقع: [https://olamip.org/delta-updates/](https://olamip.org/delta-updates/).

---

# 🧭 الحوكمة

الحوكمة، الإصدارات، وتطور البروتوكول:

- الإنجليزية: [/governance/en](governance/en/).
- الهندية: [/governance/hi](governance/hi/).
- البنغالية: [/governance/bn](governance/bn/).
- جميع اللغات: [/governance](governance/).

---

# 🧪 أمثلة

أمثلة على ملفات وقوالب OLAMIP:

- [/examples](examples/).

تشمل:

- مثال `olamip.json`.
- مثال `olamip-delta.json`.
- قوالب section/entry.
- أنماط أفضل الممارسات.

---

# 📄 الترخيص

يُنشر هذا المشروع بموجب ترخيص مفتوح. راجع ملف `/LICENSE` ومجلد `/governance` للحصول على التفاصيل.

---

<p align="center">
  <strong>OLAMIP — جعل الويب قابلًا للقراءة آليًا.</strong><br>
  <a href="https://olamip.org/">https://olamip.org/</a>
</p>