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

***7***) Commande curl 


```bash
curl -i -X POST http://localhost:8080/api/commandes -H "Content-Type: application/json" -d "{\"numero_table\":6, \"lignes\": [{\"id_plat\":4, \"quantite\":2, \"prix_unitaire\":7000}, {\"id_plat\":10, \"quantite\":1, \"prix_unitaire\":1000}]}"
```


**0.4 Suivre une requête de bout en bout**

- Completez le tableau en indiquant, pour chaque étape, **le fichier et le nom de la fonction** :

| # | Étape | Fichier | Fonction / ligne |
|---|---|---|---|
| 1 | Le formulaire est soumis | `components/PlatForm.vue` | `enregistrer()` |
| 2 | La requête HTTP est construite | `api.js` | `requete()` |
| 3 | Vite transmet `/api/...` au port 8080 | `api.js` | `fetch(BASE + chemin, options)` |
| 4 | CodeIgniter choisit le contrôleur | `Config/Routes.php` | `$routes->post('plats', 'Plats::create');` |
| 5 | Le contrôleur lit le JSON | `Api/Plats.php` | `$donnees = $this->donneesJson();` |
| 6 | Les règles de validation sont vérifiées | `Models/PlatModel.php` | `protected $validationRules` |
| 7 | La ligne est insérée | `Api/Plats.php` | `$id = $this->plats->insert($donnees);` |
| 8 | Le formulaire prévient l’écran parent | `components/PlatForm.vue` | ` emit('enregistre', resultat)` |
| 9 | L’écran recharge la liste | `views/CarteView.vue` | `surEnregistrement(plat) / await charger()`|


***8***) 

- Le code metier de la creation d'une commande est dans `Services/CommandService.php`, cette separation permet une bonne lisibilite du code