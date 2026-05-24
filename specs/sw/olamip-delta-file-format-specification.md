# Maelezo ya Muundo wa Faili ya OLAMIP-DELTA

*Hati miliki © 2025 Ralph Gonzalez – https://olamip.org*

Itifaki ya **OLAMIP-DELTA** inaeleza muundo wa faili wa delta unaotegemea JSON kwa masasisho ya nyongeza ya `olamip.json`. Maelezo haya yanalenga wasimamizi wa tovuti na watumizi ambao tayari wanafahamu data iliyopangwa, HTML, na uchapishaji wa aina ya sitemap. 

## 1. Muhtasari

`olamip.json` ni uwakilishi rasmi na kamili wa muundo na maudhui ya tovuti yako. `olamip-delta.json` ni faili ya ziada ya hiari inayoeleza mabadiliko yaliyofanyika kwenye snapshot hiyo tangu sasisho la mwisho.

Kwa pamoja, huunda mkondo wa mabadiliko unaoweza kufasiriwa na mashine:

- `olamip.json`: snapshot kamili na iliyopangwa.
- `olamip-delta.json`: kumbukumbu ya mabadiliko ya nyongeza.

---

## 2. Dhana za Msingi

- **Entry** — kipengele cha maudhui katika kiwango cha jani, kama vile makala ya blogu, ukurasa wa bidhaa, au ukurasa wa nyaraka.
- **Section / Subsection** — mkusanyiko wa ngazi ya juu, kama vile kategoria ya blogu, kategoria ya nyaraka, mkusanyiko wa bidhaa, au kundi la mradi.
- **Operation** — mojawapo ya `added`, `updated`, `removed`.
- **Delta** — mkusanyiko wa mabadiliko unaohusishwa na tarehe na unaojumuisha `added`, `updated`, na `removed`.

Umbizo hili limekusudiwa kuwa:

- **Linaeleweka na mashine kwanza**, lakini bado lisomeke kwa binadamu ili likaguliwe kwa mikono.
- **La nyongeza**, ili mifumo ya AI ibaki imesawazishwa bila kuchakata upya tovuti nzima.

---

## 3. Mahali pa Faili na Ugunduzi

### 3.1 Mahali pa Faili

`olamip.json` lazima ipangwe kwenye mzizi wa domain yako:

```text
https://yourdomain.com/olamip.json
```

`olamip-delta.json` lazima ipangwe katika saraka hiyo hiyo:

```text
https://yourdomain.com/olamip-delta.json
```

Matoleo ya delta yenye tarehe (hiari) yapo katika saraka hiyo hiyo:

```text
/olamip.json
/olamip-delta-YYYY-MM-DD.json
```

### 3.2 Njia ya Ugunduzi

Wasimamizi wa tovuti lazima watangaze mahali pa `olamip.json` kwa kutumia vitambulisho viwili vifuatavyo ndani ya `<head>` ya tovuti:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Mifumo ya AI inayofahamu `olamip.json` itaangalia kiotomatiki `olamip-delta.json` katika saraka hiyo hiyo na itatumia deltas kama ilivyoelezwa katika maelezo haya. Hakuna haja ya kuongeza `<link>` au `<meta>` tofauti kwa `olamip-delta.json`. 

---

## 4. Muundo wa Juu

