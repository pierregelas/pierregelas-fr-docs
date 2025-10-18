---
doc_id: action_minutes
titre: "ACTION - Créer une note Minutes"
type_doc: action
version: v1
version_precedente: none
date_maj: 2025-10-14
etat: courant
notes: "Implémentation v0.1: commande opérationnelle, YAML complet avec séparateurs (IMAGES/LIEN/MAJ/POST/WP), img_titre vide, maj_wp: true, post_cat: [video, minutes], nom de fichier = post_titre_full."
---




# ACTION : Créer une note _Minutes_

---

## 1. Entrées demandées à l’utilisateur

Lors du déclenchement de l’action “Créer une note Minutes”, une fenêtre demande 2 éléments essentiels :

1. **Nom du fichier vidéo**  
    Exemple :  
    `2025-06-14-15-57 - Danse et manifestation, Place Léon Blum, Paris 11e.`  
    → Ce nom doit toujours suivre la structure :  
    **année-mois-jour-heure-minute – titre complet.**  
    (avec le tiret entouré de 2 espaces “ - ” séparant la date et le titre).

2. **Lien de la vidéo**  
    Exemple :  
    `https://youtu.be/hsELNvoyOnw`


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
cover:                # auto : copie de img_filename

IMAGES: ______________________________________________________________________

img_alt:              # auto : copie de `post_titre_1`
img_descr:            # vide
img_filename:         # auto : dérivé de `Nom de fichier vidéo`
img_id:               # vide
img_legende:          # auto : copie de `post_titre_full`
img_titre:            # vide
img_url:              # vide

LIEN: ______________________________________________________________________

lien_archives:        # vide
lien_journal:         # vide
lien_projet:          # pré-rempli
  - "[[Vidéo]]"
  - "[[Minutes]]"
lien_restes:          # vide

MAJ: ______________________________________________________________________

maj_wp: true         # pré-rempli: true

POST: ______________________________________________________________________

post_cat:             # pré-rempli - slug category
  - video
  - minutes
post_date:           # auto : dérivé de `Nom du fichier vidéo`
post_descr:          # vide
post_extrait:        # vide
post_id:             # vide
post_mod:            # vide
post_perma:          # vide
post_titre_1:        # auto : dérivé de `Nom du fichier vidéo`
post_titre_2:        # auto : dérivé de `Nom du fichier vidéo`
post_titre_full:     # auto : concatène `post_titre_1` + ` ` + `post_titre_2`
post_vid_url:        # auto : copie de `Lien de la vidéo`
tags:                # vide

WP: ______________________________________________________________________

wp_carnet_link:      # vide
wp_carnet_on:        # vide
wp_status:           # vide
---
```

#### Explications

##### img_filename: # auto : dérivé de `Nom de fichier vidéo`
Utiliser les 10 premiers caractères, enlever les tirets et agglomérer. 
Ajouter un `_`. 
Ajouter les 8 premiers caractères (hors espace, ponctuation et caractères spéciaux), passés en minuscule, du titre complet, soit après le séparateur " - " (espace, virgule, espace).
Ajouter `_mvign.webp`.

> [!example] Exemple
>  `Nom du fichier vidéo` : `2025-06-14-15-57 - Danse et manifestation, Place Léon Blum, Paris 11e.`
>  
> `img_filename`: `20250614_danseetm_mvign.webp`

##### post_date: # auto : dérivé de `Nom du fichier vidéo`

Utiliser les 16 premiers caractères.
`AAAA-MM-JJ-hh-mm - titre complet.` devient `AAAA-MM-JJThh:mm:00`

> [!example] Exemple
>  `Nom du fichier vidéo` : `2025-06-14-15-57 - Danse et manifestation, Place Léon Blum, Paris 11e.`
>  
> `post_date`:  `2025-06-14T15:57:00`

##### post_titre_1: # auto : dérivé de `Nom du fichier vidéo`

Utiliser tout après le séparateur " - " (espace, tiret, espace).
Commence toujours par une majuscule, se termine toujours par un point.

> [!example] Exemple
> `Nom du fichier vidéo` : `2025-06-14-15-57 - Danse et manifestation, Place Léon Blum, Paris 11e.`
> 
> `post_titre_1`: `Danse et manifestation, Place Léon Blum, Paris 11e.`

##### post_titre_2: # auto : dérivé de `Nom du fichier vidéo`
Utiliser les 16 premiers caractères, transformer la date en langage naturel français, terminer par un point.

> [!example] Exemple
> `Nom du fichier vidéo` : `2025-06-14-15-57 - Danse et manifestation, Place Léon Blum, Paris 11e.`
> 
> `post_titre_2`: `Samedi 14 juin 2025 à 15h57.`


#### 📄 Corps de la note

```
## Vignette

![[img_filename]]

## Vidéo

![](post_vid_url)

## Notes

![[`post_titre_full` + `_notes`]]

