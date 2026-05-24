# OLAMIP Dateiformat‑Spezifikation  
*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Überblick

Die OLAMIP‑Datei (`/olamip.json`) ist ein strukturiertes JSON‑Dokument, das kuratierte Zusammenfassungen der wichtigsten Seiten einer Website bereitstellt. Sie wurde entwickelt, um von großen Sprachmodellen (LLMs) leicht verarbeitet zu werden, sodass diese den Inhalt einer Website klar, präzise und zielgerichtet verstehen, priorisieren und nutzen können.

## Speicherort der Datei

Die OLAMIP‑Datei muss im Stammverzeichnis Ihrer Domain liegen:

`https://ihredomain.com/olamip.json`

## Deklaration des Speicherorts der OLAMIP‑Datei

Um die Auffindbarkeit zu maximieren, sollten sowohl ein `<link>`‑Tag als auch ein `<meta>`‑Tag im `<head>`‑Bereich Ihrer Website eingebunden werden.

### Primäre Erkennung: `<link rel="olamip">`

- Standardisierte Praxis: Crawler durchsuchen `<link>`‑Tags bereits für Ressourcen wie canonical, sitemap oder alternate.  
- Maschinenfreundlich: Deklariert eine formale Beziehung zwischen der Seite und der OLAMIP‑Datei.  
- Interoperabel: Fügt sich nahtlos in bestehende Webstandards ein.

### Fallback‑Erkennung: `<meta name="olamip-location">`

- Menschlich lesbar: Einfach zu verstehen und einzubinden.  
- Kompatibilität: Einige Tools bevorzugen `<meta>`‑Tags.  
- Redundanz: Dient als Backup, falls ein Crawler `rel="olamip"` noch nicht unterstützt.

### Warum beide Methoden sinnvoll sind

- Zukunftssicherheit: Unterschiedliche Systeme können unterschiedliche Erkennungswege implementieren.  
- Ausfallsicherheit: Wenn eine Methode fehlschlägt, greift die andere.  
- Klarheit: Reduziert Mehrdeutigkeit über den Speicherort der OLAMIP‑Datei.

### Empfohlene Implementierung

```html
<link rel="olamip" href="https://ihredomain.com/olamip.json">
<meta name="olamip-location" content="https://ihredomain.com/olamip.json">
```

---

## Struktur der Datei

Eine gültige OLAMIP‑Datei enthält:

- `protocol`  
- `version`  
- `identity`  
- `content`  
- `metadata`  

### Grundstruktur

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": { "...": "..." },
  "content": { "...": "..." },
  "metadata": { "...": "..." }
}
```

---

## Identity‑Objekt

| Feld | Typ | Pflicht | Beschreibung |
|---|---|---:|---|
| `name` | `string` | Ja | Name der Website oder Organisation. |
| `type` | `string` | Ja | Entitätstyp, z. B. `company`, `blog`, `ecommerce`. |
| `canonical_description` | `string` | Ja | Menschlich lesbare Beschreibung. |
| `tags` | `array<string>` | Nein | Schlüsselwörter zum Themenbereich. |

---

## Content‑Objekt

Das `content`‑Objekt enthält:

- ein `overview`  
- eine Liste von `sections`  
- jede Section kann `subsections` enthalten  
- jede Section oder Subsection kann `entries` enthalten  

### Overview‑Objekt

| Feld | Typ | Pflicht | Beschreibung |
|---|---|---:|---|
| `summary` | `string` | Ja | Kurzbeschreibung des Zwecks der Website. |

---

## Section‑Objekt

Eine Section ist eine thematische oder strukturelle Gruppe von Inhalten.

### Felder einer Section

| Feld | Typ | Pflicht | Beschreibung |
|---|---|---:|---|
| `title` | `string` | Ja | Titel der Section. |
| `summary` | `string` | Ja | Beschreibung des Inhalts. |
| `url` | `string` | Ja | Kanonische URL. |
| `section_type` | `string` | Ja | Semantische Klassifikation. |
| `policy` | `string` | Nein | `"allow"` oder `"forbid"`. |
| `tags` | `array<string>` | Nein | Schlüsselwörter. |
| `priority` | `string` | Nein | `"high"`, `"medium"`, `"low"`. |
| `published` | `string` | Nein | ISO‑8601‑Datum. |
| `language` | `string` | Nein | BCP‑47‑Sprachcode. |
| `entries` | `array<Entry>` | Ja | Liste der Einträge. |
| `subsections` | `array<Section>` | Nein | Untergeordnete Sections. |

### Zulässige Werte für `section_type`

| section_type | Bedeutung |
|---|---|
| `blog_category` | Blog‑Kategorie |
| `news_section` | Nachrichtenbereich |
| `product_collection` | Produkt‑ oder Servicegruppe |
| `doc_category` | Dokumentationsbereich |
| `research_category` | Forschungsinhalte |
| `project_group` | Projekte / Portfolio |
| `content_section` | Allgemeiner Bereich |

---

## Policy‑Verhalten und Vererbung

Die `policy` steuert, ob Inhalte von KI‑Systemen verarbeitet werden dürfen.

### Vererbungsregeln

1. Policy auf Entry‑Ebene  
2. Falls nicht vorhanden: Policy der Subsection  
3. Falls nicht vorhanden: Policy der Section  
4. Falls nirgends definiert: Standardwert `"allow"`

---

## Mehrstufige Hierarchie

```text
content
└── sections[]
    ├── Section (Ebene 1)
    │     ├── entries[]
    │     └── subsections[]
    │           ├── Section (Ebene 2)
    │           │     ├── entries[]
    │           │     └── subsections[]
    │           │           └── Section (Ebene 3)
    │           │                 └── entries[]
    │           └── ...
    └── ...
