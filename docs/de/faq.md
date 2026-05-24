# Häufig gestellte Fragen (FAQ)

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Was ist OLAMIP?

OLAMIP steht für **Open Language‑Aligned Machine‑Interpretable Protocol**.  
Es ist ein einfaches, offenes Format, das es Websites ermöglicht, strukturierte Zusammenfassungen ihrer Inhalte zu veröffentlichen, damit große Sprachmodelle (LLMs) diese besser verstehen und daraus lernen können.

## Warum sollte ich OLAMIP verwenden?

Weil LLMs Ihre Website bereits crawlen, aber Schwierigkeiten haben, sie richtig zu interpretieren. Mit OLAMIP können Sie:

- KI genau beibringen, worum es auf Ihren Seiten geht.
- Ihre wertvollsten Inhalte hervorheben.
- Die Auffindbarkeit in KI‑gestützten Tools und Suchsystemen verbessern.
- Fehlinterpretationen und Halluzinationen reduzieren.

Es ist der Unterschied zwischen *Raten* und *Verstehen*.

## Gibt OLAMIP private Informationen preis?

Nein. OLAMIP fasst nur Inhalte zusammen, die bereits öffentlich auf Ihrer Website verfügbar sind.  
Es offenbart nichts, was nicht ohnehin in Ihrem HTML, Ihren Metadaten oder Ihrem SEO‑Markup sichtbar ist.

## Können Wettbewerber meine OLAMIP‑Datei nutzen?

Nicht mehr, als sie bereits Ihre öffentlichen Seiten, Metadaten oder Ihre Sitemap nutzen können.  
OLAMIP gibt Ihnen tatsächlich **mehr Kontrolle** darüber, wie KI‑Systeme Ihre Inhalte interpretieren — nicht weniger.

## Wie sieht eine `olamip.json`‑Datei aus?

