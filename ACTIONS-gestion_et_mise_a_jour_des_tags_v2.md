---
doc_id: action_gestion_tags
titre: ACTIONS - Gestion et mise à jour des tags
type_doc: action
version: v2
version_precedente: v1
date_maj: 2025-10-17
etat: courant
notes: "v2 (WP→Obsidian) : modale toujours affichée, logger conditionnel (toggle), booléens YAML natifs pour wp_update."
---


## Liste des Actions

- **`Action Mise à jour tags WP > Obsidian`**

**à planifier :** 
- **`Action vérifier les tags des notes`** (option filtrer par post_cat ou lien_projet)
	- ouvre une fenêtre de diff montrant les problèmes
		- conditions et critères à lister
		
- **`Action générer/mettre à jour des stats tags`**

## Descriptions

### Arborescence

- wp_tags:
	- ob_tags_table.md
	- wp_tags_csv
		- AAAA-MM-JJ_export_tags.csv
		- AAAA-MM-JJ_export_tags.csv
		- backup
			- ob_tags_table_AAAAMMJJ-hhmm.md


### AAAA-MM-JJ_export_tags.csv

**Format :**

| wp_tags_id                      | wp_tags_name | wp_tags_slug | wp_tags_count             |
| ------------------------------- | ------------ | ------------ | ------------------------- |
| id unique générée par wordpress | nom du tag   | slug du tag  | occurrences dans worpress |
| 72                              | Arbres       | arbres       | 49                        |

### ob_tags_table.md

**yaml :** 

```yaml
---
tags_last_udpdate: "AAAA-MM-JJ hh:mm" # auto: date et heure last update
tags_last_csv: "[[2025-10-08_export_tags.csv]]" # wikilink fichier utilisé pour last update
wp_update: false # checkbox: une mise à jour vers wordpress est nécessaire
---
```

**corps :**

| ob_tags_id                      | ob_tags_name | ob_tags_slug | ob_tags_count             | ob_tags_notes                                                            |
| ------------------------------- | ------------ | ------------ | ------------------------- | ------------------------------------------------------------------------ |
| id unique générée par wordpress | nom du tag   | slug du tag  | occurrences dans worpress | notes obsidian : obm (modifié dans obsidian) ou obc (créé dans obsidian) |
| 159                             | 1er Mai      | 1er-mai      | 9                         |                                                                          |
 ``

> [!info]
> L’id et le slug d'un tag sont FORCÉMENT UNIQUES, doublons impossibles dans wp, l’id est assigné par wp lors de sa création dans wp. 
> 
> Le couple id/slug est également UNIQUE, source de vérité : csv wordpress 
> 
> Le count est également calculé par WP.

> [!info] Process création/modification de tags dans Obsidian
> - Quand on créé un tag directement dans Obsidian (en bas de table), on ajoute `obc` dans la colonne ob_tags_notes
> 
> - Quand on modifie l'orthographe d'un slug et/ou name dans obsidian, on ajoute `obm` dans la colonne ob_tags_notes


___
## Actions 

### Action Mise à jour tags WP > Obsidian v2


- Ranger les colonnes id dans l'ordre croissant avant analyse
- L'analyse est faite par paire **`wp_id/wp_slug`**
- les updates se font uniquement dans ob_tags_table.md
commencer par la 1ère ligne du csv :

**Conditions et réactions :**

1. **`wp_id/wp_slug`** n'existe pas du tout dans la table :
    - ajouter une entrée au tableau :
        - id, slug, name, count
        - grouper dans **nouveaux tags**
        - passer à la ligne suivante
2. **`wp_id/wp_slug`** existe exactement dans la table :
    - update name if different
        - si name est différent : grouper dans **name update**
    - update count if different
        - si count est différent : grouper dans **count update**
    - passer à la ligne suivante
3. **`wp_id/wp_slug`** n'existe pas exactement dans la table :
    - id identiques, slugs différents,
        - **`ob_tags_notes`** est obm :
            - ne rien synchroniser
            - grouper dans **tags à modifier**
        - **`ob_tags_notes`** est vide :
            - update slug, name, count if different
            - grouper dans **name update**
            - passer à la ligne suivante
    - slugs identiques, id différents
        - **`wp_tags_id`** n'est pas vide, **`ob_tags_id`** est vide, **`ob_tags_notes`** est obc
            - update id, update name if different, update count if different
            - clear **`ob_tags_notes`**
            - grouper dans **id update**
            - passer à la ligne suivante
4. **Tags à créer (info seule)** :
    - ligne **locale** avec **`ob_tags_id`** vide, **`ob_tags_slug`** **absent** du CSV, et **`ob_tags_notes` = `obc`**
    - ne rien synchroniser
    - grouper dans **tags à créer** (info seule)