```

---

## Entry‑Objekt

Ein Entry ist die kleinste Inhaltseinheit.

### Felder eines Entry

| Feld | Typ | Pflicht | Beschreibung |
|---|---|---:|---|
| `title` | `string` | Ja | Titel. |
| `summary` | `string` | Ja | Kurzbeschreibung. |
| `url` | `string` | Ja | Absolute kanonische URL. |
| `content_type` | `string` | Ja | Semantischer Inhaltstyp. |
| `policy` | `string` | Nein | `"allow"` oder `"forbid"`. |
| `tags` | `array<string>` | Nein | Schlüsselwörter. |
| `priority` | `string` | Nein | `"high"`, `"medium"`, `"low"`. |
| `published` | `string` | Nein | ISO‑8601‑Datum. |
| `language` | `string` | Nein | BCP‑47‑Sprachcode. |
| `metadata` | `object` | Nein | Seitenspezifische Metadaten. |

### Warum URLs erforderlich sind

Sie dienen als kanonische Identifikatoren für:

- Deduplizierung  
- Validierung  
- Abgleich mit schema.org, Sitemaps und Crawlern  

---

## Zulässige Werte für `content_type`

(Seiten, Blog‑Artikel, Nachrichten, Produkte, Dokumentation, Forschung, Portfolio, Medien, Ressourcen usw.)

---

## Metadata‑Objekt

Beispiel:

```json
{
  "last_updated": "2026-01-21",
  "language": "de",
  "source_url": "https://www.ihrewebsite.com/",
  "copyright": "© 2026 Beispiel"
}
```

---

## Priorität: Warum Kategorien am besten funktionieren

- Klar und universell verständlich  
- Einfach zu validieren  
- Flexibel für LLM‑Pipelines  

### Werte

| Wert | Bedeutung |
|---|---|
| `"high"` | Kritischer Inhalt |
| `"medium"` | Standard |
| `"low"` | Nischen‑ oder veralteter Inhalt |

---

## Mehrsprachige Unterstützung

`language` kann definiert werden:

- auf Dateiebene (`metadata`)  
- auf Section‑Ebene  
- auf Entry‑Ebene  

Verwenden Sie BCP‑47‑Codes.

---

## Allgemeine Validierungsregeln

| Regel | Anforderung |
|---|---|
| Gültiges JSON | Keine überflüssigen Kommata. |
| Pflichtfelder | Müssen vorhanden sein. |
| Kanonische URLs | Absolut und stabil. |
| Zusammenfassungen | Unter 500 Zeichen. |
| Tags | Kleinschreibung, ein Wort. |

---

## Tag‑Format

- Kleinschreibung  
- Ein Wort  
- ASCII  
- Bindestriche für Mehrwortbegriffe  

Beispiele:

| Konzept | Gültiger Tag |
|---|---|
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

---

## OLAMIP‑Versionierung

- Parser ignorieren unbekannte Felder.  
- Publisher sollten gegen das neueste Schema validieren.

---

## Semantische Ausrichtung

OLAMIP ergänzt bestehende Webstandards durch eine klar strukturierte, KI‑optimierte Darstellung der wichtigsten Inhalte einer Website.

