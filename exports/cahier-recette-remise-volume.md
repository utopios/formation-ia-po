Cahier de recette — Remise par volume sur le panier B2B (NOVA-3)
Version 0.3 — rédigé par l'équipe de test — à valider par le PO

Objectif : vérifier l'application automatique du barème de remise par volume
pour les clients Standard et Privilège.

Pré-requis : un compte client de chaque catégorie, un catalogue avec la
référence NT-4471 à 100,00 EUR HT l'unité.

CR-01  Panier de 9 unités (900,00 EUR HT), client Standard
       Attendu : remise 0,00 EUR, total HT 900,00 EUR.

CR-02  Panier de 10 unités (1 000,00 EUR HT), client Standard
       Attendu : remise 3 %, total HT 970,00 EUR.

CR-03  Panier de 50 unités (5 000,00 EUR HT), client Standard
       Attendu : remise 6 %, total HT 4 700,00 EUR.

CR-04  Panier de 50 unités (5 000,00 EUR HT), client Privilège
       Attendu : remise 8 %, total HT 4 600,00 EUR.

CR-05  Panier de 250 unités (25 000,00 EUR HT), client Privilège
       Attendu : remise 13 %, total HT 21 750,00 EUR.

CR-06  Panier de 200 unités (20 000,00 EUR HT), client Standard
       Attendu : remise 9 %, total HT 18 200,00 EUR.

CR-07  Client Standard, panier de 3 000,00 EUR HT, frais de port 12,50 EUR
       Attendu : remise 3 % appliquée sur 3 012,50 EUR, soit 90,38 EUR.

CR-08  Panier de 5 000,00 EUR HT, client Privilège disposant d'une remise
       contractuelle de 10 %
       Attendu : remise 8 % par volume plus 10 % contractuelle.

CR-09  Le montant de la remise apparaît dans le panier avant validation
       Attendu : une ligne « Remise » visible sous le sous-total.

CR-10  La remise appliquée est journalisée
       Attendu : une entrée de journal avec le taux et l'horodatage.

CR-11  Modification de la quantité après calcul de la remise
       Attendu : la remise est recalculée.

CR-12  Performance : le recalcul du panier répond en moins de 2 secondes.
