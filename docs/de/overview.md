# OLAMIP Übersicht

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP ist ein offener Standard, der es Websites ermöglicht, klar und gezielt mit KI‑Systemen zu kommunizieren. Anstatt große Sprachmodelle (LLMs) zu zwingen, Bedeutungen aus unübersichtlichem HTML, CSS und JavaScript zu erraten, bietet OLAMIP eine saubere, strukturierte und maschineninterpretierbare Darstellung der wichtigsten Inhalte Ihrer Website.

Dieses Dokument bietet eine Einführung auf hoher Ebene in Zweck, Philosophie, Dateiformat und Aktualisierungsmodell von OLAMIP.

---

## 1. Warum OLAMIP entwickelt wurde

Moderne Websites sind für Browser gebaut, nicht für KI. Daher haben LLMs Schwierigkeiten mit:

- Layout‑lastigem HTML  
- JavaScript‑gerenderten Inhalten  
- Navigationsmenüs, Werbung und Boilerplate  
- Doppelten oder irrelevanten Seiten  

LLMs *crawlen Ihre Website bereits*, interpretieren sie jedoch häufig falsch. OLAMIP löst dieses Problem, indem es KI‑Systemen genau das liefert, was sie benötigen:

- Eine leichte JSON‑Datei (`/olamip.json`)  
- Saubere, manuell kuratierte Zusammenfassungen  
- Strukturierte Metadaten  
- Kanonische URLs  
- Eine klare Hierarchie aus Abschnitten, Unterabschnitten und Einträgen  

Durch die Einführung von OLAMIP übernehmen Sie die Kontrolle darüber, wie KI Ihre Inhalte versteht und darstellt.

---

## 2. Grundphilosophie: Mensch‑freundlich + Maschinen‑freundlich

OLAMIP ist als Kommunikationsbrücke zwischen Menschen und KI konzipiert. Jeder Teil des Protokolls muss sein:

| Prinzip | Mensch‑freundlich | Maschinen‑freundlich |
|---------|-------------------|-----------------------|
| Dateiformat | Einfach zu schreiben und zu validieren | Striktes, vorhersehbares JSON |
| Feldnamen | Intuitiv und selbsterklärend | Kleingeschrieben, schema‑konform |
| Zusammenfassungen | Klar, markenkonform | Prägnant, faktenbasiert, semantisch reich |
| Prioritätssystem | Einfache Labels (`"high"`, `"medium"`, `"low"`) | Zuordenbar zu Ranking‑Gewichten |
| Dokumentation | Klartext‑Guides | JSON‑Schemas, Validator‑Regeln |
| Tools | Generatoren, CMS‑Plugins | CLI‑Tools, Parser, Test‑Suites |

- **Für Menschen:** OLAMIP muss ohne tiefgehende technische Kenntnisse nutzbar sein.  
- **Für Maschinen:** OLAMIP muss strukturiert, eindeutig und vorhersehbar sein.

---

## 3. Die OLAMIP‑Datei (`/olamip.json`)

Eine OLAMIP‑Datei ist ein strukturiertes JSON‑Dokument, das unter folgender Adresse gehostet wird:

`https://yourdomain.com/olamip.json`

Sie enthält:

- `protocol` — muss `"OLAMIP"` sein  
- `version` — Protokollversion  
- `identity` — Informationen über die Website  
- `content` — die strukturierte Inhalts‑Hierarchie  
- `metadata` — Sprache, letzter Stand und globale Felder  

### 3.1 Discovery‑Tags

Damit KI‑Systeme Ihre OLAMIP‑Datei zuverlässig finden, fügen Sie Folgendes ein:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Beide Tags zusammen bieten Redundanz, Kompatibilität und Zukunftssicherheit.

---

## 4. Dateistruktur

### 4.1 Identity‑Objekt

Beschreibt die Website oder Organisation:

- `name` — erforderlich  
- `type` — erforderlich  
- `canonical_description` — erforderlich  
- `tags` — optional  

### 4.2 Content‑Objekt

Enthält:

- Eine Übersicht (`overview`)  
- `sections`  
- Optionale `subsections`  
- `entries` (feinste Inhaltseinheiten)  

Unterstützt unbegrenzte Verschachtelungstiefe.

### 4.3 Abschnitte (Sections)

Ein Abschnitt gruppiert verwandte Inhalte. Pflichtfelder:

- `title`  
- `summary`  
- `url`  
- `section_type`  

Optionale Felder:

- `policy` (`"allow"` oder `"forbid"`)  
- `tags`  
- `priority`  
- `published`  
- `language`  
- `subsections`  
- `entries`  

**Policy‑Vererbung:**  
Wenn nicht angegeben, wird die Policy vom übergeordneten Element geerbt. Standard ist `"allow"`.

### 4.4 Einträge (Entries)

Einträge repräsentieren einzelne Inhaltselemente wie:

- Blogartikel  
- Nachrichten  
- Produkte  
- Dokumentationsseiten  
- Forschungsarbeiten  
- Medieninhalte  

Pflichtfelder:

