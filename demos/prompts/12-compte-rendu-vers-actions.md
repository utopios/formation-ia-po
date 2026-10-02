# 12 — Compte rendu de réunion vers actions et tickets

## À quoi sert ce prompt

Transformer des notes de réunion brutes (prises au vol, transcription, fil de discussion) en un compte rendu structuré : décisions prises, actions avec responsable et échéance, questions restées ouvertes, et brouillons de tickets Jira prêts à créer. Avec le connecteur Atlassian, Claude peut ensuite créer les tickets sur votre validation explicite.

## Rôle donné à Claude

Scrum Master ou PMO méticuleux qui distingue ce qui a été décidé de ce qui a été discuté, et qui n'attribue jamais une action à quelqu'un qui ne l'a pas acceptée dans les notes.

## Contexte à fournir

- Les notes brutes (texte, même désordonné).
- La liste des participants et leurs rôles.
- Le projet Jira cible et le type de ticket habituel.
- La date de la réunion.

## Le prompt

```text
Tu es Scrum Master. Tu transformes des notes de réunion brutes en compte rendu exploitable, sans rien ajouter à ce qui a été dit.

Réunion : {objet}, le {date}. Participants et rôles : {participants}. Projet Jira cible : {clé_projet}. Notes brutes entre balises <notes>.

Produis, dans cet ordre :

1. DÉCISIONS PRISES : liste numérotée. Une décision est une phrase qui engage (« nous appliquons », « le seuil est fixé à »). Pour chacune, la citation des notes qui la porte. Ce qui a été seulement discuté n'est pas une décision : mets-le au point 3.

2. ACTIONS : tableau Action (verbe à l'infinitif, résultat observable), Responsable (uniquement une personne qui apparaît dans les notes comme l'ayant acceptée ; sinon « À ATTRIBUER »), Échéance (uniquement si elle est dans les notes ; sinon « À FIXER »), Source (citation).

3. QUESTIONS OUVERTES : ce qui a été discuté sans conclusion, avec qui doit trancher si les notes le disent.

4. BROUILLONS DE TICKETS : pour chaque action ou décision qui nécessite un travail de l'équipe produit, un brouillon au format : Type (Story / Tâche / Bug), Résumé (10 mots maximum, forme verbale), Description (contexte en deux phrases, ce qui est attendu, ce qui n'est pas inclus), Lien avec un ticket existant si les notes en citent un. Ne crée AUCUN ticket, ce sont des brouillons.

5. CE QUE JE N'AI PAS COMPRIS : passages des notes ambigus ou contradictoires, cités, avec la question à poser.

Contraintes : aucune information hors des notes. Pas d'interprétation des silences (« X semblait d'accord »). Si les notes ne disent pas qui fait quoi, écris « À ATTRIBUER », n'attribue pas au participant le plus plausible.

<notes>
{notes_brutes}
</notes>
```

Avec le connecteur Atlassian, après relecture des brouillons, la relance est : « Crée dans le projet {clé_projet} les tickets 1 et 3 tels que rédigés, rattachés à {clé_epic}, et donne-moi leurs clés. » Faites-le brouillon par brouillon les premières fois.

## Exemple rempli sur NOVA

`<notes>` = un texte de votre cru simulant une réunion d'affinage sur NOVA-2 (participants : PO, directeur commercial, un développeur, un testeur) où le directeur commercial annonce vouloir 22 % pour un grand compte, où le développeur rappelle le plafond de la spec, et où la réunion se termine sans conclusion sur ce point mais avec une décision sur l'affichage de la remise dans le panier. `{clé_projet}` = NOVA.

Ce qu'il faut voir : le 22 % en question ouverte et pas en décision ; l'affichage en décision avec citation ; un brouillon de ticket pour l'affichage, aucun pour le 22 %.

## Pièges connus

- Claude transforme des souhaits en décisions (« le directeur veut 22 % » devient « décision : appliquer 22 % »). La règle de citation limite le risque ; relisez chaque décision.
- Il attribue les actions au rôle le plus logique (le testeur « écrira les cas de test ») même si personne ne l'a dit. Cherchez les « À ATTRIBUER » : s'il n'y en a aucun sur des notes floues, il a inventé.
- Les brouillons de tickets incluent parfois des détails techniques suggérés par le développeur en réunion ; gardez-les dans la description en les attribuant (« proposition de X en réunion »), pas comme exigence.
- Avec le connecteur, ne dites jamais « crée tous les tickets » sur une première exécution : relisez et créez un par un.

## Comment l'améliorer collectivement

Comparez, une fois par mois, le compte rendu produit par Claude avec ce que les participants pensent avoir décidé ; les écarts vous disent si ce sont vos notes ou le prompt qu'il faut améliorer (le plus souvent : les notes).
