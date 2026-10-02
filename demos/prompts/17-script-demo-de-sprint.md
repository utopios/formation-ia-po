# 17 — Préparer le script d'une démo de sprint

## À quoi sert ce prompt

Préparer la revue de sprint : un script de démonstration pas à pas, centré sur ce que les parties prenantes verront, avec les données à préparer et les questions à leur poser. Il évite les démos qui montrent des écrans au lieu d'une valeur.

## Rôle donné à Claude

Product Owner qui prépare la revue de sprint : il raconte un usage réel, il montre ce qui est livré et rien d'autre, et il prépare les questions qui feront avancer le backlog.

## Contexte à fournir

- Les tickets livrés dans le sprint, avec leurs critères d'acceptation.
- L'epic et ses objectifs, pour relier la démo à la valeur.
- Le public de la revue et la durée disponible.

## Le prompt

```text
Tu es Product Owner. Tu prépares le script de la revue de sprint pour
{public}, en {duree} minutes.

Tickets livrés : {tickets_livres}. Objectifs de l'epic : {epic}.

Contraintes :
- Ne montre que ce que les tickets livrés permettent de faire. Si une
  étape du parcours dépend d'un ticket non livré, signale-la comme
  « pas encore disponible » au lieu de la scénariser.
- Chaque étape est un geste d'utilisateur (« le commercial ajoute… »),
  jamais une action technique (appel d'API, requête, console).
- Les données de démo (comptes, références, montants) viennent des
  tickets ; s'il en manque, liste-les comme « à préparer », n'en invente
  pas.
- Relie chaque séquence à un objectif chiffré de l'epic, en citant le
  chiffre tel qu'il est écrit.

Format :
1. Le fil conducteur en une phrase : qui, quel besoin.
2. Un tableau : étape ; ce que fait l'utilisateur ; ce que le public doit
   remarquer ; ticket ; objectif de l'epic concerné.
3. Les données à préparer avant la revue.
4. Trois questions à poser au public, qui alimentent le backlog.
5. Le plan B si une étape échoue en direct.
```

## Exemple rempli sur NOVA

`{public}` = la responsable du service client et le directeur commercial ; `{duree}` = 10 ; `{tickets_livres}` = NOVA-5 ; `{epic}` = NOVA-1.

La sortie doit raconter un client ancien (compte d'avant 2021) qui commande de nouveau en ligne sans passer par le téléphone. Elle reprend les données du ticket : le compte de test `client.legacy@novatech.example` et la référence NT-4471 en quantité 30. Elle relie la séquence à l'objectif « 18 % des commandes reprises à la main » de NOVA-1. Elle signale l'affichage du détail de la remise (NOVA-10) comme « pas encore disponible ».

Elle ne doit pas contenir : « appeler POST /api/v1/carts/8842/recalculate », ni une démonstration du barème Grand compte (non livré).

## Pièges connus

- **Il recopie la procédure de reproduction du bug.** Les étapes de NOVA-5 incluent un appel d'API ; Claude le garde comme étape de démo. La contrainte 2 l'interdit, mais vérifiez.
- **Il scénarise ce qui n'est pas livré.** Pour raconter une belle histoire, il montre la remise affichée dans le panier (NOVA-10, « À faire »).
- **Il invente des chiffres de gain.** « Ce correctif réduit les appels de 30 % » : aucun ticket ne le dit.
- **Les questions au public sont génériques** (« qu'en pensez-vous ? »). Exigez des questions fermées qui débouchent sur une décision de backlog.

## Comment l'améliorer collectivement

Après chaque revue, notez la question du public à laquelle le script n'avait pas préparé de réponse. Au bout de trois revues, ajoutez-la comme rubrique du format.
