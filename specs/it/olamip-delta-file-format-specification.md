# Specifica del formato file OLAMIP-DELTA

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Il protocollo **OLAMIP-DELTA** definisce un formato di file delta basato su JSON per gli aggiornamenti incrementali di `olamip.json`. Questa specifica è rivolta a webmaster e implementatori che conoscono già i dati strutturati, HTML e la pubblicazione in stile sitemap. 

## 1. Panoramica

`olamip.json` è la rappresentazione completa e autorevole della struttura e del contenuto del tuo sito. `olamip-delta.json` è un file compagno facoltativo che descrive le modifiche apportate a questo snapshot dall’ultimo aggiornamento. 

Insieme, formano un flusso di modifiche interpretabile dalle macchine:

- `olamip.json`: snapshot completo e strutturato.
- `olamip-delta.json`: registro incrementale delle modifiche.

---

## 2. Concetti chiave

- **Entry** — un elemento di contenuto a livello foglia, ad esempio un articolo di blog, una pagina prodotto o una pagina di documentazione.
- **Section / Subsection** — un raggruppamento gerarchico, ad esempio una categoria del blog, una categoria di documentazione, una collezione di prodotti o un gruppo di progetti.
- **Operation** — una tra `added`, `updated`, `removed`.
- **Delta** — un insieme di modifiche con data che contiene `added`, `updated` e `removed`.

Questo formato è progettato per essere:

- **prima di tutto interpretabile dalle macchine**, ma ancora abbastanza leggibile da poter essere verificato manualmente.
- **incrementale**, così che i sistemi di IA possano restare sincronizzati senza rielaborare l’intero sito. 

---

## 3. Posizione del file e scoperta

### 3.1 Posizione del file

`olamip.json` deve essere ospitato nella root del tuo dominio:

```text
https://yourdomain.com/olamip.json
```

`olamip-delta.json` deve essere ospitato nella stessa directory:

```text
https://yourdomain.com/olamip-delta.json
```

Le versioni datate dei delta (facoltative) risiedono nella stessa directory:

```text
/olamip.json
/olamip-delta-YYYY-MM-DD.json
```

### 3.2 Meccanismo di scoperta

I webmaster devono dichiarare la posizione di `olamip.json` usando entrambi i tag nel `<head>` del sito:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

I sistemi di IA che conoscono `olamip.json` cercheranno automaticamente `olamip-delta.json` nella stessa directory e applicheranno i delta come descritto in questa specifica. Non è necessario aggiungere un tag `<link>` o `<meta>` separato per `olamip-delta.json`. 

---

## 4. Struttura di alto livello

