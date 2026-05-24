# OLAMIP – Guide de Démarrage Rapide

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Ce guide rapide explique comment mettre en place OLAMIP sur un site web de la manière la plus simple possible.

---

## 1. Créer le fichier `olamip.json`

Placez un fichier `olamip.json` valide à la racine de votre site :

`https://yourdomain.com/olamip.json`

Commencez avec un instantané propre et minimal de vos pages les plus importantes.

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

## 2. Ajouter les balises de découverte dans votre HTML

Ajoutez ces balises dans la section `<head>` de votre page d’accueil et des pages principales :

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

---

## 3. Rédiger des résumés concis

Rédigez des résumés courts et factuels qui expliquent clairement le sujet de la page et son importance. Évitez le langage marketing, les répétitions et les descriptions vagues.

Bonnes pratiques :

- « Un guide pour débutants sur la photographie en pose longue. »
- « Détails et tarification du plan entreprise. »

À éviter :

- « La meilleure page du monde. »
- « Un contenu incroyable que vous allez adorer. »

---

## 4. Utiliser les bons types de contenu

Choisissez le `content_type` le plus précis possible :

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

## 5. Organiser avec des sections

Utilisez `sections` pour regrouper les contenus et `entries` pour les pages individuelles. Si nécessaire, ajoutez des `subsections` pour une structure plus profonde.

Exemple :

- Section : Blog  
- Sous‑section : Tutoriels  
- Entrée : Comment utiliser OLAMIP  

---

## 6. Définir la priorité de manière intentionnelle

Utilisez :

- `high` pour vos pages les plus importantes  
- `medium` pour les pages standard  
- `low` pour les pages de niche ou anciennes  

N’attribuez pas `high` à tout.

---

## 7. Utiliser le champ *policy* pour contrôler l’ingestion par l’IA

Le champ `policy` indique aux systèmes d’IA s’ils peuvent ingérer une section, une sous‑section ou une entrée.

- `"allow"` → ingestion autorisée  
- `"forbid"` → ingestion interdite  

Si vous omettez `policy`, OLAMIP hérite de la politique du parent.  
Si aucun parent n’en définit, la valeur par défaut est `"allow"`.

Exemples :

**Ignorer une section entière :**

```json
{
  "title": "Deprecated Guides",
  "summary": "Outdated content, not for AI.",
  "url": "https://yourdomain.com/old-guides/",
  "section_type": "doc_category",
  "policy": "forbid"
}
```

**Ignorer une seule page :**

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

## 8. Ajouter les métadonnées de langue

Utilisez des codes BCP‑47 tels que :

- `fr`
- `en`
- `es`
- `de`
- `pt-BR`
- `zh-CN`

Définissez la langue au niveau du fichier, de la section ou de l’entrée selon les besoins.

---

## 9. Mettre à jour le fichier régulièrement

Chaque fois que vous ajoutez, modifiez ou supprimez des pages importantes, mettez à jour `olamip.json`.

Si votre site change fréquemment, ajoutez également `olamip-delta.json` dans le même répertoire afin que les systèmes d’IA puissent rester synchronisés entre deux mises à jour complètes.

---

## 10. Valider avant publication

Avant de mettre en ligne :

- Vérifiez que le JSON est valide  
- Assurez‑vous que les URL sont absolues  
- Vérifiez la présence de tous les champs obligatoires  
- Assurez‑vous que les résumés sont clairs et concis  
- Normalisez et unifiez les tags  
- Vérifiez que le fichier est servi depuis la racine du site  

---

## Liste de vérification minimale

- [ ] Créer `olamip.json`  
- [ ] L’héberger à la racine du site  
- [ ] Ajouter les discovery tags dans `<head>`  
- [ ] Inclure sections et entrées pour les contenus clés  
- [ ] Ajouter métadonnées et langue  
- [ ] Rédiger des résumés courts et précis  
- [ ] Mettre à jour le fichier lorsque le site change  
- [ ] Ajouter `olamip-delta.json` si vous souhaitez des mises à jour incrémentielles  

---

## Par où commencer ?

Si vous ne savez pas par où commencer, implémentez OLAMIP uniquement pour vos pages principales :

- Page d’accueil  
- Index du blog  
- Meilleurs articles  
- Pages clés de produits ou services  
- Pages essentielles de documentation  

Ensuite, étendez progressivement.
