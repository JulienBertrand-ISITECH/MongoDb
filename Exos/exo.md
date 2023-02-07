Voici la base de donnees qui permet d'effectuer la serie d'exercices : 

```
db.salles.insertMany([ 
   { 
       "_id": 1, 
       "nom": "AJMI Jazz Club", 
       "adresse": { 
           "numero": 4, 
           "voie": "Rue des Escaliers Sainte-Anne", 
           "codePostal": "84000", 
           "ville": "Avignon", 
           "localisation": { 
               "type": "Point", 
               "coordinates": [43.951616, 4.808657] 
           } 
       }, 
       "styles": ["jazz", "soul", "funk", "blues"], 
       "avis": [{ 
               "date": new Date('2019-11-01'), 
               "note": NumberInt(8) 
           }, 
           { 
               "date": new Date('2019-11-30'), 
               "note": NumberInt(9) 
           } 
       ], 
       "capacite": NumberInt(300), 
       "smac": true 
   }, { 
       "_id": 2, 
       "nom": "Paloma", 
       "adresse": { 
           "numero": 250, 
           "voie": "Chemin de l'Aérodrome", 
           "codePostal": "30000", 
           "ville": "Nîmes", 
           "localisation": { 
               "type": "Point", 
               "coordinates": [43.856430, 4.405415] 
           } 
       }, 
       "avis": [{ 
               "date": new Date('2019-07-06'), 
               "note": NumberInt(10) 
           } 
       ], 
       "capacite": NumberInt(4000), 
       "smac": true 
   }, 
    { 
       "_id": 3, 
       "nom": "Sonograf", 
       "adresse": { 
           "voie": "D901", 
           "codePostal": "84250", 
           "ville": "Le Thor", 
           "localisation": { 
               "type": "Point", 
               "coordinates": [43.923005, 5.020077] 
           } 
       }, 
       "capacite": NumberInt(200), 
       "styles": ["blues", "rock"] 
   } 
]) 
```

# Exercice 1

#### Affichez l’identifiant et le nom des salles qui sont des SMAC.

Pour afficher l'identifiant et le nom des salles qui sont des SMAC, nous devons réaliser la requête suivant : 
***Entrée*** :
```js
db.salles.find({"smac": true}, {"_id": 1, "nom": 1})
```

***Sortie*** :
```js
{  _id: 1,  nom: 'AJMI Jazz Club'}

{  _id: 2,  nom: 'Paloma'}
```

Cette requête indique que nous souhaitons rechercher tout les documents qui possèdent le champ `smac` à `true`.

# Exercice 2

#### Affichez le nom des salles qui possèdent une capacité d’accueil strictement supérieure à 1000 places.

Pour afficher le nom des salles qui possèdent une capacité d'accueil strictement supérieur à 1000 places, nous devons réaliser la requête suivante :

***Entrée*** : 
```js
db.salles.find({"capacite": {$gt:1000}}, {"_id":0,"nom":1})
```

***Sortie*** :
```js
{  nom: 'Paloma'}
```

Avec cette requête, nous cherchons dans la collection 'salles' toutes les salles dont la capacité est strictement supérieur à 1000 avec l'opérateur `$gt`  et nous affichons uniquement les noms de celle-ci avec les paramètres suivants : ` {"_id":0,"nom":1}`

# Exercice 3

#### Affichez l’identifiant des salles pour lesquelles le champ adresse ne comporte pas de numéro.

Pour afficher l'identifiant des salles pour lesquelles le champ adresse ne comporte pas de numéro, il faut effectuer la requête suivante :

***Entrée*** :
```js
db.salles.find({"adresse.numero": null}, {"_id":1})
```

***Sortie*** :
```js
{  _id: 3}
```

# Exercice 4

#### Affichez l’identifiant puis le nom des salles qui ont exactement un avis.

Pour afficher l'identifiant puis le nom des salles qui ont exactement un avis, il faut réaliser la requête suivante : 

***Entrée*** : 
```js
db.salles.find({avis: { $size:1}}, {_id:1, nom:1})
```

***Sortie***
```js
{
	_id: 2,
	nom: 'Paloma'
}
```
# Exercice 5

#### Affichez tous les styles musicaux des salles qui programment notamment du blues.

Afin de pouvoir afficher tous les styles musicaux des salles qui programment  notamment du blues, il faut effectuer la requête suivant :

***Entrée*** :
```js
db.salles.find({"styles":"blues"}, { _id:0, nom:1, styles:1 })
```

***Sortie*** :
```js
{  nom: 'AJMI Jazz Club',  styles: [    'jazz',    'soul',    'funk',    'blues'  ]}

{  nom: 'Sonograf',  styles: [    'blues',    'rock'  ]}
```

# Exercice 6

#### Affichez tous les styles musicaux des salles qui ont le style « blues » en première position dans leur tableau styles.

Pour affichez tous les styles musicaux des salles qui ont le style « blues » en première position dans leur tableau styles, il faut effectuer la requête suivant :

***Entrée*** : 
```js
db.salles.find({"styles.0":"blues"}, { _id:0, nom:1, styles:1 })
```

