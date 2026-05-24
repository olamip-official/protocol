# Processo di Rilascio (Release Process)

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Questo documento definisce il processo standard che utilizzo per creare, testare e pubblicare le versioni della specifica OLAMIP e degli strumenti correlati. Per ora, tutte le release sono gestite e mantenute esclusivamente da me.

---

## 1. Versionamento

Seguo le regole del **Semantic Versioning 2.0.0**: https://semver.org/

- **MAJOR** — quando introduco modifiche incompatibili al formato o al protocollo.  
- **MINOR** — quando aggiungo funzionalità o sezioni nuove compatibili con le versioni precedenti.  
- **PATCH** — quando correggo bug, testo, o chiarisco regole senza modificare il comportamento.  

Esempio:  
`1.0.0` → `1.0.1` (patch), `1.1.0` (minor), `2.0.0` (major).

---

## 2. Modello di Branching

Utilizzo un workflow Git semplice:

- `main` — il branch stabile, sempre pronto per una release.  
- Branch di funzionalità o correzioni (es. `spec/delta-formats`, `fix/tag-rules`) — branch brevi per modifiche specifiche.  
- Branch di rilascio (es. `release/v1.1.0`) — creati immediatamente prima della pubblicazione.  

---

## 3. Prerequisiti per una Release

Prima di iniziare una release verifico che:

- Tutte le modifiche previste siano state unite in `main`.  
- Tutti i controlli automatici (linting, validazione schema, build scripts) siano superati.  
- Tutti gli issue e le note relative alla release siano chiusi o documentati.  
- `CHANGELOG.md` sia aggiornato con voci chiare e leggibili.  
- La documentazione sia aggiornata, inclusi:  
  - `file-format-specification.md`  
  - `olamip-delta-file-format-specification.md`  
  - `docs/introduction.md`  
  - `faq.md`  
- I file di esempio `olamip.json` e `olamip-delta.json` siano ancora validi secondo le regole più recenti.  

---

## 4. Creazione di una Release Candidate

1. Creare un branch di rilascio da `main`:

   ```bash
   git checkout main
   git checkout -b release/vX.Y.Z
   ```

2. Aggiornare i numeri di versione (se utilizzati in strumenti, script o asset).  
3. Aggiornare `CHANGELOG.md` con intestazione e note della versione.  
4. Effettuare il commit:

   ```bash
   git add .
   git commit -m "chore(release): prepare v1.1.0"
   ```

5. Push del branch:

   ```bash
   git push origin release/vX.Y.Z
   ```

6. Eseguire nuovamente la validazione locale o CI e verificare che tutto sia corretto.  

---

## 5. Test della Release Candidate

Poiché sono l’unico responsabile delle release, i miei test personali rappresentano il controllo principale:

- Verifico che i file di esempio `olamip.json` e `olamip-delta.json` rispettino ancora lo schema.  
- Controllo che tutti gli esempi nelle specifiche siano coerenti con le regole descritte.  
- Rileggo attentamente le sezioni aggiornate per individuare incoerenze o errori.  

Se trovo problemi significativi:

- Li correggo nel branch `release`.  
- Aggiorno `CHANGELOG.md` e faccio un nuovo commit.  
- Ripeto la validazione prima di procedere.  

---

## 6. Finalizzazione della Release

Quando la Release Candidate è stabile:

1. Taggo la release in Git:

   ```bash
   git tag -a vX.Y.Z -m "Release v1.1.0"
   git push origin vX.Y.Z
   ```

2. Creo la release su GitHub (o equivalente):

   - Utilizzo il tag `vX.Y.Z`.  
   - Copio la sezione pertinente del `CHANGELOG.md` nella descrizione della release.  
   - Allego eventuali artefatti generati (schema, strumenti CLI, snapshot tarball), se presenti.  

---

## 7. Passi Successivi alla Release

Dopo la pubblicazione:

- Aggiorno note interne o roadmap con ciò che è stato rilasciato.  
- Sposto eventuali elementi rimandati nel backlog o nelle note per la prossima release.  
- Se esiste un canale di comunicazione (es. Twitter, newsletter, GitHub Discussions), posso annunciare la nuova versione e riassumere le novità.  

---

## 8. Governance e Supervisione

Anche con un solo manutentore, mantengo un livello di governance:

- Qualsiasi modifica che influisca sullo schema principale, sui campi obbligatori o sulla versione del protocollo deve essere documentata chiaramente nel changelog.  
- Modifiche significative al processo di rilascio stesso devono essere documentate in una nota di governance o decision record sotto `governance/decision-records/`.  

---

## 9. Rollback o Patch

Se una release contiene un bug critico:

- Creo una patch il prima possibile (es. `v1.1.1`).  
- Documento chiaramente la correzione in `CHANGELOG.md`.  
- Se necessario, aggiorno la documentazione o comunico che la versione precedente è sconsigliata.  

---

## 10. Automazione e Scalabilità Futura

Automatizzo quanto più possibile:

- Passaggi ripetitivi come:  
  - Verifica della validità JSON  
  - Validazione dei file di esempio contro lo schema  
  - Generazione o linting del changelog  

Mantengo il processo semplice in modo che, se in futuro si uniranno altri contributori, il flusso possa essere condiviso facilmente.

---

Questo processo di rilascio è gestito e mantenuto da me e può essere aggiornato tramite il normale workflow ogni volta che miglioro la procedura stessa.
