# 05 — Construire une matrice de couverture exigences / cas de test

## À quoi sert ce prompt

Vérifier qu'aucune règle métier ni aucun critère d'acceptation n'est oublié par la campagne de test, et qu'aucun cas de test ne teste « rien ». La matrice croise les exigences (lignes) et les cas de test (colonnes) ; les lignes vides sont les trous de couverture.

## Rôle donné à Claude

Responsable de recette qui prépare la revue de couverture avec le PO, et qui préfère signaler un doute plutôt que cocher une case par complaisance.

## Contexte à fournir

- La liste des exigences : règles métier, critères d'acceptation, cas limites de la spec. Donnez-leur un identifiant si elles n'en ont pas, ou laissez Claude les numéroter.
- La liste des cas de test (par exemple la sortie du prompt 04).
- Le niveau de rigueur : « couverture stricte » (un cas ne couvre une exigence que s'il vérifie explicitement sa valeur) ou « couverture large ».

## Le prompt

```text
Tu es responsable de recette. Tu construis la matrice de couverture entre les exigences d'une story et les cas de test rédigés, pour la revue avec le Product Owner.

Story : {clé_ticket}. Exigences entre balises <exigences> (règles métier, critères d'acceptation, cas limites). Cas de test entre balises <tests>. Niveau : {couverture_stricte_ou_large}.

Étapes :

1. Numérote les exigences si elles ne le sont pas : EX-01, EX-02… Une exigence = une règle vérifiable. Si une phrase contient deux règles (par exemple un taux et un plafond), sépare-les en deux exigences et dis-le.
2. Construis la matrice : lignes = exigences, colonnes = cas de test, cellule = « X » si le cas vérifie explicitement l'exigence, « (x) » s'il l'exerce sans la vérifier (l'exigence est en jeu mais aucun résultat attendu ne la contrôle), vide sinon. Au format tableau Markdown.
3. Liste les EXIGENCES NON COUVERTES (ligne sans « X ») avec, pour chacune, le cas de test minimal à ajouter (une ligne : précondition, action, résultat attendu chiffré).
4. Liste les CAS DE TEST SANS EXIGENCE (colonne vide) et propose : supprimer, ou l'exigence manquante qu'ils révèlent et qu'il faut alors ajouter à la story.
5. Liste les EXIGENCES SUR-COUVERTES (quatre « X » ou plus) et dis si c'est justifié (règle à risque) ou redondant.
6. Donne un taux de couverture : exigences avec au moins un « X » divisé par nombre total d'exigences, avec le détail du calcul.

Contraintes : ne coche « X » que si le résultat attendu du cas de test contient la valeur que l'exigence impose. En cas de doute, « (x) » et une note. N'ajoute aucune exigence qui ne vient pas des textes fournis, sauf au point 4 où tu la marques « PROPOSÉE ».

<exigences>
{regles_et_criteres}
</exigences>

<tests>
{liste_des_cas_de_test}
</tests>
```

## Exemple rempli sur NOVA

`{clé_ticket}` = NOVA-3 ; `<exigences>` = les six règles métier et les six scénarios Gherkin de NOVA-3, plus les cas limites de la section 8 de la page « Spécification fonctionnelle — Tunnel de commande » ; `<tests>` = les cas produits par le prompt 04 ; niveau strict.

Résultat typique : la règle « arrondi au centime, demi vers le haut » n'est couverte par aucun scénario de NOVA-3 (tous les montants d'exemple tombent juste). C'est exactement le genre de trou que la matrice sert à révéler.

## Pièges connus

- Sans le niveau « strict », Claude coche généreusement : un cas qui passe par un panier de 5 000 EUR « couvre » à ses yeux le plafond de 15 % alors qu'il ne le vérifie pas. Le « (x) » existe pour ça.
- Le taux de couverture final est calculé sur les exigences que Claude a lui-même découpées ; deux découpages donnent deux taux. Ne comparez pas des taux entre deux exécutions, comparez les listes de trous.
- Il arrive que la matrice soit tronquée si elle dépasse une vingtaine de colonnes. Découpez alors par bloc de cas de test, ou demandez la matrice transposée.
- Les « cas de test sans exigence » sont parfois des cas légitimes issus de la spec globale (client sans catégorie) : le bon réflexe est d'ajouter l'exigence à la story, pas de supprimer le test.

## Comment l'améliorer collectivement

Gardez la liste des « trous récurrents » (arrondis, bornes de paliers, cas vides) et ajoutez-la au prompt comme point 7 : « vérifie spécialement la couverture de : … ». C'est la mémoire de vos propres campagnes.
