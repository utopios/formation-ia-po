# 18 — Comparer deux versions d'une spécification

## À quoi sert ce prompt

Lister précisément ce qui a changé entre deux versions d'une page de spécification, puis les tickets qui en sont affectés. Il sert à chaque nouvelle version d'une page de règles, avant l'affinage.

## Rôle donné à Claude

Analyste fonctionnel qui fait un « diff » métier : il relève chaque écart, mot pour mot, sans l'interpréter ni le juger.

## Contexte à fournir

- Les deux versions complètes de la page, chacune dans son fichier ou entre balises.
- Le backlog concerné, si vous voulez l'analyse d'impact.

## Le prompt

```text
Tu es analyste fonctionnel. Compare deux versions de la page
« {titre_page} » : la version {version_avant} entre balises <avant> et la
version {version_apres} entre balises <apres>.

Contraintes :
- Relève chaque différence de fond : chiffre, seuil, délai, acteur,
  condition, exception, règle ajoutée ou supprimée. Ignore la mise en
  forme et les reformulations sans changement de sens, mais compte-les.
- Pour chaque différence, cite le texte exact des deux versions avec la
  section. Si une règle n'existe que dans une version, écris « ABSENT »
  pour l'autre.
- N'explique pas pourquoi la règle a changé, et ne dis pas si c'est mieux.
- Si tu n'es pas sûr qu'une différence change le sens, classe-la
  « À CONFIRMER » au lieu de trancher.

Format :
1. Tableau : section ; texte avant ; texte après ; nature (modifié,
   ajouté, supprimé, à confirmer).
2. Nombre de reformulations sans changement de sens ignorées.
3. Si un backlog est fourni : tickets dont le texte s'appuie sur une
   règle modifiée, avec la phrase du ticket concernée.

<avant>
{texte_version_avant}
</avant>

<apres>
{texte_version_apres}
</apres>
```

## Exemple rempli sur NOVA (test par différences plantées)

NOVA n'a qu'une version de la page « Règles de remise ». Pour tester, on fabrique une fausse « version 2.3 » en copiant la v2.4 et en y **plantant quatre différences connues** :

1. §3 : plafond à 12 % au lieu de 15 % ;
2. §4 : délai du validateur à 72 heures au lieu de 48 ;
3. §6 : suppression de la phrase sur le SIREN ;
4. §1 : « sans exception » remplacé par « sauf décision du directeur commercial ».

On ajoute deux reformulations sans changement de sens (par exemple « ne dépasse en aucun cas » devient « ne peut jamais dépasser »).

`<avant>` = la fausse v2.3 ; `<apres>` = la v2.4 ; backlog = `exports/backlog-nova.md`.

Attendu : **exactement quatre différences de fond**, citées des deux côtés, et deux reformulations comptées. Côté impact : NOVA-2 (« sauf cas particulier ») pour la différence 4, NOVA-6 et NOVA-9 pour les différences 1 et 2, NOVA-8 pour la différence 3.

L'intérêt de cette méthode : on connaît la bonne réponse, on peut donc compter ce que Claude rate et ce qu'il invente.

## Pièges connus

- **Il rate les suppressions.** Une phrase absente est plus difficile à voir qu'une phrase modifiée. La différence 3 est la plus souvent manquée.
- **Il signale des reformulations comme des changements de règle**, ou l'inverse : il ne voit pas qu'un mot (« sans exception ») change tout.
- **Il interprète** : « le plafond a été relevé pour plus de souplesse commerciale ». Aucune page ne donne de raison.
- **Sur une page longue, il s'arrête en route.** Demandez « continue à partir de la section N » et vérifiez que toutes les sections sont couvertes.

## Comment l'améliorer collectivement

Gardez la fausse v2.3 de NOVA comme jeu de test. Chaque fois que le prompt est modifié, rejouez-le dessus et vérifiez qu'il trouve toujours les quatre différences.
