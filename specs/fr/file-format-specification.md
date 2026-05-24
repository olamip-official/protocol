# Spécification du Format de Fichier OLAMIP  
*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Vue d’ensemble

Le fichier OLAMIP (`/olamip.json`) est un document JSON structuré qui fournit des résumés organisés des pages les plus importantes de votre site web. Il est conçu pour être facilement analysé par les grands modèles de langage (LLM), leur permettant de comprendre, prioriser et utiliser votre contenu avec clarté, précision et intention.

## Emplacement du fichier

Le fichier OLAMIP doit être hébergé à la racine de votre domaine :

`https://votredomaine.com/olamip.json`

## Déclaration de l’emplacement du fichier OLAMIP

Pour maximiser l’adoption et garantir que les systèmes puissent localiser votre fichier OLAMIP de manière fiable, publiez à la fois une balise `<link>` et une balise `<meta>` dans la section `<head>` de votre site.

### Découverte principale : `<link rel="olamip">`

- Pratique standard : les crawlers analysent déjà les balises `<link>` pour des ressources comme canonical, sitemap ou alternate.  
- Compatible avec les machines : déclare une relation formelle entre la page et le fichier OLAMIP.  
- Interopérabilité : s’intègre naturellement aux standards web existants.

### Découverte de secours : `<meta name="olamip-location">`

- Lisible par l’humain : simple à ajouter et à comprendre.  
- Compatibilité : certains outils préfèrent analyser les balises `<meta>`.  
- Redondance : sert de solution de repli si un crawler ne supporte pas encore `rel="olamip"`.

### Pourquoi utiliser les deux ?

- Pérennité : différents systèmes peuvent adopter différentes méthodes de découverte.  
- Résilience : si l’une échoue, l’autre prend le relais.  
- Clarté : réduit toute ambiguïté sur l’emplacement du fichier OLAMIP.

### Exemple recommandé

```html
<link rel="olamip" href="https://votredomaine.com/olamip.json">
<meta name="olamip-location" content="https://votredomaine.com/olamip.json">
```

## Structure du fichier

Le fichier OLAMIP doit être un document JSON encodé en UTF‑8 contenant :

- `protocol`  
- `version`  
- `identity`  
- `content`  
- `metadata`  

### Structure générale

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

## Objet Identity

| Champ | Type | Obligatoire | Description |
|---|---|---:|---|
| `name` | `string` | Oui | Nom du site ou de l’organisation. |
| `type` | `string` | Oui | Type d’entité : `company`, `blog`, `ecommerce`, etc. |
| `canonical_description` | `string` | Oui | Description lisible par l’humain. |
| `tags` | `array<string>` | Non | Mots‑clés décrivant le domaine ou secteur. |

---

## Objet Content

L’objet `content` contient :

- un `overview`  
- une liste de `sections`  
- chaque section peut contenir des `subsections`  
- chaque section ou sous‑section peut contenir des `entries`  

### Objet Overview

| Champ | Type | Obligatoire | Description |
|---|---|---:|---|
| `summary` | `string` | Oui | Résumé concis de la finalité du site. |

---

## Spécification d’une Section

Une `Section` représente une catégorie ou un regroupement de contenu. Elle peut contenir :

- des `entries`  
- des `subsections`  

### Champs d’une Section

| Champ | Type | Obligatoire | Description |
|---|---|---:|---|
| `title` | `string` | Oui | Nom lisible de la section. |
| `summary` | `string` | Oui | Description du contenu de la section. |
| `url` | `string` | Oui | URL canonique de la section. |
| `section_type` | `string` | Oui | Classification sémantique. |
| `policy` | `string` | Non | `"allow"` ou `"forbid"`. |
| `tags` | `array<string>` | Non | Mots‑clés optionnels. |
| `priority` | `string` | Non | `"high"`, `"medium"`, `"low"`. |
| `published` | `string` | Non | Date ISO 8601. |
| `language` | `string` | Non | Code BCP‑47. |
| `entries` | `array<Entry>` | Oui | Liste d’entrées. |
| `subsections` | `array<Section>` | Non | Sous‑sections. |