Katika kiwango cha juu, `olamip-delta.json` ni kitu cha JSON chenye sehemu zifuatazo:

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "window_days": 7,
  "last_updated": "2026-04-28",
  "deltas": [
    {
      "date": "2026-04-27",
      "added": [
        {
          "title": "Mfano wa entry",
          "url": "https://example.com/page/",
          "summary": "...",
          "content_type": "blog_article"
        }
      ],
      "updated": [
        {
          "url": "https://example.com/page/",
          "summary": "Muhtasari ulioboreshwa"
        }
      ],
      "removed": [
        {
          "url": "https://example.com/removed-page/"
        }
      ]
    }
  ]
}
```

### 4.1 Sehemu za Juu Zinazohitajika

- `protocol` (string, inahitajika)  
  Lazima iwe `"OLAMIP-DELTA"`.  
  Hii hutofautisha faili ya delta na faili kuu ya OLAMIP na fomati nyingine za delta.

- `version` (string, inahitajika)  
  Lazima ilingane na mpango wa toleo uliodokumentishwa, kwa mfano `"1.0.0"`.  
  Hii husaidia watumizi kugundua mabadiliko ya schema.

- `window_days` (integer, hiari)  
  Idadi ya siku katika dirisha linalozunguka; lazima iwe chanya, kawaida 7–30.  
  Inahitajika tu ikiwa unatumia faili ya delta ya dirisha linalozunguka.

- `last_updated` (string, inahitajika)  
  Tarehe ambayo faili hii ya delta ilirekebishwa mara ya mwisho, katika fomati ya ISO-8601 `YYYY-MM-DD`.  
  Hii huwasaidia crawlers kuamua lini wapakue tena.

- `deltas` (array, inahitajika)  
  Orodha isiyo tupu ya seti za mabadiliko zilizo na tarehe.  
  Kila kipengele ni **delta object** chenye `date`, `added`, `updated`, na `removed`.

### 4.2 Array ya `deltas` na Mpangilio wa Tarehe

- `deltas` lazima zipangwe kwa `"date"` kwa mpangilio wa kupanda (ya zamani kwanza, ya karibuni mwisho).
- Kila delta object lazima iwe na field `"date"` katika fomati ya ISO-8601 `YYYY-MM-DD`.
- `deltas` lazima isiwe tupu.

Mpangilio huu si wa hiari kwa ulinganifu: mifumo ya AI huchukulia kwamba deltas zimepangwa kwa mpangilio wa muda.

---

## 5. Muundo wa Delta Object

Kila kipengele ndani ya `deltas` ni delta object lenye umbo lifuatalo:

```json
{
  "date": "2026-04-27",
  "added": [ ... ],
  "updated": [ ... ],
  "removed": [ ... ]
}
```

Kwa uwazi unaoweza kufasiriwa na mashine na uthabiti, wasimamizi wa tovuti **LAZIMA** wajumuishe sehemu zote tatu za operesheni katika kila delta object, hata kama baadhi ni arrays tupu.

Sehemu:

- `date` (string) — tarehe ya ISO-8601 `YYYY-MM-DD`.
- `added` (array) — vipengele vipya sifuri au zaidi.
- `updated` (array) — vipengele vilivyobadilishwa sifuri au zaidi.
- `removed` (array) — vipengele vilivyofutwa sifuri au zaidi.

Mfano wa delta object yenye nyongeza pekee:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Ukurasa Mpya",
      "summary": "Ukurasa mpya umeongezwa kwenye tovuti.",
      "url": "https://example.com/new-page/"
    }
  ],
  "updated": [],
  "removed": []
}
```

Mpangilio huu unahitajika hata pale operesheni moja tu inapotumika kwa siku husika.

---

## 6. Operesheni

### 6.1 `added`

Kipengele cha `added` ni **kitu kamili** cha aina husika (entry, section, au subsection). Sehemu zake lazima zilingane na maelezo ya fomati ya OLAMIP kwa aina hiyo.

Kwa **entry**:

- `title` (string, inahitajika) — kichwa kinachosomeka kwa binadamu.
- `summary` (string, inahitajika) — maelezo mafupi.
- `url` (string, inahitajika) — URL kamili ya kanuni.
- `content_type` (string, inahitajika) — kutoka taksonomia ya `content_type` ya OLAMIP.
- `tags` (array, hiari) — orodha ya tags zilizo katika herufi ndogo na token moja.
- `priority` (string, hiari) — `"high"`, `"medium"`, au `"low"`.
- `policy` (string, hiari) — `"allow"` au `"forbid"`.
- `language` (string, hiari) — msimbo wa lugha wa BCP-47.
- `metadata` (object, hiari) — data iliyopangwa mahsusi kwa kikoa.

Kwa **section au subsection**, vitu vya `added` hufuata maelezo ya OLAMIP ya kiwango cha section, zikiwa na `title`, `summary`, `url`, `section_type`, `policy`, `tags`, `priority`, na kadhalika.

