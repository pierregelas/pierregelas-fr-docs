---
doc_id: action_gestion_tags
titre: "ACTIONS - Gestion et mise à jour des tags"
type_doc: action
version: v1
version_precedente: none
date_maj: 2025-10-15
etat: courant
notes: "CSV→table depuis le dernier wp_tags/wp_tags_csv/AAAA-MM-JJ_export_tags.csv. Diff en sections A/B/C/D + I (Tags à importer dans WP). A/B/C cochés par défaut; D/I lecture seule. Zéro diff: modale OK + mise à jour YAML (tags_last_udpdate, tags_last_csv, wp_update). Table réécrite de façon déterministe (tri par slug). Backup avant écriture."
---

## Liste des Actions

- **`Action Mise à jour tags WP > Obsidian`**

**à planifier :** 
- **`Action vérifier les tags des notes`** (option filtrer par post_cat ou lien_projet)
	- ouvre une fenêtre de diff montrant les problèmes
		- conditions et critères à lister
		
- **`Action générer/mettre à jour des stats tags`**

## Description


Dossier **`wp_tags`** toujours à la racine de la vault, contient : 

- **dossier `wp_tags_csv` :** contient les exports CSV venant de wordpress, 
	- nommés toujours dans cette forme : `AAAA-MM-JJ_export_tags.csv`
		- exemple : `2025-10-08_export_tags.csv`
	- les colonnes sont TOUJOURS : 
		- **`wp_tags_id`** : id unique du tag, assigné par WP uniquement
		- **`wp_tags_name`** : le nom du tag
		- **`wp_tags_slug`** : le slug du tag
		- **`wp_tags_count`** : nombre d'occurrences du tags dans WP à la date. Servira pour des stats ultérieurement.
	- la date est la date d'export et sert de référence.
	- un nouveau fichier est exporté périodiquement, tous les fichiers antérieurs sont conservés, le dernier fichier en date servant de référence pour la mise à jour wp>obsidian.
- **fichier .md `ob_tags_table.md` :**
	- table markdown contenant :
	- yaml : 
		- **`tags_last_udpdate`** : date et heure yaml contenant la date de la dernière mise à jour de **`ob_tags_table.md`**
		- **`tags_last_csv`** : texte yaml contenant le lien wiki du dernier csv utliisé pour la dernière mise à jour
		- **`wp_update`** : checkbox informant si il faut exporter de obsidian vers WP la liste de tags contenues dans **`ob_tags_table.md`**
	- les colonnes sont TOUJOURS : 
		- **`ob_tags_id`** :  correspond à **`wp_tags_id`**
		- **`ob_tags_name`** :  correspond à **`wp_tags_name`**
		- **`ob_tags_slug`** :  correspond à **`wp_tags_slug`**
		- **`ob_tags_count`** :  correspond à **`wp_tags_count`**

 

> [!info]
> L’id et le slug d'un tag sont FORCÉMENT UNIQUES, doublons impossibles dans wp, l’id est assigné par wp lors de sa création dans wp. Le couple id/slug est également UNIQUE. Le count est également calculé par WP.

### Cas pratiques (non exhaustifs) : 

1. **Cas création WP**
- Un tag qui n'existait nul part est créé dans WP
- Il apparait dans le dernier export csv importé.
- Après lancement de l'action il est importé au complet dans **`ob_tags_table.md`**
- Il est maintenant disponible dans l'ajout de tags d'actions de création de notes (non implémenté encore)

2. **Cas création Obsidian** (manuel ou par outil)
- un tag est ajouté dans obsidian
	- soit manuellement dans **`ob_tags_table.md`**, uniquement dans les colonnes **`ob_tags_name`** et **`ob_tags_slug`**
	- soit grace à la création de tags dans les actions de création de notes (non implémenté encore), uniquement dans les colonnes **`ob_tags_name`** et **`ob_tags_slug`**
