Jeu de données: Téléchargez ou générez un jeu de données de stations météorologiques, qui incluent la date, la température, la pression atmosphérique, etc.

Préparation des données:
a. Importez les données de stations météorologiques dans MongoDB en utilisant la commande mongoimport.

b. Assurez-vous que les données sont bien structurées et propres pour une utilisation ultérieure.

```
Pensez à bien vérifier les types des colonnes  :

Country/Region : string
Lat : Decimal128
Long : Decimal128
Date : Date
day_from_jan_first : Int32
temp : Decimal128
min : Decimal128
max : Decimal128
stp : Decimal128
slp : Decimal128
dewp : Decimal128
wdsp : Decimal128
prpc : Decimal128

!!! ATTENTION les températures sont en degrés fahrenheit !!!

```

Indexation avec MongoDB:
a. Créez un index sur le champ de la date pour améliorer les performances de la recherche. Utilisez la méthode createIndex ().

```js
// Indexe créé par ordre croissant grâce à "date": 1 sur ma collection donnees.
db.donnees.createIndex({"date": 1}, {"name": "date_ascending"})
// Indexe créé par ordre décroissant grâce à "date": -1 sur ma collection donnees.
db.donnees.createIndex({"date": -1}, {"name": "date_descending"})
```

b. Vérifiez que l'index a été créé en utilisant la méthode listIndexes ().

Pour afficher les données renvoyées par la méthode `listIndexes()` sur la collection `donnees`, j'utilise une boucle `for` afin de parcourir le tableau `firstBatch` et ainsi avec la fonction `printjson` afficher chaque index présent sur ma collection.

***Entrée***
```js
var indexes = db.runCommand({ listIndexes: "donnees" }).cursor.firstBatch
for (var i = 0; i < indexes.length; i++) { 
	printjson(indexes[i])
}
```

***Sortie***
```js
{ v: 2, key: { _id: 1 }, name: '_id_' }
{ v: 2, key: { date: 1 }, name: 'date_ascending' }
{ v: 2, key: { date: -1 }, name: 'date_descending' }
```

Requêtes MongoDB:
a. Recherchez les stations météorologiques qui ont enregistré une température supérieure à 25°C pendant les mois d'été (juin à août). Utilisez la méthode find () et les opérateurs de comparaison pour trouver les documents qui correspondent à vos critères.

Dans un premier temps,  j'ai filtré mes documents sur ceux qui ont une température supérieure à 77° fahrenheit qui est l'équivalent à 25° Celsus grâce à l'opérateur `$gt`. Ensuite, j'ai filtré les dates avec les opérateurs `$gte` et `$lte` et la fonction `ISODate()` où j'y ai passé en paramètre mon intervalle de date. Enfin, je demande à projeter les `stations` grâce à `Country/Region :1` et la température avec `temp: 1` 

***Entrée***
```js
/*
N'ayant pas de mois en été (uniquement janvier/fevrier/mars), je filtre sur janvier/fevrier
*/
db.donnees.find({
    "temp": {$gt: 77},
    "Date": {
        $gte: ISODate("2020-01-01T00:00:00.000+00:00"),
        $lte: ISODate("2020-02-31T23:59:59.999+00:00")
    }
},
	{
		"_id":0,
		"Country/Region":1,
		"temp":1
	}
)
```

b. Triez les stations météorologiques par pression atmosphérique, du plus élevé au plus bas. Utilisez la méthode sort () pour trier les résultats.

Comme précédemment, j'utilise la méthode `find()` sur la collection `donnees` qui fonctionne en filtre/projection et j'utilise la méthode `sort()` qui me permet de trier par ordre croissant décroissant mes projections. Dans notre cas, je lui passe en paramètre `"slp":-1 ` qui signifie que je souhaite un trie par ordre décroissant sur mes pressions atmosphériques.

***Entrée***
```js
db.donnees.find (
	{
		"Country/Region": { $exists: 1 },
		"slp": { $exists: 1 }
	},
	{
		"_id":0,
		"Country/Region":1,
		"slp":1
	}
).sort (
	{ 
		"slp":-1 
	}
)
```

Framework d'agrégation:
a. Calculez la température moyenne par station météorologique pour chaque mois de l'année. Utilisez le framework d'agrégation de MongoDB pour effectuer des calculs sur les données et grouper les données par mois.