***Sortie*** : 
```js
{  nom: 'Sonograf',  styles: [    'blues',    'rock'  ]}
```

# Exercice 7

#### Affichez la ville des salles dont le code postal commence par 84 et qui ont une capacité strictement inférieure à 500 places (pensez à utiliser une expression régulière).

Pour affichez la ville des salles dont le code postal commence par 84 et qui ont une capacité strictement inférieure à 500 places, il faut effectuer la requête suivante :

***Entrée*** :
```js
db.salles.find(
	{		
			"adresse.codePostal": { $regex: /^84/i },
			"capacite": {$lt: 500}
	},
	{
		"_id":0,
		"adresse.ville":1
	}
)
```

***Sortie*** : 

```js
{  adresse: {    ville: 'Avignon'  }}
{  adresse: {    ville: 'Le Thor'  }}
```

# Exercice 8

#### Affichez l’identifiant pour les salles dont l’identifiant est pair ou le champ avis est absent.

Pour affichez l’identifiant pour les salles dont l’identifiant est pair ou le champ avis est absent, il faut effectuer la requête suivante :

***Entrée*** :
```js
db.salles.find(
	{		
		$or: [ { _id: {$mod: [ 2, 0 ]} }, {avis: null}]
	},
	{
		"_id":1
	}
)
```

***Sortie*** : 
```js
{  _id: 2}
{  _id: 3}
```

# Exercice 9

#### Affichez le nom des salles dont au moins un des avis comporte une note comprise entre 8 et 10 (tous deux inclus).

Pour affichez le nom des salles dont au moins un des avis comporte une note comprise entre 8 et 10 (tous deux inclus), il faut effectuer la requête suivante :

***Entrée*** :
```js
db.salles.find(
	{		
		"avis.note": { $gte: 8, $lte: 10}
	},
	{
		"nom":1
	}
)
```

***Sortie*** : 
```js
{  _id: 1,  nom: 'AJMI Jazz Club'}
{  _id: 2,  nom: 'Paloma'}
```

# Exercice 10

#### Affichez le nom des salles dont au moins un des avis comporte une date postérieure au 15/11/2019 (pensez à utiliser le type JavaScript Date).

Pour affichez le nom des salles dont au moins un des avis comporte une date postérieure au 15/11/2019, il faut effectuer la requête suivante en utilisant la fonction Date() de js :

***entrée*** :
```js
db.salles.find(
	{		
		"avis.date": {$lt: new Date("2019-11-15T00:00:00.000+00:00")}
	},
	{
		"nom":1
	}
)
```

# Exercice 11

#### Affichez le nom ainsi que la capacité des salles dont le produit de la valeur de l’identifiant par 100 est strictement supérieur à la capacité.

Pour affichez le nom ainsi que la capacité des salles dont le produit de la valeur de l’identifiant par 100 est strictement supérieur à la capacité, il faut effectuer la requête suivante :

***Entrée*** :
```js
db.salles.find(
	{		
		"_id": {$exists: 1},
		"nom": { $exists: 1 },
		"capacite": { $exists: 1 },
		$expr: { $gt: [ { $multiply: [ "$_id", 100] }, "$capacite"] }
	},
	{
		"_id":0,
		"nom":1,
		"capacite":1
	}
)
```

# Exercice 12

#### Affichez le nom des salles de type SMAC programmant plus de deux styles de musiques différents en utilisant l’opérateur $where qui permet de faire usage de JavaScript.

Pour affichez le nom des salles de type SMAC programmant plus de deux styles de musiques différents, il faut effectuer la requête suivante :

***Entrée*** :
```js
db.salles.find(
	{	
	$where:
		"this.smac == true && this.styles?.length > 2"
	},
	{
		"_id":0,
		"nom":1
	}
)
```

# Exercice 13

#### Affichez les différents codes postaux présents dans les documents de la collection salles.

Pour affichez les différents codes postaux présents dans les documents de la collection salles, il faut effectuer la requête suivante :

***Entrée*** :
```js
db.salles.find(
	{
		"adresse.codePostal": {$exists: 1}
	},
	{
		"_id":0,
		"adresse.codePostal":1
	}
)
```

***Sortie*** :
```js
{  adresse: {    codePostal: '84000'  }}
{  adresse: {    codePostal: '30000'  }}
{  adresse: {    codePostal: '84250'  }}
```

# Exercice 14

#### Mettez à jour tous les documents de la collection salles en rajoutant 100 personnes à leur capacité actuelle.

Pour mettre à jour tous les documents de la collection salles en rajoutant 100 personnes à leur capacité actuelle, il faut effectuer la requête suivante :

***Entrée***
```js
db.salles.updateMany(
	{
		"capacite": {$exists:1}
	},
	{
		$inc: {"capacite": 100}
	}
)
```

# Exercice 15

#### Ajoutez le style « jazz » à toutes les salles qui n’en programment pas.

Pour ajoutez le style « jazz » à toutes les salles qui n’en programment pas,  il faut effectuer la requête suivante avec l'opérateur `$addToSet` qui ajoute si le style `jazz` n'est pas déja présent dans le document.

