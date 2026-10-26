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


**0.5 Repérer les notions de Vue**

| Notion | À quoi ça sert | Votre exemple |
|---|---|---|
| `ref()` | une variable dont le changement met l’écran à jour | `Dans CommandeView.vue  const numeroTable = ref('')` |
| `computed()` | une valeur calculee à partir d’autres, recalculee automatiquement | `PlatList.vue`: `const groupes = computed(() => parCategorie(props.plats))` =>  la valeur de `groupe` se recalcul automatiquement quand les donnees `props.plats` change grace a la fonction `computed` |
| `v-for` | répéter un élément pour chaque élément d’une liste | `PlatList.vue`: `<li v-for="plat in liste" :key="plat.id">` => ici l'element `plat` se repete jusqu'a `liste.length` |
| `v-if` / `v-else` | afficher un élément seulement si une condition est vraie | `Panier.vue`: `<p v-if="lignes.length === 0" class="vide">Touchez un plat de la carte pour l’ajouter.</p> <ul v-else class="panier-liste">` => Le text `Touchez un plat de la carte pour l’ajouter.` s'affiche si la condition `lignes.length === 0` dans `v-if` est vrai, sinon on affiche la liste grace dans condition `v-else` |
| `v-model` | lier un champ de formulaire à une variable, dans les deux sens | `CommandeView.vue`: `<input v-model.number="numeroTable" type="number" min="1" />` => le variable `numeroTable` prend directement les données saisie sur le champs grace a `v-model` |
| `defineProps` | recevoir des données du composant parent | `Panier.vue`: `const props = defineProps({ lignes: { type: Array, required: true }, })` => le props `lignes` recoit les données transmis par le composant parent puis utiliser dans le composant fille grace au props `lignes` |
| `defineEmits` / `emit` | envoyer un événement au composant parent | `PlatForm.vue`: `const emit = defineEmits(['enregistre', 'annule'])` => Les emits `enregistre` et `annule` sont envoyer au composant parent puis traiter selon les besoins |
| `onMounted` | exécuter du code quand le composant apparaît | `CarteView.vue`: `onMounted(charger)` => Execute la fonction `charge` qui charge les données a afficher quand le composant apparait |


***9***) 

- `Panier.vue` emet `plus` et `moins` parcequ'il affiche juste les donnees envoyees par le composant parent, donc il ne peut pas les manipule directement, d'où l'utilisation des `plus` et `moins`

- - C'est `CommandeView.vue` qui modifie reellement la quantite grace au fonction `ajouter` et `retirer` 


***10***)

- `watch` sert a preremplir les champs du formulaire quand on clique sur **Modifier** d’un plat

- Sans `watch`, les champs du formulaire ne serait pas preremplis car elle verfie si un plat a ete selectionne par le bouton **Modifier** et preremplis les champs