A livello superiore, `olamip-delta.json` è un oggetto JSON con i seguenti campi:

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
          "title": "Example entry",
          "url": "https://example.com/page/",
          "summary": "...",
          "content_type": "blog_article"
        }
      ],
      "updated": [
        {
          "url": "https://example.com/page/",
          "summary": "Updated summary"
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

### 4.1 Campi obbligatori di primo livello

- `protocol` (string, obbligatorio)  
  Deve essere `"OLAMIP-DELTA"`.  
  Distingue il file delta dal file principale OLAMIP e da altri formati delta.

- `version` (string, obbligatorio)  
  Deve corrispondere allo schema di versioning documentato, ad esempio `"1.0.0"`.  
  Consente agli implementatori di rilevare cambiamenti di schema.

- `window_days` (integer, facoltativo)  
  Numero di giorni nella finestra mobile; deve essere positivo, tipicamente 7–30.  
  È richiesto solo se si usa un file delta a finestra mobile.

- `last_updated` (string, obbligatorio)  
  Data dell’ultima modifica del file delta, nel formato ISO-8601 `YYYY-MM-DD`.  
  Aiuta i crawler a decidere quando rieseguire il fetch.

- `deltas` (array, obbligatorio)  
  Array non vuoto di insiemi di modifiche datati.  
  Ogni elemento è un **oggetto delta** con `date`, `added`, `updated` e `removed`.

### 4.2 Array `deltas` e ordine delle date

- `deltas` deve essere ordinato per `"date"` in ordine crescente (dal più vecchio al più recente).
- Ogni oggetto delta deve avere un campo `"date"` nel formato ISO-8601 `YYYY-MM-DD`.
- `deltas` non deve essere vuoto.

Questo ordine non è opzionale ai fini della conformità: i sistemi di IA assumono che i delta siano ordinati cronologicamente. 

---

## 5. Struttura dell’oggetto Delta

Ogni elemento di `deltas` è un oggetto delta con la seguente forma:

```json
{
  "date": "2026-04-27",
  "added": [ ... ],
  "updated": [ ... ],
  "removed": [ ... ]
}
```

Per chiarezza e coerenza interpretabile dalle macchine, i webmaster **DEVONO** includere tutti e tre i campi operativi in ogni oggetto delta, anche se alcuni sono array vuoti.

Campi:

- `date` (string) — data ISO-8601 `YYYY-MM-DD`.
- `added` (array) — zero o più elementi aggiunti.
- `updated` (array) — zero o più elementi modificati.
- `removed` (array) — zero o più elementi rimossi.

Esempio di oggetto delta contenente solo aggiunte:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Nuova pagina",
      "summary": "Una nuova pagina è stata aggiunta al sito.",
      "url": "https://example.com/new-page/"
    }
  ],
  "updated": [],
  "removed": []
}
```

Questo schema è richiesto anche quando in un giorno si applica una sola operazione.

---

## 6. Operazioni

### 6.1 `added`

Un elemento `added` è un **oggetto completo** del tipo applicabile (entry, section o subsection). I campi devono corrispondere alla specifica OLAMIP del tipo interessato.

Per una **entry**:

- `title` (string, obbligatorio) — titolo leggibile.
- `summary` (string, obbligatorio) — descrizione concisa.
- `url` (string, obbligatorio) — URL canonico assoluto.
- `content_type` (string, obbligatorio) — dalla tassonomia `content_type` di OLAMIP.
- `tags` (array, facoltativo) — elenco di tag minuscoli e a token singolo.
- `priority` (string, facoltativo) — `"high"`, `"medium"` o `"low"`.
- `policy` (string, facoltativo) — `"allow"` o `"forbid"`.
- `language` (string, facoltativo) — codice lingua BCP-47.
- `metadata` (object, facoltativo) — dati strutturati specifici del dominio.

Per una **section o subsection**, gli oggetti in `added` seguono la specifica OLAMIP a livello section, con `title`, `summary`, `url`, `section_type`, `policy`, `tags`, `priority`, ecc.

#### Regole per `added`

- Gli elementi `added` devono includere `url`.
- Gli elementi `added` devono essere oggetti completi, almeno con tutti i campi obbligatori della specifica OLAMIP principale.
- Nessuna `url` può comparire più di una volta nello stesso oggetto delta.

Esempio:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Nuovo articolo del blog",
      "summary": "Una breve introduzione alla nuova funzionalità.",
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

Un elemento `updated` deve contenere `url` e può essere:

- un **oggetto parziale** contenente solo i campi modificati, oppure
- un **oggetto completo** contenente tutti i campi.

Per ogni elemento `updated`, i campi mancanti sono considerati **invariati**.

#### Esempio di aggiornamento parziale

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "url": "https://example.com/post/to-update/",
      "summary": "Riassunto aggiornato con nuove informazioni.",
      "tags": ["feature", "update", "docs"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Esempio di sostituzione completa

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "title": "Titolo del post aggiornato",
      "summary": "Riassunto aggiornato con nuove informazioni.",
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

#### Regole per `updated`

- Gli elementi `updated` devono contenere `url`.
- `updated` può essere un oggetto parziale o completo.
- `updated` non deve cambiare l’identità di un entry tramite riassegnazione della `url`; questa è una decisione di configurazione, non una questione del file delta.

### 6.3 `removed`

Un elemento `removed` è **minimale** e contiene solo l’identità.

```json
{
  "url": "https://example.com/page/to-remove/"
}
```

#### Regole per `removed`

- Gli elementi `removed` devono contenere `url` e **non devono contenere altri campi**.
- `url` deve essere una URL assoluta e canonica.
- `removed` si applica all’URL indicato e ai suoi discendenti strutturali (subsections ed entries), se presenti.
- Nessuna `url` può comparire più di una volta nello stesso oggetto delta.

Esempio:

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

## 7. URL e identità degli oggetti

OLAMIP-DELTA utilizza le URL canoniche come identità principale di entries, sections e subsections.

### Regole di identità

- `url` deve essere una URL assoluta, inclusi schema e dominio.
- Nessuna `url` deve comparire più di una volta nello stesso oggetto delta, né in `added`, né in `updated`, né in `removed`.
- Quando una section viene rimossa, tutti i suoi discendenti sono considerati rimossi.
- Una modifica di URL richiede un **remove** seguito da un **add**, non un aggiornamento diretto della `url`.

### Migliori pratiche per le URL

- Usa URL canoniche stabili, che cambino raramente.
- Evita parametri di query o frammenti dipendenti da sessione.
- Se una pagina cambia indirizzo, trattala come un caso di redirect e canonicalizzazione, non come un aggiornamento diretto del file delta.

---

## 8. Finestra delta mobile (consigliata)

La **finestra delta mobile** è la modalità raccomandata per `olamip-delta.json`.

In questo modello:

- Il file delta contiene le modifiche degli ultimi `window_days`.
- I sistemi di IA leggono tutti i delta nel file e li applicano in ordine cronologico.
- Questo evita di perdere aggiornamenti quando i crawler eseguono il fetch in ritardo. 

### Esempio di delta mobile (finestra di 7 giorni)

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
          "title": "Nuovo prodotto A",
          "url": "https://example.com/product/a",
          "summary": "Una nuova aggiunta al catalogo."
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
          "summary": "Descrizione aggiornata per riflettere nuove funzionalità."
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

I sistemi di IA:

- leggono tutti i delta presenti nel file,
- li applicano in ordine cronologico,
- e arrivano allo stato più recente.

---

## 9. Delta versionati (alternativa)

Alcuni webmaster preferiscono i **delta versionati**, in cui ogni file contiene le modifiche di un singolo giorno.

### Esempio di struttura

```text
/olamip.json
/olamip-delta-2026-03-01.json
/olamip-delta-2026-03-02.json
/olamip-delta-2026-03-03.json
```

### Forma del file

Ogni file versionato ha la stessa struttura logica di un file glissante, ma rappresenta solo una data di modifica:

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "date": "2026-03-02",
  "added": [
    {
      "title": "Nuovo articolo del blog",
      "url": "https://example.com/blog/new-post/",
      "summary": "Panoramica sintetica dell’aggiornamento."
    }
  ],
  "updated": [],
  "removed": []
}
```

