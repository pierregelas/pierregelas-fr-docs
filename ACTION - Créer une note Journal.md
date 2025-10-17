---
doc_id: action_journal
titre: "ACTION - Créer une note Journal"
type_doc: action
version: v1
version_precedente: none
date_maj: 2025-10-15
etat: courant
notes: "Implémentation v0.1: commande opérationnelle, post_titre_2 sans heure (“Journal du <jour date>.”), format image AAAA-MM-JJ-hh-mm_idphoto_WP.webp, YAML complet avec séparateurs visibles (IMAGES/LIEN/MAJ/POST/WP), maj_wp: true, post_cat: [photo, journal-photo], nom de fichier = post_titre_full."
---


# ACTION : Créer une note _Journal_

---

## 1. Entrées demandées à l’utilisateur

Lors du déclenchement de l’action “Créer une note Journal”, une fenêtre demande 2 éléments essentiels :

1. **Nom du dossier**  
    Exemple :  
    `2024-11-23-17-05 - Cinéma Saint-André des Arts. Paris 6e.`  
    → Ce nom doit toujours suivre la structure :  
    **année-mois-jour-heure-minute – titre complet.**  
    (avec le tiret entouré de 2 espaces “ - ” séparant la date et le titre).

2. **Nom de l'image**  
    Exemple :  
    `2024-11-23-17-05_4075037_WP.webp`
    → Ce nom doit toujours suivre la structure :
    **année-mois-jour-heure-minute_idPhoto_WP.webp

> [!warning] ATTENTION !
> idPhoto peut contenir `-` ou `_` 
> pour le code, toujours utiliser, le `premier _` ou le `dernier _` comme références, qui sont constantes.

_(Les tags ne sont pas saisis ici : ils seront ajoutés manuellement dans la note après sa création.)_

___
## 2. Structure de la note créée

Le titre de la note est créé automatiquement.
La note se compose de deux parties :  
un bloc d’informations (YAML) et un corps (contenu visible).
### Nom du fichier .md

copie de `post_titre_full`

### 🧾 Bloc d’informations FULL (YAML)

```yaml
---
cover:                # auto : copie de `Nom de l'image`

IMAGES: ______________________________________________________________________

img_alt:              # auto : copie de `post_titre_1`
img_descr:            # vide
img_filename:         # auto : copie de `Nom de l'image`
img_id:               # vide
img_legende:          # auto : copie de `post_titre_full`
img_titre:            # vide
img_url:              # vide

LIEN: ______________________________________________________________________

lien_archives:        # auto : lien wiki dérivé de 'post_titre_full'
lien_journal:         # vide
lien_projet:          # pré-rempli
  - "[[Photo]]"
  - "[[Journal photo]]"
lien_restes:          # auto : lien wiki dérivé de 'post_titre_full'

MAJ: ______________________________________________________________________

maj_wp: true         # pré-rempli: true

POST: ______________________________________________________________________

post_cat:             # pré-rempli - slug category
  - photo
  - journal-photo
post_date:           # auto : dérivé de `Nom de dossier`
post_descr:          # vide
post_extrait:        # vide
post_id:             # vide
post_mod:            # vide
post_perma:          # vide
post_titre_1:        # auto : dérivé de `Nom de dossier`
post_titre_2:        # auto : dérivé de `Nom de dossier`
post_titre_full:     # auto : concatène `post_titre_1` + ` ` + `post_titre_2`
post_vid_url:        # vide
tags:                # vide

WP: ______________________________________________________________________

wp_carnet_link:      # vide
wp_carnet_on:        # vide
wp_status:           # vide
---
```

#### Explications

##### lien_archives: # auto : lien wiki dérivé de 'post_titre_full'

- Utilise `post_titre_full`. 
- Remplacer les points, virgules, points d'exclamation par des points d'interrogation, avec un espace devant : ` ?`
- Mettre en majuscule le premier caractère après un `?`
- Remplacer `Journal` par `Archives`

``
> [!example] Exemple
> `post_titre_full`: `Lampadaire, toile d'araignée. Journal du dimanche 26 janvier 2024.`
> 
> `lien_archives`: `[[Lampadaire ? toile d'araignée ? Archives du dimanche 26 janvier 2024 ?]]`


##### lien_restes: # auto : lien wiki dérivé de 'post_titre_full'       

- Utilise `post_titre_full`. 
- Remplacer les points, virgules, points d'exclamation par des points d'interrogation, avec un espace devant : ` ?`
- Mettre en majuscule le premier caractère après un `?`
- Remplacer `Journal` par `Restes`
``
> [!example] Exemple
> `post_titre_full`: `Lampadaire, toile d'araignée. Journal du dimanche 26 janvier 2024.`
> 
> `lien_archives`: `[[Lampadaire ? toile d'araignée ? Restes du dimanche 26 janvier 2024 ?]]`

##### post_date: # auto : dérivé de `Nom de dossier`

- Utiliser les 16 premiers caractères.
- `AAAA-MM-JJ-hh-mm - titre complet.` devient `AAAA-MM-JJThh:mm:00`

> [!example] Exemple
>  `Nom du dossier` : `2020-11-28-17-12 - Immeuble, ciel. Quai de la Seine, Paris 19e.`
>  
> `post_date`:  `2020-11-28T17:12:00`

##### post_titre_1: # auto : dérivé de `Nom de dossier`

- Utiliser tout après le séparateur " - " (espace, tiret, espace).
- Commence toujours par une majuscule, se termine toujours par un point.

