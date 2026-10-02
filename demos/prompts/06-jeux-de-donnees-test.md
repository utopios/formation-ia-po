# 06 — Produire des jeux de données de test

## À quoi sert ce prompt

Obtenir un tableau de données de test réalistes et cohérentes (clients, paniers, montants) qui couvrent les paliers, les bornes et les cas d'erreur d'une règle métier, avec le résultat attendu déjà calculé et vérifiable. Le testeur les saisit ou les fait créer ; le PO les utilise pour illustrer une règle en réunion.

## Rôle donné à Claude

Analyste de test qui construit des données comme on construit une preuve : chaque ligne existe pour une raison, et le résultat attendu est calculé pas à pas.

## Contexte à fournir

- La règle métier ou le barème (texte exact, avec les bornes inclus/exclu).
- Les dimensions à croiser : catégories de client, plages de montants, présence d'une remise contractuelle, etc.
- Le nombre de lignes souhaité et le format (tableau Markdown, CSV à coller dans un tableur).
- Les contraintes de réalisme : noms fictifs, pas de données réelles.

## Le prompt

```text
Tu es analyste de test. Tu construis un jeu de données de test pour la règle métier ci-dessous, entre balises <regle>.

Dimensions à couvrir : {dimensions}. Nombre de lignes visé : {nombre} au plus. Format de sortie : {format : tableau Markdown ou CSV point-virgule}.

Méthode imposée :
1. Liste d'abord les classes d'équivalence de chaque dimension et les valeurs frontières exactes (juste en dessous, sur la borne, juste au-dessus), à partir du texte de la règle. Cite la phrase de la règle qui fixe chaque borne.
2. Construis ensuite le tableau. Colonnes : ID (JD-nn), Objectif de la ligne (10 mots maximum : quelle classe ou quelle frontière), {colonnes_d_entrée}, Résultat attendu, Détail du calcul (une ligne : formule avec les valeurs), Source (section de la règle).
3. Inclus au minimum : une ligne par palier, une ligne sur chaque borne, une ligne juste en dessous et une juste au-dessus de chaque borne, une ligne pour chaque cas d'erreur ou cas vide, une ligne où l'arrondi joue (résultat non entier au centime).
4. Termine par la liste des lignes que tu n'as PAS pu calculer parce que la règle ne dit pas quoi faire, avec la question à poser.

Contraintes : données fictives (raisons sociales inventées, identifiants CLI-nnn). Tous les montants sont au centime. Vérifie chaque calcul une seconde fois avant de répondre ; une erreur de calcul dans un jeu de données coûte une journée de recette.

<regle>
{texte_de_la_regle}
</regle>
```

## Exemple rempli sur NOVA

`<regle>` = sections 2 et 3 de `exports/confluence/02-regles-de-remise.md` (barème par volume, bonus de catégorie, plafond de 15 %) ; `{dimensions}` = catégorie (Standard, Privilège, Grand compte) et montant HT du panier ; `{nombre}` = 20 ; `{colonnes_d_entrée}` = Catégorie, Montant HT du panier.

Lignes à retrouver obligatoirement : 999,99 (0 %), 1 000,00 (3 %), 4 999,99 (3 %), 5 000,00 (6 %), 19 999,99 (6 %), 20 000,00 (9 %) pour un client Standard ; un Grand compte à 20 000,00 (9 + 4 = 13 %, sous le plafond) ; et un cas où l'arrondi joue (par exemple 1 234,56 EUR à 3 %, soit 37,0368 arrondi à 37,04).

## Pièges connus

- Les bornes inclus/exclu sont le premier lieu d'erreur : Claude met parfois 5 000,00 à 3 %. Contrôlez chaque borne à la main, c'est l'affaire d'une minute.
- Il génère des montants ronds (1 500, 7 500) qui ne font jamais jouer l'arrondi. Le point 3 l'impose ; si aucune ligne n'a un résultat en centimes « non rond », relancez.
- Sur les 20 lignes demandées, il en produit parfois 12 et considère le travail fini. Comptez.
- Il peut ajouter une colonne « TVA » ou « frais de port » non demandée et se tromper dans l'ordre de calcul. Supprimez ce qui n'est pas dans les dimensions.
- Quand la règle est silencieuse (montant négatif ?), il invente un comportement « raisonnable » au lieu de le mettre au point 4. Vérifiez que le point 4 n'est pas vide.

## Comment l'améliorer collectivement

Faites recalculer le tableau produit par Claude dans un tableur (une formule par ligne) et archivez le tableau vérifié comme jeu de référence dans le Projet : les prochaines générations pourront lui être comparées.