Eine `olamip.json` ist ein strukturiertes, JSON‑formatiertes Dokument, das die Inhalte, die Hierarchie und die Metadaten Ihrer Website so beschreibt, dass KI‑Systeme sie leicht interpretieren können.  
Die vollständige technische Struktur finden Sie in der [Dateiformat‑Spezifikation](https://olamip.org/file-format-specification) oder anhand eines realen Beispiels in der [TimeLAX.com‑Fallstudie](https://olamip.org/how-olamip-transforms-ai-discovery-a-timelax-com-use-case/).

## Wo sollte die Datei gehostet werden?

Im Stammverzeichnis Ihrer Domain:

```
https://yourdomain.com/olamip.json
```

Sie können außerdem folgende Discovery‑Tags auf Ihrer Startseite hinzufügen:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## Was bewirkt das Feld „priority“?

Es signalisiert, wie wichtig eine Seite für LLMs ist. Verwenden Sie es sparsam und bewusst:

- `high` → zentrale, geschäftskritische Inhalte  
- `medium` → Standard für die meisten Seiten  
- `low` → Nischen‑, veraltete oder weniger wertvolle Inhalte  

Wenn alles „high“ ist, ist nichts „high“.

## Warum müssen URLs enthalten sein, wenn bereits Zusammenfassungen vorhanden sind?

Zusammenfassungen sagen der KI, *was* eine Seite bedeutet — aber die URL sagt ihr, *welche* Seite es ist.  
Eine Zusammenfassung ist beschreibend, aber kein eindeutiger Identifikator.  
Die URL ist der einzige stabile, kanonische Verweis auf die tatsächliche Seite.

KI‑Systeme verwenden URLs, um:

- den vollständigen Inhalt abzurufen und zu verifizieren,
- Duplikate zu vermeiden,
- Änderungen oder Verschiebungen von Seiten zu erkennen,
- OLAMIP‑Daten mit schema.org‑Markup, Sitemaps und Crawlern zu verbinden,
- die Seite in Antworten korrekt zu zitieren.

Ohne URLs wären zwei verschiedene Seiten mit ähnlichen Zusammenfassungen nicht unterscheidbar.

## Was bewirkt das Feld „policy“ und muss ich es setzen?

Das Feld `policy` steuert, ob KI‑Systeme einen Abschnitt, eine Unterkategorie oder einen Eintrag einlesen dürfen.  
Gültige Werte sind `"allow"` und `"forbid"`.

Wenn Sie das Feld weglassen, erbt OLAMIP die Richtlinie vom nächsthöheren Element.  
Wenn kein übergeordnetes Element eine Richtlinie definiert, lautet der Standardwert `"allow"`.

Die meisten Websites können dieses Feld einfach weglassen.  
Verwenden Sie `"forbid"` nur, wenn Sie bestimmte Bereiche bewusst von der KI‑Verarbeitung ausschließen möchten.

## Was ist der Unterschied zwischen OLAMIP und traditionellen Sitemaps?

Während eine Sitemap ein Navigationsindex ist, ist OLAMIP eine **maschineninterpretierbare Wissenskarte**.

### XML‑Sitemap

- Listet URLs auf  
- Gibt Crawling‑Hinweise  
- Hilft Suchmaschinen, Seiten zu entdecken  
- Hat keine semantische Bedeutung  
- Hat keine echte Inhaltsstruktur  

### OLAMIP

- Definiert Inhaltstypen (`page`, `project`, `doc_page` usw.)  
- Definiert Abschnittstypen (`project_group`, `doc_category` usw.)  
- Bietet kanonische Beschreibungen  
- Bietet LLM‑optimierte Zusammenfassungen  
- Bietet Tags und Metadaten  
- Stellt eine explizite Hierarchie her  
- Gibt KI‑Systemen eine strukturierte Darstellung der gesamten Website  

Kurz gesagt:

- **XML‑Sitemap = „Hier sind meine Seiten.“**  
- **OLAMIP = „Das bedeutet meine Website.“**

Deshalb können KI‑Systeme OLAMIP wesentlich effektiver nutzen als eine traditionelle Sitemap.

## Ist eine OLAMIP‑Datei mit über 3.000 URLs zu groß?

Überhaupt nicht.  
Eine Datei mit 3.000 Einträgen liegt völlig im normalen Bereich.  
Selbst mit langen Zusammenfassungen beträgt die Gesamtgröße etwa 6 MB — trivial für moderne Server, Browser und KI‑Systeme.

## Wie groß ist ein typischer OLAMIP‑Eintrag?

| Länge der Zusammenfassung | Ca. Größe pro Eintrag |
|---|---|
| Kurz | 300–600 Bytes |
| Lang | 1–2 KB |

## Warum ist OLAMIP trotz großer Dateigrößen skalierbar?

- Hierarchische Struktur reduziert Wiederholungen  
- Kurze, prägnante Zusammenfassungen (meist < 500 Zeichen)  
- Effiziente Komprimierung (Gzip/Brotli reduziert 70–90 %)  

## Ab welcher Größe treten Leistungsprobleme auf?

| Dateigröße | Ca. URLs | Auswirkung |
|---|---:|---|
| 1–10 MB | 500–5.000 | Einwandfrei |
| 10–25 MB | 5.000–12.000 | Leicht langsamer |
| 25–50 MB | 12.000–25.000 | Groß, aber handhabbar |
| 50–100 MB | 25.000–50.000 | Schwer; manche Systeme werden langsamer |
| 100+ MB | 50.000+ | Zu groß für viele LLM‑Pipelines |

Für die meisten Websites bleiben OLAMIP‑Dateien mit zehntausenden Einträgen gut skalierbar.

## Wie oft sollte ich die Datei aktualisieren?

Immer wenn Sie neue Inhalte veröffentlichen oder bestehende Seiten ändern.  
Behandeln Sie sie wie eine Sitemap — aber für KI.

## Muss ich die Hauptdatei aktualisieren, wenn ich `olamip-delta.json` verwende?

Ja.  
Die Hauptdatei `olamip.json` muss immer den aktuellen Stand Ihrer Website widerspiegeln.  
Die optionale `olamip-delta.json` enthält nur die jüngsten Änderungen.

## Muss ich ein `<link>`‑Tag für `olamip-delta.json` hinzufügen?

Nein.  
Nur die Hauptdatei `olamip.json` muss im `<head>` Ihrer Website referenziert werden.  
Sobald KI‑Systeme diese Datei finden, suchen sie automatisch nach der Delta‑Datei im selben Verzeichnis.

## Werden LLMs diese Datei tatsächlich lesen?

Ja — genau das ist das Ziel.  
Mit zunehmender Verbreitung werden LLMs strukturierten Quellen wie OLAMIP Priorität geben.

## Wie kann ich testen, ob ein KI‑Bot meine `olamip.json` abrufen kann?

Probieren Sie Folgendes für OpenAI, Anthropic und Perplexity:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```
