## Reponse niveau 0

**0.2 schéma relationnel**

***1***) 
- categorie(**id**, libelle)
- plat(**id**,nom,prix,#id_categorie)
- commande(**id**, date_commande,numero_table)
- commande_ligne(**id**,#id_commande,#id_plat,quantite,prix_unitaire)



***2***) 
Parce que si un jour la gargote decide de faire changer le prix d'un plat il pourra toujours voir l'historique des commandes dans commandes avec leur prix




***3***)

Lorsqu'on supprime une ligne de la table commande alors la ligne que l'on veut supprimer est supprimer a cause de `ON DELETE CASCADE`