```

___

## 3. Exemple d’entrée et de résultat

### Entrée de l’utilisateur

| Champ                    | Valeur saisie                                                            |
| ------------------------ | ------------------------------------------------------------------------ |
| **Nom du fichier vidéo** | `2025-06-14-15-57 - Danse et manifestation, Place Léon Blum, Paris 11e.` |
| **Lien vidéo**           | `https://youtu.be/hsELNvoyOnw`                                           |


### Résultat obtenu

**Nom du fichier créé**

`Danse et manifestation, Place Léon Blum, Paris 11e. Samedi 14 juin 2025 à 15h57..md`

**Bloc YAML généré**

```yaml
---
---
cover: 20250614_danseetm_mvign.webp

IMAGES: ______________________________________________________________________

img_alt: 
  - Danse et manifestation, Place Léon Blum, Paris 11e.
img_descr:
img_filename: 
  - 20250614_danseetm_mvign.webp
img_id:
img_legende:
  - Danse et manifestation, Place Léon Blum, Paris 11e. Samedi 14 juin 2025 à 15h57.
img_titre:  
img_url:

LIEN: ______________________________________________________________________

lien_archives:
lien_journal:
lien_projet:
  - "[[Vidéo]]"
  - "[[Minutes]]"
lien_restes:

MAJ: ______________________________________________________________________

maj_wp: true

POST: ______________________________________________________________________

post_cat:
  - video
  - minutes
post_date: 2025-06-14T15:57:00
post_descr:
post_extrait: 
post_id:
post_mod:
post_perma:
post_titre_1: Danse et manifestation, Place Léon Blum, Paris 11e.
post_titre_2: Samedi 14 juin 2025 à 15h57.
post_titre_full: Danse et manifestation, Place Léon Blum, Paris 11e. Samedi 14 juin 2025 à 15h57.
post_video_url: https://youtu.be/hsELNvoyOnw
tags:

WP: ______________________________________________________________________

wp_carnet_link:
wp_carnet_on:
wp_status:
---
```

**Corps de la note**

```
## Vignette

![[20250614_danseetm_mvign.webp]]

## Vidéo

![](https://youtu.be/hsELNvoyOnw)

## Notes

![[Danse et manifestation, Place Léon Blum, Paris 11e. Samedi 14 juin 2025 à 15h57._notes]]
```



## 4. DEV : services à utiliser
*notes : les noms des fichiers .ts de services sont à définir.*

**img_filename: # auto : dérivé de `Nom de fichier vidéo`**
*Utilisé uniquement dans **`Action Minutes`***
- Utiliser les 10 premiers caractères, enlever les tirets et agglomérer. 
- Ajouter un `_`. 
- Ajouter les 8 premiers caractères (hors espace, ponctuation et caractères spéciaux), passés en minuscule, du titre complet, soit après le séparateur " - " (espace, virgule, espace).
- Ajouter `_mvign.webp`.

**post_date:  # auto : dérivé de `Nom du fichier vidéo`**
*Utilisé également dans **`Action Journal`***
- Dériver 16 premiers caractères AAAA-MM-JJ-hh-mm en AAAA-MM-JJThh:mm:00

**post_titre_2: # auto : dérivé de `Nom du fichier vidéo`**
*Base Utilisée également dans **`Action Journal`** avec modification (ajout en début de phrase de `Journal du` ou  `Restes du` ou `Archives du`, et remplacement des `.`  `,` `;` `!` par `?`)*
- Utiliser les 16 premiers caractères, transformer la date en langage naturel français, terminer par un point.

**post_titre_1: # auto : dérivé de `Nom du fichier vidéo`**
*Utilisé également dans **`Action Journal`***
- Utiliser tout après le séparateur " - " (espace, tiret, espace). Commence toujours par une majuscule, se termine toujours par un point.


## 5. Notes
notes divers par moi-même au cours des versions

## 6. Logs
inscrire ici les logs de changemnt de l'**`Action Minutes`**.

### 🗓️ 2025-10-14 — Implémentation commande «Créer une note Minutes» (plugin local)
- Commande ajoutée (id: create-minutes-note), modale à 2 champs («Nom du fichier vidéo», «Lien de la vidéo») avec validations (pattern + URL https).
- Dérivations conformes au doc: `post_date` depuis les 16 premiers caractères → ISO, `post_titre_1` après “ - ” (majuscule + point), `post_titre_2` date FR «… à hhHmm.», `post_titre_full` concaténé. 
- `img_filename` généré selon la règle Minutes: `YYYYMMDD_` + 8 chars nettoyés du titre + `_mvign.webp`. :contentReference[oaicite:2]{index=2}
- Builder YAML: toutes les clés présentes, ordre identique au doc, séparateurs **visibles comme clés YAML** (`IMAGES`, `LIEN`, `MAJ`, `POST`, `WP`), `maj_wp: true`, `post_cat: [video, minutes]`. :contentReference[oaicite:3]{index=3}
- Uniformisation de la clé d’URL vidéo: **`post_vid_url`** (et non `post_video_url`) pour rester conforme au modèle de référence. 
- `img_titre` laissé **vide** (exigence du doc). :contentReference[oaicite:5]{index=5}
- Nom du fichier créé = `post_titre_full` (conforme au doc). :contentReference[oaicite:6]{index=6}
