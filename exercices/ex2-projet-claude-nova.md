# Exercice 2 (J1) — Construire le Projet Claude « NOVA » et vérifier les citations

## Objectif

Créer un Projet Claude qui connaît la spécification NOVA, lui donner des instructions de comportement, puis vérifier que ses réponses s'appuient sur les documents et les citent. Ce Projet vous servira pendant toute la formation.

## Matériel

Les quatre pages de spécification, dans `exports/confluence/` :

- `01-specification-fonctionnelle-tunnel-commande.md`
- `02-regles-de-remise.md`
- `03-normes-et-conventions-de-developpement.md`
- `04-architecture-application-commandes.md`

## Étape 1 — Créer le Projet (5 minutes)

Dans Claude (barre latérale, « Projets », « Nouveau projet ») :

- Nom : `NOVA — Spécifications`.
- Description : une phrase qui dit à quoi sert le Projet, par exemple « Référentiel de spécification du tunnel de commande NovaTech, pour analyser les tickets et préparer la recette ».

## Étape 2 — Ajouter la connaissance (5 minutes)

Dans le panneau de droite du Projet, « Connaissances du projet », ajoutez les quatre fichiers Markdown. Vérifiez que les quatre apparaissent et que la jauge de capacité reste faible.

## Étape 3 — Écrire les instructions (10 minutes)

Toujours dans le Projet, « Définir des instructions personnalisées ». Rédigez des instructions qui couvrent au moins :

- qui vous êtes et ce que vous attendez (rôle du Projet) ;
- la règle de citation : toute affirmation sur une règle métier cite le document et la section ;
- ce que Claude doit faire quand l'information n'est pas dans les documents (le dire, ne pas compléter) ;
- la langue, le ton, le format par défaut (par exemple : français, tableaux quand il y a plus de trois éléments) ;
- ce qu'il ne doit jamais faire (inventer un chiffre, proposer une solution technique, décider à votre place).

Gardez vos instructions sous 200 mots : c'est une consigne permanente, pas un prompt.

## Étape 4 — Poser trois questions et vérifier (10 minutes)

Dans une conversation du Projet, posez ces trois questions, une par une, sans rien ajouter :

1. « Quel est le taux de remise d'un client Privilège pour un panier de 5 000 EUR HT, et d'où vient ce chiffre ? »
2. « Que se passe-t-il si le responsable commercial ne répond pas à une demande de validation de remise ? »
3. « Quel est le délai de livraison standard d'une commande ? »

Pour chaque réponse, vérifiez et notez :

- La réponse cite-t-elle le document et la section ? Ouvrez le fichier source et contrôlez la citation mot pour mot.
- Le chiffre ou la règle est-il exact ? Refaites le calcul de la question 1 à la main à partir de la page « Règles de remise ».
- Pour la question 3 : la réponse dit-elle clairement que l'information n'est pas dans les documents, ou propose-t-elle un délai ?

## Ce que vous rendez

Vos instructions de Projet (copiées dans le canal) et, pour chacune des trois questions, un verdict en une ligne : « cité et exact », « cité mais faux », « non cité », « a inventé ».

## Si une réponse ne cite pas sa source

Ne corrigez pas dans la conversation : corrigez les instructions du Projet, ouvrez une nouvelle conversation et reposez la question. C'est la différence entre réparer une réponse et réparer le système.
