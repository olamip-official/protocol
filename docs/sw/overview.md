# Muhtasari wa OLAMIP

*Haki miliki © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP ni kiwango huria kinachowezesha tovuti kuwasiliana kwa uwazi na kwa makusudi na mifumo ya akili bandia. Badala ya kulazimisha mifumo mikubwa ya lugha (LLMs) kubashiri maana kutoka kwa HTML, CSS, na JavaScript zenye kelele, OLAMIP hutoa muwakilishi safi, uliopangwa, na unaoweza kufasiriwa na mashine wa maudhui muhimu zaidi ya tovuti yako.

Hati hii inatoa utangulizi wa juu kuhusu madhumuni ya OLAMIP, falsafa yake, muundo wa faili, na mtindo wa masasisho.

---

## 1. Kwa nini OLAMIP iliundwa

Tovuti za kisasa zimeundwa kwa ajili ya vivinjari, si kwa ajili ya AI. Kwa sababu hiyo, LLMs hupata ugumu na:

- HTML yenye mpangilio mzito  
- Maudhui yanayotolewa na JavaScript  
- Menyu za urambazaji, matangazo, na vipengele vilivyojirudia  
- Kurasa za nakala au zisizo muhimu  

LLMs *tayari* hutembelea tovuti yako, lakini mara nyingi huzikosea. OLAMIP hutatua hili kwa kutoa kile ambacho mifumo ya AI inahitaji:

- Faili nyepesi ya JSON (`/olamip.json`)  
- Muhtasari safi ulioratibiwa na binadamu  
- Metadata iliyopangwa  
- URL za kikanoni  
- Muundo wazi wa sehemu, sehemu ndogo, na ingizo  

Kwa kutumia OLAMIP, unachukua udhibiti wa jinsi AI inavyoelewa na kuwakilisha maudhui yako.

---

## 2. Falsafa Kuu: Rafiki kwa Binadamu + Rafiki kwa Mashine

OLAMIP imeundwa kama daraja la mawasiliano kati ya binadamu na AI. Kila sehemu ya itifaki lazima iwe:

| Kanuni | Rafiki kwa Binadamu | Rafiki kwa Mashine |
|--------|----------------------|----------------------|
| Muundo wa Faili | Rahisi kuandika na kuthibitisha | JSON thabiti na linalotabirika |
| Majina ya Sehemu | Ya moja kwa moja na ya kueleweka | Herufi ndogo, inayofuata schema |
| Muhtasari | Lugha wazi, inayoendana na chapa | Fupi, ya ukweli, tajiri kisemantiki |
| Mfumo wa Kipaumbele | Lebo rahisi (`"high"`, `"medium"`, `"low"`) | Inaweza kubadilishwa kuwa uzani wa upangaji |
| Nyaraka | Mwongozo wa lugha rahisi | Schema za JSON, sheria za uthibitishaji |
| Zana | Vizalishaji, programu-jalizi za CMS | Zana za CLI, parser, test suite |

- **Kwa binadamu:** OLAMIP lazima iwe rahisi kutumia bila utaalamu wa kina wa kiufundi.  
- **Kwa mashine:** OLAMIP lazima iwe thabiti, ya kutabirika, na isiyo na utata.

---

## 3. Faili ya OLAMIP (`/olamip.json`)

Faili ya OLAMIP ni hati ya JSON iliyopangwa na kuhifadhiwa katika:

`https://yourdomain.com/olamip.json`

Ina:

- `protocol` — lazima iwe `"OLAMIP"`  
- `version` — toleo la itifaki  
- `identity` — utambulisho wa tovuti  
- `content` — muundo wa maudhui  
- `metadata` — lugha, tarehe ya masasisho, na mengine  

### 3.1 Tag za Ugunduzi

Ili kuhakikisha AI inaweza kupata faili yako ya OLAMIP kwa uhakika, ongeza:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Kutumia zote mbili huongeza uthabiti na utangamano.

---

## 4. Muundo wa Faili

### 4.1 Kitu cha Identity

Hufafanua tovuti au shirika:

- `name` — lazima  
- `type` — lazima  
- `canonical_description` — lazima  
- `tags` — hiari  

### 4.2 Kitu cha Content

Hujumuisha:

- `overview`  
- `sections`  
- `subsections` (hiari)  
- `entries` (vipengele vya maudhui vya mwisho)  

Muundo unaweza kuwa na kina kisicho na kikomo.

### 4.3 Sections

Sehemu hukusanya maudhui yanayohusiana. Inahitaji:

- `title`  
- `summary`  
- `url`  
- `section_type`  

Hiari:

- `policy` (`"allow"` au `"forbid"`)  
- `tags`  
- `priority`  
- `published`  
- `language`  
- `subsections`  
- `entries`  

**Urithi wa sera:**  
Ikiwa haijawekwa, inarithi kutoka kwa mzazi. Chaguo-msingi ni `"allow"`.

### 4.4 Entries

Ni vipengele vya maudhui kama:

- Makala za blogu  
- Habari  
- Bidhaa  
- Kurasa za nyaraka  
- Makala za utafiti  
- Faili za media  

