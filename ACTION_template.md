---
doc_id: action_<slug>
rAwUrl: <URL_raw_du_doc_sur_github>
titre_palette: "<Titre affiché dans la palette>"
type_doc: action
version: v0
date_maj: YYYY-MM-DD
etat: courant
---

# ACTION — <Titre lisible de l’action>

## tl,dr
- <objectif de l’action en 1 phrase>
- <état d’avancement — livré / à venir>
- <points d’attention majeurs>

## Description
<Ton contenu libre : contexte, objectifs, règles métiers, exemples, cas limites, captures…>

## Roadmap vX → vY
### ✅ Livré
- [x] <item livré 1 — YYYY-MM-DD>
- [x] <item livré 2 — YYYY-MM-DD>
### 🔜 À venir
- [ ] <item acté à livrer 1>
- [ ] <item acté à livrer 2>
### 🧪 À évaluer
- [ ] <piste à étudier 1>
- [ ] <piste à étudier 2>
### ❌ Rejeté
- [ ] <idée rejetée 1 — raison courte>
- [ ] <idée rejetée 2 — raison courte>

## Logs
- YYYY-MM-DD: <changement court, factuel, éventuels [[wikilinks]] ou liens>
- YYYY-MM-DD: <changement court>

# Exemple

```
---
doc_id: action_importer_csv_wp
rAwUrl: https://raw.githubusercontent.com/pierregelas/pierregelas-fr-docs/refs/heads/main/ACTION-importer_un_csv_de_wordpress.md
titre_palette: "Importer un CSV Wordpress"
type_doc: action
version: v0
date_maj: 2025-10-18
etat: courant
---

# ACTION — Importer un CSV WordPress

## tl,dr
- Importe un export **CSV WordPress** et crée/met à jour des notes **.md** dans un dossier choisi (défaut **NEW**), en appliquant le **YAML maître** et un **corps conditionnel** (Vignette/Vidéo/Notes).
- **Preview** (dry-run) avec compteurs: **Créés**, **MAJ**, **Erreurs** + détail des MAJ (**identiques** vs **modifiées**).
- **Règles clés**: `post_titre_full = wp_titre`, `post_id = wp_id`, `maj_wp = false`, erreurs vers **NEW/ERRORS** (note dédiée enrichie).
- **Journal** automatique dans **NEW/LOGS/** avec récap et listes (wikilinks), et **sélecteur de dossier** dans la modale.

## Description
Cette action lit un CSV exporté de WordPress, normalise et mappe chaque ligne vers le YAML maître, puis génère le **corps** selon des règles simples: **Vignette** si image, sinon **Vidéo** si lien vidéo, sinon bloc **Notes**.
[… contenu de la description conservé ici sans modification …]
En sortie, chaque note est nommée d’après `wp_titre`, accède aux wikilinks de projets via les catégories `>` et respecte le formatage YAML commun à toutes les actions.
[… fin de la description …]

## Roadmap v0 → v1
### ✅ Livré
- [x] Sélecteur de CSV (fuzzy) avec **dry-run/preview** — 2025-10-17
- [x] Sélecteur de **dossier de sortie** (défaut `NEW`, même non-existant) — 2025-10-18
- [x] Récap modale: **Créés/MAJ/Erreurs** + **Nombre d’entrées (hors en-tête)** — 2025-10-18
- [x] Comptage **MAJ identiques** vs **MAJ modifiées** — 2025-10-18
- [x] Règles strictes: `post_titre_full = wp_titre`, `post_id = wp_id`, `maj_wp = false` — 2025-10-18
- [x] Gestion **erreurs**: notes dédiées enrichies (**NEW/ERRORS**) — 2025-10-18
- [x] **Journal .md** daté dans **NEW/LOGS/** + notice finale `+n, −m, ✖e` — 2025-10-18
- [x] **Corps conditionnel** (Vignette → Vidéo → Notes) — 2025-10-18
- [x] Règle `lien_projet` depuis `wp_categories` (séparateur `>`, wikilinks) — 2025-10-17
### 🔜 À venir
- [ ] Option d’exécution: **Créer seulement** / **MAJ seulement**
- [ ] **Paramètres** de l’action (dossier défaut, options de nommage, stricte Windows) dans Settings
- [ ] Pré-aperçu des **5 premiers titres** par section (Créés/MAJ/Erreurs) dans la modale
- [ ] Normalisation douce des **en-têtes CSV** (trim, BOM/insécables)
### 🧪 À évaluer
- [ ] Log “Modifiées”: afficher **par note** les champs **modifiés** avec **avant/après** (règles de troncature)
- [ ] Multi-sélection de **plusieurs CSV** (chaînage avec récap global)
- [ ] **Barre de progression** lors de l’import
- [ ] **Tests** unitaires/intégration (mapping, YAML, corps, I/O)
### ❌ Rejeté
- [ ] Fusion “tags+catégories (slugifiés)” en tags enrichis (option) — non pertinent pour cette action
- [ ] **Auto-quoting YAML** généralisé — écarté (on **nettoie la source** `post_descr` si HTML)

## Logs
- 2025-10-18: Ajout sélecteur de dossier; notice fin d’import; **journal .md** NEW/LOGS; preview **Nombre d’entrées (hors en-tête)**
- 2025-10-18: Comptage **MAJ identiques/modifiées**; log listes par catégories (wikilinks titre seul)
- 2025-10-18: Erreurs **enrichies** (index ligne, valeurs brutes, headers) + gestion **lignes vides** en erreur
- 2025-10-18: Corps **conditionnel** (Vignette/Vidéo/Notes), `maj_wp=false`; règles strictes `wp_titre/wp_id`
- 2025-10-17: Règle `lien_projet` depuis `wp_categories` via `>`; mapping images (alt/caption/…)
- 2025-10-15: Structure initiale de l’action et doc

```