- `title`  
- `summary`  
- `url`  
- `content_type`  

Optionale Felder:

- `policy`, `tags`, `priority`, `published`, `language`, `metadata`  

URLs sind erforderlich, da sie Inhalte eindeutig identifizieren und KI‑Systemen das Überprüfen, Entduplizieren und Verknüpfen ermöglichen.

---

## 5. Metadaten und Sprachunterstützung

Das `metadata`‑Objekt kann Folgendes enthalten:

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### Mehrsprachige Unterstützung

Die Sprache kann definiert werden auf:

- Dateiebene  
- Abschnittsebene  
- Eintragsebene  

Verwenden Sie BCP‑47‑Codes wie `en`, `es`, `pt-BR`, `zh-CN`.

Dies hilft KI‑Systemen:

- Die richtigen Tokenizer zu wählen  
- Sprachvermischung zu vermeiden  
- Abrufgenauigkeit zu verbessern  
- Halluzinationen zu reduzieren  

---

## 6. Tags und Priorität

### 6.1 Tags

Tags liefern leichte semantische Hinweise. Sie müssen:

- kleingeschrieben  
- einwortig  
- ASCII  
- mit Bindestrich für Mehrwortbegriffe  
- konsistent  

Beispiele:

| Konzept | Gültiger Tag |
|---------|--------------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

### 6.2 Priorität

Werte:

- `high` — Kerninhalte  
- `medium` — Standard  
- `low` — Nischen‑ oder ältere Inhalte  

Verwenden Sie `high` sparsam (5–10 % der Einträge).

---

## 7. Delta‑Updates (`olamip-delta.json`)

Websites ändern sich ständig. OLAMIP unterstützt inkrementelle Updates über eine optionale Datei:

`/olamip-delta.json`

Diese enthält nur:

- Hinzugefügte Einträge  
- Aktualisierte Einträge  
- Entfernte URLs  

### 7.1 Warum Delta‑Updates wichtig sind

- KI‑Systeme bleiben aktuell, ohne die gesamte Datei neu zu verarbeiten.  
- Große Kataloge bleiben handhabbar.  
- Updates verbreiten sich schnell.  
- Keine Änderungen gehen verloren.  

### 7.2 Rollierendes Delta‑Fenster (empfohlen)

Alle Deltas der letzten 7–30 Tage in einer Datei speichern.  
KI‑Systeme wenden sie chronologisch an.

### 7.3 Versionierte Delta‑Dateien (Alternative)

Separate Dateien veröffentlichen:

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`  
...

Ideal für große Websites (News, E‑Commerce, Dokumentation).

### 7.4 Discovery

Nur die Hauptdatei benötigt `<link>`‑ und `<meta>`‑Tags.  
KI‑Systeme suchen automatisch im selben Verzeichnis nach Delta‑Dateien.

---

## 8. OLAMIP vs. traditionelle Sitemaps

- XML‑Sitemap: „Hier sind meine Seiten.“  
- OLAMIP: „Das bedeutet meine Website.“  

Sitemaps listen URLs.  
OLAMIP liefert:

- Zusammenfassungen  
- Inhaltstypen  
- Abschnittshierarchie  
- Tags  
- Metadaten  
- Prioritäten  
- Ingestionsrichtlinien  

Zusammen mit schema.org bietet OLAMIP KI‑Systemen ein vollständiges, kuratiertes Verständnis Ihrer Website.

---

## 9. Häufig gestellte Fragen (Kurzfassung)

### Enthüllt OLAMIP private Informationen?

Nein. Es fasst nur öffentlich zugängliche Inhalte zusammen.

### Können Wettbewerber meine Datei nutzen?

Nicht mehr als Ihre öffentlichen Seiten.

### Wie groß kann eine OLAMIP‑Datei sein?

1–10 MB ist typisch.  
Selbst 3.000+ Einträge sind normal und sicher.

### Wie oft sollte ich aktualisieren?

Immer wenn sich Inhalte ändern — behandeln Sie es wie eine Sitemap für KI.

### Muss die Hauptdatei bei Delta‑Updates aktualisiert werden?

Ja. `olamip.json` ist immer die maßgebliche Version.

### Lesen LLMs diese Datei tatsächlich?

Ja — das ist der Zweck des Protokolls.  
Sie können testen mit:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```

---

## 10. Zusammenfassung

OLAMIP verwandelt Ihre Website in eine maschineninterpretierbare Wissenskarte:

- `olamip.json` — vollständige, strukturierte Momentaufnahme  
- `olamip-delta.json` — inkrementelles Änderungsprotokoll  
- Abschnitte, Unterabschnitte, Einträge — klare Hierarchie  
- Zusammenfassungen, Tags, Prioritäten — menschlich kuratierte Bedeutung  
- Discovery‑Tags — zuverlässige Erkennung  
- Mehrsprachige Unterstützung — globale Einsatzbereitschaft  

Durch die Einführung von OLAMIP stellen Sie sicher, dass KI‑Systeme Ihre Inhalte so verstehen, wie Sie es beabsichtigen — präzise, konsistent und skalierbar.
