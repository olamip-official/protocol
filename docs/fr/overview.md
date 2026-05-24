# Aperçu d’OLAMIP

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP est une norme ouverte qui permet aux sites web de communiquer clairement et intentionnellement avec les systèmes d’intelligence artificielle. Au lieu de forcer les grands modèles de langage (LLM) à deviner le sens à partir d’un HTML, CSS et JavaScript bruyants, OLAMIP fournit une représentation propre, structurée et interprétable par machine du contenu le plus important de votre site.

Ce document présente une introduction générale à l’objectif, à la philosophie, au format de fichier et au modèle de mise à jour d’OLAMIP.

---

## 1. Pourquoi OLAMIP a été créé

Les sites modernes sont conçus pour les navigateurs, pas pour l’IA. En conséquence, les LLM ont du mal avec :

- Le HTML riche en mise en page  
- Le contenu rendu par JavaScript  
- Les menus de navigation, publicités et éléments répétitifs  
- Les pages dupliquées ou non pertinentes  

Les LLM *explorent déjà* votre site, mais l’interprètent souvent mal. OLAMIP résout ce problème en fournissant aux systèmes d’IA exactement ce dont ils ont besoin :

- Un fichier JSON léger (`/olamip.json`)  
- Des résumés propres et rédigés par des humains  
- Des métadonnées structurées  
- Des URL canoniques  
- Une hiérarchie claire de sections, sous‑sections et entrées  

En adoptant OLAMIP, vous reprenez le contrôle sur la manière dont l’IA comprend et représente votre contenu.

---

## 2. Philosophie fondamentale : convivial pour l’humain + lisible par la machine

OLAMIP est conçu comme un pont de communication entre humains et IA. Chaque partie du protocole doit être :

| Principe | Convivial pour l’humain | Convivial pour la machine |
|----------|---------------------------|-----------------------------|
| Format de fichier | Facile à écrire et à valider | JSON strict et prévisible |
| Noms des champs | Intuitifs et explicites | Minuscules, conformes au schéma |
| Résumés | Clairs, alignés sur la marque | Concis, factuels, riches sémantiquement |
| Système de priorité | Étiquettes simples (`"high"`, `"medium"`, `"low"`) | Convertissables en poids de classement |
| Documentation | Guides en langage clair | Schémas JSON, règles de validation |
| Outils | Générateurs, plugins CMS | Outils CLI, parseurs, suites de tests |

- **Pour les humains :** OLAMIP doit être simple à adopter sans expertise technique avancée.  
- **Pour les machines :** OLAMIP doit être structuré, prévisible et non ambigu.

---

## 3. Le fichier OLAMIP (`/olamip.json`)

Un fichier OLAMIP est un document JSON structuré hébergé à l’adresse :

`https://yourdomain.com/olamip.json`

Il contient :

- `protocol` — doit être `"OLAMIP"`  
- `version` — version du protocole  
- `identity` — identité du site  
- `content` — hiérarchie structurée du contenu  
- `metadata` — langue, date de mise à jour, champs globaux  

### 3.1 Balises de découverte

Pour garantir que les systèmes d’IA trouvent votre fichier OLAMIP de manière fiable, incluez :

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

L’utilisation des deux balises assure redondance, compatibilité et pérennité.

---

## 4. Structure du fichier

### 4.1 Objet Identity

Décrit le site ou l’organisation :

- `name` — requis  
- `type` — requis  
- `canonical_description` — requis  
- `tags` — optionnel  

### 4.2 Objet Content

Contient :

- Une vue d’ensemble (`overview`)  
- Des `sections`  
- Des `subsections` optionnelles  
- Des `entries` (unités de contenu les plus fines)  

La profondeur de hiérarchie est illimitée.

### 4.3 Sections

Une section regroupe du contenu lié. Champs requis :

- `title`  
- `summary`  
- `url`  
- `section_type`  

Champs optionnels :

- `policy` (`"allow"` ou `"forbid"`)  
- `tags`  
- `priority`  
- `published`  
- `language`  
- `subsections`  
- `entries`  

**Héritage de la politique :**  
Si omise, la politique est héritée des ancêtres. Valeur par défaut : `"allow"`.

### 4.4 Entrées (Entries)

Les entrées représentent des éléments de contenu individuels tels que :

- Articles de blog  
- Actualités  
- Produits  
- Pages de documentation  
- Articles de recherche  
- Éléments multimédias  

Champs requis :

- `title`  
- `summary`  
- `url`  
- `content_type`  

