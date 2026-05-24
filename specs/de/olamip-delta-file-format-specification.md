# OLAMIP-DELTA-Dateiformatspezifikation

*Urheberrecht © 2025 Ralph Gonzalez – https://olamip.org*

Das **OLAMIP-DELTA**-Protokoll definiert ein JSON-basiertes Delta-Dateiformat für inkrementelle Aktualisierungen von `olamip.json`. Diese Spezifikation richtet sich an Webmaster und Implementierer, die mit strukturierten Daten, HTML und sitemap-ähnlicher Veröffentlichung vertraut sind. 

## 1. Überblick

`olamip.json` ist die autoritative, vollständige Snapshot-Darstellung der Struktur und des Inhalts Ihrer Website. `olamip-delta.json` ist eine optionale Begleitdatei, die die Änderungen an diesem Snapshot seit der letzten Aktualisierung beschreibt. 

Zusammen bilden sie einen maschineninterpretierbaren Änderungsstrom:

- `olamip.json`: vollständiger, strukturierter Snapshot.
- `olamip-delta.json`: inkrementelles Änderungsprotokoll.

---

## 2. Kernkonzepte

- **Entry** — ein Inhaltselement auf Blattebene, z. B. Blogartikel, Produktseite oder Dokumentationsseite.
- **Section / Subsection** — eine hierarchische Gruppierung, z. B. Blog-Kategorie, Doc-Kategorie, Produktkollektion oder Projektgruppe.
- **Operation** — eine von `added`, `updated`, `removed`.
- **Delta** — ein datumsbezogener Änderungsblock mit `added`, `updated` und `removed`.

Dieses Format ist darauf ausgelegt,:

- **zuerst maschineninterpretierbar** zu sein, aber dennoch für manuelle Prüfung lesbar genug.
- **inkrementell** zu sein, damit KI-Systeme synchron bleiben können, ohne die gesamte Website neu zu verarbeiten. 

---

## 3. Dateipfad und Erkennung

### 3.1 Dateipfad

`olamip.json` muss im Stammverzeichnis Ihrer Domain gehostet werden:

```text
https://yourdomain.com/olamip.json
```

`olamip-delta.json` muss im selben Verzeichnis gehostet werden:

```text
https://yourdomain.com/olamip-delta.json
```

Versionierte Deltas (optional) liegen ebenfalls im selben Verzeichnis:

```text
/olamip.json
/olamip-delta-YYYY-MM-DD.json
```

### 3.2 Erkennungsmechanismus

Webmaster müssen den Speicherort von `olamip.json` mit beiden Tags im `<head>` der Website deklarieren:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

KI-Systeme, die `olamip.json` kennen, suchen automatisch im selben Verzeichnis nach `olamip-delta.json` und wenden die Deltas gemäß dieser Spezifikation an. Es ist nicht erforderlich, ein separates `<link>`- oder `<meta>`-Tag für `olamip-delta.json` hinzuzufügen. 

---

## 4. Top-Level-Struktur

Auf oberster Ebene ist `olamip-delta.json` ein JSON-Objekt mit den folgenden Feldern:

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

### 4.1 Erforderliche Top-Level-Felder

- `protocol` (string, erforderlich)  
  Muss `"OLAMIP-DELTA"` sein.  
  Dadurch wird die Delta-Datei von der Hauptdatei OLAMIP und von anderen Delta-Formaten unterschieden.

- `version` (string, erforderlich)  
  Muss dem dokumentierten Versionsschema entsprechen, z. B. `"1.0.0"`.  
  Dadurch können Implementierer Schemaänderungen erkennen.

- `window_days` (integer, optional)  
  Anzahl der Tage im rollierenden Fenster; muss positiv sein, typischerweise 7–30.  
  Nur erforderlich, wenn eine rollierende Delta-Datei verwendet wird.

- `last_updated` (string, erforderlich)  
  Datum der letzten Änderung dieser Delta-Datei im ISO-8601-Format `YYYY-MM-DD`.  
  Das hilft Crawlern zu entscheiden, wann erneut abgerufen werden soll.

- `deltas` (array, erforderlich)  
  Nicht leeres Array von datumsbezogenen Änderungsblöcken.  
  Jedes Element ist ein **Delta-Objekt** mit `date`, `added`, `updated` und `removed`.

