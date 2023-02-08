# Exercice 1

## Écrivez le pipeline qui affichera dans un champ nommé ville le nom de celles abritant une salle de plus de 50 personnes ainsi qu’un booléen nommé grande qui sera positionné à la valeur « vrai » lorsque la salle dépasse une capacité de 1 000 personnes. Voici le squelette du code à utiliser dans le shell :

La variable `var` est une pipeline de traitement de données qui utilise l'opérateur `Aggregation FrameWork`.

L'opérateur `$match` va filtrer les documents dont la valeur du champ `capacite` est supérieur à 50.

L'opérateur `$project` définit les champs qui doivent être inclus dans le résultats final, ainsi que les transformations à apporter au champs indiqué.

Le champ `_id` sera exclu du résultat final, deux nouveaux champs `ville` et `grande` seront ajoutés. Le champ `ville` sera défini par la syntaxe `$nom` pour accéder à la valeur du champ `nom`. Le champ `grnade` est défini en utilisant l'opérateur `$gt` pour vérifier si la salle dépasse les 1000 personnes de capacité.

```js
var pipeline = [ 
{
	$match : {
		"capacite": { $gt: 50 }
	}
},
	{
		$project: {
			"_id": 0,
			"ville": "$nom",
			"grande": {$gt: ["$capacite",1000]}
		}
	}
] 

db.salles.aggregate(pipeline) 
```
 
# Exercice 2

## Écrivez le pipeline qui affichera dans un champ nommé apres_extension la capacité d’une salle augmentée de 100 places, dans un champ nommé avant_extension sa capacité originelle, ainsi que son nom.

```js
var pipeline = [
	{
		$project: {
			"_id": 0,
			"avant_extension": "$capacite",
			"apres_extension": {$add: ["$capacite", 100]}
		}
	}
] 

db.salles.aggregate(pipeline) 
```

L'opérateur `$add` additionne des nombres ou ajoute des nombres et une date. Dans notre cas, l'opérateur `$add` pour ajouter 100 à la valeur du champ `capacite`.

# Exercice 3

## Écrivez le pipeline qui affichera, par numéro de département, la capacité totale des salles y résidant. Pour obtenir ce numéro, il vous faudra utiliser l’opérateur $substrBytes dont la syntaxe est la suivante :
```
{$substrBytes: [ < chaîne de caractères >, < indice de départ >, 
< longueur > ]} 
```

```js
var pipeline = [
{
	$match : {
		"adresse.codePostal": { $exists:1 }
	}
},
	{
		$project: {
			"_id": 0,
			"Numéro de département": {$substrBytes: ["$adresse.codePostal", 0,2] },
			"capacité total": "$capacite"
		}
	}
] 

db.salles.aggregate(pipeline) 
```

# Exercice 4

## Écrivez le pipeline qui affichera, pour chaque style musical, le nombre de salles le programmant. Ces styles seront classés par ordre alphabétique.

```js

var pipeline = [  
{ $unwind: "$styles" },
{ 
	$group: { 
		_id: "$styles", 
		count: { $sum: 1 } } 
	}, 
	{ 
		$sort: { _id: 1 } 
	}, 
	{ 
		$project: { 
			style_musical: "$_id", 
			nombre_de_salles: "$count", 
			_id: 0 
	} 
}
]
db.salles.aggregate(pipeline)
```

Cette pipeline utilise l'opérateur `$unwind` pour décomposer le tableau `styles` dans les documents de la collection `salles` permettant de compter le nombre de salles pour chaque style indépendament.

# Exercice 5

## À l’aide des buckets, comptez les salles en fonction de leur capacité :

celles de 100 à 500 places
```js
var pipeline = [
	{ 
		$bucket: { 
			groupBy: "$capacite", 
			boundaries: [100, 500],
				default: "Autre salles ayant une capacite inférieur ou supérieur", 
			output: { 
				count: { $sum: 1 } 
			} 
		} 
	}
]

db.salles.aggregate(pipeline)
```
Ce pipeline utilise l'opération `$bucket` pour regrouper les documents en fonction de la capacité, définie dans le champ `capacite`. Les limites sont définies dans le tableau `boundaries`, et les salles seront regroupées en fonction de leur capacité (supérieure à 100 et inférieure ou égale à 500). Toutes les salles dont la capacité n'entre pas dans les limites définies dans le tableau `boundaries` seront regroupées dans la catégorie `Autre salles ayant une capacite inférieur ou supérieur`. L'opération `$sum` est utilisée pour compter le nombre de documents dans chaque groupe, et l'opération `$sort` trie les groupes par ordre croissant de capacité.

celles de 500 à 5000 places
```js
var pipeline = [
	{ 
		$bucket: { 
			groupBy: "$capacite", 
			boundaries: [500, 5000],
				default: "Autre salles ayant une capacite inférieur ou supérieur", 
			output: { 
				count: { $sum: 1 } 
			} 
		} 
	}
]

db.salles.aggregate(pipeline)
```
Ce pipeline utilise l'opération `$bucket` pour regrouper les documents en fonction de la capacité, définie dans le champ `capacite`. Les limites sont définies dans le tableau `boundaries`, et les salles seront regroupées en fonction de leur capacité (supérieure à 500 et inférieure ou égale à 5000). Toutes les salles dont la capacité n'entre pas dans les limites définies dans le tableau `boundaries` seront regroupées dans la catégorie `Autre salles ayant une capacite inférieur ou supérieur`. L'opération `$sum` est utilisée pour compter le nombre de documents dans chaque groupe, et l'opération `$sort` trie les groupes par ordre croissant de capacité.

Exercice 6

Écrivez le pipeline qui affichera le nom des salles ainsi qu’un tableau nommé avis_excellents qui contiendra uniquement les avis dont la note est de 10.

```js
var pipeline = [
  {
    $unwind: "$avis"
  },
  {
    $group: {
      _id: "$nom",
      avis_excellents: { 
        $push: {
          $cond: [
            { $eq: ["$avis.note", 10] },
            "$avis",
            null
          ]
        }
      }
    }
  },
  {
    $project: {
      _id: 0,
      nom: "$_id",
      avis_excellents: {
        $filter: {
          input: "$avis_excellents",
          as: "avis",
          cond: { $ne: ["$$avis", null] }
        }
      }
    }
  }
];

db.salles.aggregate(pipeline);

```

Cette pipeline utilise les opérations suivantes :
- `$cond` : Dans notre cas, l'opérateur évalue l'expression boolean `$eq: ["$avis.note", 10]`, si il y a une note à 10 alors on retourne la note, sinon, il retourne `null`.
- `$filter` : Dans notre cas, l'opérateur prend 