# 02 — Réécrire une story avec critères d'acceptation en Gherkin

## À quoi sert ce prompt

Transformer une demande floue en story exploitable : contexte, user story, règles métier, critères d'acceptation en Gherkin (Étant donné / Quand / Alors), hors périmètre. Il est conçu pour venir après le prompt 01 ou après un échange avec le demandeur : Claude réécrit avec les réponses que vous avez obtenues, il n'invente pas celles qui manquent.

## Rôle donné à Claude

Product Owner rédacteur, qui écrit une story que l'équipe de développement peut estimer et que les testeurs peuvent recetter, et qui marque explicitement chaque décision non prise.

## Contexte à fournir

- Le texte d'origine de la story.
- Les décisions obtenues depuis (réponses du demandeur, extraits de spec), sous forme de liste.
- La page de spécification de référence si elle existe : c'est elle qui fournit les chiffres.
- Le modèle de story de votre équipe si vous en avez un (sinon, celui du prompt).

## Le prompt

```text
Tu es un Product Owner qui réécrit une user story pour qu'elle soit prête à entrer en sprint.

Tu disposes de :
- la story d'origine (entre balises <origine>),
- les décisions prises depuis sa rédaction (entre balises <decisions>),
- la spécification de référence « {titre_page} » (entre balises <spec>).

Écris la story au format suivant, en Markdown :

## Contexte
Deux à quatre phrases : le problème actuel et ce que la story change. Rien d'autre.

## User story
« En tant que {acteur}, je veux {action}, afin de {bénéfice}. » Un seul acteur. Si le texte d'origine en mélange plusieurs, choisis celui que les décisions désignent ; sinon écris « ACTEUR À CONFIRMER : … ».

## Règles métier
Liste numérotée. Chaque règle est chiffrée ou fermée (liste finie de valeurs). Chaque règle cite sa source : « (spec §{n}) » ou « (décision du {date}) ». Une règle sans source s'écrit « [À TRANCHER] » suivi de la question.

## Critères d'acceptation
Un bloc Gherkin en français (Fonctionnalité, Scénario, Étant donné, Et, Quand, Alors). Un scénario par règle métier, plus un scénario nominal, plus au moins deux scénarios d'erreur ou de cas limite. Les valeurs des scénarios sont des exemples concrets (montants, catégories, dates), jamais des variables.

## Hors périmètre
Ce que la story ne couvre pas, notamment ce que le texte d'origine évoquait sans le décider.

## Points ouverts
La liste des « [À TRANCHER] » avec le nom de la personne qui doit trancher si tu la connais, sinon « à désigner ».

Contraintes : n'invente ni chiffre ni règle ; tout ce qui n'est ni dans <origine>, ni dans <decisions>, ni dans <spec> devient un point ouvert. Pas de solution technique, pas de nom de table ni d'API.

<origine>
{texte_de_la_story_origine}
</origine>

<decisions>
{liste_des_decisions_obtenues}
</decisions>

<spec>
{texte_de_la_page_de_spec}
</spec>
```

## Exemple rempli sur NOVA

`<origine>` = NOVA-2 ; `<decisions>` = par exemple « Le demandeur confirme que l'acteur est le client professionnel authentifié ; la remise visée est le barème par volume de la spec ; la question du cumul est tranchée par la spec (non-cumul absolu) » ; `<spec>` = `exports/confluence/02-regles-de-remise.md` ; `{titre_page}` = Règles de remise.

Le résultat attendu ressemble structurellement à NOVA-3 (« Appliquer un barème de remise par volume »), qui est le contre-exemple bien rédigé du backlog. Comparez les deux : c'est un bon exercice de calibration.

## Pièges connus

- Si `<decisions>` est vide, Claude prend les chiffres de la spec pour combler et produit une story plausible qui n'a jamais été validée par le demandeur. La spec donne les règles ; elle ne dit pas ce que le demandeur voulait. Gardez les points ouverts.
- Les scénarios Gherkin de Claude utilisent volontiers des valeurs « rondes » pile sur les seuils (1 000, 5 000). C'est bien pour les paliers, mais ajoutez un scénario juste en dessous et juste au-dessus (999,99 et 1 000,00) : Claude ne le fait pas spontanément.
- Il oublie souvent le scénario « panier vide » ou « montant nul ». Demandez-le.
- Le « Contexte » a tendance à gonfler avec des bénéfices marketing non sourcés (« améliorer la satisfaction client »). Coupez.
- Il peut glisser des noms de champs ou d'API dans les règles. Ce n'est pas votre rôle ; la contrainte le lui rappelle mais relisez.

## Comment l'améliorer collectivement

Remplacez le modèle de story du prompt par celui de votre équipe (Definition of Ready comprise) et faites-le relire par un développeur et un testeur : ce sont eux les lecteurs de la story produite.
