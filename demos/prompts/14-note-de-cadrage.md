# 14 — Rédiger une note de cadrage

## À quoi sert ce prompt

Produire le premier jet d'une note de cadrage à partir des éléments dispersés d'un lancement : epic, échanges, pages de spec, contraintes connues. La note fixe le pourquoi, le quoi, le pas-quoi, les acteurs, les risques et les hypothèses ; elle sert à obtenir un accord de lancement, pas à spécifier.

## Rôle donné à Claude

Chef de projet qui structure ce qu'on lui donne, isole clairement les hypothèses de travail des faits établis, et refuse de remplir les cases vides par des généralités.

## Contexte à fournir

- Les sources : epic, notes, pages de spec, mails de cadrage.
- Le commanditaire et les parties prenantes connues.
- Le gabarit de note de votre entreprise s'il existe (sinon celui du prompt).
- Le niveau de maturité : « idée », « pré-étude », « projet décidé ».

## Le prompt

```text
Tu es chef de projet. Tu rédiges le premier jet d'une note de cadrage à partir des sources entre balises <sources>, pour un projet au stade « {maturité} ». Commanditaire : {commanditaire}.

Structure imposée (titres exacts) :

1. Contexte et problème — trois à cinq phrases, faits sourcés (document, section) uniquement.
2. Objectifs — liste numérotée ; chaque objectif est mesurable ou marqué « [indicateur à définir] ». Reprends les chiffres cibles des sources s'il y en a ; n'en invente aucun.
3. Périmètre — deux colonnes : « Dans le périmètre » / « Hors périmètre », chaque ligne avec sa source. Si une source est ambiguë sur un point, mets-le dans une troisième liste « À arbitrer ».
4. Acteurs et parties prenantes — tableau Rôle, Ce qu'il attend, Ce qu'on attend de lui, Source.
5. Contraintes — techniques, réglementaires, calendaires, budgétaires : uniquement celles des sources.
6. Hypothèses de travail — tout ce que la note suppose sans que les sources le confirment. Chaque hypothèse commence par « Nous supposons que » et se termine par « à confirmer par {qui} ».
7. Risques — tableau Risque, Impact, Probabilité (élevée / moyenne / faible, avec une justification de cinq mots), Parade envisagée ou « à définir ».
8. Jalons connus — uniquement ceux des sources ; sinon « aucun jalon arrêté ».
9. Décision demandée — une phrase : ce que le commanditaire doit valider pour lancer.

Contraintes : 700 à 1 000 mots. Pas de généralités (« améliorer l'expérience utilisateur ») sans fait derrière. Pas de solution technique au-delà de ce que les sources imposent. Toute case qui ne peut pas être remplie depuis les sources contient « [à compléter : …] » avec la question précise.

<sources>
{textes_sources}
</sources>
```

## Exemple rempli sur NOVA

`<sources>` = l'epic NOVA-1, la section 1 de `exports/confluence/01-specification-fonctionnelle-tunnel-commande.md` (objet et périmètre), la section 6 de `exports/confluence/04-architecture-application-commandes.md` (contraintes non fonctionnelles) ; `{maturité}` = projet décidé ; `{commanditaire}` = direction commerciale NovaTech.

Points à vérifier dans la sortie : les quatre objectifs de l'epic sont repris (abandon panier 31 %, 18 % de commandes reprises à la main, tablette, traçabilité) ; le module de facturation Sage est hors périmètre ; la contrainte d'interruption de 15 minutes et la réversibilité de la migration sont dans les contraintes ; les hypothèses contiennent au moins la question de la disponibilité des utilisateurs clés pour la recette (aucune source ne la traite).

## Pièges connus

- Claude remplit la section « Risques » avec des risques génériques (« résistance au changement ») non reliés aux sources. Exigez pour chaque risque le fait qui le fonde ; sinon, à la section 6 en hypothèse.
- Il invente des jalons (« T1 », « fin d'année ») si l'epic dit « exercice en cours ». « Exercice en cours » n'est pas un jalon.
- Les objectifs mesurables sont parfois complétés d'une cible inventée (« ramener l'abandon à 20 % »). L'epic donne le point de départ (31 %), pas la cible : « [indicateur à définir] ».
- La section « Hypothèses » est celle qu'il bâcle et c'est la plus importante : une note de cadrage vaut par ses hypothèses explicites. Relancez : « ajoute cinq hypothèses que la note fait implicitement ».
- Longueur : il dépasse. Demandez de couper les généralités avant les faits.

## Comment l'améliorer collectivement

Reprenez les notes de cadrage de vos trois derniers projets, extrayez les rubriques qui ont réellement servi en comité et celles qui n'ont jamais été lues, et adaptez la structure imposée du prompt en conséquence.