> [!example] Exemple
> `Nom du dossier` : `2020-11-28-17-12 - Immeuble, ciel. Quai de la Seine, Paris 19e.`
> 
> `post_titre_1`: `Immeuble, ciel. Quai de la Seine, Paris 19e.`

##### post_titre_2: # auto : dérivé de `Nom de dossier`

- Commence par `Journal du ` (Journal du + espace)
- Utiliser les 16 premiers caractères pour transformer la date en langage naturel français. 
	- `AAAA-MM-JJ-hh-mm`: `jourSemaine JJ mois AAAA`
- Terminer par un point.

> [!example] Exemple
> `Nom du dossier` : `2020-11-28-17-12 - Immeuble, ciel. Quai de la Seine, Paris 19e.`
> 
> `post_titre_2`: `Journal du samedi 28 novembre 2020.`

#### 📄 Corps de la note

```
## Photo

![[img_filename]]

## Notes

![[`post_titre_full` + `_notes`]]

```

___

## 3. Exemple d’entrée et de résultat

### Entrée de l’utilisateur

| Champ              | Valeur saisie                                       |
| ------------------ | --------------------------------------------------- |
| **Nom de dossier** | `2024-01-26-16-28 - Lampadaire, toile d'araignée.` |
| **Nom de l'image** | `2024-01-26_16_28_3109684_WP.webp`                  |


### Résultat obtenu

**Nom du fichier créé**

`Lampadaire, toile d'araignée. Journal du dimanche 26 janvier 2024..md`

**Bloc YAML généré**

```yaml
---
cover: 2024-01-26_16_28_3109684_WP.webp

IMAGES: ______________________________________________________________________

img_alt: 
  - Lampadaire, toile d'araignée.
img_descr:
img_filename: 
  - 2024-01-26_16_28_3109684_WP.webp
img_id:
img_legende:
  - Lampadaire, toile d'araignée. Journal du dimanche 26 janvier 2024.
img_titre:  
img_url:

LIEN: ______________________________________________________________________

lien_archives: "[[Lampadaire ? Toile d'araignée ? Archives du dimanche 26 janvier 2024 ?]]"
lien_journal:
lien_projet:
  - "[[Photo]]"
  - "[[Journal photo]]"
lien_restes: "[[Lampadaire ? Toile d'araignée ? Restes du dimanche 26 janvier 2024 ?]]"

MAJ: ______________________________________________________________________

maj_wp: true

POST: ______________________________________________________________________

post_cat:
  - photo
  - journal-photo
post_date: 2024-01-26T16:28:00
post_descr:
post_extrait: 
post_id:
post_mod:
post_perma:
post_titre_1: Lampadaire, toile d'araignée.
post_titre_2: Journal du dimanche 26 janvier 2024.
post_titre_full: Lampadaire, toile d'araignée. Journal du dimanche 26 janvier 2024.
post_video_url:
tags:

WP: ______________________________________________________________________

wp_carnet_link:
wp_carnet_on:
wp_status:
---
```

**Corps de la note**

```
## Photo

![[2024-01-26_16_28_3109684_WP.webp]]

## Notes

![[Lampadaire, toile d'araignée. Journal du dimanche 26 janvier 2024._notes]]

```



## 4. DEV : services à utiliser
*notes : les noms des fichiers .ts de services sont à définir.*


**post_date:  # auto : dérivé de `Nom du fichier vidéo`**
*Utilisé également dans **`Action Minutes`***
- Dériver 16 premiers caractères AAAA-MM-JJ-hh-mm en AAAA-MM-JJThh:mm:00

**post_titre_2: # auto : dérivé de `Nom du fichier vidéo`**
*Base Utilisée également dans **`Action Minutes`** (sans ajout en début de phrase de `Journal du` ou  `Restes du` ou `Archives du`, et remplacement des `.`  `,` `;` `!` par `?`)*
- Utiliser les 16 premiers caractères, transformer la date en langage naturel français, terminer par un point.

**post_titre_1: # auto : dérivé de `Nom du fichier vidéo`**
*Utilisé également dans **`Action Minutes`***
- Utiliser tout après le séparateur " - " (espace, tiret, espace). Commence toujours par une majuscule, se termine toujours par un point.


## 5. Notes
notes divers par moi-même au cours des versions
### 🗓️ Notes 2025-10-15
Je me suis trompé sur lien_projet: [[Journal Photo]], c'est : [[Journal photo]]. Remplacé partout.

## 6. Logs
inscrire ici les logs de changement de l'**`Action Journal`**.

### 🗓️ 2025-10-15 — Implémentation commande «Créer une note Journal» + correctifs
- Commande ajoutée (id: create-journal-note) avec modale 2 champs («Nom de dossier», «Nom de l’image»).
- Validations: dossier au format AAAA-MM-JJ-hh-mm - Titre…; image au format AAAA-MM-JJ-hh-mm_idphoto_WP.webp.
- Dérivations: post_date ISO; post_titre_1 extrait; post_titre_2 = "Journal du <jour date>." (sans heure); post_titre_full concaténé; lien_archives/lien_restes générés.
- YAML: toutes les clés (même vides) et séparateurs visibles (IMAGES/LIEN/MAJ/POST/WP); img_titre vide; post_vid_url vide; maj_wp: true; post_cat: [photo, journal-photo].
- Nom du fichier créé = post_titre_full.
