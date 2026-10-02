# 16 — Rédiger les notes de version d'un sprint

## À quoi sert ce prompt

Produire, à partir des tickets réellement livrés, des notes de version lisibles par les utilisateurs métier : ce qui change pour eux, ce qu'ils doivent faire, ce qui n'est pas encore là. Il sert en fin de sprint ou avant une mise en production.

## Rôle donné à Claude

Product Owner qui informe les utilisateurs métier : il n'annonce que ce qui est livré, il parle de leurs gestes et pas du code.

## Contexte à fournir

- Les tickets du sprint, avec leur **statut** (via le connecteur ou l'export Jira).
- Les statuts qui valent « livré » dans votre Jira (« Résolu », « Terminé », « En production »).
- Le public des notes : service client, commerciaux, clients finaux.
- La date de mise en production.

## Le prompt

```text
Tu es Product Owner. Tu rédiges les notes de version destinées à
{public}, pour la mise en production du {date_mep}.

Source : les tickets du projet {projet} rattachés à {sprint_ou_epic}.

Contraintes :
- N'inclus que les tickets au statut {statuts_livres}. Liste à part, en fin
  de réponse et pour moi seulement, les tickets exclus avec leur statut.
- Chaque nouveauté est décrite du point de vue de {public} : ce qui change
  dans ce qu'ils voient ou font. Aucun terme technique (code d'erreur, nom
  de technologie, de table, d'API, de migration).
- Chaque chiffre (nombre de cas, de clients, de jours) vient du ticket.
  N'en ajoute aucun.
- Si le ticket ne dit pas ce que l'utilisateur doit faire, n'invente pas
  de consigne : écris « aucune action de votre part ».

Format :
1. Titre : « Nouveautés du {date_mep} ».
2. Pour chaque ticket livré : un titre court, deux phrases maximum, puis
   « Ce que vous devez faire : … ». Référence du ticket entre parenthèses.
3. {longueur_max} mots maximum au total.
4. Pour moi : tableau des tickets exclus (clé, statut, raison de l'exclusion).
```

## Exemple rempli sur NOVA

`{public}` = le service client ; `{date_mep}` = 17 septembre 2026 ; `{projet}` = NOVA ; `{sprint_ou_epic}` = l'epic NOVA-1 ; `{statuts_livres}` = « Résolu » ; `{longueur_max}` = 150.

La sortie doit contenir **une seule nouveauté**, NOVA-5 : les clients dont le compte date d'avant 2021 peuvent de nouveau commander en ligne. Elle mentionne les 23 cas remontés en 4 jours et la remise en ordre des 412 fiches. Les 13 autres tickets apparaissent dans le tableau des exclus (NOVA-1 « En cours », les 12 autres « À faire »).

Elle ne doit pas contenir : « erreur 500 », « catégorie nulle en base », « migration », « NullPointerException », ni aucune des fonctionnalités de remise NOVA-6 à NOVA-14.

## Pièges connus

- **Sans filtre de statut, tout est annoncé comme livré.** C'est le piège trouvé à la relecture (étape 6) ; la contrainte 1 y répond.
- **Le jargon du ticket fuit.** NOVA-5 contient une trace d'erreur et du code : Claude en garde des morceaux (« erreur serveur », « champ catégorie »). Relisez avec le public en tête.
- **Il invente une consigne utilisateur**, comme « demandez au client de vider son panier ». Le ticket ne dit rien de tel.
- **Il embellit.** « Le problème est définitivement résolu » : le ticket dit qu'un correctif est livré et que 412 fiches sont reprises, pas que tout risque est écarté.

## Comment l'améliorer collectivement

Envoyez les notes une fois au vrai public et demandez ce qui leur a manqué. Ajoutez une variante « clients finaux », plus courte et sans référence de ticket.