- le tag est maintenant disponible dans l'ajout de tags d'actions de création de notes (non implémenté encore)
- **`ob_tags_slug`** **`wp_update`**: true
- à intervalle régulier Les tags créée dans obsidian sont crées dans WP (par moi-même, à la main)
- Au prochain csv importé de wp, et après l'action mise à jour, ce nouveau tag est mis à jour avec son id définitif et son count généré par wp.

3. **Cas modification de slug et name dans WP (l'id ne change pas)**
*Là je sais pas trop comment résoudre ça, aide-moi.*

4. **Cas modification de slug et name dans Obsidian d'un tag déjà présent dans WP (l'id est déjà là)**
*Là je sais pas trop comment résoudre ça, aide-moi.* La solution peut être de NE PAS MODIFIER  de slug et name dans Obsidian d'un tag déjà présent dans WP, mais de le faire directement dans WP (la modif sera mise à jour ultérieurement lors de la mise à jout)

Dans tous les cas il faut trouver la méthode la plus SIMPLE et légère, et préférer que ça implique une action de mapart plutôt que de complexifier tout.
### Action Mise à jour tags WP > Obsidian
- un nouvel export `AAAA-MM-JJ_export_tags.csv` est fait et est placé dans **dossier `wp_tags_csv`**
- Lancer l'action **`Mise à jour tags WP > Obsidian`**
- Une modale présente les diffs (avec coche), avec boutons Mettre à jour et Annuler. La modale sépare les différentes mises à jour :
**Critères :** 
	- **Nouveaux Tags :** Tout tag nouveau dans le csv doit être ajouté au tableau de **`ob_tags_table.md`** suivant ces conditions (AND)
		- id ET slug absent de **`wp_tags_table.md`**
			- ajout d'id, slug, name et count.
	- **Modification de tags :** Si l'id existe déjà dans **`ob_tags_table.md`**, mais le slug correspondant est différent : 
		- remplacer **`ob_tags_name`** par **`wp_tags_name`**
		- remplacer **`ob_tags_slug`** par **`wp_tags_slug`**
		- remplacer **`ob_tags_count`** par **`wp_tags_count`**
	- **Import d'id** : Si l'id n'existe pas dans **`ob_tags_table.md`**, mais que le slug est identique (ligne de tag sans id, créé auparavant directement dans **`ob_tags_table.md`**) :
		- ajouter **`wp_tags_id`** dans **`wp_tags_id`**
		- ajouter **`wp_tags_count`** dans **`ob_tags_count`**
	- **Tags à importer dans WP :** Si **`wp_tags_id`** n'existe pas dans **`ob_tags_id`** de **`ob_tags_table.md`** ET que le **`wp_tags_slug`** est différent : ne rien faire, cela veut dire que le tag n'a pas été créé encore dans WP
	- **Problèmes** : quand un cas de figure non listé plus haut se présente, pas de résolution maintenant, juste pour voir si d'autres cas pourraient surgir. 
		- Exemple : un tag dans **`ob_tags_table.md`** a bien id, name, slug et count, mais n'apparait pas du tout dans l'export CSV.
	- **Après validation**, changer dans le yaml de **`ob_tags_table.md`** : 
		- **`tags_last_udpdate`**: inscrire la date et heure courante
		- **`tags_last_csv`**: inscrire le lien wiki du fichier csv utilisé
		- Si : il y avait des **Tags à importer dans WP** dans la diff, passer en `true` **`wp_update`**, si ce n'est pas déjà le cas



## Options
- options de backup : Créer avant de lancer les changements à **`ob_tags_table.md`** une sauvegarde (.bak ?) de **`ob_tags_table.md`** au cas où, et ajouter un bouton Revenir à la dernière sauvegarde à la modale de l'action (si sauvegarde existante).


