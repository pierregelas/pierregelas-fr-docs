---
doc_id: action_importer_csv_wp
rAwUrl: https://raw.githubusercontent.com/pierregelas/pierregelas-fr-docs/refs/heads/main/ACTION-importer_un_csv_de_wordpress.md
titre_palette: "Importer un CSV Wordpress"
type_doc: action
version: v0
date_maj: 2025-10-15
etat: courant
---

## tl,dr

> [!info] 
> résumé tld,dr en langage naturel sans jargon de ce que cette action fait actuellement. Accompagné d'un court paragraphe sur les ajouts futurs basé sur ## Roadmap v0 to v1 ### à faire.
> quand toutes les tâches de ### à faire sont faites, proposer de passer à la version suivante et créer une nouvelle section roadmap





## Roadmap v0 to v1


> [!info] Règles pour toutes les actions
> Chaque section de roadmap va toujours d'une version entière à une autre, v0 vers v1, v1 vers v2, etc. 


### fait

> [!info]
> basé sur les logs, déplacement automatique des checkbox plus bas. La forme doit être : 
> ```
> - [ ] copie de la description - date du déplacement en section `fait` // (tout le texte en rayé)
> ```

> - [x] Import basique CSV → YAML maître, création/mise à jour idempotente par `post_id`.

- [x] Picker CSV + prévisualisation (dry‑run) avec confirmation.
- [x] Gestion erreurs → `NEW/ERRORS/` + log.
- [x] `maj_wp` = false.

### à faire
- [ ] création conditionnelle du corps de la note
- [ ] **Options**: Switch **Créer seulement / MAJ seulement**.

- [ ] **Options**: Dry‑run par défaut (toggle dans la modale).
- [ ] **Options**: Paramètres (dossier sortie, strict noms Windows). Paramètres d’action (dossier sortie, règles de nommage) via Settings.
- [ ] Normalisation d’URL renforcée (protocol‑relative `//`, `www.`) optionnelle.
- [ ] Panneau de logs consultable (export .md) au lieu de simples Notices.
- [ ] Documentation utilisateur (README + doc Obsidian).


### à évaluer

> [!info]
> On fera peut-être, c'est moi qui décide de faire en déplaçant dans la section à `faire`

- [ ] Détail de prévisualisation par ligne (premiers titres créés/MAJ/erreurs).
- [ ] Option multi‑sélection de CSV et exécution séquentielle.
- [ ] Barre de progression et estimation (grossière) sur gros CSV.
- [ ] Compat noms de fichiers **Windows** (mode strict optionnel).
- [ ] Tests unitaires `core/transform`, `yamlMaster`, `mapping.wordpress`.
- [ ] Tests d’intégration sur un échantillon CSV (3 cas: simple, multi‑images, update).
### rejeté


## Description

> [!info] 
> description précise et exhaustive des informations nécessaires à la compréhension et au développement des actions.
> Format d'entrées/sorties, avec exemples, notes, , tests, etc


### Tableau — Mapping YAML maître ↔ Source (WordPress CSV) + Règles

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
| post_titre_full                                                                | wp_titre                                          | **Égal à** `wp_titre` (copie stricte).                                                                                                                                                   |
| post_vid_url                                                                   | wp_a_videolink_gen                                | Copier tel quel si URL valide, sinon laisser vide.                                                                                                                                       |
| tags                                                                           | wp_tags                                           | **Séparer** sur `,` → `trim` → **slugify_wp** (minuscules, sans accents, espaces→`-`) → **dédupliquer** (ordre conservé) → **liste YAML** (`- item`).                                    |
| WP: ______________________________________________________________________     | *(ligne de section – émise telle quelle)*         | *(aucune)*                                                                                                                                                                               |
| wp_carnet_link                                                                 | wp_carnet_link                                    | Copier tel quel (URL ou vide).                                                                                                                                                           |
| wp_carnet_on                                                                   | wp_carnet_on                                      | **Booléen** : **true** si la valeur source est **non vide** ; **false** si **vide**.                                                                                                     |
| wp_status                                                                      | wp_status                                         | Copier tel quel (`publish / pending / draft / …`).                                                                                                                                       |

### Règle — Peuplement de `lien_projet` depuis `wp_categories`

`lien_projet` est généré **directement** depuis `wp_categories` qui contient les **names** (pas les slugs) hiérarchisés par le caractère `>` : on split sur `>`, on trim les espaces, on ignore les segments vides, on **conserve casse et accents**, on **déduplique en gardant l’ordre**, puis on transforme chaque segment en wikilink au format **"[[Name]]"** (guillemets doubles inclus) pour produire la liste YAML ; si `wp_categories` est vide/absent ⇒ `lien_projet: []` (ex.: `Vidéo>Minutes` ⇒ `[[Vidéo]]`, `[[Minutes]]`).

### Corps de la note
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


#### Exemple 1, sans wp_a_videolink_gen
- titre de la note : Arbres, immeuble, nuit. Paris. Journal du mardi 2 janvier 2024.
- wp_img_url: https://www.pierregelas.fr/wp-content/uploads/2024/11/310_9142_2024-01-02_21h07_WP.webp

résultat : 

```
## Vignette

![](https://www.pierregelas.fr/wp-content/uploads/2024/11/310_9142_2024-01-02_21h07_WP.webp)

## Notes

![[Arbres, immeuble, nuit. Paris. Journal du mardi 2 janvier 2024._notes]]
```

#### Exemple 2, avec wp_a_videolink_gen
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




### AJOUT : demander le chemin
Dans la modale, en haut, demander en liste déroulante (tous les dossiers existants dans la vault) ou enregistrer les fichiers


## Notes de test

> [!info] 
> notes de test que je copie colle au fur et à mesure, je gère cette section

## Logs

> [!info] 
> coller ici les logs de chaque session, utiliser toujours la mêe forme : 
> FORME À DÉFINIR
> géré par chatgpt pour l'update, la maintenance, les ajouts, le maintien de la cohérence de style.


- 2025-10-17 — Ajout de la modale de prévisualisation (dry‑run) avec boutons **Mettre à jour** / **Annuler**.
- 2025-10-17 — Branchement I/O Obsidian (FileSystemAdapter), picker CSV via **FuzzySuggestModal**.
- 2025-10-17 — Gestion des erreurs : création automatique de notes dans `NEW/ERRORS/` pour les lignes en échec.
- 2025-10-17 — `maj_wp` fixé à **false** (les infos viennent de WP, pas de renvoi).
- 2025-10-17 — Alignement mapping v2 : `tags` séparateur `,`, `img_id` listé **avec guillemets**, `post_date`/`post_mod` en ISO avec `T`.
- 2025-10-17 — Émetteur YAML : **auto‑quoting** des scalaires et des éléments de liste à risque (guillemets, `: `, etc.).
- 2025-10-17 — Sanitation des fichiers : conserver **?** et **!** et les `"` (compat Windows à discuter).
