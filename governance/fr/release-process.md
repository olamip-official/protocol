# Processus de Publication (Release Process)

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Ce document décrit le processus standard que j’utilise pour créer, tester et publier les versions de la spécification OLAMIP ainsi que des outils associés. Pour le moment, toutes les publications sont gérées et maintenues exclusivement par moi.

---

## 1. Gestion de Version

J’applique les règles du **Semantic Versioning 2.0.0** : https://semver.org/

- **MAJOR** — lorsque j’introduis des changements incompatibles avec les versions précédentes.  
- **MINOR** — lorsque j’ajoute de nouvelles fonctionnalités ou sections compatibles avec les versions existantes.  
- **PATCH** — lorsque je corrige des bugs, clarifie des formulations ou améliore la précision sans modifier le comportement.  

Exemple :  
`1.0.0` → `1.0.1` (patch), `1.1.0` (minor), `2.0.0` (major).

---

## 2. Modèle de Branches Git

J’utilise un workflow Git simple :

- `main` — la branche stable, toujours prête pour une publication.  
- Branches de fonctionnalités ou correctifs (ex. `spec/delta-formats`, `fix/tag-rules`) — branches courtes dédiées à des modifications spécifiques.  
- Branches de publication (ex. `release/v1.1.0`) — créées juste avant la sortie d’une version.  

---

## 3. Conditions Préliminaires à une Publication

Avant de commencer une publication, je vérifie que :

- Tous les changements prévus pour cette version sont fusionnés dans `main`.  
- Tous les contrôles automatisés (linting, validation de schéma, scripts de build) passent correctement.  
- Tous les tickets et notes liés à cette version sont fermés ou documentés.  
- `CHANGELOG.md` est mis à jour avec des entrées claires et lisibles.  
- La documentation est à jour, notamment :  
  - `file-format-specification.md`  
  - `olamip-delta-file-format-specification.md`  
  - `docs/introduction.md`  
  - `faq.md`  
- Les fichiers d’exemple `olamip.json` et `olamip-delta.json` restent valides selon les règles les plus récentes.  

---

## 4. Création d’un Release Candidate

1. Créer une branche de publication à partir de `main` :

   ```bash
   git checkout main
   git checkout -b release/vX.Y.Z
   ```

2. Mettre à jour les numéros de version (dans les outils, scripts ou artefacts si nécessaire).  
3. Mettre à jour `CHANGELOG.md` avec un en‑tête et les notes de version.  
4. Commiter les modifications :

   ```bash
   git add .
   git commit -m "chore(release): prepare v1.1.0"
   ```

5. Pousser la branche :

   ```bash
   git push origin release/vX.Y.Z
   ```

6. Relancer les validations locales ou CI et vérifier que tout est correct.  

---

## 5. Test du Release Candidate

Étant le seul responsable des publications, mes tests personnels constituent la validation principale :

- Je vérifie que les fichiers d’exemple `olamip.json` et `olamip-delta.json` respectent toujours le schéma.  
- Je m’assure que tous les exemples dans les documents de spécification sont cohérents avec les règles décrites.  
- Je relis attentivement les sections mises à jour pour détecter toute incohérence.  

Si je trouve des problèmes importants :

- Je les corrige dans la branche `release`.  
- Je mets à jour `CHANGELOG.md` et je commite à nouveau.  
- Je relance les validations avant de poursuivre.  

---

## 6. Finalisation de la Publication

Une fois le Release Candidate stable :

1. Taguer la version dans Git :

   ```bash
   git tag -a vX.Y.Z -m "Release v1.1.0"
   git push origin vX.Y.Z
   ```

2. Créer une publication GitHub (ou équivalent) :

   - Utiliser le tag `vX.Y.Z`.  
   - Copier la section correspondante de `CHANGELOG.md` dans la description de la release.  
   - Joindre les artefacts générés (schémas, outils CLI, archives snapshot), si applicable.  

---

## 7. Étapes Post‑Publication

Après la publication :

- Je mets à jour mes notes internes ou la roadmap concernant ce qui a été livré.  
- Je déplace les éléments reportés vers le backlog ou les notes de la prochaine version.  
- Si un canal de communication existe (Twitter, newsletter, GitHub Discussions), j’annonce éventuellement la nouvelle version et ses changements majeurs.  

---

## 8. Gouvernance et Supervision

Même en tant que mainteneur unique, je conserve une couche de gouvernance :

- Toute modification affectant le schéma principal, les champs obligatoires ou la version du protocole doit être documentée clairement dans le changelog.  
- Les changements importants du processus de publication doivent être consignés dans une note de gouvernance ou un enregistrement de décision sous `governance/decision-records/`.  

---

## 9. Rollback ou Patch

Si une version contient un bug critique :

- Je crée un patch dès que possible (ex. `v1.1.1`).  
- Je documente clairement la correction dans `CHANGELOG.md`.  
- Si nécessaire, je mets à jour la documentation ou j’indique que la version précédente est déconseillée.  

---

## 10. Automatisation et Évolutivité Future

J’automatise autant que possible :

- Les étapes répétitives comme :  
  - Vérification de la validité JSON  
  - Validation des fichiers d’exemple contre le schéma  
  - Génération ou linting du changelog  

Je garde le processus simple afin que, si des contributeurs rejoignent le projet à l’avenir, les mêmes étapes puissent être partagées facilement.

---

Ce processus de publication est maintenu par moi et peut être mis à jour via le workflow habituel lorsque j’améliore la procédure elle‑même.
