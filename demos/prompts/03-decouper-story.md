# 03 — Découper une story trop grosse

## À quoi sert ce prompt

Proposer un découpage d'une story ou d'un epic en stories livrables indépendamment, chacune apportant de la valeur, avec l'ordre de livraison suggéré et ce que chaque tranche laisse de côté. Le découpage est une proposition à discuter en équipe, pas une décision.

## Rôle donné à Claude

Coach agile pragmatique qui connaît les techniques de découpage (par règle métier, par acteur, par parcours, par cas nominal puis cas d'erreur, par donnée, par « spike » de levée d'incertitude) et qui explique laquelle il applique et pourquoi.

## Contexte à fournir

- La story ou l'epic à découper (texte complet).
- La taille visée (par exemple « livrable en moins d'un sprint de deux semaines par une équipe de quatre »).
- Les contraintes connues : ce qui doit sortir en premier, ce qui dépend d'une autre équipe.
- La spec si elle existe : elle révèle souvent les règles qui font des tranches naturelles.

## Le prompt

```text
Tu es un coach agile qui aide un Product Owner à découper une story trop grosse en stories livrables séparément.

Story à découper : {clé_ticket} « {résumé_ticket} », texte complet entre balises <story>. Spécification de référence entre balises <spec> (peut être vide).

Objectif de taille : chaque story résultante doit être {taille_visée}.
Contraintes connues : {contraintes}.

Procède ainsi :

1. Identifie les axes de découpage possibles pour cette story (par règle métier, par acteur, par parcours, par cas nominal puis cas d'erreur, par donnée ou par levée d'incertitude). Pour chacun, dis en une ligne s'il est pertinent ici et pourquoi.
2. Choisis un axe principal et justifie-le en trois lignes maximum.
3. Propose le découpage : un tableau avec les colonnes N°, Titre de la story (forme verbale, 10 mots maximum), « En tant que / je veux / afin de » en une ligne, Valeur livrée seule (ce qu'un utilisateur peut faire de plus quand cette story seule est en production), Dépend de (N° ou « aucune »), Ce que cette story ne fait PAS.
4. Propose un ordre de livraison et explique le premier choix : pourquoi celle-là d'abord.
5. Liste ce que le texte d'origine évoquait et qu'aucune story ne couvre, avec la raison (hors périmètre, à clarifier, spike).
6. Signale toute tranche qui n'apporte aucune valeur seule (tranche technique déguisée) et propose comment la fusionner.

Contraintes : entre trois et sept stories. N'invente aucune règle métier absente des textes ; si une tranche a besoin d'une règle non décidée, écris « nécessite de trancher : … » dans sa ligne. Pas de solution technique.

<story>
{texte_complet}
</story>

<spec>
{texte_de_la_spec_ou_vide}
</spec>
```

## Exemple rempli sur NOVA

`{clé_ticket}` = NOVA-2 ; `{taille_visée}` = livrable en un sprint de deux semaines par une équipe de quatre ; `{contraintes}` = l'export comptable (NOVA-4) est en cours dans un autre sprint et ne doit pas être touché ; `<spec>` = `exports/confluence/02-regles-de-remise.md`.

Un découpage plausible sépare : le calcul automatique du barème par volume ; l'affichage de la remise dans le panier avant validation ; l'application du taux contractuel avec la règle « la plus avantageuse » ; le circuit de validation au-delà du seuil de délégation. Votre découpage peut être différent ; ce qui compte est que chaque tranche livre quelque chose d'observable.

Variante : sur l'epic NOVA-1, le même prompt donne une carte des stories de l'epic, à comparer avec les stories NOVA-2 à NOVA-14 réellement présentes dans le backlog (exercice de l'atelier du jour 2).

## Pièges connus

- Claude adore la tranche « mettre en place la structure de données » ou « créer l'écran vide ». Ce sont des tâches techniques, pas des stories ; le point 6 les traque, mais vérifiez la colonne « Valeur livrée seule » : si elle est vide ou technique, la tranche n'est pas une story.
- Il propose fréquemment sept stories parce que la borne haute est sept. Demandez « peux-tu en faire quatre sans perdre de valeur ? » et comparez.
- L'ordre de livraison proposé privilégie souvent la logique technique (d'abord le calcul, ensuite l'affichage). Le bon ordre est celui de la valeur et du risque : discutez-le.
- Sur un texte flou, il crée des tranches sur des règles inventées. La contrainte « nécessite de trancher » doit apparaître ; si le tableau n'en contient aucune pour NOVA-2, c'est suspect.

## Comment l'améliorer collectivement

Après trois découpages réels, gardez celui qui a le mieux tenu en sprint comme « exemple de référence » dans le Projet et demandez à Claude de s'en inspirer pour le style des titres et la granularité.
