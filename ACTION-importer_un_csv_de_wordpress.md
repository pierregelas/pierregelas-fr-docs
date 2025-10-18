
# Tableau — Mapping YAML maître ↔ Source (WordPress CSV) + Règles

| **YAML maître**                                                                | **Correspondance mapping source (CSV WordPress)** | **Règles (calcul / dérivation / modif)**                                                                                                                                                 |
| ------------------------------------------------------------------------------ | ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| cover                                                                          | wp_img_url                                        | Prendre la **1ʳᵉ URL** si `wp_img_url` contient `\|\|`, sinon copier tel quel. `trim`.                                                                                                   |
| IMAGES: ______________________________________________________________________ | *(ligne de section – émise telle quelle)*         | *(aucune)*                                                                                                                                                                               |
| img_alt                                                                        | wp_img_alt                                        | Copier tel quel, `trim`. Si plusieurs valeurs séparées par `\|\|`, **convertir en liste YAML** (`- item`) avec `trim` de chaque élément.                                                 |
| img_descr                                                                      | wp_img_descr                                      | Copier tel quel, `trim`. Si plusieurs valeurs séparées par `\|\|`, **convertir en liste YAML** (`- item`) avec `trim` de chaque élément.                                                 |
| img_filename                                                                   | wp_img_filename                                   | Copier tel quel, `trim`. Si plusieurs valeurs séparées par `\|\|`, **convertir en liste YAML** (`- item`) avec `trim` de chaque élément.                                                 |
| img_id                                                                         | wp_img_id                                         | Copier tel quel, `trim`. Si plusieurs valeurs séparées par `\|\|`, **convertir en liste YAML** avec **chaque élément entre guillemets** (ex.: `- "12345"`).                              |
| img_legende                                                                    | wp_img_caption                                    | Copier tel quel, `trim`. Si plusieurs valeurs séparées par `\|\|`, **convertir en liste YAML** (`- item`) avec `trim` de chaque élément.                                                 |
| img_titre                                                                      | wp_img_titre                                      | Copier tel quel, `trim`. Si plusieurs valeurs séparées par `\|\|`, **convertir en liste YAML** (`- item`) avec `trim` de chaque élément.                                                 |
| img_url                                                                        | wp_img_url                                        | Copier tel quel, `trim`. Si plusieurs valeurs séparées par `\|\|`, **convertir en liste YAML** (`- item`) avec `trim` de chaque élément.                                                 |
| LIEN: ______________________________________________________________________   | *(ligne de section – émise telle quelle)*         | *(aucune)*                                                                                                                                                                               |
| lien_archives                                                                  | *(aucune colonne correspondante dans l’export)*   | Laisser vide.                                                                                                                                                                            |
| lien_journal                                                                   | *(aucune colonne correspondante dans l’export)*   | Laisser vide.                                                                                                                                                                            |
| lien_projet                                                                    | wp_categories                                     | **Names hiérarchiques**: `split('>')` → `trim` → **dédupliquer (ordre conservé)** → transformer chaque segment en **"[[Name]]"** (casse/accents conservés). Si vide ⇒ `lien_projet: []`. |
| lien_restes                                                                    | *(aucune colonne correspondante dans l’export)*   | Laisser vide.                                                                                                                                                                            |
| MAJ: ______________________________________________________________________    | *(ligne de section – émise telle quelle)*         | *(aucune)*                                                                                                                                                                               |
| maj_wp                                                                         | *(case à cocher calculée par l’action)*           | **Booléen**: toujours false                                                                                                                                                              |
| POST: ______________________________________________________________________   | *(ligne de section – émise telle quelle)*         | *(aucune)*                                                                                                                                                                               |
| post_cat                                                                       | wp_categories                                     | Convertir **names hiérarchiques** en **liste**: `split('>')` → `trim` → **dédupliquer (ordre conservé)**.                                                                                |
| post_date                                                                      | wp_date                                           | Remplacer l'espace par `T` (ex.: `YYYY-MM-DD HH:MM:SS` → `YYYY-MM-DDTHH:MM:SS`). Si seule la date est fournie, **ajouter** `T00:00:00`.                                                  |
| post_descr                                                                     | wp_a_descr_gen                                    | Copier tel quel, `trim`.                                                                                                                                                                 |
| post_extrait                                                                   | wp_extrait                                        | Copier tel quel.                                                                                                                                                                         |
| post_id                                                                        | wp_id                                             | Copier tel quel (clé d’**idempotence**).                                                                                                                                                 |
| post_mod                                                                       | wp_date_modified                                  | Remplacer l'espace par `T` (ex.: `YYYY-MM-DD HH:MM:SS` → `YYYY-MM-DDTHH:MM:SS`). Si seule la date est fournie, **ajouter** `T00:00:00`.                                                  |
| post_perma                                                                     | wp_perma                                          | Copier tel quel (URL absolue).                                                                                                                                                           |
| post_titre_1                                                                   | wp_a_titre_gen                                    | Copier tel quel. **Fallback**: dériver depuis `wp_titre` par split sur —, – ou : (partie gauche).                                                                                        |
| post_titre_2                                                                   | wp_a_stitre_gen                                   | Copier tel quel. **Fallback**: dériver depuis `wp_titre` par split sur —, – ou : (partie droite).                                                                                        |
|  ost_titre_full                                                                | wp_titre                                          | **Égal à** `wp_titre` (copie stricte).                                                                                                                                                   |
| post_vid_url                                                                   | wp_a_videolink_gen                                | Copier tel quel si URL valide, sinon laisser vide.                                                                                                                                       |
| tags                                                                           | wp_tags                                           | **Séparer** sur `,` → `trim` → **slugify_wp** (minuscules, sans accents, espaces→`-`) → **dédupliquer** (ordre conservé) → **liste YAML** (`- item`).                                    |
| WP: ______________________________________________________________________     | *(ligne de section – émise telle quelle)*         | *(aucune)*                                                                                                                                                                               |
| wp_carnet_link                                                                 | wp_carnet_link                                    | Copier tel quel (URL ou vide).                                                                                                                                                           |
| wp_carnet_on                                                                   | wp_carnet_on                                      | **Booléen** : **true** si la valeur source est **non vide** ; **false** si **vide**.                                                                                                     |
| wp_status                                                                      | wp_status                                         | Copier tel quel (`publish / pending / draft / …`).                                                                                                                                       |

