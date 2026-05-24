# Foire aux questions (FAQ)

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Qu’est‑ce qu’OLAMIP ?

OLAMIP signifie **Open Language‑Aligned Machine‑Interpretable Protocol**.  
C’est un format simple et ouvert qui permet aux sites web de publier des résumés structurés de leur contenu afin que les grands modèles de langage (LLM) puissent mieux les comprendre et apprendre d’eux.

## Pourquoi devrais‑je utiliser OLAMIP ?

Parce que les LLM explorent déjà votre site, mais ont du mal à l’interpréter correctement. Avec OLAMIP, vous pouvez :

- Expliquer précisément à l’IA de quoi parlent vos pages.  
- Mettre en avant votre contenu le plus précieux.  
- Améliorer la découvrabilité dans les outils et moteurs de recherche basés sur l’IA.  
- Réduire les mauvaises interprétations et les hallucinations.

C’est la différence entre une IA qui *devine* et une IA qui *comprend*.

## OLAMIP révèle‑t‑il des informations privées ?

Non. OLAMIP ne résume que du contenu déjà public sur votre site.  
Il ne révèle rien qui ne soit déjà visible dans votre HTML, vos métadonnées ou votre balisage SEO.

## Mes concurrents peuvent‑ils utiliser mon fichier OLAMIP ?

Pas plus qu’ils n’utilisent déjà vos pages publiques, vos métadonnées ou votre sitemap.  
OLAMIP vous donne en réalité **plus de contrôle** sur la manière dont les systèmes d’IA interprètent votre contenu — pas moins.

## À quoi ressemble un fichier `olamip.json` ?

