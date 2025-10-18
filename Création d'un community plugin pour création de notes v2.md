---
doc_id: creation_plugin_notes
titre: Création d’un community plugin pour création de notes
type_doc: projet
version: v2
version_precedente: v1
date_maj: 2025-10-17
etat: courant
notes:
  - "v0.1: Bootstrap + Minutes."
  - "v0.2: Journal (post_titre_2 sans heure; image AAAA-MM-JJ-hh-mm_id_WP.webp)."
  - "v0.3: Archives du futur (P1)."
  - "v0.4: Archives du futur (P2) + commande « Journal → Recalculer titres depuis post_titre_1 »; cases décochées si identiques; modale info si 0 diff."
  - "v0.5: Restes du futur (P1/P2) avec image _WP→_REI; modale Diff identique à Archives; rename proposé; modale “parfaitement à jour” si 0 diff."
---



Après l'échec de la première tentative de création de plugin, nous repartons sur une méthode plus robuste et plus fiable.

## Introduction

Création d'un community plugin pour création de notes

Basiquement pouvoir créer des notes avec des réglages précis suivant les projets, pour ensuite pouvoir export des csv au bon format pour importer dans wordpress et créer automatiquement les notes via `WP Import Export Lite`

L'objectif est d'avoir une version 0.1 parfaitement fonctionnelle avec les actions suivantes à créer :

## Manifeste

```json
{
  "id": "pierregelas-fr",
  "name": "pierregelas.fr",
  "version": "0.1.0",
  "minAppVersion": "1.5.0",
  "description": "Création automatisée de notes liées au site pierregelas.fr",
  "author": "Pierre Gelas",
  "authorUrl": "https://pierregelas.fr",
  "isDesktopOnly": true
}
```


## Actions à créer pour cette version : 

- Créer une note _Minutes_ : document de projet [[ACTION - Créer une note Minutes]]
- Créer une note _Journal_ : document de projet [[ACTION-creer_une_note_journal]]
- Créer/Mettre à jour une note Archives du futur : document de projet [[ACTION - Créer-Mettre à jour une note Archives du futur]]
- Créer/Mettre à jour une note Restes du futur : document de projet [[ACTION – Créer-Mettre à jour une note Restes du futur]]
- Journal → Recalculer titres depuis post_titre_1 : document de projet [[ACTION – Journal → Recalculer titres depuis post_titre_1]]

## Robustesse
Il est très important de voir les choses dans un cadre général et de hauteur, pour simplifier le code, l'alléger, selon les normes demandées par Obsidian pour les community manager dans le document [[AGENTS_Obsidian community plugin]]

Il est également indispensable de toujours consulter https://docs.obsidian.md/Plugins/Getting+started/Build+a+plugin et ses sous-pages qui contiennent toutes les informations nécessaires.

## Roadmap (alignée avec les logs)

### ✅ Livré

- **v0.1 — 2025-10-14 : Bootstrap + Minutes**  
    Plugin initial (template, build, watch) + action **Minutes** (modale, validations, YAML complet).
    
- **v0.2 — 2025-10-15 : Journal**  
    Action **Journal** (titre sans heure, format image `AAAA-MM-JJ-hh-mm_id_WP.webp`, YAML complet).
    
- **v0.3 — 2025-10-15 : Archives du futur (P1)**  
    Création depuis une note **Journal photo** (préconditions, dérivations titres/IMG, `_WP.webp→_BF.webp`, YAML complet).
    
- **v0.4 — 2025-10-15 : Archives du futur (P2) + commande Journal**  
    Commande **« Journal → Recalculer titres depuis post_titre_1 »** (recalcule titres/liens + rename si besoin).  
    P2: modale **Diff** avec cases **décochées si identiques**, patch YAML **in-place**, rename proposé si le titre change, modale **“parfaitement à jour”** si 0 diff.
    
- **v0.5 — 2025-10-15 : Restes du futur (P1/P2, _WP→_REI)**  
    Même mécanique qu’Archives (création + mise à jour) avec conversions d’image `_WP.webp→_REI.webp`, `post_cat=["photo","restes-du-futur"]`, `lien_projet=[[Photo]], [[Restes du futur]]`, diff A/B identique (cases décochées si identiques), rename proposé, modale **“parfaitement à jour”** si 0 diff.
    
- **v0.6 — 2025-10-17 : Tags v2 (WP→Obsidian)**  
Action **Tags** v2: modale toujours affichée (7 groupes), booléens YAML **natifs** (`wp_update`), **logger conditionnel** (ON/OFF), **onglet Paramètres**.
    
- [x] Squelette plugin v2 avec `src/core/` mutualisé.
- [x] Première action branchée (import WordPress CSV) + UI commande.
### 🔜 À venir

- **v0.6 (partie 2) — Améliorations Tags**  
Sélecteur de CSV (au lieu du “dernier” seulement) • Niveaux de verbosité du logger (normal/minimal) • Rotation des logs (conserver N derniers).
    
- **v0.7 — Ajout de tags (ajout dans les actions)**  
    possibilité d'Ajout de tags avec autocomplétion dans les fenêtres modales de création (Minutes, Journal, Archives)  
    Uniformisation de la sélection/mise à jour des `tags` dans les différentes actions.
    
- **v0.8 — Gestion du dossier catégories et synchronisation**  
    basé sur le même fonctionnement et architecture que le dossier tags
    
- **v0.? — Gestion des settings et dossier de création des notes**
	commencer à construire les settings du plugin, d'abord pour définir les chemins de création des Actions, ainsi que des dossiers tags et catégorie
	
- **v0.x — Logger commun (Minutes/Journal/Archives/Restes)**  
Brancher le logger conditionnel (ON/OFF) sur toutes les actions pour des traces uniformes.