### 4.2 `deltas`-Array und Datumsreihenfolge

- `deltas` muss nach `"date"` aufsteigend sortiert sein (älteste zuerst, neueste zuletzt).
- Jedes Delta-Objekt muss ein `"date"`-Feld im ISO-8601-Format `YYYY-MM-DD` enthalten.
- `deltas` darf nicht leer sein.

Diese Reihenfolge ist für die Konformität nicht optional: KI-Systeme nehmen eine chronologische Sortierung der Deltas an. 

---

## 5. Struktur des Delta-Objekts

Jedes Element in `deltas` ist ein Delta-Objekt mit der folgenden Form:

```json
{
  "date": "2026-04-27",
  "added": [ ... ],
  "updated": [ ... ],
  "removed": [ ... ]
}
```

Für maschineninterpretierbare Klarheit und Konsistenz **MÜSSEN** Webmaster in jedem Delta-Objekt alle drei Operationsfelder angeben, auch wenn einige davon leer sind.

Felder:

- `date` (string) — ISO-8601-Datum `YYYY-MM-DD`.
- `added` (array) — null oder mehr neu hinzugefügte Elemente.
- `updated` (array) — null oder mehr geänderte Elemente.
- `removed` (array) — null oder mehr gelöschte Elemente.

Beispiel eines Delta-Objekts nur mit Hinzufügungen:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Neue Seite",
      "summary": "Eine neue Seite wurde der Website hinzugefügt.",
      "url": "https://example.com/new-page/"
    }
  ],
  "updated": [],
  "removed": []
}
```

Dieses Muster ist erforderlich, auch wenn an einem Tag nur eine Operation zutrifft.

---

## 6. Operationen

### 6.1 `added`

Ein `added`-Element ist ein **vollständiges Objekt** des jeweiligen Typs (Entry, Section oder Subsection). Die Felder müssen mit der OLAMIP-Dateiformatspezifikation für diesen Typ übereinstimmen.

Für ein **Entry**:

- `title` (string, erforderlich) — menschenlesbarer Titel.
- `summary` (string, erforderlich) — kurze Beschreibung.
- `url` (string, erforderlich) — kanonische absolute URL.
- `content_type` (string, erforderlich) — aus der OLAMIP-`content_type`-Taxonomie.
- `tags` (array, optional) — Liste aus kleingeschriebenen, einwortigen Tags.
- `priority` (string, optional) — `"high"`, `"medium"` oder `"low"`.
- `policy` (string, optional) — `"allow"` oder `"forbid"`.
- `language` (string, optional) — BCP-47-Sprachcode.
- `metadata` (object, optional) — domänenspezifische strukturierte Daten.

Für eine **Section oder Subsection** folgen die `added`-Objekte der OLAMIP-Section-Spezifikation mit `title`, `summary`, `url`, `section_type`, `policy`, `tags`, `priority` usw.

#### Regeln für `added`

- `added`-Elemente müssen `url` enthalten.
- `added`-Elemente müssen vollständige Objekte sein, mindestens mit allen erforderlichen Feldern der Haupt-OLAMIP-Spezifikation.
- Keine `url` darf innerhalb desselben Delta-Objekts mehr als einmal vorkommen.

Beispiel:

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Neuer Blogbeitrag",
      "summary": "Eine kurze Einführung in die neue Funktion.",
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

Ein `updated`-Element muss `url` enthalten und darf entweder sein:

- ein **partielles Objekt**, das nur geänderte Felder enthält, oder
- ein **vollständiges Objekt**, das alle Felder enthält.

Bei jedem `updated`-Element gelten fehlende Felder als **unverändert**.

#### Beispiel für partielle Aktualisierung

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "url": "https://example.com/post/to-update/",
      "summary": "Aktualisierte Zusammenfassung mit neuen Informationen.",
      "tags": ["feature", "update", "docs"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Beispiel für vollständigen Ersatz

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "title": "Aktualisierter Beitragstitel",
      "summary": "Aktualisierte Zusammenfassung mit neuen Informationen.",
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

#### Regeln für `updated`

- `updated`-Elemente müssen `url` enthalten.
- `updated` darf ein partielles Objekt oder ein vollständiges Objekt sein.
- `updated` darf die Identität eines Entries nicht durch Umleitung der `url` verändern; das ist eine Konfigurationsentscheidung, keine Delta-Datei-Frage.

### 6.3 `removed`

Ein `removed`-Element ist **minimal** und ausschließlich identitätsbezogen.

```json
{
  "url": "https://example.com/page/to-remove/"
}
```

#### Regeln für `removed`

- `removed`-Elemente müssen `url` enthalten und **dürfen keine weiteren Felder** enthalten.
- `url` muss eine absolute, kanonische URL sein.
- `removed` gilt für die angegebene URL und ihre strukturellen Nachkommen (Subsections und Entries), falls vorhanden.
- Keine `url` darf innerhalb desselben Delta-Objekts mehr als einmal vorkommen.

Beispiel:

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

## 7. URL und Objektidentität

OLAMIP-DELTA verwendet kanonische URLs als primäre Identität für Entries, Sections und Subsections.

### Identitätsregeln

- `url` muss eine absolute URL mit Schema und Domain sein.
- Keine `url` darf innerhalb desselben Delta-Objekts mehr als einmal erscheinen, weder in Hinzufügen, Aktualisieren noch Entfernen.
- Wenn eine Section entfernt wird, gelten alle ihre Nachkommen als entfernt.
- Eine URL-Änderung erfordert **remove** gefolgt von **add**, nicht eine direkte Aktualisierung der `url`.

### Best Practices für URLs

- Verwenden Sie stabile, kanonische URLs, die sich möglichst selten ändern.
- Vermeiden Sie Abfrageparameter oder sessionspezifische Fragmente.
- Wenn eine Seite umzieht, behandeln Sie das als Redirect- und Kanonisierungsfrage, nicht als direkte Aufgabe der Delta-Datei.

---

## 8. Rollierendes Delta-Fenster (empfohlen)

Das **rollierende Delta-Fenster** ist der empfohlene Modus für `olamip-delta.json`.

In diesem Modell:

- Die Delta-Datei enthält Änderungen der letzten `window_days`.
- KI-Systeme lesen alle Deltas in der Datei und wenden sie in chronologischer Reihenfolge an.
- Dadurch werden verpasste Aktualisierungen vermieden, wenn Crawler verzögert abrufen. 

### Beispiel für ein rollierendes Delta (7-Tage-Fenster)

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
          "title": "Neues Produkt A",
          "url": "https://example.com/product/a",
          "summary": "Eine neue Ergänzung des Katalogs."
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
          "summary": "Beschreibung wurde um neue Funktionen ergänzt."
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

KI-Systeme:

- lesen alle Deltas in der Datei,
- wenden sie in chronologischer Reihenfolge an,
- und gelangen so zum neuesten Stand.

---

## 9. Versionierte Deltas (Alternative)

Einige Webmaster bevorzugen **versionierte Deltas**, bei denen jede Datei die Änderungen für einen einzelnen Tag enthält.

### Beispielstruktur

```text
/olamip.json
/olamip-delta-2026-03-01.json
/olamip-delta-2026-03-02.json
/olamip-delta-2026-03-03.json
```

### Dateiformat

Jede versionierte Datei hat dieselbe logische Struktur wie eine rollierende Datei, repräsentiert aber nur ein Änderungsdatum:

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "date": "2026-03-02",
  "added": [
    {
      "title": "Neuer Blogbeitrag",
      "url": "https://example.com/blog/new-post/",
      "summary": "Kurzer Überblick über die Aktualisierung."
    }
  ],
  "updated": [],
  "removed": []
}
```

