# Panoramica di OLAMIP

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP è uno standard aperto che permette ai siti web di comunicare in modo chiaro e intenzionale con i sistemi di intelligenza artificiale. Invece di costringere i large language models (LLM) a indovinare il significato da HTML, CSS e JavaScript rumorosi, OLAMIP fornisce una rappresentazione pulita, strutturata e interpretabile dalle macchine dei contenuti più importanti del tuo sito.

Questo documento offre una panoramica generale dello scopo, della filosofia, del formato del file e del modello di aggiornamento di OLAMIP.

---

## 1. Perché OLAMIP è stato creato

I siti moderni sono costruiti per i browser, non per l’IA. Di conseguenza, gli LLM hanno difficoltà con:

- HTML ricco di layout  
- Contenuti generati da JavaScript  
- Menu di navigazione, pubblicità e boilerplate  
- Pagine duplicate o irrilevanti  

Gli LLM *analizzano già* il tuo sito, ma spesso lo interpretano male. OLAMIP risolve questo problema fornendo ai sistemi di IA ciò di cui hanno realmente bisogno:

- Un file JSON leggero (`/olamip.json`)  
- Riassunti puliti e curati manualmente  
- Metadati strutturati  
- URL canonici  
- Una gerarchia chiara di sezioni, sottosezioni e voci  

Adottando OLAMIP, riprendi il controllo su come l’IA comprende e rappresenta i tuoi contenuti.

---

## 2. Filosofia di base: adatto agli esseri umani + adatto alle macchine

OLAMIP è progettato come un ponte di comunicazione tra esseri umani e IA. Ogni parte del protocollo deve essere:

| Principio | Adatto agli esseri umani | Adatto alle macchine |
|-----------|---------------------------|------------------------|
| Formato del file | Facile da scrivere e validare | JSON rigoroso e prevedibile |
| Nomi dei campi | Intuitivi e autoesplicativi | Minuscoli, conformi allo schema |
| Riassunti | Linguaggio chiaro e coerente con il brand | Concisi, fattuali, ricchi semanticamente |
| Sistema di priorità | Etichette semplici (`"high"`, `"medium"`, `"low"`) | Convertibili in pesi di ranking |
| Documentazione | Guide in linguaggio semplice | Schemi JSON, regole dei validator |
| Strumenti | Generator, plugin CMS | CLI tools, parser, test suite |

- **Per gli esseri umani:** OLAMIP deve essere facile da adottare senza competenze tecniche avanzate.  
- **Per le macchine:** OLAMIP deve essere strutturato, prevedibile e non ambiguo.

---

## 3. Il file OLAMIP (`/olamip.json`)

Un file OLAMIP è un documento JSON strutturato ospitato all’indirizzo:

`https://yourdomain.com/olamip.json`

Contiene:

- `protocol` — deve essere `"OLAMIP"`  
- `version` — versione del protocollo  
- `identity` — identità del sito  
- `content` — gerarchia strutturata dei contenuti  
- `metadata` — lingua, ultimo aggiornamento e altri campi globali  

### 3.1 Tag di individuazione (Discovery Tags)

Per garantire che i sistemi di IA trovino il file OLAMIP in modo affidabile, includi:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

L’uso di entrambi garantisce ridondanza, compatibilità e longevità.

---

## 4. Struttura del file

### 4.1 Oggetto Identity

Descrive il sito o l’organizzazione:

- `name` — obbligatorio  
- `type` — obbligatorio  
- `canonical_description` — obbligatorio  
- `tags` — opzionale  

### 4.2 Oggetto Content

Contiene:

- Una panoramica (`overview`)  
- `sections`  
- `subsections` opzionali  
- `entries` (unità di contenuto più granulari)  

Supporta una profondità di annidamento illimitata.

### 4.3 Sezioni (Sections)

Una sezione raggruppa contenuti correlati. Campi obbligatori:

- `title`  
- `summary`  
- `url`  
- `section_type`  

Campi opzionali:

- `policy` (`"allow"` o `"forbid"`)  
- `tags`  
- `priority`  
- `published`  
- `language`  
- `subsections`  
- `entries`  

**Ereditarietà della policy:**  
Se omessa, la policy viene ereditata dagli elementi superiori. Valore predefinito: `"allow"`.

### 4.4 Voci (Entries)

Le voci rappresentano elementi di contenuto individuali come:

- Articoli di blog  
- Notizie  
- Prodotti  
- Pagine di documentazione  
- Articoli di ricerca  
- Contenuti multimediali  

