# Maswali Yanayoulizwa Mara kwa Mara (FAQ)

*Haki miliki © 2025 Ralph Gonzalez – https://olamip.org*

## OLAMIP ni nini?

OLAMIP ni kifupi cha **Open Language‑Aligned Machine‑Interpretable Protocol**.  
Ni umbizo rahisi na wazi linalowezesha tovuti kuchapisha muhtasari uliopangwa wa maudhui yao ili mifumo mikubwa ya lugha (LLMs) iweze kuyaelewa na kujifunza kutoka kwayo kwa ufanisi zaidi.

## Kwa nini nitumie OLAMIP?

Kwa sababu LLMs tayari zinatembelea tovuti yako, lakini zinapata ugumu kuifasiri. Kwa kutumia OLAMIP, unaweza:

- Kufundisha AI kwa usahihi kuhusu yaliyomo kwenye kurasa zako.  
- Kuonyesha maudhui yako yenye thamani zaidi.  
- Kuboresha upatikanaji katika zana na utafutaji unaoendeshwa na AI.  
- Kupunguza tafsiri potofu na “hallucinations”.

Ni tofauti kati ya AI inayokisia na AI inayofahamu.

## Je, OLAMIP hufichua taarifa za faragha?

Hapana. OLAMIP hutoa muhtasari tu wa maudhui ambayo tayari ni ya umma kwenye tovuti yako.  
Haufichui chochote ambacho hakionekani kwenye HTML yako, metadata, au alama zako za SEO.

## Je, washindani wanaweza kutumia faili yangu ya OLAMIP?

Sio zaidi ya jinsi wanavyoweza kutumia kurasa zako za umma, metadata, au sitemap.  
Kwa kweli, OLAMIP hukupa **udhibiti zaidi** juu ya jinsi mifumo ya AI inavyotafsiri maudhui yako — si kidogo.

## Faili ya `olamip.json` inaonekanaje?

