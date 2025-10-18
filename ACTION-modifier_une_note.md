
## Description
L'action ouvre une modale et permet de modifier tous les yaml de la note. 
en validant les modifications sont appliquées à la note, et la checkbox maj_wp est activée.

## Roadmap

- [ ] Dans un premier temps nous allons implémenter uniquement l'ajout et suppression de tags dans le champs tags. Nous rajouterons d'autres champs dans des versions ultérieures. 



## ACTION-modifier_une_note (v0.1 tags)

*Dans un premier temps nous allons implémenter uniquement l'ajout et suppression de tags dans le champs tags. Nous rajouterons d'autres champs dans des versions ultérieures.* 

Les tags sont proposés uniquement à partir de la liste des slugs des tags dans ob_tags_table.md (toujours dans wp_tags, qui est à la racine de la vault), soit la colonne obs_tags_slug.

La fenêtre modale affiche les tags déjà présents.
boutons Annuler et Valider
On peut effacer un tag existant. 
On peut ajouter un tag à la liste.

à la validation maj_wp est activé

Note de mutualisation : Cette fonction d'ajout/suppression de tags basé sur obs_tags_table.md va être utilisé après dans des actions comme Créer une note minutes, à laquelle on rajoutera la possibilité d'ajouter des tags dans la fen^tre de création. 


## Logs