# Exercice 1 (J1) — Trois prompts naïfs à réécrire


## Objectif

Sentir, sur trois cas de votre métier, la différence entre une question posée à Claude « comme à un moteur de recherche » et un prompt construit (rôle, contexte, contrainte, format, source). Vous n'avez pas besoin de connaître NOVA pour cet exercice.

## Les trois prompts naïfs

Voici trois demandes telles qu'on les voit tous les jours. Envoyez-les d'abord telles quelles à Claude, dans un chat vide, et lisez la réponse.

**Prompt naïf A**

```text
Écris-moi une user story pour la gestion des remises.
```

**Prompt naïf B**

```text
Fais-moi des cas de test pour le panier.
```

**Prompt naïf C**

```text
Résume cette spec pour mon directeur.
```

(Pour le C, collez à la suite n'importe quel document de spécification de votre choix d'une à deux pages, ou la page `exports/confluence/01-specification-fonctionnelle-tunnel-commande.md`.)

## Consigne

1. Pour chaque prompt naïf, notez en une ligne ce que Claude a dû inventer pour répondre (l'acteur ? le produit ? les règles ? le format ? le destinataire ?).
2. Réécrivez chaque prompt en respectant les cinq blocs vus ce matin : rôle, contexte, contrainte, format, source à citer. Utilisez des variables entre accolades pour ce que vous ne savez pas encore, et remplissez-les avec un cas réel de votre produit si vous en avez un (sinon, avec NOVA : remises sur un tunnel de commande B2B).
3. Envoyez vos trois prompts réécrits et comparez les réponses aux premières : qu'est-ce qui a disparu (inventions) et qu'est-ce qui est apparu (questions, structure, citations) ?
4. Pour un des trois, ajoutez une contrainte négative explicite (« n'invente aucun chiffre », « ne propose pas de solution technique ») et observez ce que ça change.

## Contraintes

- Pas plus de 15 lignes par prompt réécrit : un bon prompt PO est dense, pas long.
- Chaque prompt réécrit doit contenir au moins une contrainte et un format explicites.
- Interdiction de demander à Claude de réécrire le prompt à votre place pour cet exercice (vous le ferez plus tard, quand vous saurez juger le résultat).

## Ce que vous rendez

Un message dans le canal de la formation avec vos trois prompts réécrits et, pour chacun, une phrase : « ce que la réécriture a supprimé ».

## Pour aller plus loin si vous avez fini

Prenez un de vos trois prompts et retirez-lui un bloc (le rôle, puis le format, puis la contrainte) en observant à chaque fois ce qui se dégrade. Vous saurez lequel des cinq blocs compte le plus pour ce type de tâche.