#### Kanuni za `added`

- Vipengele vya `added` lazima viwe na `url`.
- Vipengele vya `added` lazima viwe objects kamili, angalau vyenye sehemu zote zinazohitajika na maelezo ya OLAMIP kuu.
- Hakuna `url` linaloweza kuonekana zaidi ya mara moja ndani ya delta object moja.

Mfano:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Makala Mpya ya Blogu",
      "summary": "Utangulizi mfupi wa kipengele kipya.",
      "url": "https://example.com/blog/new-feature/",
      "content_type": "blog_article",
      "tags": ["feature", "new"],
      "priority": "medium"
    }
  ],
  "updated": [],
  "removed": []
}
```

### 6.2 `updated`

Kipengele cha `updated` lazima kiwe na `url` na kinaweza kuwa:

- **object ya sehemu** yenye sehemu zilizobadilika pekee, au
- **object kamili** yenye sehemu zote.

Kwa kila kipengele cha `updated`, sehemu ambazo hazipo huhesabiwa kuwa **hazijabadilika**.

#### Mfano wa sasisho la sehemu

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "url": "https://example.com/post/to-update/",
      "summary": "Muhtasari uliosasishwa wenye habari mpya.",
      "tags": ["feature", "update", "docs"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Mfano wa kubadilisha kabisa

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "title": "Kichwa cha Posti Kilichosasishwa",
      "summary": "Muhtasari uliosasishwa wenye habari mpya.",
      "url": "https://example.com/post/to-update/",
      "content_type": "blog_article",
      "tags": ["feature", "update", "docs"],
      "priority": "medium"
    }
  ],
  "added": [],
  "removed": []
}
```

#### Kanuni za `updated`

- Vipengele vya `updated` lazima viwe na `url`.
- `updated` inaweza kuwa object ya sehemu au object kamili.
- `updated` hairuhusiwi kubadilisha utambulisho wa entry kupitia kubadili `url`; hilo ni suala la usanidi, si la faili ya delta.

### 6.3 `removed`

Kipengele cha `removed` ni **cha chini kabisa** na kinahusu utambulisho pekee.

```json
{
  "url": "https://example.com/page/to-remove/"
}
```

#### Kanuni za `removed`

- Vipengele vya `removed` lazima viwe na `url` na **visitie pamoja na sehemu nyingine yoyote**.
- `url` lazima iwe ya moja kwa moja na ya kanuni.
- `removed` inatumika kwa URL iliyotolewa na vizazi vyake vya kimuundo (subsections na entries), ikiwa vipo.
- Hakuna `url` linaloweza kuonekana zaidi ya mara moja ndani ya delta object moja.

Mfano:

```json
{
  "date": "2026-04-27",
  "removed": [
    {
      "url": "https://example.com/old-page/"
    }
  ],
  "added": [],
  "updated": []
}
```

---

## 7. URL na Utambulisho wa Kitu

OLAMIP-DELTA hutumia canonical URLs kama utambulisho mkuu wa entries, sections, na subsections.

### Kanuni za utambulisho

- `url` lazima iwe URL kamili, ikiwa na scheme na domain.
- Hakuna `url` linaloweza kuonekana zaidi ya mara moja ndani ya delta object moja, iwe katika kuongeza, kusasisha, au kuondoa.
- Section ikiondolewa, vizazi vyake vyote huchukuliwa kuwa vimeondolewa.
- Mabadiliko ya URL yanahitaji **remove** ikifuatiwa na **add**, si kusasisha moja kwa moja `url`.

### Mbinu bora kwa URLs

- Tumia canonical URLs thabiti ambazo hubadilika mara chache.
- Epuka query parameters au vipande vinavyotegemea session.
- Ikiwa ukurasa umehamishwa, shughulikia kama suala la redirects na canonicalization, si kama sasisho la moja kwa moja ndani ya delta file.

---

## 8. Dirisha la Delta Linalozunguka (Linalopendekezwa)

**Dirisha la delta linalozunguka** ndilo hali inayopendekezwa kwa `olamip-delta.json`.

Katika mtindo huu:

- Faili ya delta ina mabadiliko ya siku za mwisho `window_days`.
- Mifumo ya AI husoma deltas zote ndani ya faili na kuzitekeleza kwa mpangilio wa muda.
- Hii huzuia kupotea kwa sasisho pale crawlers zinapochelewa kuchukua data.

### Mfano wa Delta Linalozunguka (dirisha la siku 7)

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "window_days": 7,
  "last_updated": "2026-04-28",
  "deltas": [
    {
      "date": "2026-04-22",
      "added": [
        {
          "title": "Bidhaa Mpya A",
          "url": "https://example.com/product/a",
          "summary": "Ongezeko jipya kwenye orodha."
        }
      ],
      "updated": [],
      "removed": []
    },
    {
      "date": "2026-04-23",
      "added": [],
      "updated": [
        {
          "url": "https://example.com/product/b",
          "summary": "Maelezo yameboreshwa ili kuonyesha vipengele vipya."
        }
      ],
      "removed": []
    },
    {
      "date": "2026-04-24",
      "added": [],
      "updated": [],
      "removed": [
        {
          "url": "https://example.com/product/c"
        }
      ]
    }
  ]
}
```

Mifumo ya AI:

- husoma deltas zote kwenye faili,
- huzitekeleza kwa mpangilio wa muda,
- na hufikia hali ya mwisho kabisa.

---

## 9. Delta Zenye Tarehe Tofauti (Chaguo Mbadala)

Baadhi ya wasimamizi wa tovuti hupendelea **deltas zenye matoleo**, ambapo kila faili ina mabadiliko ya siku moja.

### Mfano wa Mpangilio

```text
/olamip.json
/olamip-delta-2026-03-01.json
/olamip-delta-2026-03-02.json
/olamip-delta-2026-03-03.json
```

### Umbo la Faili

Kila faili yenye toleo ina muundo wa kimantiki sawa na faili ya linalozunguka, lakini inawakilisha tarehe moja tu ya mabadiliko:

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "date": "2026-03-02",
  "added": [
    {
      "title": "Makala Mpya ya Blogu",
      "url": "https://example.com/blog/new-post/",
      "summary": "Muhtasari mfupi wa sasisho."
    }
  ],
  "updated": [],
  "removed": []
}
```

### Kanuni za Uingizaji

Mifumo ya AI inapaswa:

- kugundua faili zote `olamip-delta-*.json` katika saraka ile ile ya `olamip.json`,
- kuzipanga kwa `date` kwa mpangilio wa kupanda,
- na kutumia zile tu zilizo ndani ya dirisha lililosanidiwa, kwa mfano siku 7–30 zilizopita.

Njia hii inafaa sana kwa:

- tovuti za habari zenye masasisho ya kila siku,
- katalogi za e-commerce zenye idadi kubwa,
- majukwaa ya nyaraka yenye mizunguko ya utoaji haraka.

---

## 10. Mabadiliko ya Kimuundo: Sections na Subsections

OLAMIP-DELTA hutumia muundo ule ule wa delta kwa **sections** na **subsections** kama inavyofanya kwa **entries**. Hii huwezesha mifumo ya AI kuelewa si tu kurasa zipi zipo, bali pia jinsi zilivyopangwa.

### 10.1 Kuongeza Section au Subsection

Unapoongeza section au subsection, jumuisha **kitu kamili cha section** ndani ya `added`.

Mfano:

```json
{
  "date": "2026-05-01",
  "added": [
    {
      "title": "Mafunzo",
      "summary": "Miongozo ya hatua kwa hatua kwa wanaoanza na watumiaji wa hali ya juu.",
      "url": "https://example.com/tutorials/",
      "section_type": "blog_category",
      "policy": "allow",
      "tags": ["tutorials", "beginner", "advanced"],
      "priority": "medium"
    }
  ],
  "updated": [],
  "removed": []
}
```

#### Kanuni

- Vitu vya `added` vya section lazima viwe na `title`, `summary`, `url`, na `section_type`.
- Tumia `section_type` kuonyesha jukumu la kisemantiki, kama `blog_category`, `doc_category`, `product_collection`, au `research_category`.
- `priority` na `policy` husaidia mifumo ya AI kuelewa umuhimu na kanuni za uingizaji kwa mti mzima wa ndani.