- [ ] Migrer progressivement les autres actions sur `@core/*` (sans casse).
- [ ] Paramétrage centralisé (Settings tab) pour options communes (dossiers, nommage, compat Windows, normalisation).
- [ ] Panneau “Logs d’import” intégré (historique, export .md).
- [ ] Tests unitaires & intégration (CI locale).
- [ ] Publication Community Plugin : checklist Obsidian (manifest, versions, README, icône).
- [ ] Documentation développeur (diagrammes de flux, conventions YAML maître).
- [ ] Internationalisation minimale (EN/FR) des libellés UI.
- [ ] Télémetrie optionnelle (opt‑in) pour métriques anonymisées (volumétrie import).

> Encadré (hors-scope, à noter pour plus tard)
> 
> - **Paramètres du plugin** (toggles simples, valeurs par défaut).
>     
> - **Tests rapides** (quelques fonctions clés).
>     
> - **Packaging release** (build propre + ZIP build minimal).  
>     Ces éléments ne sont pas requis par tes docs actuels, je les note juste pour garder une trajectoire propre.
>
## Logs

### 🗓️ 2025-10-14 — v0.1 (bootstrap & Minutes)
- Base plugin en place (template cloné, structure `src/`, esbuild→`main.js`, `isDesktopOnly: true`, watch OK).
- Action Minutes opérationnelle (modale, validations, dérivations, YAML complet avec séparateurs IMAGES/LIEN/MAJ/POST/WP, création du fichier = `post_titre_full`). Test OK.

### 🗓️ 2025-10-15 — v0.2 (Journal)
- Action Journal opérationnelle: modale, validations, dérivations, YAML complet avec séparateurs visibles, création de la note.
- Corrections clés: post_titre_2 sans heure; format image AAAA-MM-JJ-hh-mm_idphoto_WP.webp.

### 🗓️ 2025-10-15 — v0.3 (Archives du futur — Processus 1)
- Création depuis une note Journal photo; précondition `post_cat` contient `journal-photo`; stop si la note cible existe déjà (via `lien_archives`).
- Titres dérivés du `lien_archives`; `post_titre_1` sans point ajouté si déjà terminé par `?`/`!`; `img_alt` = `post_titre_1` des Archives.
- Image: `_WP.webp` → `_BF.webp`; YAML complet avec séparateurs visibles (IMAGES/LIEN/MAJ/POST/WP); `post_cat` = [photo, archives-du-futur]; `maj_wp: true`.
- Corps: **Photo** (image `_BF.webp`) et **Notes** (`[[post_titre_full_notes]]`).

### 🗓️ 2025-10-15 — v0.4 (Archives du futur — P2 + commande Journal)
- Commande: « Journal → Recalculer titres depuis post_titre_1 » (recalcule post_titre_2, post_titre_full, img_alt, img_legende, lien_archives, lien_restes; rename de la note si nécessaire).
- Archives P2: modale Diff avec cases décochées si identiques; patch YAML in-place; rename proposé si le titre change; modale d’information si 0 changement.

### 🗓️ 2025-10-15 — v0.5 (Restes du futur — P1 & P2)
- P1: création depuis Journal photo via `lien_restes`; titres dérivés avec mot-clé **Restes**; `post_cat=["photo","restes-du-futur"]`; `lien_projet=[[Photo]], [[Restes du futur]]`; `lien_restes:` vide; `lien_archives:` copié; image `_WP.webp` → **`_REI.webp`**; `cover` = image.
- P2: résolution par `lien_restes` ou fallback `post_date` + `post_cat` contient `restes-du-futur`; Diff (A) `post_date`, `tags` (cases cochées seulement si différents); Diff (B) si le titre a changé: `post_titre_1/2/full`, `img_filename` **(_REI_)** + `cover`, `img_alt`, `img_legende`, `lien_journal`, `lien_archives`; rename proposé; modale “parfaitement à jour” si 0 diff.

- **2025-10-17 10:52** — v0.6 (Tags v2, partie 1)  
    Modale toujours affichée (fallback info si erreur), `wp_update` en booléen natif (plus de guillemets), **logger Markdown conditionnel** (ON/OFF), settings tab “pierregelas.fr”, robustesse d’écriture des logs.
    
- **2025-10-16 18:40** — v0.5 (Restes du futur P1/P2)  
    Création + mise à jour « Restes » (mêmes dérivations qu’Archives), conversion `_WP.webp → _REI.webp`, validations & préconditions OK.
    
- **2025-10-17 13:50** — Infra & publication GitHub (plugin + docs)
    - Dépôt public **pierregelas-fr** publié (alignement identité: `id` manifest ⇄ dossier, `package.json` mis à jour).
    - README remplacé par une version projet (installation, commandes).
    - Build consolidé: `tsconfig` (ES2020, strict) et `esbuild` target ES2020.
    - Dépôt public **pierregelas-fr-docs** créé; dossier local `docs/` branché en dépôt Git (miroir 1:1) + README d’index.
    - Ajout des URLs GitHub dans les documents de projet (références externes).

- 2025-10-17 — Mise en place de l’architecture **`src/core/*`** (types, transform, yamlMaster, mapping, files, csv, upsert, log).
- 2025-10-17 — Ajout de l’action **Importer un CSV WordPress** (`src/actions/importWordpress.ts`) et de la commande UI.
- 2025-10-17 — Picker CSV (FuzzySuggestModal) + **prévisualisation** (dry‑run) avec confirmation.
- 2025-10-17 — Gestion erreurs automatique → `NEW/ERRORS/`.
- 2025-10-17 — `maj_wp` fixé à **false** pour tous les imports.
- 2025-10-17 — Émetteur YAML robuste (auto‑quoting scalaires/listes).