Campi obbligatori:

- `title`  
- `summary`  
- `url`  
- `content_type`  

Campi opzionali:

- `policy`, `tags`, `priority`, `published`, `language`, `metadata`  

Gli URL sono obbligatori perché identificano in modo univoco i contenuti e consentono all’IA di verificarli, deduplicarli e collegarli.

---

## 5. Metadati e supporto linguistico

L’oggetto `metadata` può includere:

```json
{
  "last_updated": "2026-01-21",
  "language": "it",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### Supporto multilingue

La lingua può essere definita a livello di:

- file  
- sezione  
- voce  

Usa codici BCP‑47 come `it`, `en`, `es`, `pt-BR`, `zh-CN`.

Questo aiuta l’IA a:

- scegliere il tokenizer corretto  
- evitare mescolanze linguistiche  
- migliorare la precisione del recupero  
- ridurre le allucinazioni  

---

## 6. Tag e priorità

### 6.1 Tag

I tag forniscono indizi semantici leggeri. Devono essere:

- minuscoli  
- una sola parola  
- ASCII  
- con trattini per concetti multi‑parola  
- coerenti  

Esempi:

| Concetto | Tag valido |
|----------|------------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

### 6.2 Priorità

Valori:

- `high` — contenuti principali  
- `medium` — predefinito  
- `low` — contenuti di nicchia o datati  

Usa `high` con parsimonia (5–10% delle voci).

---

## 7. Aggiornamenti Delta (`olamip-delta.json`)

I siti web cambiano continuamente. OLAMIP supporta aggiornamenti incrementali tramite un file opzionale:

`/olamip-delta.json`

Contiene solo:

- voci aggiunte  
- voci aggiornate  
- URL rimossi  

### 7.1 Perché gli aggiornamenti delta sono importanti

- I sistemi di IA rimangono aggiornati senza rielaborare l’intero file.  
- I cataloghi grandi restano gestibili.  
- Gli aggiornamenti si propagano rapidamente.  
- Nessuna modifica viene persa.  

### 7.2 Finestra delta scorrevole (consigliata)

Conserva tutti i delta degli ultimi 7–30 giorni in un unico file.  
I sistemi di IA li applicano in ordine cronologico.

### 7.3 File delta versionati (alternativa)

Pubblica file separati:

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`  
...

Ideale per siti ad alto volume (notizie, e‑commerce, documentazione).

### 7.4 Individuazione

Solo il file OLAMIP principale richiede tag `<link>` e `<meta>`.  
I sistemi di IA cercano automaticamente file delta nella stessa directory.

---

## 8. OLAMIP vs sitemap tradizionali

- Sitemap XML: “Ecco le mie pagine.”  
- OLAMIP: “Ecco cosa significa il mio sito.”  

Le sitemap elencano URL.  
OLAMIP fornisce:

- riassunti  
- tipi di contenuto  
- gerarchia delle sezioni  
- tag  
- metadati  
- priorità  
- politiche di ingestione  

Insieme a schema.org, OLAMIP offre ai sistemi di IA una comprensione completa e curata del tuo sito.

---

## 9. Domande frequenti (Sintesi)

### OLAMIP espone informazioni private?

No. Riassume solo contenuti pubblici.

### I concorrenti possono usare il mio file?

Non più di quanto già usino le tue pagine pubbliche.

### Quanto può essere grande un file OLAMIP?

1–10 MB è comune.  
Anche oltre 3.000 voci è normale e sicuro.

### Quanto spesso va aggiornato?

Ogni volta che i contenuti cambiano — trattalo come una sitemap per l’IA.

### Devo aggiornare il file principale se uso i delta?

Sì. `olamip.json` è sempre la versione autorevole.

### Gli LLM leggeranno davvero questo file?

Sì — è esattamente lo scopo del protocollo.  
Puoi testare con:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```

---

## 10. Sintesi

OLAMIP trasforma il tuo sito in una mappa della conoscenza interpretabile dalle macchine:

- `olamip.json` — istantanea completa e strutturata  
- `olamip-delta.json` — registro delle modifiche incrementali  
- sezioni, sottosezioni, voci — gerarchia chiara  
- riassunti, tag, priorità — significato curato manualmente  
- discovery tags — ingestione affidabile  
- supporto multilingue — pronto per il mondo  

Adottando OLAMIP, garantisci che i sistemi di IA comprendano i tuoi contenuti esattamente come intendi — con precisione, coerenza e su larga scala.
