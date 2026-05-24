# OLAMIP – Guida Rapida

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Questa guida rapida mostra come implementare OLAMIP su un sito web nel modo più semplice possibile.

---

## 1. Crea `olamip.json`

Posiziona un file `olamip.json` valido nella root del tuo sito:

`https://yourdomain.com/olamip.json`

Inizia con un’istantanea piccola e pulita delle tue pagine più importanti.

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

## 2. Aggiungi i tag di individuazione nel tuo HTML

Inserisci questi tag nella sezione `<head>` della homepage e delle pagine principali:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

---

## 3. Mantieni i riassunti concisi

Scrivi riassunti brevi e fattuali che spiegano chiaramente di cosa tratta la pagina e perché è rilevante. Evita linguaggio promozionale, testo duplicato e descrizioni vaghe.

Esempi corretti:

- “Una guida introduttiva alla fotografia a lunga esposizione.”
- “Dettagli del prodotto e prezzi per il piano enterprise.”

Da evitare:

- “La pagina migliore del mondo.”
- “Contenuti incredibili che amerai.”

---

## 4. Usa i content type corretti

Scegli il `content_type` più specifico possibile:

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

## 5. Organizza con le sezioni

Usa `sections` per raggruppare contenuti e `entries` per pagine individuali. Se necessario, aggiungi `subsections` per una struttura più profonda.

Esempio:

- Sezione: Blog  
- Sottosezione: Tutorials  
- Entry: How to Use OLAMIP  

---

## 6. Imposta la priorità con intenzione

Usa:

- `high` per le pagine più importanti  
- `medium` per le pagine normali  
- `low` per contenuti di nicchia o datati  

Non impostare tutto su `high`.

---

## 7. Usa il campo policy per controllare l’ingestione da parte dell’IA

Il campo `policy` indica ai sistemi di IA se possono o meno ingerire una sezione, sottosezione o entry.

- `"allow"` → consenti l’ingestione  
- `"forbid"` → escludi contenuti specifici  

Se omesso, OLAMIP eredita la policy dal livello superiore. Se nessun livello la definisce, il valore predefinito è `"allow"`.

Esempi:

**Escludere un’intera sezione:**

```json
{
  "title": "Deprecated Guides",
  "summary": "Outdated content, not for AI.",
  "url": "https://yourdomain.com/old-guides/",
  "section_type": "doc_category",
  "policy": "forbid"
}
```

**Escludere una singola pagina:**

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

## 8. Aggiungi i metadati della lingua

Usa codici linguistici BCP‑47 come:

- `en`
- `es`
- `fr`
- `de`
- `it`
- `pt-BR`
- `zh-CN`

Puoi impostare la lingua a livello di file, sezione o entry.

---

## 9. Aggiorna il file regolarmente

Ogni volta che aggiungi, modifichi o rimuovi pagine importanti, aggiorna `olamip.json`.

Se il sito cambia spesso, mantieni anche `olamip-delta.json` nella stessa directory per consentire all’IA di restare aggiornata tra un refresh completo e l’altro.

---

## 10. Valida prima di pubblicare

Prima di andare online, verifica che:

- Il JSON sia valido  
- Gli URL siano assoluti  
- I campi obbligatori siano presenti  
- I riassunti siano chiari e brevi  
- I tag siano normalizzati e coerenti  
- Il file sia servito dalla root del sito  

---

## Checklist di implementazione minima

- [ ] Crea `olamip.json`  
- [ ] Ospitalo nella root del sito  
- [ ] Aggiungi i discovery tag nell’`<head>`  
- [ ] Includi sezioni ed entry per i contenuti principali  
- [ ] Aggiungi metadata e lingua  
- [ ] Mantieni i riassunti brevi e accurati  
- [ ] Aggiorna il file quando il sito cambia  
- [ ] Aggiungi `olamip-delta.json` se vuoi aggiornamenti incrementali  

---

## Da dove iniziare

Se non sai da dove cominciare, implementa OLAMIP solo per le pagine principali:

- Homepage  
- Indice del blog  
- Articoli migliori  
- Pagine chiave di prodotti o servizi  
- Pagine importanti della documentazione  

Poi espandi gradualmente.