`olamip.json` ni hati ya JSON iliyopangwa inayofafanua maudhui ya tovuti yako, muundo wake, na metadata kwa njia ambayo mifumo ya AI inaweza kuielewa kwa urahisi.  
Kwa maelezo ya kiufundi kamili, tembelea [Maelezo ya Umbizo la Faili](https://olamip.org/file-format-specification) au uangalie mfano halisi kupitia [uchambuzi wa TimeLAX.com](https://olamip.org/how-olamip-transforms-ai-discovery-a-timelax-com-use-case/).

## Ninafaa kuihifadhi wapi?

Katika mzizi wa tovuti yako:

```
https://yourdomain.com/olamip.json
```

Unaweza pia kuongeza alama za utambuzi kwenye ukurasa wako wa nyumbani:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## Sehemu ya “priority” hufanya nini?

Inaonyesha umuhimu wa ukurasa kwa LLMs. Tumia kwa uangalifu:

- `high` → maudhui muhimu na ya msingi.  
- `medium` → chaguo-msingi kwa kurasa nyingi.  
- `low` → maudhui ya kipekee, ya zamani, au yenye thamani ndogo.

Ukikiweka kila kitu kuwa `high`, basi hakuna kitakachokuwa cha juu kweli.

## Kwa nini ni lazima nijumuishe URL ikiwa muhtasari tayari upo?

Muhtasari unaeleza *maana* ya ukurasa, lakini URL inaeleza *ukurasa wenyewe*.  
Muhtasari ni maelezo tu, lakini si kitambulisho cha kipekee.  
URL ndiyo rejea pekee ya kudumu na ya kikanoni ya ukurasa halisi.

Mifumo ya AI hutumia URL kwa:

- Kupata na kuthibitisha maudhui kamili.  
- Kuepuka kurudia maudhui.  
- Kugundua kurasa zinapohama au kubadilika.  
- Kuunganisha data ya OLAMIP na schema.org, sitemaps, na crawlers.  
- Kuitaja ukurasa inapotoa majibu.

Bila URL, kurasa mbili tofauti zenye muhtasari unaofanana zinaweza kuonekana sawa.

## Sehemu ya “policy” hufanya nini, na je, ni lazima niijaze?

Sehemu ya `policy` inadhibiti kama mifumo ya AI inaruhusiwa kusoma sehemu, sehemu ndogo, au ingizo.  
Thamani zinazokubalika ni `"allow"` na `"forbid"`.

Ukiacha sehemu hii, OLAMIP hurithi sera kutoka kwa mzazi wa karibu.  
Ikiwa hakuna mzazi aliyeweka sera, thamani chaguo-msingi ni `"allow"`.

Tovuti nyingi zinaweza kuacha sehemu hii wazi.  
Tumia `"forbid"` tu ikiwa unataka kuzuia kurasa au sehemu fulani zisichakatwe na AI.

## Tofauti kati ya OLAMIP na sitemap ya kawaida ni ipi?

Wakati sitemap ni orodha ya urambazaji, OLAMIP ni **ramani ya maarifa inayoweza kufasiriwa na mashine**.

### Sitemap ya XML

- Inaorodhesha URL.  
- Inatoa vidokezo vya crawling.  
- Husaidia injini za utafutaji kugundua kurasa.  
- Haina maana ya kisemantiki.  
- Haina muundo wa maudhui zaidi ya URL.

### OLAMIP

- Hufafanua aina za maudhui (`page`, `project`, `doc_page`, n.k.).  
- Hufafanua aina za sehemu (`project_group`, `doc_category`, n.k.).  
- Hutoa maelezo ya kikanoni.  
- Hutoa muhtasari ulioboreshwa kwa LLMs.  
- Hutoa vitambulisho na metadata.  
- Huanzisha muundo wa wazi wa kihierarkia.  
- Hutoa mfumo wa AI ramani ya maudhui ya tovuti nzima.

Kwa kifupi:

- **Sitemap XML = “Hizi ni kurasa zangu.”**  
- **OLAMIP = “Hivi ndivyo tovuti yangu inamaanisha.”**

Ndiyo maana mifumo ya AI inaweza kutumia OLAMIP kwa ufanisi zaidi kuliko sitemap ya kawaida.

## Je, faili ya OLAMIP yenye zaidi ya URL 3,000 ni kubwa sana?

Hapana kabisa.  
Faili yenye ingizo 3,000 bado iko ndani ya viwango salama.  
Hata ikiwa na muhtasari mrefu, ukubwa wake ungekuwa takriban MB 6 — mdogo sana kwa seva, vivinjari, na mifumo ya AI ya kisasa.

## Ukubwa wa kawaida wa ingizo la OLAMIP ni upi?

| Urefu wa Muhtasari | Ukubwa wa Takriban |
|---|---|
| Mfupi | 300–600 bytes |
| Mrefu | 1–2 KB |

## Ni nini kinachofanya OLAMIP iweze kupanuka licha ya faili kubwa?

- Muundo wa kihierarkia hupunguza kurudia.  
- Muhtasari mfupi (mara nyingi < herufi 500).  
- Ubanaji mzuri (Gzip au Brotli hupunguza ukubwa kwa 70–90%).

## Faili ya OLAMIP inaweza kuwa kubwa kiasi gani kabla ya matatizo ya utendaji kutokea?

| Ukubwa wa Faili | Takriban URL | Athari |
|---|---:|---|
| 1–10 MB | 500–5,000 | Nzuri kabisa. Kawaida. |
| 10–25 MB | 5,000–12,000 | Bado nzuri. Kiasi polepole. |
| 25–50 MB | 12,000–25,000 | Kubwa lakini inadhibitika. |
| 50–100 MB | 25,000–50,000 | Nzito. Baadhi ya mifumo inaweza kupungua kasi. |
| 100+ MB | 50,000+ | Kubwa mno kwa baadhi ya pipeline za LLM. |

Tovuti nyingi zinaweza kushughulikia makumi ya maelfu ya ingizo bila matatizo.

## Ni mara ngapi ninapaswa kuisasisha?

Kila unapochapisha maudhui mapya au kusasisha kurasa zilizopo.  
Ishughulikie kama sitemap — lakini kwa AI.

## Je, ninahitaji kusasisha faili kuu ikiwa ninatumia `olamip-delta.json`?

Ndiyo.  
Faili kuu `olamip.json` lazima ionyeshe hali ya sasa ya tovuti yako kila wakati.  
Faili ya hiari `olamip-delta.json` ina mabadiliko ya hivi karibuni tu.

## Je, ninahitaji kuongeza `<link>` kwa `olamip-delta.json`?

Hapana.  
Ni faili kuu `olamip.json` pekee inayohitaji kurejelewa kwenye sehemu ya `<head>` ya tovuti yako.  
Mara tu mifumo ya AI inapojua mahali ilipo, itatafuta faili ya delta kiotomatiki katika folda hiyo hiyo.

## Je, LLMs zitasoma faili hii kweli?

Ndiyo — huo ndio lengo.  
Kadiri matumizi yanavyoongezeka, LLMs zitapendelea vyanzo vilivyopangwa kama OLAMIP.

## Ninawezaje kujaribu kama bot ya AI inaweza kupata `olamip.json` yangu?

Jaribu amri hizi kwa OpenAI, Anthropic, na Perplexity:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```
