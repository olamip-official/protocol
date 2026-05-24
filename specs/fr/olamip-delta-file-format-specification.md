# Spécification du format de fichier OLAMIP-DELTA

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Le protocole **OLAMIP-DELTA** définit un format de fichier delta basé sur JSON pour les mises à jour incrémentales de `olamip.json`. Cette spécification s’adresse aux webmasters et aux implémenteurs qui connaissent déjà les données structurées, le HTML et la publication de type sitemap. 

## 1. Vue d’ensemble

`olamip.json` est la représentation complète et faisant autorité de la structure et du contenu de votre site. `olamip-delta.json` est un fichier compagnon facultatif qui décrit les changements apportés à ce snapshot depuis la dernière mise à jour. 

Ensemble, ils forment un flux de changements interprétable par machine :

- `olamip.json` : snapshot complet et structuré.
- `olamip-delta.json` : journal de modifications incrémental.

---

## 2. Concepts de base

- **Entry** — un élément de contenu au niveau feuille, par exemple un article de blog, une page produit ou une page de documentation.
- **Section / Subsection** — un regroupement hiérarchique, par exemple une catégorie de blog, une catégorie de documentation, une collection de produits ou un groupe de projets.
- **Operation** — l’une des valeurs `added`, `updated`, `removed`.
- **Delta** — un ensemble de changements daté contenant `added`, `updated` et `removed`.

Ce format est conçu pour être :

- **d’abord interprétable par machine**, tout en restant suffisamment lisible pour être vérifié manuellement.
- **incrémental**, afin que les systèmes d’IA puissent rester synchronisés sans retraiter l’ensemble du site. 

---

## 3. Emplacement du fichier et découverte

### 3.1 Emplacement du fichier

`olamip.json` doit être hébergé à la racine de votre domaine :

```text
https://yourdomain.com/olamip.json
```

`olamip-delta.json` doit être hébergé dans le même répertoire :

```text
https://yourdomain.com/olamip-delta.json
```

Les deltas versionnés (facultatifs) se trouvent dans le même répertoire :

```text
/olamip.json
/olamip-delta-YYYY-MM-DD.json
```

### 3.2 Mécanisme de découverte

Les webmasters doivent déclarer l’emplacement de `olamip.json` à l’aide des deux balises suivantes dans le `<head>` du site :

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Les systèmes d’IA qui connaissent `olamip.json` rechercheront automatiquement `olamip-delta.json` dans le même répertoire et appliqueront les deltas comme décrit dans cette spécification. Il n’est pas nécessaire d’ajouter une balise `<link>` ou `<meta>` séparée pour `olamip-delta.json`. 

---

## 4. Structure de niveau supérieur

Au niveau racine, `olamip-delta.json` est un objet JSON avec les champs suivants :

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

### 4.1 Champs racine obligatoires

- `protocol` (string, obligatoire)  
  Doit être `"OLAMIP-DELTA"`.  
  Cela distingue le fichier delta du fichier principal OLAMIP et d’autres formats delta.

- `version` (string, obligatoire)  
  Doit correspondre au schéma de version documenté, par exemple `"1.0.0"`.  
  Cela permet aux implémenteurs de détecter les changements de schéma.

- `window_days` (integer, facultatif)  
  Nombre de jours dans la fenêtre glissante ; doit être positif, généralement 7–30.  
  Requis uniquement si vous utilisez un fichier delta glissant.

- `last_updated` (string, obligatoire)  
  Date de la dernière modification du fichier delta, au format ISO-8601 `YYYY-MM-DD`.  
  Cela aide les crawlers à décider quand récupérer à nouveau le fichier.

- `deltas` (array, obligatoire)  
  Tableau non vide d’ensembles de changements datés.  
  Chaque élément est un **objet delta** contenant `date`, `added`, `updated` et `removed`.

### 4.2 Tableau `deltas` et ordre des dates

