# 09 — Synthétiser une spécification pour le COMEX

## À quoi sert ce prompt

Transformer un document de spécification ou un epic en une note d'une page pour un comité de direction : enjeu, ce qui change pour le client et pour l'entreprise, décisions attendues du comité, risques, calendrier tel qu'il est connu. Sans jargon, sans détail d'implémentation, sans chiffre qui ne soit pas dans la source.

## Rôle donné à Claude

Directeur de projet qui rédige pour des décideurs pressés : une page, des phrases courtes, chaque chiffre sourcé, et la question posée au comité clairement isolée.

## Contexte à fournir

- Le ou les documents sources (epic, pages de spec).
- Ce que vous attendez du comité : information, arbitrage, validation d'un budget, choix entre options.
- Les contraintes de forme de votre entreprise (longueur, rubriques imposées).

## Le prompt

```text
Tu es directeur de projet. Tu rédiges une note d'une page pour le comité de direction à partir des documents entre balises <sources>.

Objet de la note : {objet}. Ce que le comité doit faire de cette note : {information / arbitrage / validation}, sur la question suivante : {question_posée_au_comité}.

Structure imposée (titres exacts, longueur totale 350 à 450 mots) :

**En une phrase** — ce dont il s'agit et pourquoi maintenant.
**Le problème aujourd'hui** — trois puces maximum, chacune avec un fait chiffré tiré des sources et sa référence entre parenthèses (nom du document, section).
**Ce qui change** — pour le client, pour les équipes internes, pour la direction. Une puce chacun.
**Ce que nous demandons au comité** — la décision attendue, formulée pour qu'on puisse répondre oui, non, ou choisir une option. Si plusieurs options existent dans les sources, un tableau option / avantage / inconvénient / coût ou délai si connu.
**Risques et ce que nous faisons** — trois lignes maximum.
**Calendrier** — uniquement les jalons présents dans les sources ; si aucun, écris « calendrier non arrêté dans les documents disponibles ».

Contraintes strictes : aucun terme technique (pas de nom de technologie, d'API, de base de données). Aucun chiffre qui ne soit pas dans les sources ; s'il en manque un qui serait utile, signale-le dans une ligne finale « Données à obtenir avant le comité ». Pas de superlatif, pas de « stratégique », pas de « innovant ». Ton neutre, phrases de moins de 25 mots.

<sources>
{textes_sources}
</sources>
```

## Exemple rempli sur NOVA

`<sources>` = la description de l'epic NOVA-1 et les sections 1 à 4 de `exports/confluence/02-regles-de-remise.md` ; `{objet}` = refonte du tunnel de commande, volet remises ; `{question_posée_au_comité}` = valider que le plafond de 15 % et la validation nominative par le directeur commercial s'appliquent sans exception aux grands comptes.

Chiffres attendus, tous sourcés dans l'epic : 340 clients professionnels, taux d'abandon de 31 %, 18 % des commandes reprises à la main, interruption maximale de 15 minutes. Tout autre chiffre est une invention.

## Pièges connus

- Claude ajoute volontiers un gain attendu (« réduction de 30 % des appels ») qui n'existe dans aucune source. La contrainte l'interdit ; vérifiez chaque chiffre contre la référence entre parenthèses.
- Le jargon revient par la bande : « API Sage », « migration ». Remplacez par « le logiciel de facturation », « la reprise des données ».
- La question au comité est souvent diluée dans un paragraphe. Elle doit être isolée et fermée.
- La note dépasse fréquemment 450 mots. Demandez « coupe de 20 % sans retirer de chiffre ».
- Le ton dérive vers la promotion du projet. Un COMEX se méfie des notes qui vendent ; restez factuel.

## Comment l'améliorer collectivement

Après un vrai comité, notez les questions que les dirigeants ont posées et que la note n'anticipait pas ; ajoutez-les au prompt comme « le comité demande systématiquement : … ».