Champs optionnels :

- `policy`, `tags`, `priority`, `published`, `language`, `metadata`  

Les URL sont obligatoires car elles identifient le contenu de manière unique et permettent à l’IA de vérifier, dédupliquer et croiser les pages.

---

## 5. Métadonnées et prise en charge des langues

L’objet `metadata` peut inclure :

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### Prise en charge multilingue

La langue peut être définie au niveau :

- du fichier  
- de la section  
- de l’entrée  

Utilisez des codes BCP‑47 comme `en`, `fr`, `es`, `pt-BR`, `zh-CN`.

Cela aide l’IA à :

- choisir les bons tokenizers  
- éviter les mélanges de langues  
- améliorer la précision de recherche  
- réduire les hallucinations  

---

## 6. Tags et priorité

### 6.1 Tags

Les tags fournissent des indices sémantiques légers. Ils doivent être :

- en minuscules  
- en un seul mot  
- ASCII  
- séparés par des tirets pour les concepts multi‑mots  
- cohérents  

Exemples :

| Concept | Tag valide |
|---------|------------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

### 6.2 Priorité

Valeurs :

- `high` — contenu phare  
- `medium` — valeur par défaut  
- `low` — contenu de niche ou ancien  

Utilisez `high` avec parcimonie (5–10 % des entrées).

---

## 7. Mises à jour Delta (`olamip-delta.json`)

Les sites évoluent constamment. OLAMIP prend en charge les mises à jour incrémentielles via un fichier compagnon optionnel :

`/olamip-delta.json`

Ce fichier contient uniquement :

- des entrées ajoutées  
- des entrées mises à jour  
- des URL supprimées  

### 7.1 Pourquoi les deltas sont importants

- Les systèmes d’IA restent à jour sans retraiter le fichier complet.  
- Les catalogues volumineux restent gérables.  
- Les mises à jour se propagent rapidement.  
- Aucun changement n’est perdu.  

### 7.2 Fenêtre Delta glissante (recommandée)

Conservez les deltas des 7–30 derniers jours dans un seul fichier.  
Les systèmes d’IA les appliquent dans l’ordre chronologique.

### 7.3 Fichiers Delta versionnés (alternative)

Publiez des fichiers séparés :

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`  
...

Idéal pour les sites à fort volume (actualités, e‑commerce, documentation).

### 7.4 Découverte

Seul le fichier OLAMIP principal nécessite des balises `<link>` et `<meta>`.  
Les systèmes d’IA recherchent automatiquement les fichiers delta dans le même répertoire.

---

## 8. OLAMIP vs sitemaps traditionnels

- Sitemap XML : « Voici mes pages. »  
- OLAMIP : « Voici ce que signifie mon site. »  

Les sitemaps listent des URL.  
OLAMIP fournit :

- des résumés  
- des types de contenu  
- une hiérarchie de sections  
- des tags  
- des métadonnées  
- des priorités  
- des politiques d’ingestion  

Avec schema.org, OLAMIP offre aux systèmes d’IA une compréhension complète et soigneusement organisée de votre site.

---

## 9. FAQ (résumé)

### OLAMIP expose‑t‑il des informations privées ?

Non. Il ne résume que du contenu public.

### Les concurrents peuvent‑ils utiliser mon fichier ?

Pas plus qu’ils n’utilisent déjà vos pages publiques.

### Quelle taille peut avoir un fichier OLAMIP ?

1–10 Mo est courant.  
Même 3 000+ entrées est normal et sûr.

### À quelle fréquence dois‑je le mettre à jour ?

Chaque fois que le contenu change — traitez‑le comme un sitemap pour l’IA.

### Dois‑je mettre à jour le fichier principal si j’utilise des deltas ?

Oui. `olamip.json` est toujours la source de vérité.

### Les LLM liront‑ils réellement ce fichier ?

Oui — c’est précisément le but du protocole.  
Vous pouvez tester avec :

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```

---

## 10. Résumé

OLAMIP transforme votre site en une carte de connaissance interprétable par machine :

- `olamip.json` — votre instantané structuré complet  
- `olamip-delta.json` — votre journal de modifications incrémentielles  
- Sections, sous‑sections, entrées — hiérarchie claire  
- Résumés, tags, priorités — sens organisé par des humains  
- Balises de découverte — ingestion fiable  
- Support multilingue — préparation mondiale  

En adoptant OLAMIP, vous garantissez que les systèmes d’IA comprennent votre contenu comme vous l’entendez — avec précision, cohérence et à grande échelle.
