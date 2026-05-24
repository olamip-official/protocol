# Domande frequenti (FAQ)

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Che cos’è OLAMIP?

OLAMIP è l’acronimo di **Open Language‑Aligned Machine‑Interpretable Protocol**.  
È un formato semplice e aperto che consente ai siti web di pubblicare riassunti strutturati dei propri contenuti, in modo che i Large Language Models (LLM) possano comprenderli e apprendere da essi in modo più efficace.

## Perché dovrei usare OLAMIP?

Perché gli LLM già analizzano il tuo sito, ma spesso faticano a interpretarlo correttamente. Con OLAMIP puoi:

- Insegnare all’IA esattamente di cosa parlano le tue pagine.  
- Evidenziare i contenuti più preziosi.  
- Migliorare la visibilità nei motori di ricerca e negli strumenti basati sull’IA.  
- Ridurre fraintendimenti e allucinazioni.

È la differenza tra un’IA che *indovina* e un’IA che *capisce*.

## OLAMIP rivela informazioni private?

No. OLAMIP riassume solo contenuti già pubblici sul tuo sito.  
Non rivela nulla che non sia già visibile nel tuo HTML, nei metadati o nel markup SEO.

## I concorrenti possono usare il mio file OLAMIP?

Non più di quanto possano già usare le tue pagine pubbliche, i tuoi metadati o la tua sitemap.  
In realtà, OLAMIP ti offre **maggiore controllo** su come i sistemi di IA interpretano i tuoi contenuti — non meno.

## Come appare un file `olamip.json`?

Un file `olamip.json` è un documento strutturato in formato JSON che descrive i contenuti, la gerarchia e i metadati del tuo sito in un modo facilmente interpretabile dai sistemi di IA.  
Per esplorare la struttura tecnica completa, visita la pagina della [specifica del formato](https://olamip.org/file-format-specification) oppure consulta un esempio reale nella [case study di TimeLAX.com](https://olamip.org/how-olamip-transforms-ai-discovery-a-timelax-com-use-case/).

## Dove devo ospitare il file?

Nella root del tuo dominio:

```
https://yourdomain.com/olamip.json
```

Puoi anche aggiungere tag di discovery nella tua homepage:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## A cosa serve il campo “priority”?

Indica quanto è importante una pagina per gli LLM. Usalo con moderazione:

- `high` → contenuti fondamentali e strategici.  
- `medium` → valore predefinito per la maggior parte delle pagine.  
- `low` → contenuti di nicchia, datati o di scarso valore.

Se tutto è “high”, allora niente lo è davvero.

## Perché devo includere gli URL se fornisco già i riassunti?

I riassunti dicono all’IA *cosa significa* una pagina, ma gli URL dicono *qual è* la pagina.  
Un riassunto è descrittivo, ma non è un identificatore univoco.  
L’URL è l’unico riferimento canonico e stabile alla pagina reale.

I sistemi di IA usano gli URL per:

- Recuperare e verificare il contenuto completo.  
- Evitare duplicati.  
- Rilevare spostamenti o modifiche delle pagine.  
- Collegare i dati OLAMIP al markup schema.org, alle sitemap e ai crawler.  
- Citare correttamente la pagina nelle risposte generate.

Senza URL, due pagine diverse con riassunti simili potrebbero sembrare identiche.

## A cosa serve il campo “policy” e devo impostarlo?

Il campo `policy` controlla se i sistemi di IA possono ingerire una sezione, una sottosezione o una voce.  
Valori validi: `"allow"` e `"forbid"`.

Se ometti il campo, OLAMIP eredita automaticamente la policy dal nodo padre più vicino.  
Se nessun padre definisce una policy, il valore predefinito è `"allow"`.

La maggior parte dei siti può semplicemente omettere questo campo.  
Usa `"forbid"` solo se vuoi escludere esplicitamente alcune pagine o sezioni.

## Qual è la differenza tra OLAMIP e una sitemap tradizionale?

Una sitemap è un indice di navigazione, mentre OLAMIP è **una mappa di conoscenza interpretabile dalle macchine**.

### Sitemap XML

- Elenca gli URL.  
- Fornisce indicazioni di crawling.  
- Aiuta i motori di ricerca a scoprire le pagine.  
- Non ha significato semantico.  
- Non definisce una vera gerarchia di contenuti.

### OLAMIP

- Definisce tipi di contenuto (`page`, `project`, `doc_page`, ecc.).  
- Definisce tipi di sezioni (`project_group`, `doc_category`, ecc.).  
- Fornisce descrizioni canoniche.  
- Fornisce riassunti ottimizzati per gli LLM.  
- Fornisce tag e metadati.  
- Stabilisce una gerarchia esplicita.  
- Offre ai sistemi di IA una rappresentazione strutturata dell’intero sito.

In altre parole:

- **Sitemap XML = “Ecco le mie pagine.”**  
- **OLAMIP = “Ecco cosa significa il mio sito.”**

Ecco perché i sistemi di IA possono usare OLAMIP molto più efficacemente di una sitemap tradizionale.

## Un file OLAMIP con più di 3.000 URL è troppo grande?

Assolutamente no.  
Un file con 3.000 voci rientra perfettamente nei limiti normali.  
Anche con riassunti lunghi, la dimensione totale sarebbe circa 6 MB — trascurabile per server, browser e sistemi di IA moderni.

## Qual è la dimensione tipica di una voce OLAMIP?

| Lunghezza del riassunto | Dimensione approssimativa |
|---|---|
| Breve | 300–600 byte |
| Lungo | 1–2 KB |

## Cosa rende OLAMIP scalabile nonostante le dimensioni dei file?

- Struttura gerarchica che riduce la ripetizione.  
- Riassunti concisi (di solito meno di 500 caratteri).  
- Compressione efficiente (Gzip o Brotli riduce del 70–90%).

## Quanto può diventare grande un file OLAMIP prima che sorgano problemi di prestazioni?

| Dimensione file | URL approx. | Impatto |
|---|---:|---|
| 1–10 MB | 500–5.000 | Perfettamente normale |
| 10–25 MB | 5.000–12.000 | Leggero rallentamento |
| 25–50 MB | 12.000–25.000 | Grande ma gestibile |
| 50–100 MB | 25.000–50.000 | Pesante; alcuni sistemi possono rallentare |
| 100+ MB | 50.000+ | Troppo grande per molte pipeline LLM |

La maggior parte dei webmaster può gestire file OLAMIP con decine di migliaia di voci senza problemi.

## Ogni quanto dovrei aggiornarlo?

Ogni volta che pubblichi nuovi contenuti o modifichi pagine esistenti.  
Trattalo come una sitemap — ma per l’IA.

## Devo aggiornare il file principale se uso `olamip-delta.json`?

Sì.  
Il file principale `olamip.json` deve sempre riflettere lo stato attuale del tuo sito.  
Il file opzionale `olamip-delta.json` contiene solo le modifiche recenti.

## Devo aggiungere un tag `<link>` per `olamip-delta.json`?

No.  
Solo il file principale `olamip.json` deve essere referenziato nella sezione `<head>` del sito.  
Una volta individuato, i sistemi di IA cercheranno automaticamente il file delta nella stessa directory.

## Gli LLM leggeranno davvero questo file?

Sì — questo è l’obiettivo.  
Con l’aumentare dell’adozione, gli LLM daranno priorità a fonti strutturate come OLAMIP.

## Come posso testare se un bot di IA può recuperare il mio `olamip.json`?

Prova questi comandi per OpenAI, Anthropic e Perplexity:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```
