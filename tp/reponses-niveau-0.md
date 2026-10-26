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


***4***) Repere 3 donnees absurde

- acceptation de l'insertion donnee negatif (prix)
- acceptation de 2 plats de meme nom lors de insertion dans la carte
- acceptation de donnee erone pour categorie par exemple j'ai insere coca cola avec comme categorie soupe et ca marche

---

**0.3 Interroger l’API à la main**


***5***)

Pour la requete `b` le code http est 201. C'est pas 200 parce que c'est une creation

***6***) 

Pour la requete `d` la suppresion est bloque par PHP a cause de cette ligne

```php
return $this->erreur('Ce plat figure déjà dans des commandes, il ne peut pas être supprimé.', 409);
```

