# Specifica del formato file OLAMIP

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Panoramica

Il file OLAMIP (`/olamip.json`) è un documento JSON strutturato che fornisce riepiloghi curati delle pagine più importanti del tuo sito web. È progettato per essere facilmente analizzato dai modelli linguistici di grandi dimensioni (LLM), consentendo loro di comprendere, prioritizzare e utilizzare i tuoi contenuti con chiarezza, precisione e intenzione.

## Posizione del file

Il file OLAMIP deve essere ospitato alla radice del tuo dominio:

`https://yourdomain.com/olamip.json`

## Dichiarazione della posizione del file OLAMIP

Per massimizzare l’adozione e garantire che i sistemi possano individuare in modo affidabile il tuo file OLAMIP, pubblica sia un tag `<link>` sia un tag `<meta>` nella sezione `<head>` del tuo sito.

### Scoperta primaria: `<link rel="olamip">`

- Pratica standardizzata: crawler e parser già analizzano i tag `<link>` per risorse come canonical, sitemap e alternate.
- Machine-friendly: dichiara una relazione formale tra la pagina e il file OLAMIP.
- Interoperabilità: si inserisce perfettamente negli standard web esistenti, rendendo più facile l’adozione da parte dei sistemi AI senza gestione speciale.

### Scoperta di fallback: `<meta name="olamip-location">`

- Leggibile dall’uomo: semplice da aggiungere e comprendere per i webmaster.
- Compatibilità: alcuni parser e strumenti preferiscono esaminare i tag `<meta>`.
- Ridondanza: funge da backup se un crawler non supporta ancora `rel="olamip"`.

### Perché usarli entrambi è più forte

- A prova di futuro: con la crescita dell’adozione OLAMIP, sistemi diversi potrebbero implementare la scoperta in modi diversi. Includerli entrambi garantisce che nessun sistema resti escluso.
- Resilienza: se un metodo fallisce, l’altro fornisce un fallback.
- Facilità d’integrazione: gli sviluppatori possono scegliere il metodo che meglio si adatta alla propria pipeline, senza costringere i webmaster a indovinare.
- Fiducia e chiarezza: segnali doppi riducono l’ambiguità e rendono esplicito il luogo in cui si trova il file OLAMIP.

### Implementazione consigliata

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Includendo entrambi i tag, rendi il tuo file OLAMIP individuabile dal maggior numero possibile di crawler, validatori e sistemi AI.

## Struttura del file

Il file OLAMIP deve essere un documento JSON valido codificato in UTF-8 contenente:

- `protocol`
- `version`
- `identity`
- `content`
- `metadata`

