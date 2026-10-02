# Atelier 2 (J2) — Feature réelle et lab d'analyse de backlog


## Objectifs

- Partie A : produire, depuis l'epic NOVA-1, un jeu de stories, de critères d'acceptation et de cas de test, puis critiquer cette production avec une check-list, pour apprendre à ne pas se laisser impressionner par une sortie bien mise en forme.
- Partie B : faire analyser le backlog NOVA par Claude, puis lui faire confronter le ticket NOVA-2 à la page « Règles de remise ». Attendu : au moins trois incohérences entre les tickets et la spécification, chacune avec double citation, dans un rapport exploitable en revue de backlog.

## Matériel

- Le Projet « NOVA — Spécifications » du jour 1 (quatre pages en connaissance).
- Le connecteur Atlassian, ou `exports/backlog-nova.md` déposé dans le Projet.
- Les prompts 02, 03, 04, 11 et 13 de la bibliothèque.

---

## Partie A — De l'epic à la feature, puis critique (1 h 15)

### A1 — Découpage de l'epic (20 minutes)

Appliquez le prompt 03 à l'epic NOVA-1 (« lis l'epic NOVA-1 » ou son texte depuis l'export), avec comme spec la page « Règles de remise » et la « Spécification fonctionnelle — Tunnel de commande ». Taille visée : stories livrables en un sprint de deux semaines.

Sans regarder les stories NOVA-2 à NOVA-14, obtenez une carte de cinq à sept stories. Puis seulement, listez les stories réelles du backlog et comparez : qu'avez-vous en plus, en moins ? Notez les écarts, sans conclure encore.

### A2 — Une story complète (25 minutes)

Choisissez une des stories de votre carte, celle qui vous paraît la plus prioritaire. Appliquez le prompt 02 pour la rédiger avec ses critères Gherkin, en donnant comme `<decisions>` uniquement ce que la spec permet de décider. Puis appliquez le prompt 04 pour obtenir ses cas de test.

Vous avez maintenant une story, des critères, des cas de test : une « feature » prête à présenter. Sauvegardez la sortie telle quelle avant la suite.

### A3 — Critique avec la check-list (30 minutes)

