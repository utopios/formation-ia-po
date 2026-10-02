# 11 — Confronter un ticket à une page de spécification (avec citations obligatoires)

## À quoi sert ce prompt

Avant de lancer un développement, vérifier qu'une user story ne contredit pas la spécification en vigueur. C'est le prompt le plus rentable de la bibliothèque : une incohérence trouvée avant le sprint coûte une heure de discussion ; trouvée en recette, elle coûte un sprint.

## Rôle donné à Claude

Analyste fonctionnel méticuleux, qui ne juge pas la story mais la compare ligne à ligne à un document de référence, et qui refuse d'affirmer quoi que ce soit sans citer les deux textes.

## Contexte à fournir

- Le texte complet du ticket (ou sa clé si le connecteur Atlassian est actif : « lis le ticket {clé} »).
- Le texte complet de la page de spécification (ou son titre si le connecteur est actif : « lis la page Confluence {titre} de l'espace {espace} »).
- Si vous travaillez dans un Projet Claude où les pages sont en connaissance, il suffit de nommer la page.

## Le prompt

```text
Tu es un analyste fonctionnel chargé de vérifier la cohérence entre une user story Jira et la spécification en vigueur, avant qu'elle n'entre en sprint.

Tu compares deux textes :
1. La user story {clé_ticket} : « {résumé_ticket} ». Son texte complet est ci-dessous entre balises <ticket>.
2. La page de spécification « {titre_page} » (version {version_page}). Son texte complet est ci-dessous entre balises <spec>.

Règles absolues :
- Tu ne t'appuies QUE sur ces deux textes. Si une information manque, tu écris « non précisé dans les documents fournis ».
- Chaque incohérence que tu signales DOIT comporter une citation textuelle exacte du ticket ET une citation textuelle exacte de la spec (entre guillemets, avec le numéro de section de la spec). Sans double citation, tu ne la signales pas.
- Tu distingues trois catégories : CONTRADICTION (le ticket dit le contraire de la spec), IMPRÉCISION (le ticket reste flou là où la spec a déjà tranché), HORS SPEC (le ticket demande quelque chose que la spec ne prévoit pas ou exclut).
- Tu n'inventes aucun chiffre, aucune règle, aucun acteur.

Format de réponse :
1. Un tableau avec les colonnes : N°, Catégorie, Sujet, Citation du ticket, Citation de la spec (avec section), Ce qu'il faut trancher.
2. Une liste des points du ticket que la spec confirme (pour ne pas les rouvrir en réunion).
3. Trois questions à poser au demandeur du ticket, formulées pour obtenir une réponse chiffrée ou un oui/non.
4. Une phrase de conclusion : le ticket peut-il entrer en sprint tel quel ? Oui / Non, et pourquoi en une ligne.

<ticket>
{texte_complet_du_ticket}
</ticket>

<spec>
{texte_complet_de_la_page}
</spec>
```

## Exemple rempli sur NOVA

Variables : `{clé_ticket}` = NOVA-2 ; `{résumé_ticket}` = Améliorer la remise des commandes ; `{titre_page}` = Règles de remise ; `{version_page}` = 2.4 ; `{texte_complet_du_ticket}` = description de NOVA-2 (export `backlog-nova.md`) ; `{texte_complet_de_la_page}` = `exports/confluence/02-regles-de-remise.md`.

Avec le connecteur Atlassian, la fin du prompt devient simplement :

```text
Lis le ticket NOVA-2 du projet NOVA et la page « Règles de remise » de l'espace « NOVA — Spécifications », puis applique les règles ci-dessus.
```

Ce que vous devez voir apparaître au minimum : la question du cumul des remises (le ticket ouvre une exception, la spec la ferme), et le fait que le ticket ne chiffre rien là où la spec a un barème, un plafond et des seuils de délégation.

## Pièges connus

- Sans la règle de double citation, Claude produit des incohérences plausibles mais paraphrasées ; vous ne pouvez plus vérifier. Gardez la règle.
- Claude a tendance à ajouter des « bonnes pratiques » (accessibilité, performance) qui ne sont ni dans le ticket ni dans la spec. Le format à trois catégories limite ce débordement ; s'il persiste, ajoutez « aucune recommandation hors des deux textes ».
- Si vous collez un extrait de la spec au lieu de la page entière, Claude signalera des « absences » qui n'en sont pas. Donnez la page complète.
- Un même écart peut être compté deux fois sous deux angles (par exemple « pas de chiffre » et « rôle non nommé » pour la même phrase). Ce n'est pas faux, mais dédoublonnez avant de rapporter.
- Claude peut classer « IMPRÉCISION » ce que vous jugez « CONTRADICTION ». La catégorie est un point de départ de discussion, pas un verdict.

## Comment l'améliorer collectivement

Après chaque usage réel, notez dans le Projet partagé les incohérences que Claude a ratées et celles qu'il a inventées ; au bout de cinq usages, ajoutez au prompt une ligne « attention particulière à : … » construite sur ces ratés.