- `deltas` doit être trié par `"date"` dans l’ordre croissant (du plus ancien au plus récent).
- Chaque objet delta doit contenir un champ `"date"` au format ISO-8601 `YYYY-MM-DD`.
- `deltas` ne doit pas être vide.

Cet ordre n’est pas optionnel pour la conformité : les systèmes d’IA supposent que les deltas sont triés chronologiquement. 

---

## 5. Structure de l’objet Delta

Chaque élément de `deltas` est un objet delta de la forme suivante :

```json
{
  "date": "2026-04-27",
  "added": [ ... ],
  "updated": [ ... ],
  "removed": [ ... ]
}
```

Pour garantir la clarté et la cohérence interprétables par machine, les webmasters **DOIVENT** inclure les trois champs d’opération dans chaque objet delta, même si certains sont des tableaux vides.

Champs :

- `date` (string) — date ISO-8601 `YYYY-MM-DD`.
- `added` (array) — zéro ou plusieurs éléments ajoutés.
- `updated` (array) — zéro ou plusieurs éléments modifiés.
- `removed` (array) — zéro ou plusieurs éléments supprimés.

Exemple d’un objet delta ne contenant que des ajouts :

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Nouvelle page",
      "summary": "Une nouvelle page a été ajoutée au site.",
      "url": "https://example.com/new-page/"
    }
  ],
  "updated": [],
  "removed": []
}
```

Ce schéma est requis même lorsqu’une seule opération s’applique un jour donné.

---

## 6. Opérations

### 6.1 `added`

Un élément `added` est un **objet complet** du type applicable (entry, section ou subsection). Les champs doivent correspondre à la spécification OLAMIP du type concerné.

Pour une **entry** :

- `title` (string, obligatoire) — titre lisible par un humain.
- `summary` (string, obligatoire) — description concise.
- `url` (string, obligatoire) — URL canonique absolue.
- `content_type` (string, obligatoire) — issu de la taxonomie `content_type` d’OLAMIP.
- `tags` (array, facultatif) — liste de tags en minuscules et à un seul token.
- `priority` (string, facultatif) — `"high"`, `"medium"` ou `"low"`.
- `policy` (string, facultatif) — `"allow"` ou `"forbid"`.
- `language` (string, facultatif) — code de langue BCP-47.
- `metadata` (object, facultatif) — données structurées spécifiques au domaine.

Pour une **section ou subsection**, les objets `added` suivent la spécification OLAMIP au niveau section, avec `title`, `summary`, `url`, `section_type`, `policy`, `tags`, `priority`, etc.

#### Règles pour `added`

- Les éléments `added` doivent contenir `url`.
- Les éléments `added` doivent être des objets complets, au minimum avec tous les champs obligatoires de la spécification OLAMIP principale.
- Aucune `url` ne peut apparaître plus d’une fois dans le même objet delta.

Exemple :

```json
{
  "date": "2026-04-27",
  "added": [
    {
      "title": "Nouvel article de blog",
      "summary": "Une courte introduction à la nouvelle fonctionnalité.",
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

Un élément `updated` doit contenir `url` et peut être :

- un **objet partiel** contenant uniquement les champs modifiés, ou
- un **objet complet** contenant tous les champs.

Pour tout élément `updated`, les champs absents sont considérés comme **inchangés**.

#### Exemple de mise à jour partielle

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "url": "https://example.com/post/to-update/",
      "summary": "Résumé mis à jour avec de nouvelles informations.",
      "tags": ["feature", "update", "docs"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Exemple de remplacement complet

```json
{
  "date": "2026-04-27",
  "updated": [
    {
      "title": "Titre de l’article mis à jour",
      "summary": "Résumé mis à jour avec de nouvelles informations.",
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

#### Règles pour `updated`

- Les éléments `updated` doivent contenir `url`.
- `updated` peut être un objet partiel ou complet.
- `updated` ne doit pas changer l’identité d’une entry via une réaffectation de `url` ; cela relève d’une décision de configuration, pas du fichier delta.

### 6.3 `removed`

Un élément `removed` est **minimal** et ne contient que l’identité.

```json
{
  "url": "https://example.com/page/to-remove/"
}
```

#### Règles pour `removed`

- Les éléments `removed` doivent contenir `url` et **ne doivent contenir aucun autre champ**.
- `url` doit être une URL absolue et canonique.
- `removed` s’applique à l’URL donnée et à ses descendants structurels (subsections et entries), le cas échéant.
- Aucune `url` ne peut apparaître plus d’une fois dans le même objet delta.

Exemple :

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

## 7. URL et identité des objets

OLAMIP-DELTA utilise les URL canoniques comme identité principale des entries, sections et subsections.

### Règles d’identité

- `url` doit être une URL absolue, comprenant le schéma et le domaine.
- Aucune `url` ne doit apparaître plus d’une fois dans le même objet delta, ni dans `added`, ni dans `updated`, ni dans `removed`.
- Lorsqu’une section est supprimée, tous ses descendants sont considérés comme supprimés.
- Un changement d’URL nécessite un **remove** suivi d’un **add**, et non une mise à jour directe de `url`.

### Bonnes pratiques pour les URL

- Utilisez des URL canoniques stables, qui changent rarement.
- Évitez les paramètres de requête ou les fragments dépendant de sessions.
- Si une page change d’adresse, traitez cela comme une question de redirection et de canonicalisation, pas comme une mise à jour directe du fichier delta.

---

## 8. Fenêtre delta glissante (recommandée)

La **fenêtre delta glissante** est le mode recommandé pour `olamip-delta.json`.

Dans ce modèle :

- Le fichier delta contient les changements des `window_days` derniers jours.
- Les systèmes d’IA lisent tous les deltas du fichier et les appliquent dans l’ordre chronologique.
- Cela évite de manquer des mises à jour lorsque les crawlers récupèrent les données avec retard. 

### Exemple de delta glissant (fenêtre de 7 jours)

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
          "title": "Nouveau produit A",
          "url": "https://example.com/product/a",
          "summary": "Un nouvel ajout au catalogue."
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
          "summary": "Description mise à jour pour refléter de nouvelles fonctionnalités."
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

Les systèmes d’IA :

- lisent tous les deltas du fichier,
- les appliquent dans l’ordre chronologique,
- puis arrivent à l’état le plus récent.

---

## 9. Deltas versionnés (alternative)

Certains webmasters préfèrent des **deltas versionnés**, où chaque fichier contient les changements d’une seule journée.

### Exemple d’organisation

```text
/olamip.json
/olamip-delta-2026-03-01.json
/olamip-delta-2026-03-02.json
/olamip-delta-2026-03-03.json
```

### Forme du fichier

Chaque fichier versionné a la même structure logique qu’un fichier glissant, mais il ne représente qu’une seule date de changement :

```json
{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "date": "2026-03-02",
  "added": [
    {
      "title": "Nouvel article de blog",
      "url": "https://example.com/blog/new-post/",
      "summary": "Aperçu bref de la mise à jour."
    }
  ],
  "updated": [],
  "removed": []
}
```

### Règles d’ingestion

Les systèmes d’IA devraient :

- détecter tous les fichiers `olamip-delta-*.json` dans le même répertoire que `olamip.json`,
- les trier par `date` dans l’ordre croissant,
- n’appliquer que ceux situés dans la fenêtre configurée, par exemple les 7 à 30 derniers jours.

Cette approche est particulièrement utile pour :

- les sites d’actualité avec des mises à jour quotidiennes fréquentes,
- les catalogues e-commerce à grand volume,
- les plateformes de documentation avec des cycles de publication rapides.

---

## 10. Changements structurels : Sections et Subsections

OLAMIP-DELTA applique le même schéma delta aux **sections** et **subsections** qu’aux **entries**. Cela permet aux systèmes d’IA de comprendre non seulement quelles pages existent, mais aussi comment elles sont organisées.

### 10.1 Ajouter une Section ou une Subsection

Lors de l’ajout d’une section ou d’une subsection, incluez un **objet section complet** dans `added`.

Exemple :

```json
{
  "date": "2026-05-01",
  "added": [
    {
      "title": "Tutoriels",
      "summary": "Guides étape par étape pour débutants et utilisateurs avancés.",
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

#### Règles

- Les objets `added` pour les sections doivent contenir `title`, `summary`, `url` et `section_type`.
- Utilisez `section_type` pour refléter le rôle sémantique, par exemple `blog_category`, `doc_category`, `product_collection` ou `research_category`.
- `priority` et `policy` aident les systèmes d’IA à comprendre l’importance et les règles d’ingestion pour tout le sous-arbre.

### 10.2 Mettre à jour une Section ou une Subsection

Incluez `url` et tout champ modifié.

Exemple :

```json
{
  "date": "2026-05-02",
  "updated": [
    {
      "url": "https://example.com/blog/tutorials/",
      "title": "Tutoriels pas à pas",
      "summary": "Guides pour débutants et utilisateurs avancés, avec exemples de code et bonnes pratiques.",
      "tags": ["tutorials", "beginner", "advanced", "code-examples"]
    }
  ],
  "added": [],
  "removed": []
}
```

#### Règles

- Les sections mises à jour doivent contenir `url`.
- `updated` peut inclure n’importe quelle sous-partie des autres champs.
- Les champs absents sont considérés comme inchangés.

### 10.3 Supprimer une Section ou une Subsection

Incluez seulement `url` dans `removed`.

Exemple :

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

#### Règles

- Les éléments `removed` pour les sections doivent contenir uniquement `url` et aucun autre champ.
- L’URL doit être l’URL canonique de la section ou de la subsection supprimée.
- Lorsqu’une section est supprimée, tous ses descendants sont également considérés comme supprimés.

---

## 11. Conformité

Les implémenteurs et webmasters devraient considérer les points suivants comme des règles de conformité.

### 11.1 Règles MUST

Un fichier OLAMIP-DELTA conforme **DOIT** :

- déclarer `protocol: "OLAMIP-DELTA"`,
- définir `version` selon le schéma de version documenté,
- contenir un tableau `deltas` non vide,
- faire en sorte que chaque objet delta contienne un `date` au format ISO-8601 `YYYY-MM-DD`,
- trier `deltas` par `date` dans l’ordre croissant,
- inclure les trois champs d’opération (`added`, `updated`, `removed`) dans chaque objet delta, même s’ils sont vides,
- s’assurer que `url` dans toutes les opérations est une URL absolue et canonique,
- s’assurer qu’aucune `url` n’apparaît plus d’une fois dans le même objet delta,
- s’assurer que `added` et `updated` contiennent `url`,
- s’assurer que `removed` contient `url` et aucun autre champ,
- traiter les champs absents dans `updated` comme inchangés,
- traiter `removed` comme la suppression de l’URL donnée et de ses descendants de l’index. 

### 11.2 Règles SHOULD

Une implémentation conforme **DEVRAIT** :

- adopter par défaut la **fenêtre delta glissante**, sauf si le workflow du site convient mieux aux deltas versionnés,
- maintenir `last_updated` à jour afin que les crawlers et validateurs sachent quand le fichier est récent,
- utiliser des URL canoniques stables et rarement modifiées pour éviter toute ambiguïté lors de l’application des deltas,
- maintenir `window_days` généralement entre 7 et 30 jours selon la fréquence de publication du site,
- pour les deltas versionnés, conserver suffisamment de fichiers dans la fenêtre configurée, par exemple les 7 à 30 derniers jours, afin d’éviter toute perte de mise à jour en cas de retard de crawl.

### 11.3 Règles MAY

Une implémentation conforme **PEUT** :

- utiliser le format delta versionné (`olamip-delta-YYYY-MM-DD.json`) plutôt qu’un fichier glissant unique si cela convient mieux au pipeline de déploiement,
- publier `olamip-delta.json` plus ou moins fréquemment que `olamip.json`, tant que la relation entre le snapshot et le delta reste claire et cohérente,
- ajouter des métadonnées de diagnostic ou de journalisation sous une clé séparée en dehors des champs du protocole, par exemple un objet `diagnostics`, tant que cela n’entre pas en conflit avec les champs obligatoires ni ne modifie le sens du delta.

---

## 12. Bonnes pratiques

Pour garantir une compatibilité robuste et durable avec les systèmes d’IA, les webmasters devraient suivre les bonnes pratiques suivantes lors de la génération de `olamip-delta.json`.

### 12.1 Automatiser la génération des deltas

- Reliez la génération du delta à votre CMS ou à votre pipeline de déploiement.
- À chaque publication, modification ou suppression de contenu, mettez `olamip-delta.json` à jour en conséquence.
- Validez le schéma du fichier avant déploiement afin d’éviter les erreurs de syntaxe ou les champs invalides.

### 12.2 Garder `olamip.json` comme source faisant autorité

- Conservez un `olamip.json` valide et à jour.
- Les nouveaux systèmes d’IA peuvent démarrer à partir du fichier complet, puis appliquer les deltas ensuite.
- Ne traitez pas `olamip-delta.json` comme un remplacement du snapshot principal.

### 12.3 Utiliser des URL canoniques stables

- Utilisez des URL absolues avec schéma et domaine.
- Évitez les motifs d’URL qui changent fréquemment, sauf si le système du site, par exemple les redirections, est suffisamment robuste pour préserver la signification canonique.
- Si l’URL d’une page change réellement, traitez cela comme un scénario « remove-then-add » et non comme une mise à jour directe de `url` dans le delta.

### 12.4 Maintenir la cohérence des résumés et des métadonnées

- Les résumés dans les éléments `added` et `updated` doivent être cohérents avec le contenu réel.
- Les petits ajustements de formulation qui ne changent pas le sens peuvent être publiés comme delta, mais ils devraient être minimisés pour éviter un churn inutile.
- Les tags doivent être normalisés, en minuscules, à un seul mot, et reliés par des tirets lorsqu’il s’agit de termes composés ; ils doivent refléter des concepts stables. 

---

## 13. Ressources connexes

Pour une compréhension complète de l’écosystème OLAMIP, les webmasters devraient aussi consulter :

- **[Spécification du format de fichier OLAMIP](https://olamip.org/file-format-specification/)**  
  Décrit la structure de `olamip.json` et définit les champs pour les entries, sections et subsections sur lesquels repose le format delta.

- **[Foire aux questions OLAMIP](https://olamip.org/frequently-asked-questions/)**  
  Explique les questions de déploiement, de découverte et d’exploitation pour les webmasters.

- **[Spécification du format de fichier OLAMIP-DELTA](https://olamip.org/delta-updates/)**  
  Schéma JSON officiel de `olamip-delta.json`, utilisable pour valider et lint les fichiers delta automatiquement.

- **[Pourquoi OLAMIP est un standard supérieur à LLMs.txt](https://olamip.org/why-olamip-is-a-superior-standard-to-llms-txt/)**  
  Explique les avantages d’OLAMIP et d’OLAMIP-DELTA par rapport à des formats de métadonnées plus simples et non structurés.

---

Ceci conclut la spécification du format de fichier OLAMIP-DELTA. Les webmasters et implémenteurs peuvent utiliser ce document comme référence pour générer, valider et consommer `olamip-delta.json` en environnement de production.