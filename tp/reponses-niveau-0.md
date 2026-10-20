## Reponse niveau 0

**0.2 schéma relationnel**

***1***) 
- categorie(**id**, libelle)
- plat(**id**,nom,prix,#id_categorie)
- commande(**id**, date_commande,numero_table)
- commande_ligne(**id**,#id_commande,#id_plat,quantite,prix_unitaire)


