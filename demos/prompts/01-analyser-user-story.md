# 01 — Analyser une user story avant le sprint

## À quoi sert ce prompt

Obtenir en deux minutes un diagnostic structuré d'une story avant l'affinage : ce qui est clair, ce qui manque, ce qui est ambigu, et les questions à poser au demandeur. Il ne réécrit pas la story (voir le prompt 02), il la diagnostique.

## Rôle donné à Claude

Product Owner expérimenté qui relit la story d'un collègue, avec bienveillance mais sans complaisance, et qui ne comble jamais les trous par des suppositions.

## Contexte à fournir

- Le texte complet de la story (ou sa clé avec le connecteur : « lis le ticket {clé} »).
- Le nom du produit et l'acteur principal si la story ne les dit pas.
- Facultatif : la page de spécification de référence, pour que les questions soient pertinentes.

## Le prompt

```text
Tu es un Product Owner expérimenté. Tu relis une user story avant sa séance d'affinage pour dire à son auteur ce qui manque, sans la réécrire et sans rien supposer à sa place.

Produit : {nom_du_produit}. Story : {clé_ticket} « {résumé_ticket} ». Texte complet entre balises <story>.

Analyse en six points, dans cet ordre :

1. CE QUE LA STORY DEMANDE, reformulé en une phrase « En tant que…, je veux…, afin de… ». Si un des trois éléments est absent du texte, écris « ABSENT » à sa place plutôt que de l'inventer.
2. CE QUI EST CLAIR : les éléments exploitables tels quels, chacun avec la citation du texte qui le porte.
3. CE QUI EST AMBIGU : chaque formulation qui peut se lire de deux façons, avec la citation exacte et les deux lectures possibles.
4. CE QUI MANQUE pour qu'un développeur et un testeur puissent travailler sans revenir vers toi : règles chiffrées, acteurs, cas d'erreur, périmètre exclu, critères d'acceptation. Une ligne par manque.
5. LES RISQUES : contradictions internes, dépendances sous-entendues (« ne pas casser… »), urgence non justifiée.
6. LES QUESTIONS À POSER AU DEMANDEUR : au plus huit, chacune formulée pour obtenir une réponse chiffrée, une liste fermée ou un oui/non. Classe-les de la plus bloquante à la moins bloquante.

Contraintes : n'invente aucune règle métier, aucun chiffre, aucun acteur. Ne propose pas de solution technique. Termine par un verdict en une ligne : « Prête pour l'affinage » ou « À compléter avant l'affinage », justifié par le point le plus bloquant.

<story>
{texte_complet_de_la_story}
</story>
```

## Exemple rempli sur NOVA

`{nom_du_produit}` = tunnel de commande B2B NovaTech ; `{clé_ticket}` = NOVA-2 ; `{résumé_ticket}` = Améliorer la remise des commandes ; `<story>` = la description de NOVA-2 dans `exports/backlog-nova.md`.

Ce que vous devez obtenir : un « En tant que » où l'acteur hésite entre le commercial et le client (les deux apparaissent dans le texte), au moins cinq ambiguïtés (« client important », « montant élevé », « plus intéressante », « quand c'est possible », « sauf cas particulier »), et des questions qui demandent des seuils chiffrés.

## Pièges connus

- Claude propose spontanément un « En tant que » complet et fluide même quand l'acteur n'est pas dans le texte. La consigne « ABSENT » le retient, mais vérifiez : la phrase reformulée doit être traçable au texte.
- Il classe parfois en « clair » un élément qui n'est qu'affirmé avec assurance (« Attention à ne pas casser l'export vers la comptabilité » est une contrainte, pas une exigence claire : quel export, quel format ?).
- Les questions ont tendance à être ouvertes (« Quels sont les besoins des commerciaux ? »). Si c'est le cas, relancez : « reformule chaque question pour qu'on puisse y répondre par un nombre ou un oui/non ».
- Sur une story bien rédigée (NOVA-3), Claude cherche quand même des manques et en trouve des mineurs. C'est normal ; le verdict final doit refléter la proportion, pas le nombre brut.

## Comment l'améliorer collectivement

Notez, story après story, quelles questions produites par Claude ont réellement servi en affinage ; au bout d'un mois, remplacez la liste générique du point 4 par les manques les plus fréquents de votre équipe.
