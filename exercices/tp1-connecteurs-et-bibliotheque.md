# Atelier 1 (J1) — Connecteurs Atlassian et bibliothèque de prompts

Durée : 1 h 30. Individuel pour la connexion, collectif pour la bibliothèque.

## Objectifs

1. Connecter votre compte Atlassian à Claude Desktop par OAuth et vérifier que Claude lit le projet NOVA.
2. Faire faire à Claude trois opérations de lecture typiques d'un PO via le connecteur.
3. Contribuer un prompt à la bibliothèque commune, au format de la bibliothèque, testé sur NOVA.

## Prérequis

- Claude Desktop installé et connecté à votre abonnement (Pro ou Team).
- Votre compte Atlassian invité sur le site NOVA (vous avez reçu l'invitation par mail ; acceptez-la et connectez-vous une fois dans le navigateur avant l'atelier).
- Le dossier `prompts/` de la formation sous la main.

## Partie 1 — Connexion OAuth (20 minutes)

1. Dans Claude Desktop : Paramètres, Connecteurs, Atlassian, « Autoriser ». Une fenêtre du navigateur s'ouvre sur Atlassian : connectez-vous avec le compte invité sur NOVA, choisissez le site NOVA si plusieurs sites sont proposés, acceptez les permissions.
2. De retour dans Claude Desktop, le connecteur apparaît comme actif. Ouvrez une nouvelle conversation (hors Projet pour l'instant).
3. Vérification : envoyez

```text
Liste les tickets du projet NOVA avec leur clé, leur type et leur résumé, triés par clé.
```

Vous devez obtenir 14 tickets, de NOVA-1 à NOVA-14. Si Claude vous demande d'autoriser l'action, acceptez ; si la liste est incomplète, demandez « continue jusqu'à NOVA-14 ».

4. Deuxième vérification, côté Confluence :

```text
Liste les pages de l'espace Confluence « NOVA — Spécifications » avec leur titre et leur date de dernière modification.
```

Vous devez voir quatre pages.

Notez ce qui s'est passé à chaque étape (demande d'autorisation, nombre de résultats, temps de réponse) : ce sont vos premières observations pour la grille du jour 3.

## Partie 2 — Trois opérations de lecture (25 minutes)

Toujours via le connecteur, faites réaliser à Claude ces trois opérations et vérifiez chaque résultat dans Jira ou Confluence (ouvrez l'outil, comparez) :

**Opération A — Recherche ciblée.**

```text
Recherche dans le projet NOVA les tickets dont le résumé ou la description parle de « remise » et de « validation ». Donne la clé, le résumé, et la phrase exacte qui contient les deux mots.
```

Vérifiez que les phrases citées existent réellement dans les tickets.

**Opération B — Lecture croisée.**

```text
Lis le ticket NOVA-5 et la section « Dette technique connue » de la page « Architecture de l'application de gestion des commandes ». Ce bug était-il annoncé par la page d'architecture ? Cite les deux textes.
```

**Opération C — Synthèse pour quelqu'un d'autre.**

```text
Lis l'epic NOVA-1 et rédige, en 100 mots maximum et sans aucun terme technique, ce que cette refonte change pour un commercial itinérant. Chaque affirmation doit venir de l'epic.
```

Pour chaque opération, notez : le résultat était-il exact ? Complet ? Y avait-il quelque chose d'inventé ? Combien de temps avez-vous mis à le vérifier ?

## Partie 3 — Contribuer un prompt à la bibliothèque (40 minutes)

La bibliothèque contient quatorze prompts (`prompts/README.md`). Chaque participant en ajoute un, sur une tâche de son quotidien qui n'y figure pas encore ou qui y figure mal. Exemples de manques : préparer une démo de sprint, rédiger la réponse à un client mécontent, comparer deux versions d'une spec, préparer un ordre du jour de rétrospective, rédiger les notes de version.

1. Choisissez la tâche et écrivez-la en une phrase : « ce prompt sert à … quand … ».
2. Rédigez le prompt avec les cinq blocs (rôle, contexte, contrainte, format, source), les variables entre accolades, en vous inspirant du format des fichiers existants. Vous pouvez demander à Claude de vous aider à le structurer, à condition de le tester vous-même ensuite.
3. Testez-le sur NOVA (un ticket, une page, via le connecteur ou l'export) au moins deux fois, avec deux variantes, et notez ce qu'il invente ou rate : c'est la section « Pièges connus », obligatoire.
4. Rédigez le fichier au format de la bibliothèque : titre, à quoi il sert, rôle, contexte à fournir, le prompt, l'exemple rempli sur NOVA, les pièges, « comment l'améliorer collectivement ».
5. Déposez-le :
   - sur Team : dans le Projet Claude partagé « Bibliothèque de prompts PO » (connaissances du Projet), et annoncez-le dans le canal ;
   - sur Pro : dans le dossier partagé de la formation, avec un nom `NN-<sujet>.md` (numéro suivant).
6. Relisez le prompt d'un autre participant et testez-le une fois : indiquez-lui un piège qu'il n'avait pas vu.

## Variante « exports » si le connecteur échoue

Si l'OAuth échoue (compte non invité, site non proposé, blocage réseau) :

1. Dans votre Projet « NOVA — Spécifications » (exercice 2), ajoutez `exports/backlog-nova.md` aux connaissances ; les quatre pages de `exports/confluence/` y sont déjà.
2. Partie 1 : remplacez la vérification par « Liste les tickets du fichier backlog-nova.md avec clé, type, résumé ». Résultat attendu identique : 14 tickets.
3. Partie 2 : les trois opérations fonctionnent à l'identique en remplaçant « lis le ticket X » par « dans backlog-nova.md, prends le ticket X » et « lis la page Y » par « dans le fichier Y ».
4. Partie 3 : inchangée.

Notez dans votre livrable que vous avez travaillé en mode export, et ce qui vous a bloqué : c'est une information utile pour le déploiement dans votre entreprise.

## Livrables

- Votre fiche d'observation des parties 1 et 2 (ce qui a marché, ce qui a été inventé, temps de vérification).
- Votre prompt au format de la bibliothèque, déposé et annoncé.
- Le piège que vous avez trouvé dans le prompt d'un collègue.

## Règles de prudence avec le connecteur

- Aujourd'hui vous ne faites que lire. Claude sait aussi créer et modifier des tickets et des pages : n'utilisez pas ces capacités avant l'atelier du jour 2, et jamais sans relire le brouillon avant d'autoriser l'écriture.
- Le site NOVA est un bac à sable. Sur votre vrai site, l'accès de Claude est celui de votre compte : ce que vous pouvez modifier, il peut le modifier si vous le lui demandez.