5. **problèmes** (fourre-tout, sans conditions prédéfinies) :
mettre ici tout ce qui n’a pas pu être traité par les autres règles et qui présente une disparité; 
- si identique ⇒ ne pas afficher; 
- montrer simplement les valeurs présentes (CSV ↔ table), sans action.
#### Modale de diff

7 groupes : 
- **nouveaux tags**
- **id update**
- **name update**
- **count update**
- **tags à créer**
- **tags à modifier**
- **problèmes**

- bouton Mettre à jour
- bouton Annuler
- bouton Revenir au backup (si backup existe)

#### backup :
Créer avant de lancer les changements à **`ob_tags_table.md`** une sauvegarde (.bak ?) de **`ob_tags_table.md`** au cas où, et ajouter un bouton Revenir à la dernière sauvegarde à la modale de l'action (si sauvegarde existante). On ne garde qu'un seul backup.

**Modale (sélection & comportements)**

- Groupes **actionnables** (cochés par défaut): **Nouveaux tags**, **ID update**, **Name update**, **Count update**.
- Groupes **lecture seule** (décochés et non cochables): **Tags à créer**, **Tags à modifier** (pour la partie `slug/name` protégée par `obm`), **Problèmes**.
- S’il n’y a **aucun item actionnable**, on affiche une **modale d’information** listant au besoin **Tags à créer**/**Problèmes**, sinon le message “La table des tags est parfaitement à jour !”.

**Écriture (après validation)**

- **Backup** unique du fichier: `wp_tags/backup/ob_tags_table_YYYYMMDD-HHmm.md`.
- **Table**: on **reconstruit** la table à partir de l’état final, **triée** (stabilité visuelle).
- **YAML** de `ob_tags_table.md` mis à jour:
    - `tags_last_udpdate`: horodatage actuel.
    - `tags_last_csv`: `[[AAAA-MM-JJ_export_tags.csv]]`.
    - `wp_update`: **booléen natif** (checkbox) → `true` s’il reste au moins un tag **local** avec `ob_tags_id` **vide** et `ob_tags_slug` **absent** du CSV (i.e. des **Tags à créer**), sinon `false`.

**Notes d’implémentation**

- On ne **remplit jamais** `ob_tags_id` pour un tag **créé localement**; l’id reste **vide** tant qu’il n’existe pas dans WP. Le marqueur d’intention est **`obc`**.
- `obm` protège les **`slug/name`** locaux contre les remplacements automatiques; les **`count`** restent synchronisables.
- Le **tri** ne sert qu’à l’affichage stable; la logique d’analyse ne dépend pas du tri.
___
## Notes relatives aux autres actions
### Actions de création de notes
Après avoir crée cette Action mise à jour tags v2, nous allons implémenter une nouvelle fonctionnalité dans les actions Journal et Minutes : ajouter des tags à la note, avec auto-complétion, qui viendront se placer dans le yaml **tags** de la note, en liste yaml. Cette fonction pourra également créer un nouveau tag (slug et name), qui viendront s'ajouter à la fin du tableau **`ob_tags_table.md`**, in ajoutant "obc" à la colonne ob_tags_notes (sert à repérer les ajouts dans obsidian). 
Cette fonction d'ajouts de tags pourra resservir pour n'importe quelle nouvelle action non créée encore utilisant le système de tags, donc bien penser à la mutualisation pour le futur.

### Actions de gestion et mise à jour des catégories wp
à planifier.
Utilisera la même méthode, la même arborescence, les mêmes critères de mise à jour et les mêmes actions supplémentaires que les actions tags.

## Logs

- **2025-10-17 10:52** — v2 (WP → Obsidian) + logger conditionnel  
  - Modale toujours affichée (avec fallback d’info si erreur d’ouverture).  
  - **YAML booléens natifs**: `wp_update: true|false` (plus de guillemets) via `yamlPatch.ts` typé.  
  - **Logger Markdown**: fichiers `wp_tags/logs_tests/YYYYMMDD-HHmm_update-tags-from-last-csv.md` (entrées, diff texte, sorties).  
  - **Paramètre**: toggle *Écrire des logs d’actions* (ON par défaut).  
  - **Robustesse**: écriture de fichiers consolidée (`getFileByPath`/`getAbstractFileByPath`).  
  - Zéro actionnable: mise à jour YAML + log “informative” (si logging ON).  

## Roadmap

- Mettre en forme la note de logs qui ne va pas

## Encadré (hors-scope, à noter pour plus tard)

> - **Sélecteur de CSV** (choisir un fichier précis au lieu du “dernier” automatiquement).
>     
> - **Niveaux de verbosité du logger** (normal/minimal) pour alléger certains logs.
>     
> - **Rotation des logs** (conserver les _N_ derniers fichiers, ex. 30).
>