# 08 — Relire un cahier de recette

## À quoi sert ce prompt

Faire relire un cahier de recette (ou un plan de test) avant la campagne : cas incomplets, résultats attendus non vérifiables, préconditions impossibles, doublons, écarts avec la story. Le cahier n'est pas réécrit : il est annoté, avec la ligne concernée et la correction proposée.

## Rôle donné à Claude

Relecteur de recette expérimenté qui se met dans la peau de la personne qui exécutera les tests un vendredi à 17 h : tout ce qui demande d'interpréter est une anomalie.

## Contexte à fournir

- Le cahier de recette (texte, tableau ou export).
- La story ou les critères d'acceptation qu'il est censé couvrir.
- Le profil des exécutants (testeurs de métier, utilisateurs clés, prestataire) : le niveau de détail attendu en dépend.

## Le prompt

```text
Tu es relecteur d'un cahier de recette. Ta lecture est celle de la personne qui exécutera les tests sans connaître le projet : tout ce qui l'obligerait à deviner est une anomalie à corriger.

Cahier de recette entre balises <cahier>. Story ou critères couverts entre balises <reference>. Exécutants : {profil_des_executants}.

Relis chaque cas de test et signale, dans un tableau (Cas, Anomalie, Gravité, Correction proposée) :
- RÉSULTAT NON VÉRIFIABLE : le résultat attendu n'a pas de valeur observable (« le montant est correct », « la page s'affiche bien »). Propose la valeur ou le message exact si la référence le donne, sinon « à préciser par le PO ».
- PRÉCONDITION MANQUANTE OU IMPOSSIBLE : le cas suppose un état non décrit ou impossible à obtenir avec les données indiquées.
- ÉTAPE AMBIGUË : une étape peut être exécutée de deux façons.
- ÉCART AVEC LA RÉFÉRENCE : le résultat attendu contredit la story ou les critères (citation des deux).
- DOUBLON : deux cas vérifient la même chose ; dis lequel garder.
- CALCUL FAUX : un montant ou un total attendu est incorrect au regard de la règle ; montre le bon calcul.

Puis :
1. Les critères de la référence qu'aucun cas ne couvre.
2. Les cas du cahier qui ne correspondent à aucun critère (à justifier ou à retirer).
3. Une estimation de la charge d'exécution : nombre de cas, cas les plus longs, et ce qui pourrait être regroupé.
4. Un verdict : le cahier est-il exécutable tel quel par {profil_des_executants} ? Oui / Non, avec les trois corrections prioritaires.

Contraintes : ne réécris pas le cahier, annote-le. Pas de nouveau cas de test sauf au point 1, sous forme d'une ligne par critère non couvert. N'invente pas de résultat attendu : s'il n'est pas déductible de la référence, écris « à préciser ».

<cahier>
{texte_du_cahier_de_recette}
</cahier>

<reference>
{story_ou_criteres}
</reference>
```

## Exemple rempli sur NOVA

`<cahier>` = les cas de test produits par le prompt 04 sur NOVA-3, volontairement dégradés par vos soins (retirez une valeur attendue, glissez une erreur de calcul, dupliquez un cas) ; `<reference>` = NOVA-3 ; `{profil_des_executants}` = deux utilisateurs clés de l'administration des ventes, sans profil technique.

Ce que Claude doit trouver : les dégradations que vous avez introduites. S'il en manque une, vous savez ce que le prompt ne voit pas.

## Pièges connus

- Claude est plus sévère sur la forme (numérotation, style) que sur le fond (calcul faux). Si le tableau d'anomalies ne contient aucun « CALCUL FAUX » alors que vous en avez glissé un, demandez explicitement « recalcule chaque montant attendu ».
- Il propose des résultats attendus « raisonnables » là où la consigne dit « à préciser ». Chaque valeur qu'il propose doit être traçable à la référence.
- Sur un cahier long, la relecture s'affaiblit vers la fin. Découpez en lots de 15 à 20 cas.
- L'estimation de charge (point 3) est une opinion, pas une mesure. Ne la reportez pas telle quelle dans un planning.

## Comment l'améliorer collectivement

Tenez une liste des anomalies réellement rencontrées pendant les campagnes (ce qui a fait perdre du temps aux exécutants) et ajoutez les catégories manquantes au prompt : l'objectif est que le prompt reconnaisse vos défauts habituels, pas les défauts en général.