## Notes relatives aux autres actions
### Actions de création de notes
Après avoir crée cette Action mise à jour tags, nous allons implémenter une nouvelle fonctionnalité dans les actions Journal et Minutes : ajouter des tags à la note, avec auto-complétion, qui viendront se placer dans le yaml **tags** de la note, en liste yaml. Cette fonction pourra également créer un nouveau tag (slug et name), qui viendront s'ajouter à la fin du tableau **`ob_tags_table.md`**. C'est comme ça que sont repérés les **Tags à importer dans WP**, ils n'ont qu'un slug et un name, pas d'id et count, et ces slug et name sont absents du csv WP.
Cette fonction d'ajouts de tags pourra resservir pour n'importe quelle nouvelle action non créée encore utilisant le système de tags, donc bien penser à la mutualisation pour le futur.

### Actions de gestion et mise à jour des catégories wp
à planifier.
Utilisera la même méthode, la même arborescence, les mêmes critères de mise à jour et les mêmes actions supplémentaires que les actions tags.

## Logs

### 🗓️ 2025-10-15 — Définition de la méthode (CSV → table Obsidian)
- CSV attendu: colonnes wp_tags_id, wp_tags_name, wp_tags_slug, wp_tags_count (UTF-8/BOM/CRLF ok).
- Normalisation: id entier ou vide, slug trim+lower (sans espaces), name trim, count entier ≥ 0.
- Appariement: clé primaire=id quand présent; sinon fallback=slug.
- Catégories de diff:
  A. Nouveaux (CSV→Obsidian) → ajout complet (id/slug/name/count).
  B. Mises à jour (id identique) → remplacer name/slug/count par valeurs CSV.
  C. Import d’ID (slug identique, id manquant localement) → remplir id + mettre à jour count.
  D. Problèmes (lecture seule) → doublons/anomalies signalés.

### 🗓️ 2025-10-15 — Modale & écriture (UX et sécurité)
- Modale: sections A/B/C cochées par défaut si différents; D décochée (lecture seule); affichage Avant → Après; boutons Annuler/Appliquer.
- Zéro diff: modale d’information « La table des tags est parfaitement à jour ! ».
- Écriture:
  - Backup: wp_tags/backup/ob_tags_table_YYYYMMDD-HHmm.md.
  - YAML de ob_tags_table.md: tags_last_udpdate (maintenant), tags_last_csv ([[AAAA-MM-JJ_export_tags.csv]]), wp_update=true si présence de tags locaux sans id dont le slug n’existe pas dans le CSV; sinon false.
  - Table: reconstruction complète et déterministe, tri par slug; colonnes ob_tags_id | ob_tags_name | ob_tags_slug | ob_tags_count.

### 🗓️ 2025-10-15 — Plan de tests validé (A/C/D1/I/0)
- **A — Nouveaux (CSV → local)**: ajout d’un tag inédit dans le CSV → Section A (coché) → Appliquer → ligne ajoutée dans la table, YAML mis à jour (date, CSV, `wp_update` recalculé).
- **C — Import d’ID (slug identique, id manquant local)**: ligne locale sans `id` avec `slug` existant dans le CSV → Section C (coché) → Appliquer → `id` rempli + `count` mis à jour.
- **D1 — Doublon de slug en local**: deux lignes locales avec le même `slug` → Section D (lecture seule), aucune écriture.
- **I — Tags à importer dans WP**: ligne locale sans `id` avec `slug` absent du CSV → Section I (lecture seule), table inchangée; YAML mis à jour (date, CSV, `wp_update: true`).
- **0 — Zéro diff pur**: table et CSV identiques, aucun “I” → modale “parfaitement à jour” + YAML mis à jour (date, CSV, `wp_update: false`).

### ℹ️ Clarifications
- **D2 — Doublon de slug dans le CSV**: non testé (WordPress garantit l’unicité du `slug`).
- **D3 — Collision de slug lors d’une mise à jour (B)**: retiré des tests (hors besoin).
