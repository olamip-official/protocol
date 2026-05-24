<p align="center">
  <img src="assets/banner.jpg" alt="Bannière OLAMIP" width="100%">
</p>

<p align="center">
  <img src="assets/logo.png" alt="Logo OLAMIP" width="140">
</p>

<h1 align="center">Protocole OLAMIP</h1>

<p align="center">
  <strong>Open Language‑Aligned Machine‑Interpretable Protocol</strong><br>
  Un standard multilingue, structuré et lisible par machine pour des sites Web compréhensibles par l’IA.
</p>

<p align="center">
  <a href="https://olamip.org/">Site Web</a> •
  <a href="https://olamip.org/file-format-specification/">Spécification du format de fichier</a> •
  <a href="https://olamip.org/delta-updates/">OLAMIP‑DELTA</a> •
  <a href="https://olamip.org/frequently-asked-questions/">FAQ</a>
</p>

---

# 📘 Vue d’ensemble

OLAMIP est un protocole ouvert et multilingue qui permet aux sites Web de décrire leur structure, leur sens et leur intention dans un format **interprétable par machine**. Il fournit une représentation JSON propre que les grands modèles de langage (LLM) peuvent analyser de manière fiable, sans avoir à interpréter HTML, CSS ou JavaScript. [web:50][web:58]

Ce dépôt contient :

- **La spécification officielle OLAMIP**.
- **Le protocole de mise à jour incrémentale OLAMIP‑DELTA**.
- **La documentation multilingue et les सामग्री de gouvernance**.
- **Des exemples et des modèles**.

---

# 🌐 Index des langues

La documentation OLAMIP est disponible en plusieurs langues. Chaque catégorie (Specs, Docs, Governance) contient des dossiers parallèles :

```text
/specs/<lang>
/docs/<lang>
/governance/<lang>
```

### Langues prises en charge

| Langue | Code | Specs | Docs | Gouvernance |
|---|---|---|---|---|
| Anglais | en | [/specs/en](specs/en/) | [/docs/en](docs/en/) | [/governance/en](governance/en/) |
| Espagnol | es | [/specs/es](specs/es/) | [/docs/es](docs/es/) | [/governance/es](governance/es/) |
| Russe | ru | [/specs/ru](specs/ru/) | [/docs/ru](docs/ru/) | [/governance/ru](governance/ru/) |
| Japonais | ja | [/specs/ja](specs/ja/) | [/docs/ja](docs/ja/) | [/governance/ja](governance/ja/) |
| Chinois (simplifié) | zh‑CN | [/specs/zh-CN](specs/zh-CN/) | [/docs/zh-CN](docs/zh-CN/) | [/governance/zh-CN](governance/zh-CN/) |
| Portugais | pt | [/specs/pt](specs/pt/) | [/docs/pt](docs/pt/) | [/governance/pt](governance/pt/) |
| Hindi | hi | [/specs/hi](specs/hi/) | [/docs/hi](docs/hi/) | [/governance/hi](governance/hi/) |
| Bengali | bn | [/specs/bn](specs/bn/) | [/docs/bn](docs/bn/) | [/governance/bn](governance/bn/) |

> D’autres langues sont ajoutées régulièrement.

---

# 📑 Structure du dépôt

```text
/README.md              → Page d’accueil principale du dépôt
/CHANGELOG.md           → Journal global des modifications
/LICENSE                → Licence
/assets/                → Logos, bannières, diagrammes

/specs/                 → Spécifications officielles OLAMIP & OLAMIP‑DELTA
    /en/ /es/ /ru/ /ja/ /zh-CN/ /pt/ /hi/ /bn/

/docs/                  → Documentation, guides, explications
    /en/ /es/ /ru/ /ja/ /zh-CN/ /pt/ /hi/ /bn/

/governance/            → Gouvernance, versions, processus
    /en/ /es/ /ru/ /ja/ /zh-CN/ /pt/ /hi/ /bn/

/examples/              → Exemples de fichiers OLAMIP et modèles
```

---

# 📜 Spécifications

### **Spécification principale**
- Anglais : [/specs/en](specs/en/).
- Version du site : [https://olamip.org/file-format-specification/](https://olamip.org/file-format-specification/).

### **OLAMIP‑DELTA**
- Anglais : [/specs/en](specs/en/).
- Version du site : [https://olamip.org/delta-updates/](https://olamip.org/delta-updates/).

---

# 🧭 Gouvernance

Gouvernance, versions et évolution du protocole :

- Anglais : [/governance/en](governance/en/).
- Hindi : [/governance/hi](governance/hi/).
- Bengali : [/governance/bn](governance/bn/).
- Toutes les langues : [/governance](governance/).

---

# 🧪 Exemples

Exemples de fichiers et de modèles OLAMIP :

- [/examples](examples/).

Contient :

- Exemple `olamip.json`.
- Exemple `olamip-delta.json`.
- Modèles de section/entry.
- Schémas de bonnes pratiques.

---

# 📄 Licence

Ce projet est publié sous une licence ouverte. Consultez le fichier `/LICENSE` et le dossier `/governance` pour plus de détails.

---

<p align="center">
  <strong>OLAMIP — Rendre le Web lisible par les machines.</strong><br>
  <a href="https://olamip.org/">https://olamip.org/</a>
</p>