Inahitaji:

- `title`  
- `summary`  
- `url`  
- `content_type`  

Hiari:

- `policy`, `tags`, `priority`, `published`, `language`, `metadata`  

URL ni lazima kwa sababu hutambulisha maudhui kipekee na huruhusu AI kuthibitisha na kuondoa marudio.

---

## 5. Metadata na Usaidizi wa Lugha

Mfano wa metadata:

```json
{
  "last_updated": "2026-01-21",
  "language": "sw",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### Usaidizi wa Lugha Nyingi

Lugha inaweza kufafanuliwa katika:

- kiwango cha faili  
- kiwango cha sehemu  
- kiwango cha ingizo  

Tumia misimbo ya BCP‑47 kama `sw`, `en`, `es`, `pt-BR`.

Hii husaidia AI:

- kuchagua tokenizer sahihi  
- kuepuka kuchanganya lugha  
- kuboresha usahihi wa utafutaji  
- kupunguza “hallucinations”  

---

## 6. Tags na Kipaumbele

### 6.1 Tags

Tags hutoa vidokezo vya kisemantiki. Lazima ziwe:

- herufi ndogo  
- neno moja  
- ASCII  
- zenye viunganishi kwa maneno mengi  
- thabiti  

Mfano:

| Dhana | Tag Sahihi |
|--------|------------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

### 6.2 Kipaumbele

Viwango:

- `high` — maudhui muhimu  
- `medium` — chaguo-msingi  
- `low` — maudhui ya niche au ya zamani  

Tumia `high` kwa kiasi (5–10%).

---

## 7. Masasisho ya Delta (`olamip-delta.json`)

Tovuti hubadilika kila mara. OLAMIP inaunga mkono masasisho ya hatua kwa hatua kupitia faili ya hiari:

`/olamip-delta.json`

Ina:

- ingizo jipya  
- ingizo lililosasishwa  
- URL zilizofutwa  

### 7.1 Kwa nini Delta ni Muhimu

- AI inabaki na taarifa mpya bila kuchakata faili nzima.  
- Katalogi kubwa zinadhibitika.  
- Masasisho huenea haraka.  
- Hakuna mabadiliko yanayopotea.  

### 7.2 Dirisha la Delta Linalosogea (Pendekezwa)

Hifadhi masasisho ya siku 7–30 katika faili moja.  
AI hutumia kwa mpangilio wa muda.

### 7.3 Faili za Delta Zenye Toleo (Mbadala)

Mfano:

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`

Inafaa kwa tovuti zenye mabadiliko mengi (habari, e‑commerce, nyaraka).

### 7.4 Ugunduzi

Faili kuu pekee huhitaji `<link>` na `<meta>`.  
AI hutafuta faili za delta katika folda hiyo hiyo.

---

## 8. OLAMIP dhidi ya Sitemaps za Kawaida

- Sitemap ya XML: “Hizi ni kurasa zangu.”  
- OLAMIP: “Hivi ndivyo tovuti yangu inamaanisha.”  

Sitemaps huorodhesha URL.  
OLAMIP hutoa:

- muhtasari  
- aina za maudhui  
- muundo wa sehemu  
- tags  
- metadata  
- kipaumbele  
- sera za usomaji  

Pamoja na schema.org, OLAMIP hutoa uelewa kamili wa tovuti kwa AI.

---

## 9. Maswali ya Mara kwa Mara (Muhtasari)

### Je, OLAMIP hufichua taarifa binafsi?

Hapana. Hutoa muhtasari wa maudhui ya umma tu.

### Je, washindani wanaweza kutumia faili yangu?

Sio zaidi ya kutumia kurasa zako za umma.

### Faili ya OLAMIP inaweza kuwa kubwa kiasi gani?

1–10 MB ni kawaida.  
Hata zaidi ya ingizo 3,000 ni salama.

### Ni mara ngapi inapaswa kusasishwa?

Kila mara maudhui yanapobadilika — ichukulie kama sitemap ya AI.

### Je, faili kuu inahitaji kusasishwa ikiwa natumia delta?

Ndiyo. `olamip.json` ndiyo rejea kuu.

### Je, LLMs zitasoma faili hii kweli?

Ndiyo — huo ndio madhumuni ya itifaki.  
Unaweza kujaribu kwa:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```

---

## 10. Muhtasari

OLAMIP hubadilisha tovuti yako kuwa ramani ya maarifa inayoweza kufasiriwa na mashine:

- `olamip.json` — picha kamili ya muundo  
- `olamip-delta.json` — rekodi ya mabadiliko ya hatua kwa hatua  
- sections, subsections, entries — muundo wazi  
- muhtasari, tags, kipaumbele — maana iliyopangwa na binadamu  
- discovery tags — usomaji wa uhakika  
- msaada wa lugha nyingi — utayari wa kimataifa  

Kwa kutumia OLAMIP, unahakikisha AI inaelewa maudhui yako jinsi ulivyokusudia — kwa usahihi, uthabiti, na kwa kiwango kikubwa.