### 10.2 Kusasisha Section au Subsection

Jumuisha `url` pamoja na sehemu yoyote iliyobadilika.

Mfano:

```json
{
  "date": "2026-05-02",
  "updated": [
    {
      "url": "https://example.com/blog/tutorials/",
      "title": "Mafunzo ya Hatua kwa Hatua",
      "summary": "Miongozo kwa wanaoanza na watumiaji wa hali ya juu, pamoja na mifano ya msimbo na mbinu bora.",
      "tags": ["tutorials", "beginner", "advanced", "code-examples"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Kanuni

- Sections zilizosasishwa lazima ziwe na `url`.
- `updated` inaweza kujumuisha sehemu yoyote ya fields nyingine.
- Fields ambazo hazipo huhesabiwa kuwa hazijabadilika.

### 10.3 Kuondoa Section au Subsection

Jumuisha tu `url` ndani ya `removed`.

Mfano:

```json
{
  "date": "2026-05-03",
  "removed": [
    {
      "url": "https://example.com/blog/retired/"
    }
  ],
  "added": [],
  "updated": []
}
```

#### Kanuni

- Vipengele vya `removed` vya section lazima viwe na `url` pekee na visijumuishe fields nyingine.
- URL lazima iwe canonical URL ya section au subsection iliyoondolewa.
- Section ikiondolewa, vizazi vyake vyote pia huhesabiwa kuwa vimeondolewa.

---

## 11. Ulinganifu

Watengenezaji na wasimamizi wa tovuti wanapaswa kuchukulia yafuatayo kama kanuni za ulinganifu.

### 11.1 Kanuni za MUST

Faili ya OLAMIP-DELTA inayolingana **LAZIMA**:

- itangaze `protocol: "OLAMIP-DELTA"`,
- iweke `version` kwa thamani inayolingana na schema ya toleo iliyodokumentishwa,
- iwe na array `deltas` isiyo tupu,
- kila delta object iwe na `date` katika fomati ya ISO-8601 `YYYY-MM-DD`,
- zipange `deltas` kwa kupanda kulingana na `date`,
- zijumuishe sehemu zote tatu za operesheni (`added`, `updated`, `removed`) katika kila delta object, hata kama ni tupu,
- zihakikishe kwamba `url` katika operesheni zote ni URL kamili na canonical,
- zihakikishe hakuna `url` linalojirudia zaidi ya mara moja ndani ya delta object moja,
- zihakikishe kwamba `added` na `updated` zina `url`,
- zihakikishe kwamba `removed` ina `url` pekee na hakuna fields nyingine,
- zitendee fields ambazo hazipo katika `updated` kama hazijabadilika,
- zitendee `removed` kama kuondoa URL husika na vizazi vyake kutoka kwenye index.

### 11.2 Kanuni za SHOULD

Utekelezaji unaolingana **UNAPASWA**:

- kupitisha kwa chaguo-msingi **dirisha la delta linalozunguka**, isipokuwa mtiririko wa uchapishaji unafaa zaidi kwa deltas zenye matoleo,
- kuweka `last_updated` likiwa sahihi ili crawlers na validators wajue wakati faili ni jipya,
- kutumia canonical URLs thabiti zisizobadilika mara kwa mara ili kuzuia utata wakati wa kutumia deltas,
- kuweka `window_days` kwa kawaida kati ya siku 7 na 30 kulingana na kasi ya uchapishaji ya tovuti,
- kwa deltas zenye matoleo, kuhifadhi idadi ya kutosha ya faili ndani ya dirisha lililosanidiwa, kwa mfano siku 7–30 zilizopita, ili kuepuka kupotea kwa sasisho ikiwa crawler itachelewa.

### 11.3 Kanuni za MAY

Utekelezaji unaolingana **UNAWEZA**:

- kutumia fomati ya delta yenye tarehe (`olamip-delta-YYYY-MM-DD.json`) badala ya faili moja linalozunguka ikiwa inafaa zaidi kwa pipeline ya deployment,
- kuchapisha `olamip-delta.json` kwa kasi tofauti na `olamip.json`, mradi uhusiano kati ya snapshot na delta ubaki wazi na thabiti,
- kuongeza metadata ya uchunguzi au logging chini ya ufunguo tofauti nje ya fields za protocol, kwa mfano object `diagnostics`, mradi isivuruge fields zinazohitajika au kubadili maana ya delta.

---

## 12. Mbinu Bora

Ili kuhakikisha uoanifu thabiti na wa muda mrefu na mifumo ya AI, wasimamizi wa tovuti wanapaswa kufuata mbinu bora zifuatazo wakati wa kutengeneza `olamip-delta.json`.

### 12.1 Oatomatisha uundaji wa delta

- Unganisha uundaji wa delta na CMS yako au pipeline ya deployment.
- Kila mara maudhui yanapochapishwa, kusasishwa, au kuondolewa, sasisha `olamip-delta.json` ipasavyo.
- Hakiki schema ya faili kabla ya deployment ili kuepuka makosa ya sintaksia au fields zisizo halali.

### 12.2 Fanya `olamip.json` ibaki ya mamlaka

- Dumisha `olamip.json` ikiwa halali na ya kisasa.
- Mifumo mipya ya AI inaweza kuanza kutoka faili kamili kisha kutumia deltas zinazofuata.
- Usichukulie `olamip-delta.json` kama mbadala wa snapshot kuu.

### 12.3 Tumia canonical URLs thabiti

- Tumia URLs kamili zenye scheme na domain.
- Epuka mifumo ya URL inayobadilika mara kwa mara isipokuwa mfumo wa tovuti, kama vile redirects, ni thabiti vya kutosha kudumisha maana ya canonical.
- Ikiwa URL ya ukurasa imebadilika kweli, ichukulie kama “remove-then-add”, si sasisho la moja kwa moja la `url` ndani ya delta.

### 12.4 Dumisha uwiano wa summary na metadata

- Summaries katika vipengele vya `added` na `updated` lazima ziwe na ulinganifu na maudhui halisi.
- Marekebisho madogo ya maneno ambayo hayabadili maana yanaweza kuwasilishwa kama delta, lakini yapunguzwe ili kuepuka churn isiyo ya lazima.
- Tags lazima ziwe sanifu, kwa herufi ndogo, neno moja, na ziwe na viungo vya hyphen kwa maneno yenye sehemu zaidi ya moja; lazima zionyeshe dhana thabiti.

---

## 13. Rasilimali Zinazohusiana

Kwa uelewa kamili wa mfumo wa OLAMIP, wasimamizi wa tovuti pia wanapaswa kusoma:

- **[Maelezo ya Muundo wa Faili ya OLAMIP](https://olamip.org/file-format-specification/)**  
  Inaeleza muundo wa `olamip.json` na inafafanua fields za entries, sections, na subsections zinazounda msingi wa fomati ya delta.

- **[Maswali Yanayoulizwa Mara kwa Mara ya OLAMIP](https://olamip.org/frequently-asked-questions/)**  
  Inaeleza masuala ya deployment, discovery, na uendeshaji kwa wasimamizi wa tovuti.

- **[Maelezo ya Muundo wa Faili ya OLAMIP-DELTA](https://olamip.org/delta-updates/)**  
  Schema rasmi ya JSON ya `olamip-delta.json`, inayoweza kutumika kuthibitisha na kufanya lint ya faili za delta kiotomatiki.

- **[Kwa Nini OLAMIP ni Kiwango Bora kuliko LLMs.txt](https://olamip.org/why-olamip-is-a-superior-standard-to-llms-txt/)**  
  Inaeleza faida za OLAMIP na OLAMIP-DELTA dhidi ya fomati rahisi na zisizo na muundo wa metadata.

---

Hii inakamilisha maelezo ya muundo wa faili ya OLAMIP-DELTA. Wasimamizi wa tovuti na watumizi wanaweza kutumia hati hii kama rejea ya kutengeneza, kuthibitisha, na kutumia `olamip-delta.json` katika mazingira ya uzalishaji.