# OLAMIP – Schnellstart

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Dieser Schnellstart zeigt, wie man OLAMIP auf einer Website auf die einfachste Weise implementiert.

---

## 1. Erstelle die Datei `olamip.json`

Lege eine gültige `olamip.json`‑Datei im Stammverzeichnis deiner Website ab:

`https://yourdomain.com/olamip.json`

Beginne mit einer kleinen, sauberen Momentaufnahme deiner wichtigsten Seiten.

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

## 2. Füge Discovery‑Tags in dein HTML ein

Füge diese Tags in den `<head>`‑Bereich deiner Startseite und wichtiger Seiten ein:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

---

## 3. Halte Zusammenfassungen kurz und präzise

Schreibe kurze, sachliche Zusammenfassungen, die erklären, worum es auf der Seite geht und warum sie wichtig ist. Vermeide Marketing‑Sprache, doppelte Inhalte und vage Beschreibungen.

Gute Beispiele:

- „Ein Einsteigerleitfaden für Langzeitbelichtung in der Fotografie.“
- „Produktdetails und Preise für den Enterprise‑Tarif.“

Schlechte Beispiele:

- „Die beste Seite im ganzen Internet.“
- „Unglaublicher Inhalt, den du lieben wirst.“

---

## 4. Verwende die richtigen Content‑Typen

Wähle den spezifischsten `content_type`:

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

## 5. Organisiere Inhalte mit Sections

Nutze `sections` für Inhaltsgruppen und `entries` für einzelne Seiten. Falls nötig, füge `subsections` hinzu, um eine tiefere Struktur zu schaffen.

Beispiel:

- Section: Blog  
- Subsection: Tutorials  
- Entry: How to Use OLAMIP  

---

## 6. Setze Prioritäten bewusst

Verwende:

- `high` für deine wichtigsten Seiten  
- `medium` für normale Seiten  
- `low` für Nischen‑ oder ältere Inhalte  

Markiere nicht alles als `high`.

---

## 7. Steuere die KI‑Verarbeitung mit dem Policy‑Feld

Das Feld `policy` legt fest, ob KI‑Systeme eine Section, Subsection oder ein Entry verarbeiten dürfen.

- `"allow"` → Verarbeitung erlauben  
- `"forbid"` → Verarbeitung verhindern  

Wenn `policy` fehlt, wird sie vom übergeordneten Element geerbt.  
Wenn kein Vorfahre eine Policy definiert, gilt `"allow"` als Standard.

Beispiele:

**Eine ganze Section ausschließen:**

```json
{
  "title": "Deprecated Guides",
  "summary": "Outdated content, not for AI.",
  "url": "https://yourdomain.com/old-guides/",
  "section_type": "doc_category",
  "policy": "forbid"
}
```

**Eine einzelne Seite ausschließen:**

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

## 8. Füge Sprach‑Metadaten hinzu

Verwende BCP‑47‑Sprachcodes wie:

- `de`
- `en`
- `fr`
- `es`
- `pt-BR`
- `zh-CN`

Du kannst die Sprache auf Datei‑, Section‑ oder Entry‑Ebene festlegen.

---

## 9. Aktualisiere die Datei regelmäßig

Aktualisiere `olamip.json`, wenn du wichtige Seiten hinzufügst, änderst oder entfernst.

Wenn deine Website sich häufig ändert, pflege zusätzlich `olamip-delta.json` im selben Verzeichnis, damit KI‑Systeme zwischen vollständigen Aktualisierungen auf dem neuesten Stand bleiben.

---

## 10. Validieren vor der Veröffentlichung

Stelle sicher, dass:

- das JSON gültig ist  
- alle URLs absolut sind  
- Pflichtfelder vorhanden sind  
- Zusammenfassungen klar und kurz sind  
- Tags normalisiert und konsistent sind  
- die Datei aus dem Root‑Pfad ausgeliefert wird  

---

## Minimal‑Checkliste für die Implementierung

- [ ] `olamip.json` erstellen  
- [ ] Datei im Root der Website hosten  
- [ ] Discovery‑Tags im `<head>` einfügen  
- [ ] Sections und Entries für wichtige Inhalte hinzufügen  
- [ ] Metadaten und Sprache definieren  
- [ ] Zusammenfassungen kurz und korrekt halten  
- [ ] Datei bei Änderungen der Website aktualisieren  
- [ ] `olamip-delta.json` hinzufügen, falls inkrementelle Updates gewünscht sind  

---

## Empfohlener Startpunkt

Wenn du nicht weißt, wo du anfangen sollst, beginne mit OLAMIP für deine wichtigsten Seiten:

- Startseite  
- Blog‑Übersicht  
- Beste Artikel  
- Wichtige Produkt‑ oder Service‑Seiten  
- Zentrale Dokumentationsseiten  

Erweitere danach Schritt für Schritt.