### Regole di ingestione

I sistemi di IA dovrebbero:

- rilevare tutti i file `olamip-delta-*.json` nella stessa directory di `olamip.json`,
- ordinarli per `date` in ordine crescente,
- applicare solo quelli che rientrano nella finestra configurata, ad esempio gli ultimi 7–30 giorni.

Questo approccio è particolarmente utile per:

- siti di notizie con aggiornamenti quotidiani frequenti,
- cataloghi e-commerce ad alto volume,
- piattaforme di documentazione con cicli di rilascio rapidi.

---

## 10. Modifiche strutturali: Sections e Subsections

OLAMIP-DELTA applica lo stesso schema delta alle **sections** e **subsections** come fa per le **entries**. Ciò consente ai sistemi di IA di capire non solo quali pagine esistono, ma anche come sono organizzate.

### 10.1 Aggiungere una Section o una Subsection

Quando aggiungi una section o una subsection, includi un **oggetto section completo** in `added`.

Esempio:

```json
{
  "date": "2026-05-01",
  "added": [
    {
      "title": "Tutorial",
      "summary": "Guide passo passo per principianti e utenti avanzati.",
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

#### Regole

- Gli oggetti `added` per le section devono contenere `title`, `summary`, `url` e `section_type`.
- Usa `section_type` per riflettere il ruolo semantico, per esempio `blog_category`, `doc_category`, `product_collection` o `research_category`.
- `priority` e `policy` aiutano i sistemi di IA a comprendere l’importanza e le regole di ingestione per l’intero sottoalbero.

### 10.2 Aggiornare una Section o una Subsection

Includi `url` e tutti i campi modificati.

Esempio:

```json
{
  "date": "2026-05-02",
  "updated": [
    {
      "url": "https://example.com/blog/tutorials/",
      "title": "Tutorial passo passo",
      "summary": "Guide per principianti e utenti avanzati, con esempi di codice e best practice.",
      "tags": ["tutorials", "beginner", "advanced", "code-examples"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Regole

- Le section aggiornate devono contenere `url`.
- `updated` può includere qualsiasi sottoinsieme degli altri campi.
- I campi mancanti sono considerati invariati.

### 10.3 Rimuovere una Section o una Subsection

Includi solo `url` in `removed`.

Esempio:

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

#### Regole

- Gli elementi `removed` delle section devono contenere solo `url` e nessun altro campo.
- L’URL deve essere l’URL canonica della section o subsection rimossa.
- Quando una section viene rimossa, tutti i suoi discendenti sono considerati rimossi.

---

## 11. Conformità

Implementatori e webmaster dovrebbero considerare i seguenti punti come regole di conformità.

### 11.1 Regole MUST

Un file OLAMIP-DELTA conforme **DEVE**:

- dichiarare `protocol: "OLAMIP-DELTA"`,
- impostare `version` a un valore conforme allo schema di versione documentato,
- contenere un array `deltas` non vuoto,
- far sì che ogni oggetto delta contenga un `date` nel formato ISO-8601 `YYYY-MM-DD`,
- ordinare `deltas` in ordine crescente per `date`,
- includere tutti e tre i campi operativi (`added`, `updated`, `removed`) in ogni oggetto delta, anche se vuoti,
- garantire che `url` in tutte le operazioni sia una URL assoluta e canonica,
- garantire che nessuna `url` compaia più di una volta nello stesso oggetto delta,
- garantire che `added` e `updated` contengano `url`,
- garantire che `removed` contenga `url` e nessun altro campo,
- trattare i campi mancanti in `updated` come invariati,
- trattare `removed` come rimozione dell’URL indicato e dei suoi discendenti dall’indice. 

### 11.2 Regole SHOULD

Un’implementazione conforme **DOVREBBE**:

- adottare per default la **finestra delta mobile**, a meno che il flusso di pubblicazione non sia più adatto ai delta versionati,
- mantenere `last_updated` aggiornato in modo che crawler e validator sappiano quando il file è recente,
- utilizzare URL canoniche stabili e raramente modificate per evitare ambiguità nell’applicazione dei delta,
- mantenere `window_days` in genere tra 7 e 30 giorni, in base alla frequenza di pubblicazione del sito,
- per i delta versionati, conservare abbastanza file all’interno della finestra configurata, ad esempio gli ultimi 7–30 giorni, per evitare perdite di aggiornamenti in caso di ritardi del crawler.

### 11.3 Regole MAY

Un’implementazione conforme **PUÒ**:

- utilizzare il formato delta versionato (`olamip-delta-YYYY-MM-DD.json`) invece di un singolo file mobile, se meglio si adatta alla pipeline di deployment,
- pubblicare `olamip-delta.json` con una frequenza diversa da `olamip.json`, purché il rapporto tra snapshot e delta resti chiaro e coerente,
- aggiungere metadati diagnostici o di logging sotto una chiave separata fuori dai campi del protocollo, ad esempio un oggetto `diagnostics`, purché non entri in conflitto con i campi richiesti o cambi il significato del delta.

---

## 12. Best practice

Per garantire compatibilità robusta e duratura con i sistemi di IA, i webmaster dovrebbero seguire queste best practice nella generazione di `olamip-delta.json`.

### 12.1 Automatizzare la generazione dei delta

- Collega la generazione del delta al tuo CMS o alla pipeline di deployment.
- Ogni volta che un contenuto viene pubblicato, aggiornato o rimosso, aggiorna `olamip-delta.json` di conseguenza.
- Valida lo schema del file prima del deployment per evitare errori di sintassi o campi non validi.

### 12.2 Mantenere `olamip.json` autorevole

- Conserva un `olamip.json` valido e aggiornato.
- I nuovi sistemi di IA possono iniziare dal file completo e poi applicare i delta successivi.
- Non trattare `olamip-delta.json` come sostituto dello snapshot principale.

### 12.3 Usare URL canoniche stabili

- Usa URL assolute con schema e dominio.
- Evita pattern di URL che cambiano spesso, a meno che il sistema del sito, per esempio i redirect, non sia abbastanza robusto da preservare il significato canonico.
- Se l’URL di una pagina cambia davvero, trattalo come un caso “remove-then-add”, non come un aggiornamento diretto di `url` nel delta.

### 12.4 Mantenere coerenti summary e metadati

- I summary negli elementi `added` e `updated` devono essere coerenti con il contenuto reale.
- Piccole correzioni di formulazione che non cambiano il significato possono essere pubblicate come delta, ma vanno ridotte al minimo per evitare churn inutile.
- I tag devono essere normalizzati, in minuscolo, a una sola parola, e collegati da trattini per i termini composti; devono riflettere concetti stabili. 

---

## 13. Risorse correlate

Per una comprensione completa dell’ecosistema OLAMIP, i webmaster dovrebbero consultare anche:

- **[Specifica del formato file OLAMIP](https://olamip.org/file-format-specification/)**  
  Descrive la struttura di `olamip.json` e definisce i campi per entries, sections e subsections su cui si basa il formato delta.

- **[Domande frequenti OLAMIP](https://olamip.org/frequently-asked-questions/)**  
  Spiega le questioni di deployment, discovery e operation per i webmaster.

- **[Specifica del formato file OLAMIP-DELTA](https://olamip.org/delta-updates/)**  
  Schema JSON ufficiale per `olamip-delta.json`, utilizzabile per validare e lintare automaticamente i file delta.

- **[Perché OLAMIP è uno standard superiore a LLMs.txt](https://olamip.org/why-olamip-is-a-superior-standard-to-llms-txt/)**  
  Spiega i vantaggi di OLAMIP e OLAMIP-DELTA rispetto a formati di metadati più semplici e non strutturati.

---

Questo conclude la specifica del formato file OLAMIP-DELTA. Webmaster e implementatori possono usare questo documento come riferimento per generare, validare e consumare `olamip-delta.json` in ambienti di produzione. 