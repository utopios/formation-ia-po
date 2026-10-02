# Exercice 5 (J2) — Cas de test et matrice de couverture depuis NOVA-3

Durée : 30 minutes. Individuel ou binôme PO / testeur.

## Objectif

Depuis une story correctement écrite, NOVA-3 « Appliquer un barème de remise par volume sur les commandes B2B », produire des cas de test manuels puis la matrice de couverture, et découvrir ce que même une bonne story laisse sans test. Vous utiliserez les prompts 04 puis 05.

## Matériel

- NOVA-3 : via le connecteur (« lis le ticket NOVA-3 ») ou dans `exports/backlog-nova.md`. La story contient six règles métier et six scénarios Gherkin.
- La section 8 « Cas limites recensés » de la page « Spécification fonctionnelle — Tunnel de commande » (dans la connaissance du Projet).
- Les prompts `prompts/04-generer-cas-de-test.md` et `prompts/05-matrice-de-couverture.md`.

## Déroulé

**Temps 1 — Cas de test (12 minutes).** Appliquez le prompt 04 à NOVA-3 avec ce contexte : interface web client, compte client professionnel authentifié ; comptes disponibles : un Standard, un Privilège, un Standard avec remise contractuelle de 10 %.

Vérification obligatoire avant de passer au temps 2 :

- Comptez les cas : il en faut au moins autant que de scénarios Gherkin.
- Recalculez à la main trois résultats attendus, dont au moins un sur une borne de palier (5 000,00 EUR est dans le palier 6 % ; 4 999,99 dans le palier 3 %).
- Repérez tout résultat attendu qui n'est pas déductible de la story : il doit être marqué « NON SPÉCIFIÉ », pas inventé.

**Temps 2 — Matrice (12 minutes).** Appliquez le prompt 05 en niveau « strict », avec en exigences : les six règles métier de NOVA-3, les six scénarios, et les cas limites de la section 8 de la spec qui concernent le calcul de remise. En cas de test : ceux du temps 1.

Lisez d'abord la liste des exigences non couvertes, avant la matrice elle-même.

**Temps 3 — Décision (6 minutes).** Pour chaque exigence non couverte, décidez : ajouter un cas de test (lequel, en une ligne), ou considérer que l'exigence n'est pas du ressort de cette story (pourquoi). Pour chaque cas de test sans exigence : le retirer, ou proposer l'exigence manquante à ajouter à la story.

## Questions à vous poser

- Quelle règle de NOVA-3 n'est testée par aucun de ses propres scénarios ? Comment l'auriez-vous vu sans la matrice ?
- Combien de cas de test « HORS CRITÈRES » proposés par Claude au temps 1 sont réapparus comme exigences légitimes au temps 2 ?
- Le taux de couverture affiché signifie-t-il quelque chose en dehors de cette exécution ?

## Contraintes

- Aucun résultat attendu accepté sans vérification du calcul.
- La matrice est un tableau : si Claude vous la décrit en prose, demandez le tableau.

## Ce que vous rendez

La liste des cas de test (tableau récapitulatif), la matrice, la liste des exigences non couvertes avec votre décision pour chacune.