## Règle — Peuplement de `lien_projet` depuis `wp_categories`

`lien_projet` est généré **directement** depuis `wp_categories` qui contient les **names** (pas les slugs) hiérarchisés par le caractère `>` : on split sur `>`, on trim les espaces, on ignore les segments vides, on **conserve casse et accents**, on **déduplique en gardant l’ordre**, puis on transforme chaque segment en wikilink au format **"[[Name]]"** (guillemets doubles inclus) pour produire la liste YAML ; si `wp_categories` est vide/absent ⇒ `lien_projet: []` (ex.: `Vidéo>Minutes` ⇒ `[[Vidéo]]`, `[[Minutes]]`).

## Logs
- 2025-10-17 — Ajout de la modale de prévisualisation (dry‑run) avec boutons **Mettre à jour** / **Annuler**.
- 2025-10-17 — Branchement I/O Obsidian (FileSystemAdapter), picker CSV via **FuzzySuggestModal**.
- 2025-10-17 — Gestion des erreurs : création automatique de notes dans `NEW/ERRORS/` pour les lignes en échec.
- 2025-10-17 — `maj_wp` fixé à **false** (les infos viennent de WP, pas de renvoi).
- 2025-10-17 — Alignement mapping v2 : `tags` séparateur `,`, `img_id` listé **avec guillemets**, `post_date`/`post_mod` en ISO avec `T`.
- 2025-10-17 — Émetteur YAML : **auto‑quoting** des scalaires et des éléments de liste à risque (guillemets, `: `, etc.).
- 2025-10-17 — Sanitation des fichiers : conserver **?** et **!** et les `"` (compat Windows à discuter).

## Roadmap
- [x] Import basique CSV → YAML maître, création/mise à jour idempotente par `post_id`.
- [x] Picker CSV + prévisualisation (dry‑run) avec confirmation.
- [x] Gestion erreurs → `NEW/ERRORS/` + log.
- [x] `maj_wp` = false.
- [ ] création conditionnelle du corps de la note
- [ ] Détail de prévisualisation par ligne (premiers titres créés/MAJ/erreurs).
- [ ] Option multi‑sélection de CSV et exécution séquentielle.
- [ ] Barre de progression et estimation (grossière) sur gros CSV.
- [ ] Option Dry‑run par défaut (toggle dans la modale).
- [ ] Paramètres d’action (dossier sortie, règles de nommage) via Settings.
- [ ] Normalisation d’URL renforcée (protocol‑relative `//`, `www.`) optionnelle.
- [ ] Compat noms de fichiers **Windows** (mode strict optionnel).
- [ ] Tests unitaires `core/transform`, `yamlMaster`, `mapping.wordpress`.
- [ ] Tests d’intégration sur un échantillon CSV (3 cas: simple, multi‑images, update).
- [ ] Panneau de logs consultable (export .md) au lieu de simples Notices.
- [ ] Documentation utilisateur (README + doc Obsidian).


