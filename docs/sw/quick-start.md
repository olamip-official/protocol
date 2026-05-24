# Mwongozo wa Haraka wa OLAMIP

*Haki miliki © 2025 Ralph Gonzalez – https://olamip.org*

Mwongozo huu wa haraka unaonyesha jinsi ya kutekeleza OLAMIP kwenye tovuti kwa njia rahisi zaidi.

## 1. Tengeneza `olamip.json`

Weka faili halali ya `olamip.json` kwenye mzizi wa tovuti yako:

`https://yourdomain.com/olamip.json`

Anza na muhtasari mdogo na safi wa kurasa zako muhimu zaidi.

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

## 2. Ongeza tag za ugunduzi kwenye HTML yako

Ongeza hizi kwenye sehemu ya `<head>` ya ukurasa wako wa nyumbani na kurasa muhimu:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## 3. Andika muhtasari kwa ufupi

Andika muhtasari mfupi, wa ukweli, unaoeleza ukurasa unahusu nini na kwa nini ni muhimu. Epuka lugha ya kibiashara, maandishi yanayojirudia, na maelezo yasiyoeleweka.

Nzuri:

- “Mwongozo wa wanaoanza kuhusu upigaji picha wa long exposure.”
- “Maelezo ya bidhaa na bei kwa mpango wa enterprise.”

Epuka:

- “Ukurasa bora zaidi kwenye mtandao.”
- “Maudhui ya kushangaza utakayoyapenda.”

## 4. Tumia aina sahihi za maudhui

Chagua `content_type` iliyo sahihi zaidi:

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

## 5. Panga kwa kutumia sections

Tumia `sections` kwa makundi ya maudhui na `entries` kwa kurasa za mtu mmoja mmoja. Ikiwa inahitajika, ongeza `subsections` ili kuongeza kina.

Mfano:

- Section: Blog  
- Subsection: Tutorials  
- Entry: How to Use OLAMIP  

## 6. Weka kipaumbele kwa makusudi

Tumia:

- `high` kwa kurasa zako muhimu zaidi.  
- `medium` kwa kurasa za kawaida.  
- `low` kwa kurasa za niche au za zamani.  

Usiweke kila kitu kuwa `high`.

## 7. Tumia policy kudhibiti usomaji wa AI

Sehemu ya `policy` inaambia mifumo ya AI kama inaruhusiwa kusoma sehemu, sehemu ndogo, au ingizo.

- Tumia `"allow"` kuruhusu kusomwa.  
- Tumia `"forbid"` kuzuia sehemu au ukurasa fulani.  

Ukiacha `policy`, OLAMIP hurithi sera kutoka kwa mzazi. Ikiwa hakuna mzazi aliyeweka sera, thamani chaguo-msingi ni `"allow"`.

Kwa tovuti nyingi, unaweza kuacha `policy` isipokuwa pale unapotaka AI iruke maudhui fulani.

Mifano:

- Ruka sehemu nzima:

  ```json
  {
    "title": "Deprecated Guides",
    "summary": "Outdated content, not for AI.",
    "url": "https://yourdomain.com/old-guides/",
    "section_type": "doc_category",
    "policy": "forbid"
  }
  ```

- Ruka ukurasa mmoja:

  ```json
  {
    "title": "Internal Test Page",
    "summary": "Page used for staging only.",
    "url": "https://yourdomain.com/test/",
    "content_type": "page",
    "policy": "forbid"
  }
  ```

## 8. Ongeza metadata ya lugha

Tumia misimbo ya lugha ya BCP‑47 kama:

- `en`
- `es`
- `fr`
- `de`
- `pt-BR`
- `zh-CN`

Weka lugha kwenye kiwango cha faili, sehemu, au ingizo.

## 9. Sasisha faili mara kwa mara

Kila unapoongeza, kubadilisha, au kuondoa kurasa muhimu, sasisha `olamip.json`.

Ikiwa tovuti yako hubadilika mara nyingi, tumia pia `olamip-delta.json` kwenye folda hiyo hiyo ili AI ibaki na taarifa mpya kati ya masasisho makubwa.

## 10. Hakiki kabla ya kuchapisha

Kabla ya kuweka hadharani, hakikisha:

- JSON ni halali.  
- URLs ni kamili (absolute).  
- Sehemu muhimu zipo.  
- Muhtasari ni wazi na mfupi.  
- Tags zimenormalishwa na thabiti.  
- Faili inatolewa kutoka kwenye mzizi wa tovuti.  

## Orodha ya ukaguzi ya utekelezaji wa msingi

- [ ] Tengeneza `olamip.json`.  
- [ ] Iweke kwenye mzizi wa tovuti.  
- [ ] Ongeza discovery tags kwenye `<head>`.  
- [ ] Ongeza sections na entries kwa maudhui muhimu.  
- [ ] Ongeza metadata na lugha.  
- [ ] Andika muhtasari mfupi na sahihi.  
- [ ] Sasisha kila mara tovuti inapobadilika.  
- [ ] Ongeza `olamip-delta.json` ikiwa unataka masasisho ya hatua kwa hatua.  

## Mahali pazuri pa kuanzia

Ikiwa hujui pa kuanzia, tekeleza OLAMIP kwa kurasa zako kuu tu:

- Ukurasa wa nyumbani  
- Blog index  
- Makala bora  
- Kurasa muhimu za bidhaa au huduma  
- Kurasa muhimu za nyaraka  

Kisha panua hatua kwa hatua.