Passez votre feature à la check-list ci-dessous. Pour chaque point, répondez oui ou non et notez la preuve (citation de votre sortie et, s'il y a lieu, de la spec). Faites-le sans Claude d'abord ; vous pourrez ensuite lui demander de faire le même contrôle et comparer.

**Check-list de critique d'une feature produite avec l'IA**

Sourçage
- [ ] Chaque règle métier chiffrée a une source identifiable (section de spec ou décision explicite) ?
- [ ] Aucun chiffre, seuil, délai ou acteur n'apparaît qui ne soit ni dans l'epic, ni dans la spec, ni dans vos décisions ?
- [ ] Les points non tranchés sont listés comme ouverts et non comblés par des valeurs « raisonnables » ?

Cohérence
- [ ] La story ne contredit aucune section de la page « Règles de remise » (non-cumul, plafond de 15 %, seuils de délégation, délai de 48 h) ?
- [ ] La story ne contredit pas la « Spécification fonctionnelle » (ordre de calcul, arrondi, montant minimum de commande, cas limites de la section 8) ?
- [ ] L'acteur de la story est un acteur de la section 2 de la spécification fonctionnelle ?

Testabilité
- [ ] Chaque scénario Gherkin a des valeurs concrètes et un « Alors » observable ?
- [ ] Les scénarios couvrent les bornes des paliers (juste en dessous, sur la borne) et pas seulement des valeurs rondes ?
- [ ] Au moins un scénario de cas vide ou d'erreur ?
- [ ] Les calculs des résultats attendus sont justes (recalculez-en trois, dont un sur une borne) ?

Périmètre
- [ ] La section « Hors périmètre » existe et mentionne ce que l'epic évoque sans que la story le traite ?
- [ ] Aucune solution technique (table, API, technologie) n'est glissée dans les règles ou les critères ?

Forme
- [ ] Le texte est collable dans Jira sans réécriture ?
- [ ] La longueur est comparable à celle de NOVA-3 (la story de référence du backlog), pas trois fois plus ?

Comptez vos « non ». Puis demandez à Claude, dans la même conversation :

```text
Relis la story, les critères et les cas de test que tu as produits en appliquant la check-list suivante. Pour chaque point, réponds oui ou non avec la preuve. Ne corrige rien pour l'instant.
```

(collez la check-list). Comparez ses réponses aux vôtres : où s'est-il donné un « oui » que vous lui refusez ? C'est le point central du débrief.

Enfin, corrigez la feature par relance, point par point, et gardez les deux versions.

---

## Partie B — Lab : analyse du backlog et confrontation NOVA-2 / Règles de remise (1 h 15)

### B1 — Audit du backlog (35 minutes)

Appliquez le prompt 13 à l'epic NOVA-1 :

- avec le connecteur : « Recherche tous les tickets du projet NOVA (JQL : project = NOVA ORDER BY key), lis la page « Règles de remise », puis applique l'analyse » ;
- avec l'export : `<backlog>` = `exports/backlog-nova.md`, `<spec>` = la page « Règles de remise » (déjà dans le Projet).

Contrôles à faire sur la sortie :

1. L'inventaire compte 14 lignes (NOVA-1 à NOVA-14). Sinon, relancez pour les manquants.
2. La partie « Incohérences avec la spécification » comporte, pour chaque ligne, une citation du ticket et une citation de la spec avec la section. Supprimez mentalement toute ligne sans double citation.
3. Pour trois incohérences au moins, ouvrez le ticket et la page, et vérifiez les citations mot pour mot.

Si la sortie s'est arrêtée en route (analyse longue), demandez « continue à partir de la partie 3 ». Si l'analyse est trop générale, relancez avec : « relis les sections 3, 4, 5, 6, 7 et 9 de la page Règles de remise et, pour chacune des stories NOVA-6 à NOVA-14, dis si elle contredit une de ces sections, avec citations ».

### B2 — Confrontation ciblée NOVA-2 / Règles de remise (25 minutes)

Appliquez le prompt 11 avec NOVA-2 et la page « Règles de remise ». Puis :

1. Vérifiez chaque ligne du tableau : citation du ticket exacte ? citation de la spec exacte et bonne section ? catégorie (contradiction, imprécision, hors spec) défendable ?
2. Fusionnez les lignes qui décrivent le même écart sous deux angles.
3. Choisissez les trois questions que vous poseriez réellement au demandeur de NOVA-2 et reformulez-les si elles ne sont pas fermées.
4. Testez la robustesse : ouvrez une nouvelle conversation, reposez la même demande sans la règle de double citation (retirez la phrase du prompt). Comparez le nombre et la vérifiabilité des écarts trouvés.

### B3 — Rapport de revue de backlog (15 minutes)

Rédigez (avec Claude, à partir de vos sorties vérifiées) une page « Revue de backlog NOVA — écarts avec la spécification » :

- un tableau : ticket, sujet, catégorie, citation du ticket, citation de la spec (section), décision proposée (corriger le ticket / demander un avenant à la spec / clore) ;
- au minimum trois écarts vérifiés par vous ;
- une liste des tickets sans écart détecté ;
- une phrase honnête sur ce que l'analyse n'a pas pu couvrir.

Avec le connecteur, vous pouvez demander à Claude de créer cette page dans l'espace « NOVA — Spécifications » sous un titre préfixé par votre nom, après relecture complète du brouillon et sur votre autorisation explicite. Ne modifiez aucun ticket existant.

## Variante « exports »

Toute la partie B fonctionne avec `exports/backlog-nova.md` et `exports/confluence/02-regles-de-remise.md` déposés dans le Projet. Pour B3, le rapport est rendu en Markdown dans le canal au lieu d'une page Confluence.

## Livrables

- Partie A : la feature avant et après critique, votre check-list remplie, et les points où Claude s'est évalué plus généreusement que vous.
- Partie B : le rapport de revue de backlog avec au moins trois écarts vérifiés et cités, et une note comparant les résultats avec et sans la règle de double citation.

## Critères de réussite

- Chaque écart rapporté est vérifiable en moins d'une minute par quelqu'un qui ouvre le ticket et la page.
- Aucun écart rapporté ne repose sur une règle que la spec ne contient pas.
- Vous savez dire ce que Claude a raté, pas seulement ce qu'il a trouvé.
