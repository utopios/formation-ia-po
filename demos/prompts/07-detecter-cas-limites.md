# 07 — Détecter les cas limites d'une règle métier

## À quoi sert ce prompt

Avant d'écrire les critères d'acceptation ou de valider une story, faire le tour systématique des situations que la règle ne dit pas comment traiter : valeurs vides, bornes, concurrence, données historiques, erreurs d'un système tiers. C'est la question « et si… ? » posée méthodiquement.

## Rôle donné à Claude

Testeur exploratoire chevronné, qui raisonne par catégories de cas limites et qui distingue ce que la règle tranche déjà de ce qu'elle laisse ouvert.

## Contexte à fournir

- La règle ou la story (texte complet).
- Le contexte d'exécution : qui déclenche, sur quel canal, avec quelles données.
- Facultatif : la liste des cas limites déjà recensés dans la spec, pour que Claude ne les répète pas et se concentre sur les nouveaux.

## Le prompt

```text
Tu es un testeur exploratoire. Tu cherches les cas limites d'une règle métier : les situations où quelqu'un pourrait légitimement se demander « et là, que doit faire l'application ? ».

Règle ou story analysée : {clé_ou_titre}, texte complet entre balises <texte>. Cas limites déjà recensés (à ne pas répéter) entre balises <deja_connus>.

Parcours systématiquement ces familles et, pour chacune, propose les cas pertinents ici (zéro cas est une réponse acceptable si la famille ne s'applique pas) :
- Valeurs vides, nulles, absentes (champ non renseigné, liste vide, client sans attribut)
- Bornes et frontières (juste en dessous, sur la borne, juste au-dessus ; minimum et maximum)
- Temps (changement de mois, expiration, fuseau, données créées avant la règle)
- Concurrence et répétition (deux actions simultanées, action rejouée, double clic)
- Droits et acteurs (qui n'a pas le droit mais essaie, délégation, remplaçant)
- Données incohérentes ou historiques (données d'avant la migration, doublons)
- Systèmes tiers (indisponibles, lents, répondant une erreur)
- Annulation et modification après coup

Pour chaque cas, un tableau : N°, Famille, Cas (une phrase concrète avec des valeurs), Statut (TRANCHÉ si le texte dit quoi faire, avec citation ; OUVERT sinon), Comportement attendu (si TRANCHÉ : citation ; si OUVERT : « à décider », suivi de deux options possibles sans en recommander une), Gravité si mal géré (bloquant / gênant / cosmétique).

Termine par : les cinq cas OUVERTS à trancher en priorité, et pourquoi ces cinq.

Contraintes : ne décide rien à la place du PO. Un cas OUVERT reste ouvert même si « la réponse semble évidente ». Pas de cas techniques internes (base de données, mémoire) sauf s'ils ont un effet visible pour l'utilisateur.

<texte>
{texte_de_la_regle_ou_story}
</texte>

<deja_connus>
{liste_ou_vide}
</deja_connus>
```

## Exemple rempli sur NOVA

`<texte>` = NOVA-3 (règles métier et Gherkin) ; `<deja_connus>` = la section 8 « Cas limites recensés » de `exports/confluence/01-specification-fonctionnelle-tunnel-commande.md`.

Cas à retrouver : le client qui change de catégorie en cours de mois (la spec 01 le tranche : jamais en cours de mois), la remise contractuelle exactement égale au barème (laquelle s'applique, et est-ce visible ?), le panier dont le prix figé depuis 72 heures est recalculé et change de palier, le Grand compte alors que NOVA-3 ne parle que de Standard et Privilège.

## Pièges connus

- Claude marque TRANCHÉ des cas qu'il tranche lui-même par bon sens. Chaque TRANCHÉ doit avoir une citation ; sans citation, c'est OUVERT.
- Il propose des cas techniques (« timeout de la base ») même quand la contrainte les exclut. Ignorez.
- La famille « Temps » est souvent traitée superficiellement. Relancez avec le contexte : « la recatégorisation a lieu le 1er du mois, les prix sont figés 72 heures ».
- Il recommande une option malgré la consigne (« l'option A semble préférable »). C'est votre décision, pas la sienne.
- Trop de cas tue la liste : si vous obtenez 40 lignes, demandez les 15 les plus probables en production.

## Comment l'améliorer collectivement

Après chaque incident de production, vérifiez si le cas était dans une liste produite par ce prompt ; s'il n'y était pas, ajoutez sa famille ou son exemple au prompt. La liste des familles doit refléter vos incidents, pas une théorie.