### Struttura di alto livello

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": { "...": "..." },
  "content": { "...": "..." },
  "metadata": { "...": "..." }
}
```

## Oggetto Identity

| Campo | Tipo | Obbligatorio | Descrizione |
|---|---|---:|---|
| `name` | `string` | Sì | Nome del sito web o dell’organizzazione. |
| `type` | `string` | Sì | Tipo di entità, come `company`, `blog` o `ecommerce`. |
| `canonical_description` | `string` | Sì | Descrizione leggibile del sito. |
| `tags` | `array<string>` | No | Parole chiave opzionali che descrivono il dominio o il settore. |

## Oggetto Content

L’oggetto `content` contiene:

- Un `overview`
- Un elenco di `sections`
- Ogni sezione può contenere `subsections`
- Ogni sezione o sottosezione può contenere `entries`

Questo supporta gerarchie multilivello.

### Oggetto Overview

| Campo | Tipo | Obbligatorio | Descrizione |
|---|---|---:|---|
| `summary` | `string` | Sì | Una spiegazione concisa dello scopo del sito web. |

## Specifica dell’oggetto Section

Una `Section` rappresenta una categoria, una raccolta o un raggruppamento di contenuti. Le sezioni possono contenere:

- `entries` (elementi di contenuto)
- `subsections` (oggetti Section annidati)

Ciò consente una profondità di nesting illimitata.

### Campi Section

| Campo | Tipo | Obbligatorio | Descrizione |
|---|---|---:|---|
| `title` | `string` | Sì | Nome leggibile della sezione. |
| `summary` | `string` | Sì | Descrizione del contenuto della sezione. |
| `url` | `string` | Sì | URL canonico della sezione. |
| `section_type` | `string` | Sì | Classificazione semantica dalla tassonomia. |
| `policy` | `string` | No | `"allow"` o `"forbid"`. Vedi le regole di ereditarietà sotto. |
| `tags` | `array<string>` | No | Parole chiave opzionali. |
| `priority` | `string` | No | `"high"`, `"medium"` o `"low"`. |
| `published` | `string` | No | Data ISO 8601. |
| `language` | `string` | No | Usa i codici lingua BCP-47. |
| `entries` | `array<Entry>` | Sì | Array di oggetti Entry. |
| `subsections` | `array<Section>` | No | Array di oggetti Section annidati. |

### Valori consentiti per `section_type`

| `section_type` | Significato |
|---|---|
| `blog_category` | Raggruppa articoli di blog. |
| `news_section` | Raggruppa articoli di notizie. |
| `product_collection` | Raggruppa prodotti o servizi. |
| `doc_category` | Raggruppa pagine di documentazione. |
| `research_category` | Raggruppa articoli di ricerca o dataset. |
| `project_group` | Raggruppa progetti di portfolio. |
| `content_section` | Fallback generico. |

## Comportamento della policy ed ereditarietà

Il campo `policy` controlla se i sistemi AI sono autorizzati a ingerire il contenuto rappresentato da una Section, Subsection o Entry. I valori validi sono `"allow"` e `"forbid"`. Questo campo è opzionale a tutti i livelli della struttura OLAMIP.

### Comportamento predefinito

Se il campo `policy` viene omesso a un dato livello, la policy effettiva viene determinata tramite ereditarietà. Se nessun antenato definisce una policy, il valore predefinito è `"allow"`.

### Regole di ereditarietà

I sistemi AI devono determinare la policy effettiva per ogni Entry seguendo quest’ordine:

1. **Policy a livello Entry**
   Se l’Entry definisce `policy`, quel valore è autorevole.

2. **Policy a livello Subsection**
   Se l’Entry omette `policy`, il sistema AI deve controllare la Subsection più vicina che la contiene.

3. **Policy a livello Section**
   Se né l’Entry né la Subsection definiscono `policy`, il sistema AI deve usare la policy definita a livello Section.

4. **Policy predefinita**
   Se nessun antenato definisce una `policy`, il valore effettivo è `"allow"`.

### Uso previsto per i webmaster

- Per rendere l’intero sito ingeribile dai sistemi AI, ometti il campo `policy` ovunque.
- Per controllare l’ingestione, usa `"allow"` e `"forbid"` in modo selettivo a qualsiasi livello della gerarchia.
- Una policy applicata a una Section o Subsection si applica automaticamente a tutti i discendenti, salvo override.

### Requisiti per i sistemi AI

I sistemi AI devono:

- Applicare il default `"allow"` solo quando non esiste alcuna policy esplicita nella catena degli antenati.
- Rispettare la policy effettiva determinata tramite ereditarietà.
- Trattare `"forbid"` come un divieto assoluto di ingestione.
- Trattare `"allow"` come permesso di ingerire il contenuto rappresentato da quel nodo.

## Diagramma della gerarchia multilivello

```text
content
└── sections[]
    ├── Section (Level 1)
    │     ├── entries[]
    │     └── subsections[]
    │           ├── Section (Level 2)
    │           │     ├── entries[]
    │           │     └── subsections[]
    │           │           └── Section (Level 3)
    │           │                 └── entries[]
    │           └── ...
    └── ...