***Entrée***
```js
db.salles.updateMany(
	{
		"styles": {$exists:1}
	},
	{
		$addToSet: {"styles": "jazz"}
	}
)
```

# Exercice 16

#### Retirez le style «funk» à toutes les salles dont l’identifiant n’est égal ni à 2, ni à 3.

Pour retirez le style «funk» à toutes les salles dont l’identifiant n’est égal ni à 2, ni à 3, il faut effectuer la requête suivante :

***Entrée***
```js
db.salles.updateMany(
	{
		"styles": {$exists:1},
		_id: {$nin: [2,3]}
	},
	{
		$pull: {"styles": "funk"}
	}
)
```

# Exercice 17

#### Ajoutez un tableau composé des styles «techno» et « reggae » à la salle dont l’identifiant est 3.

Pour ajoutez un tableau composé des styles «techno» et « reggae » à la salle dont l’identifiant est 3, il faut effectuer la requête suivante :

***Entrée***
```js
db.salles.updateMany(
	{
		"styles": {$exists:1},
		_id: {$in: [3]}
	},
	{
		$addToSet: {"styles": ["techno", "reggae" ]}
	}
)
```

# Exercice 18

#### Pour les salles dont le nom commence par la lettre P (majuscule ou minuscule), augmentez la capacité de 150 places et rajoutez un champ de type tableau nommé contact dans lequel se trouvera un document comportant un champ nommé telephone dont la valeur sera « 04 11 94 00 10 ».

Pour ajouter 150 places et un champ de type tableau nommé contact dans lequel se trouvera un document comportant un champ nommé telephone dont la valeur sera « 04 11 94 00 10 » pour toutes les salles dont le nom commence par la lettre p/P, il faut réaliser la requête suivante :

***Entrée***
```js
db.salles.updateMany(
	{
		"nom": { $regex: "^[Pp]" }
	},
	{
		$inc: {"capacite": 150},
			$set: {	"contact": [{"telephone": "0411940010"}] }
	}
)
```

# Exercice 19

#### Pour les salles dont le nom commence par une voyelle (peu importe la casse, là aussi), rajoutez dans le tableau avis un document composé du champ date valant la date courante et du champ note valant 10 (double ou entier). L’expression régulière pour chercher une chaîne de caractères débutant par une voyelle suivie de n’importe quoi d’autre est [^aeiou]+$.

***Entrée***
```js
db.salles.updateMany(
	{
		"nom": { $regex: "[^aeiou]+$" }
	},
	{
		$addToSet: { 
						"avis": [{"date": new Date(), "note": 10}]
					}
	}
)
```

# Exercice 20

#### En mode upsert, vous mettrez à jour tous les documents dont le nom commence par un z ou un Z en leur affectant comme nom « Pub Z », comme valeur du champ capacite 50 personnes (type entier et non décimal) et en positionnant le champ booléen smac à la valeur « false ».

***Entrée***
```js
db.salles.updateMany(
	{
		"nom": { $regex: "^[Zz]" }
	},
	{
		$set: 
			{
				"nom": "Pub Z",
				"capacite": Int32("50"),
				"smac": "false",
				"id":{
		"bsonType": "objectId"
		}
			}
	},
	{
		"upsert": true
	}
)
```

# Exercice 21

#### Affichez le décompte des documents pour lesquels le champ _id est de type « objectId ».

***Entrée***
```js
db.salles.find
(
	{
		"_id": 
			{
				$type: ["objectId"]
			}
	}
).count()
```

# Exercice 22

#### Pour les documents dont le champ _id n’est pas de type « objectId », affichez le nom de la salle ayant la plus grande capacité. Pour y parvenir, vous effectuerez un tri dans l’ordre qui convient tout en limitant le nombre de documents affichés pour ne retourner que celui qui comporte la capacité maximale.

***Entrée***
```js
db.salles.find
(
	{
		"_id":
			{
				$not:{ $type: ["objectId"] }
			}
	}
).sort({"capacite":-1}).limit(1)
```

# Exercice 23

#### Remplacez, sur la base de la valeur de son champ _id, le document créé à l’exercice 20 par un document contenant seulement le nom préexistant et la capacité, que vous monterez à 60 personnes.

***Entrée***
```js
db.salles.updateOne
(
	{
		"_id": ObjectId("63e266882445e89cf656627d")
	},
	{
		$unset: { smac: "" },
		$inc: { "capacite": 10}
	}
)
```

# Exercice 24

#### Effectuez la suppression d’un seul document avec les critères suivants : le champ _id est de type « objectId » et la capacité de la salle est inférieure ou égale à 60 personnes.

***Entrée***
```js
db.salles.deleteOne
(
	{
		"_id": ObjectId("63e266882445e89cf656627d"),
		"capacite": {$lte: 60}
	}
)
```

# Exercice 25

#### À l’aide de la méthode permettant de trouver un seul document et de le mettre à jour en même temps, réduisez de 15 personnes la capacité de la salle située à Nîmes.

***Entrée***
```js
db.salles.updateOne
(
	{
		"adresse.ville": {$eq: "Nîmes"}
	},
	{
		$inc: { "capacite": -15}
	}
)
```