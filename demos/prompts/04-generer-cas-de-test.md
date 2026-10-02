# 04 — Générer des cas de test depuis des critères d'acceptation

## À quoi sert ce prompt

Passer des critères d'acceptation (Gherkin ou liste) à des cas de test fonctionnels rédigés : préconditions, étapes, données, résultat attendu, avec l'identifiant du critère couvert. Utile pour un testeur fonctionnel qui prépare une campagne, ou pour un PO qui veut vérifier que ses critères sont testables.

## Rôle donné à Claude

Testeur fonctionnel rigoureux, qui n'invente pas de comportement attendu : quand le critère ne dit pas ce qui doit se passer, il l'écrit.

## Contexte à fournir

- Les critères d'acceptation (le bloc Gherkin de la story, ou la liste des règles).
- Le contexte d'exécution : interface web, tablette, API, rôle qui exécute.
- Les jeux de données disponibles s'ils existent (comptes de test, références produits), sinon Claude proposera des valeurs à créer.

## Le prompt

```text
Tu es un testeur fonctionnel. À partir des critères d'acceptation d'une story, tu rédiges les cas de test manuels que l'équipe de recette exécutera.

Story : {clé_ticket} « {résumé_ticket} ». Critères d'acceptation entre balises <criteres>. Contexte d'exécution : {contexte_execution}. Comptes et données de test disponibles : {donnees_disponibles}.

Pour chaque critère, produis au moins un cas de test au format suivant :

- ID : CT-{clé_ticket}-nn
- Critère couvert : nom du scénario Gherkin ou numéro de la règle
- Type : nominal / limite / erreur
- Préconditions : état du système et données nécessaires, avec des valeurs concrètes
- Étapes : liste numérotée, une action observable par étape, du point de vue de l'utilisateur
- Résultat attendu : ce que l'utilisateur voit ou reçoit, avec les valeurs exactes attendues (montants au centime, messages exacts s'ils sont spécifiés)
- Source du résultat attendu : citation du critère ou de la règle ; si le résultat n'est pas déductible des critères fournis, écris « NON SPÉCIFIÉ : à confirmer » au lieu d'un résultat

Puis :

1. Un tableau récapitulatif : ID, critère couvert, type, priorité (bloquant / majeur / mineur) avec la justification en cinq mots.
2. La liste des critères qui ne sont pas testables tels quels (ambigus, sans valeur observable) et ce qu'il faudrait y ajouter.
3. Les cas de test que tu recommandes d'ajouter alors qu'aucun critère ne les couvre (au plus cinq), marqués « HORS CRITÈRES » : ce sont des propositions à valider par le PO, pas des exigences.

Contraintes : pas de cas de test technique (API, base de données) sauf si le contexte d'exécution le demande. Les valeurs de test sont concrètes et calculées correctement : vérifie chaque montant.

<criteres>
{bloc_gherkin_ou_regles}
</criteres>
```

## Exemple rempli sur NOVA

`{clé_ticket}` = NOVA-3 ; `<criteres>` = le bloc Gherkin et la section « Règles métier » de NOVA-3 dans `exports/backlog-nova.md` ; `{contexte_execution}` = interface web client, compte client professionnel authentifié ; `{donnees_disponibles}` = un compte Standard, un compte Privilège, un compte Standard avec remise contractuelle de 10 %.

Vérifiez au moins un calcul à la main : pour un panier Standard de 5 000,00 EUR HT, la remise est de 6 %, soit 300,00 EUR, montant final 4 700,00 EUR. Si un cas de test de Claude dit autre chose, il a mal lu le palier « inclus / exclu ».

## Pièges connus

- Les erreurs de calcul aux bornes des paliers sont le défaut le plus fréquent : 5 000,00 est dans le palier 6 % (inclus), 4 999,99 dans le palier 3 %. Contrôlez chaque montant frontière.
- Claude invente des messages d'erreur (« Votre remise a été appliquée avec succès ») qui ne sont dans aucun critère. La consigne « NON SPÉCIFIÉ » doit apparaître à la place ; sinon, relancez.
- Il produit facilement des étapes techniques (« appeler l'endpoint recalculate ») même pour une recette manuelle. Reformulez : « du point de vue de l'utilisateur ».
- Les cas « HORS CRITÈRES » sont souvent les plus intéressants (panier vide, client sans catégorie, montant négatif) mais ce sont des propositions : ne les ajoutez à la campagne qu'après validation.
- Sur un long bloc Gherkin, il peut sauter un scénario. Comptez : autant de cas de test au minimum que de scénarios.

## Comment l'améliorer collectivement

Faites exécuter les cas générés par un testeur qui n'a pas participé à leur rédaction et notez ceux qu'il n'a pas pu dérouler sans poser de question : ce sont les formulations à corriger dans le prompt (souvent la précision des préconditions).