### Valeurs autorisées pour `section_type`

| section_type | Signification |
|---|---|
| `blog_category` | Catégorie d’articles de blog. |
| `news_section` | Section d’actualités. |
| `product_collection` | Produits ou services. |
| `doc_category` | Documentation. |
| `research_category` | Travaux de recherche. |
| `project_group` | Projets ou portfolio. |
| `content_section` | Catégorie générique. |

---

## Règles de politique et héritage

Le champ `policy` contrôle si le contenu peut être ingéré par les systèmes d’IA.

### Règles d’héritage

1. Politique définie au niveau de l’Entry  
2. Sinon, politique de la sous‑section  
3. Sinon, politique de la section  
4. Sinon, valeur par défaut : `"allow"`

Les systèmes d’IA doivent respecter strictement ces règles.

---

## Diagramme hiérarchique

```text
content
└── sections[]
    ├── Section (Niveau 1)
    │     ├── entries[]
    │     └── subsections[]
    │           ├── Section (Niveau 2)
    │           │     ├── entries[]
    │           │     └── subsections[]
    │           │           └── Section (Niveau 3)
    │           │                 └── entries[]
    │           └── ...
    └── ...
```

---

## Objet Entry

Une `Entry` représente l’unité de contenu la plus granulaire.

### Champs d’une Entry

| Champ | Type | Obligatoire | Description |
|---|---|---:|---|
| `title` | `string` | Oui | Titre lisible. |
| `summary` | `string` | Oui | Résumé concis. |
| `url` | `string` | Oui | URL canonique absolue. |
| `content_type` | `string` | Oui | Classification sémantique. |
| `policy` | `string` | Non | `"allow"` ou `"forbid"`. |
| `tags` | `array<string>` | Non | Mots‑clés. |
| `priority` | `string` | Non | `"high"`, `"medium"`, `"low"`. |
| `published` | `string` | Non | Date ISO 8601. |
| `language` | `string` | Non | Code BCP‑47. |
| `metadata` | `object` | Non | Métadonnées spécifiques. |

### Pourquoi les URLs sont obligatoires

Elles servent d’identifiant canonique pour la déduplication, la validation et la correspondance avec schema.org, les sitemaps et les crawlers.

---

## Types autorisés pour `content_type`

(blogs, news, produits, documentation, recherche, portfolio, médias, ressources, etc.)

---

## Métadonnées

Exemple :

```json
{
  "last_updated": "2026-01-21",
  "language": "fr",
  "source_url": "https://www.votresite.com/",
  "copyright": "© 2026 Exemple"
}
```

---

## Priorité : pourquoi des catégories fonctionnent mieux

- Clarté universelle  
- Validation simple  
- Interprétation cohérente par les LLM  

### Valeurs

| Valeur | Signification |
|---|---|
| `"high"` | Contenu critique. |
| `"medium"` | Par défaut. |
| `"low"` | Contenu secondaire. |

---

## Support multilingue

Définir `language` :

- au niveau du fichier (dans `metadata`)  
- au niveau des sections  
- au niveau des entrées  

Utiliser les codes BCP‑47.

---

## Règles générales de validation

| Règle | Exigence |
|---|---|
| JSON valide | Pas de virgules finales. |
| Champs obligatoires | Respect du schéma. |
| URLs canoniques | Absolues et stables. |
| Résumés | < 500 caractères. |
| Tags | minuscules, un seul mot. |

---

## Format des tags

- minuscules  
- un seul mot  
- ASCII  
- tirets pour les concepts multi‑mots  

Exemples :

| Concept | Tag valide |
|---|---|
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

---

## Versionnement OLAMIP

- Les parseurs ignorent les champs inconnus.  
- Les éditeurs doivent valider contre le dernier schéma.

---

## Alignement sémantique

OLAMIP complète les standards existants en fournissant un cadre structuré, lisible par les humains et optimisé pour les systèmes d’IA.

