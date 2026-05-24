# Release‑Prozess (Release Process)

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Dieses Dokument beschreibt den Standardprozess, den ich für das Erstellen, Testen und Veröffentlichen von Releases der OLAMIP‑Spezifikation sowie der zugehörigen Tools verwende. Derzeit werden alle Releases ausschließlich von mir verwaltet.

---

## 1. Versionierung

Ich folge **Semantic Versioning 2.0.0**: https://semver.org/

- **MAJOR** — bei Änderungen, die nicht abwärtskompatibel sind.  
- **MINOR** — bei neuen Funktionen oder Abschnitten, die abwärtskompatibel sind.  
- **PATCH** — bei Fehlerkorrekturen, Textanpassungen oder Klarstellungen ohne Verhaltensänderung.  

Beispiel:  
`1.0.0` → `1.0.1` (Patch), `1.1.0` (Minor), `2.0.0` (Major).

---

## 2. Branching‑Modell

Ich verwende einen einfachen Git‑Workflow:

- `main` — der stabile Branch, jederzeit bereit für ein Release.  
- Feature‑ oder Fix‑Branches (z. B. `spec/delta-formats`, `fix/tag-rules`) — kurzlebige Branches für Änderungen.  
- Release‑Branches (z. B. `release/v1.1.0`) — werden direkt vor der Veröffentlichung erstellt.  

---

## 3. Voraussetzungen für ein Release

Vor Beginn eines Releases stelle ich sicher, dass:

- Alle geplanten Änderungen in `main` gemergt wurden.  
- Alle automatisierten Checks (Linting, Schema‑Validierung, Build‑Skripte) erfolgreich sind.  
- Alle Issues und Notizen für dieses Release geschlossen oder dokumentiert sind.  
- `CHANGELOG.md` aktualisiert wurde und klare, verständliche Einträge enthält.  
- Die Dokumentation aktuell ist, einschließlich:  
  - `file-format-specification.md`  
  - `olamip-delta-file-format-specification.md`  
  - `docs/introduction.md`  
  - `faq.md`  
- Die Beispiel‑Dateien `olamip.json` und `olamip-delta.json` weiterhin den neuesten Regeln entsprechen.  

---

## 4. Erstellen eines Release Candidate

1. Release‑Branch aus `main` erstellen:

   ```bash
   git checkout main
   git checkout -b release/vX.Y.Z
   ```

2. Versionsnummern aktualisieren (falls in Tools, Skripten oder Assets verwendet).  
3. `CHANGELOG.md` mit Header und Versionshinweisen aktualisieren.  
4. Änderungen committen:

   ```bash
   git add .
   git commit -m "chore(release): prepare v1.1.0"
   ```

5. Branch pushen:

   ```bash
   git push origin release/vX.Y.Z
   ```

6. Lokale oder CI‑Validierung erneut ausführen und sicherstellen, dass alles korrekt ist.  

---

## 5. Testen des Release Candidate

Da ich Releases allein verwalte, ist mein persönlicher Testprozess der wichtigste Schritt:

- Ich überprüfe, dass die Beispiel‑Dateien `olamip.json` und `olamip-delta.json` weiterhin dem Schema entsprechen.  
- Ich stelle sicher, dass alle Beispiele in den Spezifikationsdokumenten den beschriebenen Regeln folgen.  
- Ich lese alle aktualisierten Abschnitte sorgfältig durch, um Inkonsistenzen zu finden.  

Falls größere Probleme auftreten:

- Ich behebe sie direkt im `release`‑Branch.  
- Ich aktualisiere `CHANGELOG.md` und committe erneut.  
- Ich validiere alles erneut, bevor ich fortfahre.  

---

## 6. Finalisierung des Releases

Sobald der Release Candidate stabil ist:

1. Release in Git taggen:

   ```bash
   git tag -a vX.Y.Z -m "Release v1.1.0"
   git push origin vX.Y.Z
   ```

2. GitHub‑Release erstellen:

   - Tag `vX.Y.Z` verwenden.  
   - Den entsprechenden Abschnitt aus `CHANGELOG.md` in die Release‑Beschreibung einfügen.  
   - Falls vorhanden: generierte Artefakte anhängen (Schemas, CLI‑Tools, Snapshot‑Archive).  

---

## 7. Schritte nach dem Release

Nach der Veröffentlichung:

- Ich aktualisiere interne Notizen oder Roadmaps über die ausgelieferten Änderungen.  
- Ich verschiebe verschobene Punkte in den Backlog oder in die Planung für das nächste Release.  
- Falls es Kommunikationskanäle gibt (z. B. Twitter, Newsletter, GitHub Discussions), kündige ich die neue Version optional an.  

---

## 8. Governance und Aufsicht

Auch als alleiniger Maintainer halte ich eine Governance‑Ebene aufrecht:

- Änderungen am Kernschema, an Pflichtfeldern oder an der Protokollversion müssen klar im Changelog dokumentiert werden.  
- Bedeutende Änderungen am Release‑Prozess selbst müssen in einer Governance‑Notiz oder einem Decision Record unter `governance/decision-records/` festgehalten werden.  

---

## 9. Rollback oder Patch‑Releases

Falls ein Release einen kritischen Fehler enthält:

- Ich erstelle so schnell wie möglich ein Patch‑Release (z. B. `v1.1.1`).  
- Ich dokumentiere die Korrektur klar in `CHANGELOG.md`.  
- Falls nötig, aktualisiere ich Dokumentation oder Hinweise, um die vorherige Version als veraltet zu kennzeichnen.  

---

## 10. Automatisierung und zukünftige Skalierung

Ich automatisiere so viel wie möglich:

- Wiederkehrende Schritte wie:  
  - JSON‑Validierung  
  - Schema‑Validierung der Beispiel‑Dateien  
  - Generierung oder Linting von Changelog‑Texten  

Ich halte den Prozess bewusst einfach, damit er später problemlos mit weiteren Mitwirkenden geteilt werden kann.

---

Dieser Release‑Prozess wird von mir gepflegt und kann jederzeit über den normalen Workflow aktualisiert werden, wenn ich Verbesserungen daran vornehme.
