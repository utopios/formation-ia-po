# 13 — Analyser un backlog (complétude, incohérences, doublons)

## À quoi sert ce prompt

Prendre de la hauteur sur un lot de tickets avant un PI planning, une revue de backlog ou une reprise de projet : repérer ce qui manque, ce qui se contredit, ce qui fait doublon et ce qui n'a pas de source dans la spécification.

## Rôle donné à Claude

Product Owner senior en mission d'audit de backlog, qui produit un constat structuré et sourcé, pas un plan d'action.

## Contexte à fournir

- Le backlog : soit via le connecteur (« recherche JQL project = NOVA »), soit l'export `exports/backlog-nova.md` déposé dans le Projet ou glissé dans le chat.
- Optionnel mais très recommandé : la ou les pages de spécification de référence, pour la partie « fondement dans la spec ».
- Le périmètre de l'analyse : l'epic concerné, la date de la revue.

## Le prompt

```text
Tu es un Product Owner senior chargé d'auditer le backlog de l'epic {clé_epic} « {titre_epic} » avant {occasion : PI planning, revue de backlog, reprise}.

Tu disposes de :
- la liste des tickets de l'epic (ci-dessous entre balises <backlog>, ou via le connecteur Jira : recherche « parent = {clé_epic} »),
- la ou les pages de spécification de référence (ci-dessous entre balises <spec>, ou dans les connaissances du Projet).

Analyse à produire, dans cet ordre :

1. INVENTAIRE : tableau clé, type, résumé, une ligne « ce que le ticket demande » en 15 mots maximum. Aucun ticket omis.

2. COMPLÉTUDE : pour chaque story, indique si elle contient (Oui/Non/Partiel) : un acteur identifié, un besoin formulé, une valeur métier, des règles chiffrées, des critères d'acceptation, un périmètre exclu. Score sur 6.

3. INCOHÉRENCES AVEC LA SPÉCIFICATION : pour chaque écart, cite textuellement le ticket ET la section de la spec concernée. Classe en CONTRADICTION / IMPRÉCISION / HORS SPEC. Sans double citation, ne signale pas.

4. DOUBLONS ET RECOUVREMENTS : tickets qui traitent le même besoin ou qui se marchent dessus, avec la phrase de chacun qui le montre.

5. DÉPENDANCES IMPLICITES : tickets qui ne peuvent pas être livrés sans un autre, et pourquoi.

6. CE QUE JE NE PEUX PAS ÉVALUER : ce qui nécessite une information absente des documents (priorité métier, capacité de l'équipe, etc.). Ne le devine pas.

Contraintes : aucune règle, aucun chiffre, aucun acteur inventé. Si un point est ambigu, dis-le au lieu de trancher. Réponse en français.

<backlog>
{contenu_du_backlog}
</backlog>

<spec>
{contenu_des_pages_de_spec}
</spec>
```

## Exemple rempli sur NOVA

`{clé_epic}` = NOVA-1 ; `{titre_epic}` = Refonte du tunnel de commande ; `{occasion}` = revue de backlog du sprint 12 ; `<backlog>` = `exports/backlog-nova.md` ; `<spec>` = `exports/confluence/02-regles-de-remise.md` (ajoutez la page 01 si vous voulez la partie panier et validation).

Avec le connecteur : « Recherche tous les tickets du projet NOVA (JQL : project = NOVA ORDER BY key), lis la page « Règles de remise », puis applique l'analyse ci-dessus. »

## Pièges connus

- Sur 14 tickets, Claude peut « oublier » les derniers de l'inventaire si le contexte est long. Comptez les lignes du tableau : il doit y en avoir autant que de tickets.
- La partie 3 est la plus utile et la plus fragile : Claude trouve facilement les contradictions chiffrées, beaucoup moins les nuances de gouvernance (qui décide, qui valide). Relancez avec « relis les sections 3, 4 et 5 de la spec et cherche ce que chaque ticket dit sur qui valide ».
- Claude propose spontanément des priorités ou une roadmap. Ce n'est pas demandé et ce n'est pas fondé : ignorez ou coupez cette partie.
- Le score de complétude est une échelle de votre cru : deux relances donneront deux scores différents pour la même story. Utilisez-le pour trier, pas pour juger.

## Comment l'améliorer collectivement

Gardez la sortie du premier audit comme référence dans le Projet ; à la revue suivante, demandez à Claude « qu'est-ce qui a changé depuis l'audit du {date} ? » plutôt que de repartir de zéro, et consignez ce qui a été mal détecté.
