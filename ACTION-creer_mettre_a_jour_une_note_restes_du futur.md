---
doc_id: action_restes_du_futur
titre: "ACTION - Créer-Mettre à jour une note Restes du futur"
type_doc: action
version: v1
version_precedente: none
date_maj: 2025-10-15
etat: courant
notes: "Même mécanique que 'Archives du futur'. Spécificités: lien source = lien_restes; mots-clés de titres = 'Restes'; post_cat=['photo','restes-du-futur']; lien_projet=[[Photo]], [[Restes du futur]]; image _WP→_REI; fallback P2 par post_date + post_cat=restes-du-futur; modale Diff identique (cases décochées si identiques; rename proposé; modale 'parfaitement à jour' si 0 diff)."
---

# Différences avec « Archives du futur »

- Source de lien côté Journal: utiliser `lien_restes` (pas `lien_archives`).
- Mot-clé des titres: **Restes** remplace **Archives** dans `post_titre_1`, `post_titre_2`, `post_titre_full`.
- Catégorie de la note cible: `post_cat: ["photo","restes-du-futur"]`.
- Projet: `lien_projet: [[Photo]], [[Restes du futur]]`.
- Dans la note Restes: `lien_restes:` **vide**; `lien_archives:` **copié** depuis la note Journal.
- Image: conversion `_WP.webp` → **`_REI.webp`** (et `cover` = même nom).
- P2 fallback: recherche par `post_date` + `post_cat` contient `restes-du-futur`.
- Modale Diff: identique à Archives (cases décochées si valeurs identiques; renommage proposé si le titre cible change; modale “parfaitement à jour” si 0 changement).

## Logs

### 🗓️ 2025-10-15 — Processus 1 (création Restes du futur)
- Source: note **Journal photo** (`post_cat` contient `journal-photo`).
- Titre cible: dérivé de `lien_restes` (texte sans `[[ ]]`), mot-clé **Restes**.
- YAML cible: mêmes clés qu’Archives; différences: `post_cat: ["photo","restes-du-futur"]`, `lien_projet: [[Photo]], [[Restes du futur]]`, `lien_restes:` vide, `lien_archives:` copié depuis Journal.
- Image: `_WP.webp` → **`_REI.webp`**; `cover` = image; `img_alt` = `post_titre_1`; `img_legende` = `post_titre_full`.
- Corps: **Photo** (image `_REI.webp`) et **Notes** (`[[post_titre_full_notes]]`).

### 🗓️ 2025-10-15 — Processus 2 (mise à jour Restes du futur)
- Résolution: via `lien_restes` ou fallback par `post_date` + `post_cat` contient `restes-du-futur`.
- Diff (A): `post_date`, `tags` (cases cochées **seulement** si différents).
- Diff (B), si le titre Journal a changé: `post_titre_1/2/full` (mot-clé **Restes**), `img_filename` **(_REI_)** + `cover`, `img_alt`, `img_legende`, `lien_journal`, `lien_archives` (copié depuis Journal); cases cochées **seulement** si différents.
- Rename: proposé si `post_titre_full` change (nom de fichier = nouveau titre).
- Zéro différence: modale d’information « La note “Restes du futur” est parfaitement à jour ! ».
