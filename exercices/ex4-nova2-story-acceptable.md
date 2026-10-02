# Exercice 4 (J2) — De NOVA-2 à une story acceptable

Durée : 30 minutes. Individuel, dans le Projet NOVA.

## Objectif

Partir du ticket le plus flou du backlog, NOVA-2 « Améliorer la remise des commandes », et en faire une story qu'une équipe pourrait estimer et un testeur recetter : critères d'acceptation en Gherkin, périmètre exclu, points restant à trancher, et proposition de découpage. Vous utiliserez les prompts 01, 02 et 03 de la bibliothèque, dans cet ordre.

## Matériel

- Le texte de NOVA-2 : via le connecteur (« lis le ticket NOVA-2 ») ou dans `exports/backlog-nova.md`.
- La page « Règles de remise », déjà dans la connaissance du Projet.
- Les prompts `prompts/01-analyser-user-story.md`, `prompts/02-reecrire-story-gherkin.md`, `prompts/03-decouper-story.md`.

## Déroulé

**Temps 1 — Diagnostic (8 minutes).** Appliquez le prompt 01 à NOVA-2. Lisez les ambiguïtés et les questions produites. Choisissez les cinq questions que vous poseriez réellement au demandeur, et pour chacune, imaginez la réponse la plus probable en vous appuyant sur la page « Règles de remise » : ce seront vos « décisions » pour le temps 2. Notez-les sous forme de liste datée.

Attention : une décision que la spec ne permet pas de prendre reste un point ouvert. Ne l'inventez pas.

**Temps 2 — Réécriture (12 minutes).** Appliquez le prompt 02 avec, dans `<decisions>`, la liste du temps 1. Relisez la story produite avec cette grille :

- Un seul acteur, cohérent avec la spec ?
- Chaque règle métier est chiffrée et cite sa source (section de la spec ou décision) ?
- Le bloc Gherkin contient au moins un scénario par règle, un nominal, deux cas d'erreur ou limite ?
- Les valeurs des scénarios sont concrètes et les calculs sont justes ? (Vérifiez-en un à la main.)
- Les points ouverts correspondent bien à ce que ni le ticket, ni vos décisions, ni la spec ne tranchent ?

Corrigez par relance ce qui ne passe pas la grille. Ne corrigez pas à la main : dites à Claude ce qui est faux et pourquoi.

**Temps 3 — Découpage (10 minutes).** Appliquez le prompt 03 à votre story réécrite (pas au ticket d'origine). Vérifiez que chaque tranche a une « valeur livrée seule » observable par un utilisateur et qu'aucune n'est une tâche technique déguisée. Choisissez la tranche que vous mettriez en premier et écrivez en deux lignes pourquoi.

## Contraintes

- Rien qui ne vienne du ticket, de la spec ou de vos décisions explicites. Si vous voyez un chiffre dont vous ne savez pas d'où il vient, demandez à Claude sa source.
- Pas de solution technique dans la story : ni nom de table, ni API.
- Gardez la conversation intacte, elle servira à l'exercice 6.

## Ce que vous rendez

La story réécrite (Markdown), la liste de vos décisions, la liste des points ouverts, le tableau de découpage, et le nom de la première tranche avec sa justification.

## Pour vous situer

Le ticket NOVA-3 « Appliquer un barème de remise par volume » est une story bien rédigée du même backlog. Ne le lisez qu'après avoir terminé, puis comparez : qu'a-t-il que la vôtre n'a pas, et inversement ?