## Corps de la note
Dans l'ordre :
Pour toutes : (si plusieurs entrées prendre la 1ère)
```
## Vignette

![](wp_img_url)
```

Si post_vid_url / wp_a_videolink_gen existe (non vide) :

```
## Vidéo

![](wp_a_videolink_gen)
```

Pour toutes : 

```
## Notes

![[nomdelanote + _notes]]
```


### Exemple 1, sans wp_a_videolink_gen
- titre de la note : Arbres, immeuble, nuit. Paris. Journal du mardi 2 janvier 2024.
- wp_img_url: https://www.pierregelas.fr/wp-content/uploads/2024/11/310_9142_2024-01-02_21h07_WP.webp

résultat : 

```
## Vignette

![](https://www.pierregelas.fr/wp-content/uploads/2024/11/310_9142_2024-01-02_21h07_WP.webp)

## Notes

![[Arbres, immeuble, nuit. Paris. Journal du mardi 2 janvier 2024._notes]]
```

### Exemple 2, avec wp_a_videolink_gen
- titre de la note : Ravie et Raoûl glandent près de la fenêtre. Samedi 30 novembre 2024 à 17h51.
- wp_img_url: https://www.pierregelas.fr/wp-content/uploads/2025/06/2024-11-30-17-51_ravieetr_mvign.webp
- wp_a_videolink_gen: https://youtu.be/aEdyZYL0lWs

```
## Vignette

![](https://www.pierregelas.fr/wp-content/uploads/2025/06/2024-11-30-17-51_ravieetr_mvign.webp)

## Vidéo 

![](https://youtu.be/aEdyZYL0lWs)

## Notes

![[Ravie et Raoûl glandent près de la fenêtre. Samedi 30 novembre 2024 à 17h51._notes]]
```

## AJOUT : demander le chemin
Dans la modale, en haut, demander en liste déroulante (tous les dossiers existants dans la vault) ou enregistrer les fichiers

## Notes de test
le titre de la note (wp_title) n'est pas complet ! il manque le point à la fin, exemple : 

le yaml : 
```yaml
---
cover: https://www.pierregelas.fr/wp-content/uploads/2024/08/policebadaud_vign.webp
IMAGES: ______________________________________________________________________
img_alt:
- Intervention policière et badauds, avenue Gambetta, Paris 20e. Samedi 10 août 2024 à 10h50.
img_descr: []
img_filename:
- policebadaud_vign.webp
img_id:
- "6079"
img_legende:
- Intervention policière et badauds, avenue Gambetta, Paris 20e. Samedi 10 août 2024 à 10h50.
img_titre:
- policebadaud_vign
img_url:
- https://www.pierregelas.fr/wp-content/uploads/2024/08/policebadaud_vign.webp
LIEN: ______________________________________________________________________
lien_archives:
lien_journal:
lien_projet:
- "[[Vidéo]]"
- "[[Minutes]]"
lien_restes:
MAJ: ______________________________________________________________________
maj_wp: false
POST: ______________________________________________________________________
post_cat:
- Vidéo
- Minutes
post_date: 2024-08-10T08:50:00
post_descr:
post_extrait:
post_id: 6078
post_mod: 2024-08-11T14:19:36
post_perma: https://www.pierregelas.fr/intervention-policiere-et-badauds-avenue-gambetta-paris-20e-samedi-10-aout-2024-a-10h50/
post_titre_1: Intervention policière et badauds, avenue Gambetta, Paris 20e.
post_titre_2: Samedi 10 août 2024 à 10h50.
post_titre_full: Intervention policière et badauds, avenue Gambetta, Paris 20e. Samedi 10 août 2024 à 10h50.
post_vid_url: https://youtu.be/e7zVJHkpYfY
tags:
- paris-20e
- police
WP: ______________________________________________________________________
wp_carnet_link:
wp_carnet_on: false
wp_status: publish
---
```

le titre de la note : Intervention policière et badauds, avenue Gambetta, Paris 20e. Samedi 10 août 2024 à 10h50

devrait être : Intervention policière et badauds, avenue Gambetta, Paris 20e. Samedi 10 août 2024 à 10h50.

Donc wp_title n'est pas utilisé exactement ! C'est important qu'il le soit car si j'exporte ensuite un csv d'obsidian pour modif vers wordpress, cela va modifier le titre du post sur wordpress

## Logs