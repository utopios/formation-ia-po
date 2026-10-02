# 19 — Préparer l'ordre du jour d'une rétrospective

## À quoi sert ce prompt

Préparer une rétrospective à partir des faits du sprint : un ordre du jour minuté, les faits à présenter, les questions d'ouverture. Il évite la rétrospective générique où l'on parle de tout et où l'on ne décide rien.

## Rôle donné à Claude

Scrum Master qui prépare la rétrospective : il part des faits, il ne cherche pas de coupable et il ne tire pas les conclusions à la place de l'équipe.

## Contexte à fournir

- Les faits du sprint : tickets prévus et livrés, incidents, bugs remontés, chiffres disponibles.
- Les actions décidées à la rétrospective précédente, et leur suivi.
- La durée et le nombre de participants.

## Le prompt

```text
Tu es Scrum Master. Tu prépares la rétrospective du sprint {sprint} pour
une équipe de {nombre} personnes, en {duree} minutes.

Faits du sprint entre balises <faits>. Actions de la rétrospective
précédente entre balises <actions_precedentes>.

Contraintes :
- Pars uniquement des faits fournis. N'invente aucune cause, aucun
  ressenti d'équipe, aucun chiffre.
- Formule les faits sans désigner de personne ni de responsable.
- Propose des questions d'ouverture, pas des réponses : la conclusion
  appartient à l'équipe.
- Choisis un seul format d'animation, adapté aux faits, et justifie ce
  choix en une phrase.

Format :
1. Les faits marquants, en cinq lignes maximum, chacun avec sa source.
2. Le suivi des actions précédentes : action, faite ou non, d'après les
   faits ; « inconnu » si les faits ne le disent pas.
3. L'ordre du jour minuté, total = {duree} minutes.
4. Trois questions d'ouverture.
5. Ce que la rétrospective doit produire : au plus deux actions, avec un
   responsable et une échéance à définir en séance.

<faits>
{faits_du_sprint}
</faits>

<actions_precedentes>
{actions_precedentes}
</actions_precedentes>
```

## Exemple rempli sur NOVA

`{sprint}` = le sprint du 12 septembre ; `{nombre}` = 6 ; `{duree}` = 60. `<faits>` = le ticket NOVA-5 (mise en production du 12 septembre, erreur au recalcul du panier pour les comptes d'avant 2021, 23 cas en 4 jours, 23 commandes reprises par téléphone, correctif et reprise de 412 fiches). Ajoutez la page d'architecture, section 8 « Dette technique connue », qui annonçait le problème. `<actions_precedentes>` = « aucune fournie ».

La sortie doit faire ressortir un fait central : **le défaut était connu et documenté avant la mise en production** (page d'architecture §8). Les questions d'ouverture doivent porter sur ce point : comment une dette connue est-elle passée en production ? Le suivi des actions précédentes doit dire « inconnu ».

Elle ne doit pas contenir : un responsable désigné (« l'équipe back aurait dû… »), une cause inventée (« manque de tests », alors que les faits ne le disent pas), des ressentis (« l'équipe était stressée »).

## Pièges connus

- **Il désigne un coupable**, même implicitement : « le développeur n'a pas géré le cas null ». La contrainte 2 doit être vérifiée phrase par phrase.
- **Il conclut à la place de l'équipe.** Il propose directement « ajouter des tests sur les données anciennes » comme action. C'est peut-être juste, mais c'est à l'équipe de le trouver.
- **Il invente le suivi des actions précédentes** quand on ne lui en donne pas : « l'action sur la revue de code a été partiellement réalisée ».
- **Il propose un format à la mode** (speed boat, starfish) sans lien avec les faits. Exigez la justification.

## Comment l'améliorer collectivement

Après chaque rétrospective, notez si les questions d'ouverture ont lancé la discussion ou non. Ajoutez au fichier les deux questions qui ont le mieux marché, comme exemples.
