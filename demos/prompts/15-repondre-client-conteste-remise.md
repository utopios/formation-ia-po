# 15 — Répondre à un client qui conteste sa remise

## À quoi sert ce prompt

Préparer, à partir des règles de remise en vigueur, la réponse à un client professionnel qui conteste le taux appliqué à sa commande ou qui demande davantage. Le prompt sépare l'analyse sourcée, destinée au PO, du projet de réponse, destiné au client. Il ne remplace pas la vérification de la commande dans l'outil.

## Rôle donné à Claude

Chargé de relation client qui prépare une réponse pour validation par le PO : courtois avec le client, rigoureux sur les règles, et qui ne s'engage jamais à la place de l'entreprise.

## Contexte à fournir

- Le message du client, mot pour mot.
- Sa catégorie (Standard, Privilège, Grand compte) ou « inconnue ».
- Le montant HT des articles de la commande, hors frais de port.
- Le taux constaté (dans l'outil, ou « selon le client, non vérifié »).
- Le canal (mail, téléphone, message).
- Dans le Projet : la page « Règles de remise » et la « Spécification fonctionnelle ».

## Le prompt

```text
Tu es chargé de relation client chez {entreprise}. Tu prépares, pour
validation par le Product Owner, la réponse à un client professionnel qui
conteste sa remise.

Message du client entre balises <client>. Catégorie du client :
{categorie_client}. Montant HT des articles : {montant_ht_articles}. Taux
constaté : {taux_constate}. Canal : {canal}.

Contraintes :
- Utilise uniquement les pages « Règles de remise » et « Spécification
  fonctionnelle » du Projet. N'invente aucun taux, seuil, délai ni acteur.
- Si une affirmation du client contredit les règles, ne la confirme pas :
  signale l'incohérence et dis ce qu'il faut vérifier.
- Si la catégorie est inconnue, donne le taux attendu pour chaque catégorie
  possible et demande-la-moi ; ne la suppose pas.
- Ne commente jamais les conditions accordées à un autre client.
- Ne promets aucun geste commercial ni aucun taux non prévu par le barème :
  si le client en demande un, indique la règle applicable et qui doit
  valider, et formule au client qu'une demande est étudiée.

Format :
1. Analyse pour moi, en tableau : fait ; règle applicable ; page et section ;
   conclusion. Montre le calcul de chaque montant.
2. Ce que je dois vérifier avant d'envoyer, en liste.
3. Projet de réponse au client, {longueur_max} mots maximum, signé
   {signataire}, sans vocabulaire interne (pas de « palier », « seuil de
   délégation », « catégorie », nom d'outil ou de ticket) et sans citation
   de section.

<client>
{message_client}
</client>
```

## Exemple rempli sur NOVA

`{entreprise}` = NovaTech Industries ; `<client>` = « Je n'ai pas eu de remise sur ma commande de 4 800 EUR HT, alors qu'un collègue d'une autre société a eu 6 % sur 5 200 EUR. Pourquoi ? » ; `{categorie_client}` = inconnue ; `{montant_ht_articles}` = 4 800 EUR ; `{taux_constate}` = 0 % selon le client, non vérifié ; `{canal}` = mail ; `{longueur_max}` = 120 ; `{signataire}` = le service commercial.

Ce que la sortie doit contenir :

- **L'analyse.** 3 % pour un Standard, 5 % pour un Privilège, 7 % pour un Grand compte (Règles de remise §2), avec le calcul : 144,00 €, 240,00 € et 336,00 € de remise. Elle dit que « pas de remise » est incohérent avec §1, §2 et §5, et rappelle qu'un client sans catégorie est traité comme Standard (Spec §3).
- **Les vérifications.** Le taux enregistré dans le journal des remises (Spec §9), le montant HT hors frais de port, la catégorie du client.
- **La réponse au client.** Elle explique la remise de 3 % (ou le taux de sa catégorie), annonce une vérification et rappelle que 6 % s'applique à partir de 5 000 € HT. Elle ne dit rien de l'autre société.

Ce qu'elle ne doit pas contenir : « aucune remise n'était applicable », une catégorie supposée, une promesse de geste commercial.

## Pièges connus

- **Il confirme la prémisse du client.** Sans la contrainte « ne la confirme pas », Claude explique pourquoi le client n'a rien eu au lieu de relever que ce n'est pas possible. C'est l'erreur la plus fréquente.
- **Il suppose la catégorie.** « Votre compte étant Standard… » alors qu'on a écrit « inconnue ». Vérifiez la première phrase de l'analyse.
- **Il promet.** Face à une demande d'alignement sur un concurrent, la première version écrivait « nous pouvons vous proposer 14 % ». Un geste commercial passe par §8 et §4, jamais par un mail.
- **Le jargon fuit dans le mail.** « Palier », « seuil de délégation », « directeur commercial doit valider » : le client n'a pas à connaître notre circuit interne.
- **Il ne voit pas le backlog.** Sans le connecteur ou l'export Jira, il ne relie pas « je ne vois pas ma remise » au ticket d'affichage (NOVA-10). Ajoutez « cherche dans le backlog un ticket lié » si vous l'avez.
- **Le calcul.** Recalculez au moins un montant, surtout près d'une borne (4 999,99 € → 3 % ; 5 000,00 € → 6 %).

## Comment l'améliorer collectivement

Après chaque usage réel, notez dans les pièges la réclamation que le prompt a mal traitée. Au bout de cinq réclamations différentes (contestation de taux, demande d'alignement, remise de bienvenue refusée, code campagne expiré, cumul demandé), le prompt couvre l'essentiel des cas. Une variante « à dire au téléphone en 30 secondes » serait utile au service client.