Un fichier `olamip.json` est un document structuré au format JSON qui décrit le contenu, la hiérarchie et les métadonnées de votre site d’une manière facilement interprétable par les systèmes d’IA.  
Pour explorer la structure technique complète, consultez la [spécification du format de fichier](https://olamip.org/file-format-specification) ou examinez un exemple réel avec l’étude de cas [TimeLAX.com](https://olamip.org/how-olamip-transforms-ai-discovery-a-timelax-com-use-case/).

## Où dois‑je héberger le fichier ?

À la racine de votre domaine :

```
https://yourdomain.com/olamip.json
```

Vous pouvez également ajouter des balises de découverte à votre page d’accueil :

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## À quoi sert le champ « priority » ?

Il indique l’importance d’une page pour les LLM. Utilisez‑le avec parcimonie :

- `high` → contenu essentiel et stratégique.  
- `medium` → valeur par défaut pour la plupart des pages.  
- `low` → contenu de niche, ancien ou de faible valeur.

Si tout est marqué « high », alors rien ne l’est vraiment.

## Pourquoi dois‑je inclure les URL si des résumés sont déjà fournis ?

Les résumés indiquent à l’IA *ce que signifie* une page, mais les URL indiquent *quelle page* c’est.  
Un résumé est descriptif, mais ce n’est pas un identifiant unique.  
L’URL est la seule référence canonique et stable vers la page réelle.

Les systèmes d’IA utilisent les URL pour :

- Récupérer et vérifier le contenu complet.  
- Éviter d’indexer des doublons.  
- Détecter les déplacements ou modifications de pages.  
- Relier les données OLAMIP au balisage schema.org, aux sitemaps et aux crawlers.  
- Citer correctement la page dans les réponses générées.

Sans URL, deux pages différentes avec des résumés similaires seraient indiscernables.

## À quoi sert le champ « policy » et dois‑je le définir ?

Le champ `policy` contrôle si les systèmes d’IA sont autorisés à ingérer une section, une sous‑section ou une entrée.  
Valeurs possibles : `"allow"` et `"forbid"`.

Si vous omettez ce champ, OLAMIP hérite automatiquement de la politique du parent le plus proche.  
Si aucun parent ne définit de politique, la valeur par défaut est `"allow"`.

La plupart des sites peuvent simplement ignorer ce champ.  
Utilisez `"forbid"` uniquement si vous souhaitez exclure explicitement certaines pages ou sections.

## Quelle est la différence entre OLAMIP et un sitemap traditionnel ?

Un sitemap est un index de navigation, tandis qu’OLAMIP est **une carte de connaissance interprétable par machine**.

### Sitemap XML

- Liste les URL.  
- Fournit des indications de crawl.  
- Aide les moteurs de recherche à découvrir les pages.  
- Ne contient aucune sémantique.  
- Ne définit aucune hiérarchie de contenu.

### OLAMIP

- Définit des types de contenu (`page`, `project`, `doc_page`, etc.).  
- Définit des types de sections (`project_group`, `doc_category`, etc.).  
- Fournit des descriptions canoniques.  
- Fournit des résumés optimisés pour les LLM.  
- Fournit des tags et métadonnées.  
- Établit une hiérarchie explicite.  
- Donne aux systèmes d’IA une représentation structurée de l’ensemble du site.

En résumé :

- **Sitemap XML = « Voici mes pages. »**  
- **OLAMIP = « Voici ce que signifie mon site. »**

C’est pourquoi les systèmes d’IA peuvent utiliser OLAMIP beaucoup plus efficacement qu’un sitemap traditionnel.

## Un fichier OLAMIP contenant plus de 3 000 URL est‑il trop volumineux ?

Pas du tout.  
Un fichier contenant 3 000 entrées reste largement dans les limites normales.  
Même avec des résumés longs, la taille totale serait d’environ 6 Mo — négligeable pour les serveurs, navigateurs et systèmes d’IA modernes.

## Quelle est la taille typique d’une entrée OLAMIP ?

| Longueur du résumé | Taille approximative |
|---|---|
| Court | 300–600 octets |
| Long | 1–2 Ko |

## Qu’est‑ce qui rend OLAMIP évolutif malgré la taille potentielle des fichiers ?

- Structure hiérarchique réduisant la répétition.  
- Résumés concis (souvent < 500 caractères).  
- Compression efficace (Gzip ou Brotli réduit la taille de 70–90 %).  

## À partir de quelle taille les performances commencent‑elles à être affectées ?

| Taille du fichier | Nombre approx. d’URL | Impact |
|---|---:|---|
| 1–10 Mo | 500–5 000 | Parfaitement normal |
| 10–25 Mo | 5 000–12 000 | Légèrement plus lent |
| 25–50 Mo | 12 000–25 000 | Grand mais gérable |
| 50–100 Mo | 25 000–50 000 | Lourd, certains systèmes peuvent ralentir |
| 100+ Mo | 50 000+ | Trop volumineux pour de nombreux pipelines LLM |

La plupart des sites restent dans des limites sûres même avec des dizaines de milliers d’entrées.

## À quelle fréquence dois‑je le mettre à jour ?

Chaque fois que vous publiez un nouveau contenu ou modifiez des pages existantes.  
Traitez‑le comme un sitemap — mais pour l’IA.

## Dois‑je mettre à jour le fichier principal si j’utilise `olamip-delta.json` ?

Oui.  
Le fichier principal `olamip.json` doit toujours refléter l’état actuel de votre site.  
Le fichier optionnel `olamip-delta.json` ne contient que les changements récents.

## Dois‑je ajouter une balise `<link>` pour `olamip-delta.json` ?

Non.  
Seul le fichier principal `olamip.json` doit être référencé dans la section `<head>` de votre site.  
Une fois que les systèmes d’IA connaissent son emplacement, ils recherchent automatiquement le fichier delta dans le même répertoire.

## Les LLM liront‑ils réellement ce fichier ?

Oui — c’est précisément l’objectif.  
À mesure que l’adoption progresse, les LLM donneront la priorité aux sources structurées comme OLAMIP.

## Comment tester si un bot d’IA peut récupérer mon fichier `olamip.json` ?

Essayez ceci pour OpenAI, Anthropic et Perplexity :

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```
