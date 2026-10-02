# Bibliothèque de prompts PO — formation « IA Générative pour Product Owners »

Cette bibliothèque contient quatorze prompts prêts à l'emploi pour les tâches quotidiennes d'un Product Owner, d'un chef de projet, d'un Scrum Master ou d'un testeur fonctionnel. Tous s'utilisent dans une fenêtre de chat Claude (Claude Desktop ou claude.ai), sans aucun outil technique. Chaque prompt est illustré sur le fil rouge NOVA (NovaTech Industries, refonte du tunnel de commande).

## Sommaire

| N° | Fichier | Usage | Moment typique |
|---|---|---|---|
| 01 | `01-analyser-user-story.md` | Analyser une story avant sprint | Affinage |
| 02 | `02-reecrire-story-gherkin.md` | Réécrire une story avec critères d'acceptation Gherkin | Affinage |
| 03 | `03-decouper-story.md` | Découper une story trop grosse | Affinage, planification |
| 04 | `04-generer-cas-de-test.md` | Générer des cas de test depuis des critères | Préparation de la recette |
| 05 | `05-matrice-de-couverture.md` | Construire une matrice de couverture exigences / tests | Préparation de la recette |
| 06 | `06-jeux-de-donnees-test.md` | Produire des jeux de données de test | Préparation de la recette |
| 07 | `07-detecter-cas-limites.md` | Détecter les cas limites d'une règle métier | Affinage, recette |
| 08 | `08-relire-cahier-de-recette.md` | Relire un cahier de recette | Avant la campagne |
| 09 | `09-synthese-comex.md` | Synthétiser une spécification pour le COMEX | Communication |
| 10 | `10-reformuler-non-technique.md` | Reformuler pour une partie prenante non technique | Communication |
| 11 | `11-confronter-ticket-spec.md` | Confronter un ticket à une page de spec, avec citations | Affinage, contrôle |
| 12 | `12-compte-rendu-vers-actions.md` | Compte rendu de réunion vers actions et tickets | Après une réunion |
| 13 | `13-analyser-backlog.md` | Analyser un backlog (complétude, incohérences) | Revue de backlog |
| 14 | `14-note-de-cadrage.md` | Rédiger une note de cadrage | Lancement |

## La structure d'un bon prompt PO

Un prompt qui fonctionne de façon reproductible contient cinq blocs. Les prompts de cette bibliothèque les suivent tous, dans cet ordre.

1. **Le rôle.** Qui Claude doit être : « analyste fonctionnel », « testeur de recette », « rédacteur pour un comité de direction ». Le rôle fixe le vocabulaire, le niveau de détail et les réflexes. Un rôle vague (« assistant ») donne une réponse vague.
2. **Le contexte.** Ce que Claude doit savoir pour ne pas inventer : le produit, l'acteur, le document de référence, le moment du projet. Dans un Projet Claude, une partie du contexte vit dans les instructions et les fichiers de connaissance ; le prompt n'a alors qu'à nommer la source (« la page Règles de remise »).
3. **La contrainte.** Ce que Claude n'a pas le droit de faire : inventer un chiffre, sortir des documents fournis, proposer des priorités. C'est le bloc que les débutants oublient et celui qui évite le plus d'ennuis.
4. **Le format.** La forme exacte attendue : tableau avec colonnes nommées, Gherkin, liste numérotée, nombre de lignes. Un format précis est aussi un moyen de contrôle : si le tableau a 12 lignes pour 14 tickets, vous voyez tout de suite qu'il en manque deux.
5. **La source à citer.** Pour tout ce qui touche à une règle métier : exiger la citation textuelle et la section. Une affirmation sans citation ne se vérifie pas ; une affirmation citée se vérifie en dix secondes.

Deux habitudes complètent la structure :

- **Les variables entre accolades.** Chaque prompt de la bibliothèque isole ce qui change d'un usage à l'autre (`{clé_ticket}`, `{titre_page}`). Remplacez-les toutes avant d'envoyer : une accolade oubliée fait dériver la réponse.
- **La relance prévue.** Un bon prompt ne vise pas la réponse parfaite du premier coup ; il vise une première réponse vérifiable, puis une relance ciblée (« relis la section 4 et reprends la ligne 3 »).

## Ce qu'un prompt ne remplace pas

Claude ne connaît pas votre produit. Il connaît ce que vous lui donnez. Un prompt excellent avec un mauvais contexte donne une réponse fausse et bien écrite, ce qui est pire qu'une réponse visiblement mauvaise. Avant d'envoyer, posez-vous la question : « avec ces seuls documents, un nouvel arrivant compétent pourrait-il répondre ? ». Si non, Claude non plus.

## Comment l'équipe versionne la bibliothèque

**Sur un abonnement Team ou Enterprise** : créez un Projet Claude « Bibliothèque de prompts PO », partagé avec l'équipe. Mettez chaque prompt en fichier de connaissance (un fichier par prompt, ce dossier tel quel) et, dans les instructions du Projet, une phrase du type « Quand on te demande un prompt de la bibliothèque, restitue-le tel quel, puis demande les variables à remplir ». Toute modification passe par un remplacement du fichier ; notez la date et l'auteur en tête du fichier. La mémoire du Projet garde les retours d'usage si vous les y consignez explicitement (« retiens que le prompt 11 rate les nuances de gouvernance »).

**Sur un abonnement Pro (pas de partage de Projet)** : le dossier partagé de l'entreprise (SharePoint, Drive, Confluence) fait office de dépôt. Une page Confluence par prompt fonctionne très bien, et le connecteur Atlassian permet ensuite de dire à Claude « lis la page Prompt 11 et applique-le à NOVA-2 ». Chacun importe la bibliothèque dans son propre Projet personnel.

Dans les deux cas, trois règles de gouvernance :

1. Un prompt entre dans la bibliothèque quand il a été utilisé au moins trois fois par deux personnes différentes avec un résultat jugé utile.
2. Chaque fichier garde sa section « Pièges connus » à jour : c'est la mémoire collective des ratés, plus précieuse que le prompt lui-même.
3. Une revue trimestrielle de vingt minutes : on retire ce qui ne sert plus, on fusionne les doublons, on met à jour les exemples.

## Conventions des fichiers

Chaque fichier suit le même plan : à quoi sert le prompt, rôle donné à Claude, contexte à fournir, le prompt complet copiable (bloc `text`, variables entre accolades), un exemple rempli sur NOVA, les pièges connus (ce que le modèle invente ou rate), et une ligne « comment l'améliorer collectivement ».

Les exports utilisés dans les exemples se trouvent dans `../exports/` : `backlog-nova.md` (les 14 tickets), `backlog-nova.csv` et `confluence/` (les 4 pages de spécification).
