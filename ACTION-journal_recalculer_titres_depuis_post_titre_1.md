---
doc_id: action_journal_recalc_from_titre1
titre: "ACTION - Journal → Recalculer titres depuis post_titre_1"
type_doc: action
version: v1
version_precedente: none
date_maj: 2025-10-15
etat: courant
notes: "Commande utilitaire exécutée sur une note Journal photo. Recalcule les titres et liens depuis post_titre_1, met à jour le YAML in-place et renomme le fichier si nécessaire. À lancer avant P2 (Archives/Restes) lorsqu’on modifie post_titre_1."
---

# Objet
- Recalculer **toutes les dérivations** du Journal à partir de `post_titre_1` et `post_date`.
- Mettre à jour **uniquement le YAML** in-place (préserve ordre et séparateurs), puis **renommer** la note si le nom ≠ `post_titre_full`.
- À utiliser **avant** les actions P2 « Archives/Restes du futur » après un changement de `post_titre_1`.

# Détails (succinct)
- `post_titre_2` = `Journal du <jour date>.` (**sans l’heure**, ponctuation finale « . »).
- `post_titre_full` = `post_titre_1` + espace + `post_titre_2`.
- `img_alt` = `post_titre_1`.
- `img_legende` = `post_titre_full`.
- `lien_archives` = construit depuis `post_titre_full` (wikilink `[[…]]`).
- `lien_restes` = construit depuis `post_titre_full` (wikilink `[[…]]`).
- **Renommage** du fichier si nécessaire → nouveau nom = `post_titre_full`.

# Non modifié
- Corps de note.
- `tags`, `post_descr`, `post_extrait`, `post_id`, `post_mod`, `post_perma`, `post_vid_url`, `wp_*`.

## Logs

### 🗓️ 2025-10-15 — Commande ajoutée
- Création de la commande **« Journal → Recalculer titres depuis post_titre_1 »**.
- Recalcule titres (`post_titre_2`, `post_titre_full`), image (`img_alt`, `img_legende`), liens (`lien_archives`, `lien_restes`) et renomme la note si besoin.