```

Questa struttura supporta:

- News → Politics → Opinion → Articles
- Docs → API → Authentication → Pages
- Store → Clothing → Men → Jackets → Products
- Research → Physics → Quantum → Papers

## Specifica dell’oggetto Entry

Una `Entry` è l’unità di contenuto più granulare. Esempi includono:

- Articolo di blog
- Articolo di notizie
- Pagina prodotto
- Pagina di documentazione
- Articolo scientifico
- Progetto di portfolio
- Pagina legale
- Risorsa scaricabile

### Campi Entry

| Campo | Tipo | Obbligatorio | Descrizione |
|---|---|---:|---|
| `title` | `string` | Sì | Titolo leggibile. |
| `summary` | `string` | Sì | Descrizione concisa del contenuto. |
| `url` | `string` | Sì | URL canonico assoluto. |
| `content_type` | `string` | Sì | Classificazione semantica dalla tassonomia. |
| `policy` | `string` | No | `"allow"` o `"forbid"`. Come per Sections/Subsections. |
| `tags` | `array<string>` | No | Parole chiave opzionali. |
| `priority` | `string` | No | `"high"`, `"medium"` o `"low"`. |
| `published` | `string` | No | Data di pubblicazione ISO 8601. |
| `language` | `string` | No | Usa i codici lingua BCP-47. |
| `metadata` | `object` | No | Informazioni strutturate specifiche del dominio o della pagina. |

### Perché gli URL sono obbligatori

Il campo `url` è essenziale perché funge da identificatore canonico del contenuto. Mentre i riepiloghi trasmettono il significato del contenuto, gli URL collegano quel significato a una posizione specifica e verificabile sul web. I sistemi AI utilizzano gli URL per deduplicazione, recupero, validazione e cross-referencing con schema.org, sitemap e crawler.

### Valori consentiti per `content_type`

#### Pagine generali

| `content_type` | Significato |
|---|---|
| `page` | Pagina di contenuto standard. |
| `landing_page` | Pagina di marketing o campagna. |
| `legal_page` | Termini, privacy, disclaimer. |

#### Blog

| `content_type` | Significato |
|---|---|
| `blog_article` | Un post del blog. |

#### News

| `content_type` | Significato |
|---|---|
| `news_article` | Un articolo di notizie. |

#### E-commerce

| `content_type` | Significato |
|---|---|
| `product` | Una pagina prodotto. |
| `service` | Un’offerta di servizio. |

#### Documentazione

| `content_type` | Significato |
|---|---|
| `doc_page` | Una pagina di documentazione o di aiuto. |

#### Ricerca

| `content_type` | Significato |
|---|---|
| `research_paper` | Articolo accademico o scientifico. |
| `dataset` | Dataset di ricerca. |

#### Portfolio

| `content_type` | Significato |
|---|---|
| `project` | Progetto di portfolio o case study. |

#### Media / Risorse

| `content_type` | Significato |
|---|---|
| `media_item` | Video, audio, galleria, ecc. |
| `resource` | Materiale scaricabile o di riferimento. |

## Array e campi multi-valore

Alcuni campi OLAMIP sono progettati per contenere più di un valore. Ogni volta che un campo contiene più elementi, come `tags`, o qualsiasi lista personalizzata definita nel campo opzionale `metadata` di un oggetto Entry, deve essere espresso come array JSON.

Gli array sono racchiusi tra parentesi quadre (`[ ]`) e contengono una sequenza di valori stringa separati da virgola. L’uso degli array garantisce che i sistemi AI possano interpretare in modo affidabile i dati multi-valore senza ambiguità. Ogni elemento dell’array deve essere una stringa autonoma e l’ordine degli elementi deve rimanere coerente quando porta significato semantico.

Il campo `metadata` viene utilizzato per memorizzare informazioni strutturate specifiche del dominio o della pagina che vanno oltre i campi core di OLAMIP. Questo campo consente agli editori di includere dettagli machine-interpretable rilevanti per il loro settore o tipo di contenuto, offrendo ai sistemi AI segnali contestuali più ricchi senza modificare il protocollo principale.

## Oggetto Metadata

Un tipico oggetto metadata può apparire così:

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

## Perché la priorità categoriale funziona meglio

| Vantaggio | Spiegazione |
|---|---|
| Chiarezza e coerenza | `"high"/"medium"/"low"` è universalmente interpretabile. |
| Più semplice per gli editori | Nessun punteggio numerico necessario. |
| Più facile da validare | Gli strumenti possono rilevare gli abusi. |
| Flessibile per pipeline LLM | I modelli possono mappare internamente le categorie in pesi. |

### Linee guida per il campo Priority

| Valore | Significato |
|---|---|
| `"high"` | Contenuto di punta, mission-critical. Usare con parsimonia. |
| `"medium"` | Predefinito per la maggior parte dei contenuti. |
| `"low"` | Contenuto di nicchia, obsoleto o di scarso valore. |

### Best practice

| Raccomandazione | Motivo |
|---|---|
| Limita `"high"` al 5–10% | Mantiene significativo il segnale. |
| Usa `"medium"` come default | Garantisce coerenza. |
| Usa `"low"` per contenuti di nicchia/legacy | Riduce il rumore. |
| Rivedi le priorità regolarmente | Mantiene il file accurato. |

### Perché è importante

Gli LLM possono usare `priority` per:

- Allocare più attenzione durante il training.
- Ordinare le pagine nelle attività di retrieval.
- Filtrare i contenuti meno rilevanti.

Se ogni pagina è contrassegnata come `"high"`, il segnale perde significato e i contenuti più preziosi si perdono nel rumore.

## Supporto multilingue

Per supportare pienamente siti web multilingue, definisci `language` a:

- Livello file, come default globale, dentro `metadata`.
- Livello section, come override opzionale.
- Livello entry, come override opzionale.

Questo è essenziale per:

- Blog multilingue.
- Siti di notizie internazionali.
- Siti di ricerca con articoli in più lingue.
- Store e-commerce con pagine prodotto localizzate.

## Formato lingua

Usa i codici lingua BCP-47, lo standard globale usato da:

- schema.org.
- L’attributo HTML `lang`.
- W3C.
- Motori di ricerca.
- Principali pipeline LLM.

### Esempi

| Lingua | Codice |
|---|---|
| English | `en` |
| Spanish | `es` |
| French | `fr` |
| German | `de` |
| Portuguese (Brazil) | `pt-BR` |
| Chinese (Simplified) | `zh-CN` |
| Arabic | `ar` |

## Perché è importante per i sistemi AI

Gli LLM usano i metadati linguistici per:

- Scegliere il tokenizer corretto.
- Applicare il modello di summarization appropriato.
- Evitare di mescolare lingue negli embedding.
- Migliorare l’accuratezza del retrieval.
- Ridurre le allucinazioni nei contesti multilingue.
- Supportare ricerca e traduzione cross-lingua.

Senza campi lingua espliciti, i sistemi AI devono indovinare, e spesso sbagliano.

## Regole generali di validazione

| Regola | Requisito |
|---|---|
| JSON valido | Nessuna virgola finale o struttura malformata. |
| Campi obbligatori | Section ed entry devono includere i campi richiesti. |
| URL canonici | Devono essere assoluti e stabili. |
| Lunghezza summary | Inferiore a 500 caratteri. |
| Tags | Stringhe in minuscolo, di una sola parola. |
| Subsections | Devono seguire lo schema Section. |

## Scopo dei tag

I tag forniscono segnali compatti e machine-interpretable che aiutano i sistemi AI a comprendere le relazioni tematiche all’interno del sito. Supportano clustering, retrieval e allineamento semantico, ma solo se seguono un formato prevedibile e uniforme.

I tag forniscono ai sistemi AI indizi semantici leggeri sui temi, sugli argomenti o sulle categorie associate a un entry. Non sostituiscono i summary o la gerarchia delle section; invece, fungono da segnali supplementari che aiutano i modelli a:

- Raggruppare contenuti correlati.
- Inferire domini tematici.
- Migliorare l’accuratezza del retrieval.
- Ridurre l’ambiguità tra pagine con titoli simili.
- Supportare il ragionamento cross-page.

I tag sono opzionali ma fortemente raccomandati per siti con grandi inventari di contenuti o argomenti sovrapposti.

## Formato obbligatorio dei tag

I tag devono sempre seguire un rigoroso pattern di normalizzazione per garantire coerenza tra editori e interpretazione prevedibile da parte dei sistemi AI.

- Minuscolo.
- Stringhe di una sola parola.
- Solo caratteri ASCII.
- Nessuno spazio.
- Nessuna punteggiatura eccetto i trattini.
- Niente camelCase o snake_case.

Questa normalizzazione assicura che i tag rimangano token non ambigui, facilmente indicizzabili, embeddabili e confrontabili.

## Tag multi-parola

Alcuni concetti sono naturalmente composti da più parole. Per preservare il significato mantenendo il requisito di una sola parola, i tag multi-parola devono essere convertiti in forme con trattini.

### Regole di formattazione

- Converti tutte le lettere in minuscolo.
- Sostituisci gli spazi con trattini.
- Rimuovi la punteggiatura salvo che faccia parte di un termine standard.
- Evita underscore, camelCase o concatenazione.
- Mantieni i tag concisi e semanticamente significativi.

### Esempi

| Concetto | Tag valido | Tag non validi |
|---|---|---|
| Los Angeles | `los-angeles` | `Los Angeles`, `los_angeles`, `losangeles` |
| Machine Learning | `machine-learning` | `MachineLearning`, `machine_learning` |
| Data Science | `data-science` | `data science`, `datascience` |
| Customer Support | `customer-support` | `customerSupport`, `customer support` |

La hyphenation preserva i confini semantici mantenendo i tag machine-friendly e leggibili.

## Considerazioni multilingue per i tag

I metadati di lingua determinano come i tag devono essere interpretati. I tag stessi non devono essere tradotti, ma devono rimanere coerenti con il contesto linguistico dell’entry.

### Regole per siti multilingue

- I tag devono essere scritti nella stessa lingua dell’entry, salvo che il concetto sia globalmente standardizzato (ad esempio `javascript`).
- Se lo stesso concetto appare in più lingue, ogni versione linguistica deve usare il proprio tag localizzato.
- Le regole di hyphenation rimangono le stesse in tutte le lingue.
- Evita di mescolare lingue in un singolo tag.
- Usa i codici lingua BCP-47 a livello di file, section o entry per chiarire l’interpretazione.

### Esempi

| Lingua dell’entry | Concetto | Tag valido |
|---|---|---|
| `en` | climate change | `climate-change` |
| `es` | cambio climático | `cambio-climatico` |
| `de` | künstliche intelligenz | `kunstliche-intelligenz` |

I tag rimangono token normalizzati, ma la loro forma linguistica dovrebbe corrispondere al contenuto che descrivono.

## Interazione dei tag con i sistemi AI

I tag aiutano i sistemi AI a:

- Raggruppare entry correlate.
- Disambiguare argomenti simili.
- Migliorare la precisione del retrieval.
- Supportare il mapping cross-lingua.
- Ridurre il rischio di allucinazioni rafforzando il contesto.

I tag non sono segnali di ranking, ma migliorano l’interpretabilità e riducono l’ambiguità, soprattutto in siti grandi o multilingue.

## Versioning del file OLAMIP

| Linea guida | Scopo |
|---|---|
| I parser ignorano i campi sconosciuti. | Garantisce compatibilità futura. |
| Gli editori validano contro lo schema più recente. | Garantisce correttezza. |

## Allineamento semantico

OLAMIP completa HTML, sitemap e altri metadati strutturati fornendo un livello semantico più chiaro e più adatto all’AI. Se HTML è la presentazione, OLAMIP è l’interpretazione curatoriale del tuo sito web.

## Esempio di struttura del file

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": {
    "name": "Sito Esempio",
    "type": "blog",
    "canonical_description": "Sito di esempio per la dimostrazione di OLAMIP.",
    "tags": ["technology", "documentation"]
  },
  "content": {
    "overview": {
      "summary": "Questo sito contiene documentazione, articoli e risorse relative alla tecnologia."
    },
    "sections": [
      {
        "title": "Documentazione",
        "summary": "Guide tecniche e riferimenti.",
        "url": "https://yourdomain.com/docs/",
        "section_type": "doc_category",
        "policy": "allow",
        "tags": ["docs", "reference"],
        "priority": "high",
        "published": "2026-01-21",
        "language": "it",
        "entries": [
          {
            "title": "Inizio Rapido",
            "summary": "Guida breve per iniziare.",
            "url": "https://yourdomain.com/docs/getting-started",
            "content_type": "doc_page",
            "policy": "allow",
            "tags": ["getting-started", "guide"],
            "priority": "high",
            "published": "2026-01-21",
            "language": "it",
            "metadata": {
              "reading_time": "5 min"
            }
          }
        ]
      }
    ]
  },
  "metadata": {
    "last_updated": "2026-01-21",
    "language": "it",
    "source_url": "https://yourdomain.com/olamip.json",
    "copyright": "© 2026 Example"
  }
}
```

## Note di implementazione

- Tutti gli URL devono essere assoluti.
- Tutte le stringhe devono essere UTF-8.
- Evita campi ambigui o eccessivi.
- Usa summary brevi e accurati.
- Mantieni coerenza nei nomi in tutto il file.

## Conclusione

OLAMIP offre un modo chiaro, strutturato e interpretabile dalle macchine per descrivere i contenuti web. Seguendo questa specifica, i webmaster consentono ai sistemi AI di comprendere la gerarchia del sito, prioritizzare le pagine importanti, rispettare le policy di ingestione e lavorare con maggiore accuratezza, trasparenza e affidabilità.