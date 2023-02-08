# Exercice 1

## Un bref examen de vos fichiers journaux a révélé que la plupart des requêtes effectuées sur la collection salles cible des capacités ainsi que des départements, comme ceci :

```
db.salles.find({"capacite": {$gt: 500}, "adresse.codePostal": /^30/}) 
db.salles.find({"adresse.codePostal": /^30/, "capacite": {$lte: 400}}) 
```

Que proposez-vous comme index qui puisse couvrir ces requêtes ?

Afin de pouvoir couvrir ces requêtes, je propose la requête suivante qui permet de créer un nouvel `Index` sur la collection `salles`.  

Pour cela, j'utilise la fonction `createIndex()` où je lui passe en paramètre mes champs `capacite` et `adresse.codePostal` et en option la valeur `idx_capacite_codePostal` dans le champ `name` afin de nommer mon index créé.

```js
db.salles.createIndex(
	{
		"capacite": 1,
		"adresse.codePostal": 1
	},
	{
		"name": "idx_capacite_codePostal"
	}
)
```

## Détruisez ensuite l’index créé.

Pour suppriumer mon index créé, j'utilise tout simplement la fonction `dropIndex()` et je lui passe en paramètre le nom de l'index que je souhaite supprimer. Dans ce cas, le nom de l'index sera `idx_capacite_codePostal`.

```js
db.salles.dropIndex("idx_capacite_codePostal")
```