### Ingest-Regeln

KI-Systeme sollten:

- alle Dateien `olamip-delta-*.json` im selben Verzeichnis wie `olamip.json` erkennen,
- sie nach `date` aufsteigend sortieren,
- und nur diejenigen anwenden, die innerhalb des konfigurierten Fensters liegen, z. B. der letzten 7–30 Tage.

Dieser Ansatz ist besonders nützlich für:

- Nachrichtenseiten mit häufigen täglichen Aktualisierungen,
- umfangreiche E-Commerce-Kataloge,
- Dokumentationsplattformen mit schnellen Release-Zyklen.

---

## 10. Strukturelle Änderungen: Sections und Subsections

OLAMIP-DELTA wendet dasselbe Delta-Muster auf **Sections** und **Subsections** an wie auf **Entries**. So verstehen KI-Systeme nicht nur, welche Seiten existieren, sondern auch, wie sie organisiert sind.

### 10.1 Section oder Subsection hinzufügen

Beim Hinzufügen einer Section oder Subsection fügen Sie ein **vollständiges Section-Objekt** in `added` ein.

Beispiel:

```json
{
  "date": "2026-05-01",
  "added": [
    {
      "title": "Tutorials",
      "summary": "Schritt-für-Schritt-Anleitungen für Einsteiger und Fortgeschrittene.",
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

#### Regeln

- `added`-Objekte für Sections müssen `title`, `summary`, `url` und `section_type` enthalten.
- Verwenden Sie `section_type`, um die semantische Rolle widerzuspiegeln, z. B. `blog_category`, `doc_category`, `product_collection` oder `research_category`.
- `priority` und `policy` helfen KI-Systemen, Bedeutung und Ingest-Regeln für den gesamten Unterbaum zu verstehen.

### 10.2 Section oder Subsection aktualisieren

Fügen Sie `url` und alle geänderten Felder hinzu.

Beispiel:

```json
{
  "date": "2026-05-02",
  "updated": [
    {
      "url": "https://example.com/blog/tutorials/",
      "title": "Schritt-für-Schritt-Tutorials",
      "summary": "Anleitungen für Einsteiger und Fortgeschrittene mit Codebeispielen und Best Practices.",
      "tags": ["tutorials", "beginner", "advanced", "code-examples"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Regeln

- Aktualisierte Sections müssen `url` enthalten.
- `updated` darf jede Teilmenge der übrigen Felder enthalten.
- Fehlende Felder gelten als unverändert.

### 10.3 Section oder Subsection entfernen

Fügen Sie in `removed` nur `url` ein.

Beispiel:

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

#### Regeln

- `removed`-Elemente für Sections müssen nur `url` enthalten und dürfen keine weiteren Felder haben.
- Die URL muss die kanonische URL der entfernten Section oder Subsection sein.
- Wenn eine Section entfernt wird, gelten alle ihre Nachkommen ebenfalls als entfernt.

---

## 11. Konformität

Implementierer und Webmaster sollten die folgenden Punkte als Konformitätsregeln betrachten.

### 11.1 MUST-Regeln

Eine konforme OLAMIP-DELTA-Datei **MUSS**:

- `protocol: "OLAMIP-DELTA"` deklarieren.
- `version` auf einen Wert gemäß dem dokumentierten Versionsschema setzen.
- Ein nicht leeres Array `deltas` enthalten.
- Jedes Delta-Objekt mit einem `date` im ISO-8601-Format `YYYY-MM-DD` versehen.
- `deltas` aufsteigend nach `date` sortieren.
- In jedem Delta-Objekt alle drei Operationsfelder (`added`, `updated`, `removed`) angeben, auch wenn sie leer sind.
- Sicherstellen, dass `url` in allen Operationen eine absolute, kanonische URL ist.
- Sicherstellen, dass keine `url` innerhalb desselben Delta-Objekts mehr als einmal vorkommt.
- Sicherstellen, dass `added` und `updated` `url` enthalten.
- Sicherstellen, dass `removed` nur `url` und keine weiteren Felder enthält.
- Fehlende Felder in `updated` als unverändert behandeln.
- `removed` als Entfernen der angegebenen URL und ihrer Nachkommen aus dem Index behandeln. 

### 11.2 SHOULD-Regeln

Eine konforme Implementierung **SOLLTE**:

- standardmäßig das **rollierende Delta-Fenster** verwenden, sofern der Veröffentlichungsworkflow nicht besser zu versionierten Deltas passt,
- `last_updated` aktuell halten, damit Crawler und Validatoren wissen, wann die Datei neu ist,
- stabile, kanonische URLs verwenden, die sich selten ändern, um Mehrdeutigkeiten zu vermeiden,
- `window_days` typischerweise zwischen 7 und 30 Tagen halten, abhängig von der Veröffentlichungsfrequenz der Website,
- bei versionierten Deltas genügend Dateien innerhalb des Fensters aufbewahren, z. B. die letzten 7–30 Tage, damit bei verzögerten Crawlern keine Aktualisierungen verloren gehen.

### 11.3 MAY-Regeln

Eine konforme Implementierung **DARF**:

- statt einer rollierenden Datei das versionierte Delta-Format (`olamip-delta-YYYY-MM-DD.json`) verwenden, wenn das besser zur Bereitstellung passt,
- `olamip-delta.json` häufiger oder seltener als `olamip.json` veröffentlichen, sofern die Beziehung zwischen Snapshot und Delta klar und konsistent bleibt,
- diagnostische oder Logging-Metadaten unter einem separaten Schlüssel außerhalb der Protokollfelder hinzufügen, z. B. ein `diagnostics`-Objekt, sofern dies die Pflichtfelder nicht widerspricht oder die Bedeutung des Deltas verändert.

---

## 12. Best Practices

Um langfristige Kompatibilität mit KI-Systemen sicherzustellen, sollten Webmaster beim Erzeugen von `olamip-delta.json` die folgenden Best Practices beachten.

### 12.1 Delta-Erzeugung automatisieren

- Verknüpfen Sie die Delta-Erzeugung mit Ihrem CMS oder Deployment-Pipeline.
- Aktualisieren Sie `olamip-delta.json` jedes Mal, wenn Inhalte veröffentlicht, geändert oder entfernt werden.
- Validieren Sie das Schema vor dem Deployment, um Syntaxfehler oder ungültige Felder zu vermeiden.

### 12.2 `olamip.json` autoritativ halten

- Bewahren Sie eine gültige und aktuelle `olamip.json`-Datei auf.
- Neue KI-Systeme können zunächst von der vollständigen Datei bootstrappen und danach Deltas anwenden.
- Behandeln Sie `olamip-delta.json` nicht als Ersatz für den Haupt-Snapshot.

### 12.3 Stabile kanonische URLs verwenden

- Verwenden Sie absolute URLs mit Schema und Domain.
- Vermeiden Sie URL-Muster, die sich häufig ändern, es sei denn, das Webseitensystem, z. B. Redirects, ist robust genug, um die kanonische Bedeutung zu erhalten.
- Wenn sich die URL einer Seite wirklich ändert, behandeln Sie das als „remove-then-add“ und nicht als direkte `url`-Aktualisierung im Delta.

### 12.4 Zusammenfassungen und Metadaten konsistent halten

- Zusammenfassungen in `added`- und `updated`-Elementen sollten mit dem tatsächlichen Inhalt übereinstimmen.
- Kleine sprachliche Änderungen ohne Bedeutungsänderung können zwar als Delta veröffentlicht werden, sollten aber minimiert werden, um unnötige Änderungen zu vermeiden.
- Tags sollten normalisiert sein: kleingeschrieben, einwortig und bei Mehrwortbegriffen mit Bindestrich verbunden; sie sollten stabile Konzepte widerspiegeln. 

---

## 13. Verwandte Ressourcen

Für ein vollständiges Verständnis des OLAMIP-Ökosystems sollten Webmaster außerdem Folgendes lesen:

- **[OLAMIP-Dateiformatspezifikation](https://olamip.org/file-format-specification/)**  
  Erklärt die Struktur von `olamip.json` und definiert die Felder für Entries, Sections und Subsections, auf denen das Delta-Format aufbaut.

- **[Häufig gestellte Fragen zu OLAMIP](https://olamip.org/frequently-asked-questions/)**  
  Erläutert Fragen zu Bereitstellung, Auffindbarkeit und Betrieb für Webmaster.

- **[OLAMIP-DELTA-Dateiformatspezifikation](https://olamip.org/delta-updates/)**  
  Offizielles JSON-Schema für `olamip-delta.json`, das zur automatischen Validierung und zum Linting von Delta-Dateien verwendet werden kann.

- **[Warum OLAMIP ein besserer Standard als LLMs.txt ist](https://olamip.org/why-olamip-is-a-superior-standard-to-llms-txt/)**  
  Erklärt die Vorteile von OLAMIP und OLAMIP-DELTA gegenüber einfacheren, unstrukturierten Metadatenformaten.

---

Dies schließt die OLAMIP-DELTA-Dateiformatspezifikation ab. Webmaster und Implementierer können dieses Dokument als Referenz verwenden, um `olamip-delta.json` in Produktionsumgebungen zu erzeugen, zu validieren und zu konsumieren.