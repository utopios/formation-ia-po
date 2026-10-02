# 10 — Reformuler pour une partie prenante non technique

## À quoi sert ce prompt

Traduire un texte technique (commentaire de développeur, ticket de bug, note d'architecture, compte rendu d'incident) en explication compréhensible par une partie prenante métier précise, sans perdre l'information qui compte pour elle et sans la rassurer à tort.

## Rôle donné à Claude

Product Owner qui fait l'interface entre l'équipe technique et le métier : il traduit, il ne déforme pas, et il dit ce qu'il ne sait pas.

## Contexte à fournir

- Le texte technique d'origine.
- Le destinataire précis (fonction, ce qu'il connaît, ce qui l'inquiète) : « la directrice du service client, qui reçoit les plaintes et ne connaît pas le système ».
- Le canal : mail, message instantané, oral en réunion.
- Ce que le destinataire doit faire après lecture (rien, décider, informer ses équipes).

## Le prompt

```text
Tu es Product Owner. Tu traduis un texte technique pour une partie prenante métier, sans perdre ce qui compte pour elle et sans minimiser.

Texte d'origine entre balises <technique>. Destinataire : {destinataire_et_ce_qu_il_connait}. Canal : {canal}. Après lecture, le destinataire doit : {action_attendue}.

Rédige la reformulation en respectant :
- Longueur : {longueur : par exemple 120 mots maximum}.
- Aucun terme technique : pas de nom de technologie, de composant, de code d'erreur, de commande. Si un terme est indispensable, explique-le entre parenthèses en six mots.
- Ordre : 1) ce qui s'est passé ou va se passer, du point de vue des personnes concernées ; 2) qui est touché et combien, si le texte le dit ; 3) ce qui est fait ; 4) ce que le destinataire doit faire ou savoir ; 5) quand il aura des nouvelles.
- Chaque information factuelle vient du texte d'origine. Ce que le texte ne dit pas (cause définitive, date de résolution) s'écrit « nous ne le savons pas encore », jamais une estimation rassurante.

Après la reformulation, ajoute une section « Ce que j'ai retiré et pourquoi » : les éléments techniques omis, et pour chacun si l'omission est sans risque pour le destinataire ou si tu recommandes de garder une mention.

<technique>
{texte_technique}
</technique>
```

## Exemple rempli sur NOVA

`<technique>` = la description du bug NOVA-5 (erreur 500 au recalcul du panier pour les clients sans catégorie), trace d'erreur et diff de code compris ; `{destinataire_et_ce_qu_il_connait}` = la responsable du service client, qui a remonté les 23 cas et ne connaît pas le système ; `{canal}` = mail ; `{action_attendue}` = informer ses équipes de la marche à suivre pour les clients concernés ; `{longueur}` = 150 mots maximum.

La reformulation doit contenir : 23 cas en 4 jours, clients dont le compte date d'avant 2021, impossibilité de commander en ligne, correction livrée et reprise des 412 fiches, et ne doit contenir ni « NullPointerException » ni « catégorie nulle en base » ni « merge request ».

## Pièges connus

- Claude rassure : « le problème est résolu » alors que le texte dit qu'un correctif est livré et qu'une reprise de données est en cours. Ce n'est pas la même chose ; exigez la nuance.
- Il garde des termes qu'il croit grand public (« API », « migration », « backend »). Ils ne le sont pas.
- Il ajoute des formules de politesse et des excuses au nom de l'entreprise : à vous de décider si c'est votre ton.
- La section « Ce que j'ai retiré » est souvent la plus utile : c'est là qu'on repère qu'une information importante (« les clients concernés ont dû commander par téléphone ») a disparu.
- Pour un canal oral, demandez une version « à dire en 30 secondes » : la version écrite ne se lit pas à voix haute.

## Comment l'améliorer collectivement

Faites lire la reformulation au vrai destinataire une fois et demandez-lui ce qu'il n'a pas compris ou ce qui lui a manqué ; consignez sa réponse dans les pièges. Trois retours suffisent à calibrer le prompt pour une population donnée.