Afin de pouvoir calculez la température moyenne par station météorologique pour chaque mois de l'année, j'ai réalisé une requête en utilisant l'opérateur `pipeline` sur la collection `donnees`. 
- Dans un premier temps, j'utilise l'opérateur `$match` qui filtre sur les documents qui possèdent un champ `Date` existant.
- Ensuite, j'ai utilisé l'opérateur `$project` qui me permet de projeter seulement la `température`, les `mois` qui sont créé grâce à l'opérateur `$month` qui me retourne le mois de ma date sous la forme d'un nombre compris entre 1 et 12 (dans notre cas, 1 à 3 car nous n'avons seulement que les trois premiers mois.) et les "stations" (issues de mon champ `Country/Region`). 
- J'ai par la suite groupé mes documents grâce à l'opérateur `$group` par les mois et les stations en calculant la température moyenne au moyen de l'opérateur `$avg`. 
- Enfin, j'ai réalisé un tri via la méthode `sort` sur les mois et les stations par ordre croissant/alphabetique

***Entrée***
```js
/*
month: 1 = Janvier
month: 2 = Février
month: 3 = Mars
*/
var pipeline = [
{
	$match: {
		"Date": { $exists: 1 }
	}
},
	{
	    $project: {
	      temp: 1,
	      month: { $month: "$Date"},
	      "Country/Region": 1
	    }
	},
  {
    $group: { 
	    _id: { month: "$month", stations: "$Country/Region" },
	    "Température_moyenne": { $avg: "$temp" }
    }
  },
  {
    $sort: {
      "_id.month": 1,
      "_id.stations":1
    }
  }
]

db.donnees.aggregate(pipeline)

```

b. Trouvez la station météorologique qui a enregistré la plus haute température en été. Utilisez le framework d'agrégation de MongoDB pour effectuer des calculs sur les données et trouver la valeur maximale.

Afin de Trouvez la station météorologique qui a enregistré la plus haute température, j'ai réalisé une requête en utilisant l'opérateur `pipeline` sur la collection `donnees`. 
- Dans un premier temps, j'utilise l'opérateur `$match` qui filtre sur les documents qui possèdent un champ `Date` existant et un champ `max`.
- Ensuite, j'ai utilisé l'opérateur `$project` qui me permet de projeter seulement la `température maximum`, les `mois` qui sont créé grâce à l'opérateur `$month` qui me retourne le mois de ma date sous la forme d'un nombre compris entre 1 et 12 (dans notre cas, 1 à 3 car nous n'avons seulement que les trois premiers mois.) et les "stations" (issues de mon champ `Country/Region`). 
- Puis j'utilise l'opérateur `$match` qui filtre sur les documents qui possèdent un `mois` en `Février` grâce à l'opérateur `$in`.
- J'ai par la suite groupé mes documents grâce à l'opérateur `$group` par les stations en ne prenant que les valeurs maximum dans chaque station.
- De plus, j'ai réalisé un tri via l'opérateur `$sort` en triant les `température maximum` par ordre décroissant.
- Enfin, avec l'opérateur `$limit` j'ai limité l'affiche au premier élément qui grâce au trie réalisé m'affiche mon document ayant la `température maximum`.

***Entrée***
```js
/*
month: 1 = Janvier
month: 2 = Février
month: 3 = Mars

N'ayant pas de mois en été, j'ai pris les valeurs de janvier/fevrier/Mars
*/
var pipeline = [
  {
    $match: {
      "Date": { $exists: 1 },
      "max": { $exists: 1 }
    }
  },
  {
    $project: {
      max: 1,
      month: { $month: "$Date" },
      station: "$Country/Region"
    }
  },
  {
    $match: {
      "month": { $in: [2] }
    }
  },
  {
    $group: {
      _id: "$station",
      maxTemperature: { $max: "$max" }
    }
  },
  {
    $sort: {
      "maxTemperature": -1
    }
  },
  {
    $limit: 1
  }
]

db.donnees.aggregate(pipeline)

```

Export de la base de données:
a. Exportez les résultats des requêtes dans un fichier CSV pour un usage ultérieur. Utilisez la commande mongoexport pour exporter des données de MongoDB.

Pour réaliser un export, il faut tout simplement réalisé la requête suivant dans un `shell` :

***Entrée***
```js
mongoexport --collection=donnees --db=meteo --out=meteo.json
```