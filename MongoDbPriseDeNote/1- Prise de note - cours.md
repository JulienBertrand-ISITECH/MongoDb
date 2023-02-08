#### La notion JSON
mongoDb est un language proche du JSON/JS.
MongoDb va stocker une représentation binaire du format JSON.
Le format ets BSON pour Binary SON.
BSON -> JSON
JSON -> BSON

Taille max d'un fichier BSON = 16Mo max. (1200 pages d'un livre = 3Mo)

**JSON** (JavaScript Object Notation) est un format d'échange de données léger.

Il est facile pour les humains de lire et d'écrire. Il est facile pour les machines d'analyser et de générer. Il est basé sur un sous-ensemble de la norme de langage de programmation JavaScript ECMA-262 3e édition - décembre 1999. JSON est un format de texte complètement indépendant du langage mais utilise des conventions familières aux programmeurs de la famille C de langages, y compris C, C++, C#, Java, JavaScript, Perl, Python et bien d'autres. Ces propriétés font de JSON un langage d'échange de données idéal.

JSON est construit sur deux structures :

-   Une collection de paires nom/valeur. Dans divers langages, ceci est réalisé sous la forme d'un _objet_ , d'un enregistrement, d'un struct, d'un dictionnaire, d'une table de hachage, d'une liste à clés ou d'un tableau associatif.
-   Une liste ordonnée de valeurs. Dans la plupart des langages, ceci est réalisé sous la forme d'un _tableau_ , d'un vecteur, d'une liste ou d'une séquence.

Ce sont des structures de données universelles. Pratiquement tous les langages de programmation modernes les prennent en charge sous une forme ou une autre. Il est logique qu'un format de données interchangeable avec les langages de programmation soit également basé sur ces structures.

En JSON, ils prennent ces formes :

Un _objet_ est un ensemble non ordonné de paires nom/valeur. Un objet commence par { accolade gauche et se termine par } accolade droite . Chaque nom est suivi de : deux- points et les paires nom/valeur sont séparées par , virgule .

![](https://www.json.org/img/object.png)

Un _tableau_ est une collection ordonnée de valeurs. Un tableau commence par [ crochet gauche et se termine par ] crochet droit . Les valeurs sont séparées par , virgule .

![](https://www.json.org/img/array.png)

Une _valeur_ peut être une _chaîne_ entre guillemets doubles, ou un _nombre_ , ou true ou false ou null , ou un _objet_ ou un _tableau_ . Ces structures peuvent être imbriquées.

![](https://www.json.org/img/value.png)

Une _chaîne_ est une séquence de zéro ou plusieurs caractères Unicode, entourés de guillemets doubles, utilisant des échappements antislash. Un caractère est représenté par une seule chaîne de caractères. Une chaîne ressemble beaucoup à une chaîne C ou Java.

![](https://www.json.org/img/string.png)

Un _nombre_ ressemble beaucoup à un nombre C ou Java, sauf que les formats octal et hexadécimal ne sont pas utilisés.

![](https://www.json.org/img/number.png)

Un espace blanc peut être inséré entre n'importe quelle paire de jetons. À l'exception de quelques détails d'encodage, cela décrit complètement le langage.

![](https://www.json.org/img/whitespace.png)

Ceci représente un objet vide en JSON
```JSON
{
	
}
```

Les propriétés d'un objet JSON sont materialisées par un ou plusieurs pauires cle/valeur :
``` JSON
{
	cle: valeur, // cle/valeur
	cle: {cle: valeur}, //Une valeur peut aussi être un objet
	monTableau: [] // Une valeur peut être un tableau
}
```

**Le nom des clefs doivent être unique.**

JSON prend en charge de façon native plusieurs types de données :
- boolean
- numerique
- chaine de caractère
- tableau
- objet
- null (indique une abscence de valeur)

MongoDB vient ajouter des types :
- **Le type Date** : stocké sous la forme d'un entier signé de 8 octets. (depuis le 01/01/1970 à minuit epoque UNIX => 1675678615 pour le 6 fevrier 2023 à 10h16 et 55 sec)
- **Le type Object/ID** : stocké sur 12 octets , utilisé en interne par mongoDB afin de générer des identifiants.
- **Le type NumberLong et le type NumberInt** : Par défault, MongoDB considère toutes valeurs numériques comme un nombre à virgule codé sur 8 octets. **Représentent des entiers sign&s sur 8 et 4 octets.**
- **Le type BinDate** : Pour stocker des chaînes de caractères ne possédant pas de représentation dans l'encodage UTF-8, ou n'importe quel contenu binaire.

#### Execution de mongoDB
``` Bash
mongod

Ou 

mongod --port 27017 --dbpath /data/databases --logpath /tmp/mongodb // ou un autre port selon la fonfig

log --logappend
```

Par défault, le port d'écoute est le 27017.

#### Création d'une base de donnée MongoDB

Normalement, nous n'avons pas besoin de toucher à :
- Admin
- config
- local

#### Commande MongoDB
``` javascript

/* commande de base */
// La commande principale consiste à vérifier la version installée du serveur MongoDB et Mongo Shell
mongod --version

// loste les commandes
help() 

db.stats() // 

// voir la bdd
show databases 

// voir les collections
show collection 

// permet de changer de bdd
use [nom de la db] 

// créé un utilisateur possédant les pleins pouvoir sur toutes les db
db.createUser({"user": "superUser", "pwd" : "password", "roles": [ {"role": "userAdmin", "db", "admin"}, "readWriteAnyDatabase" ]}) 


```

##### Créer une bdd
```js
// Créé une collection avec une_cle et une_valeur.
db.uneCollection.insertOne({"une-cle": "une_valeur"}) 

// Variable global qui stocke le nom de la base en cours d\'utilisation
"db"

// affiche les documents de la collections de la base en cours d\'éxécution.
db.uneCollection.find() 
```

##### Suprimer une BDD
```js
use [NOMDELABDDASUPPRIMER]
db.dropDatabase() // ne pas oublier les () sinon ca ne fonctionne pas !
```


##### Affiche les stats de la bdd
``` js
db.runCommand({"collstats": "uneCollection"})

db.adminCommand("currentOp")
```


##### Gestion des collections
###### Les collations

Les collations servent à définir les règles sur les chaines de caractères.
```
{
	**local**: <string>,,
	caseLevel: <boolean>,
	caseFirst: <string>,
	strength: <entier>,
	numericOrdering: <boolean>,
	....
}
```

Au sein de ce type de document, le champ local est obligatoire.
- Deux valeur pour la local : FR et ??

###### Creer une collection
```js
db.createCollection("maCollection", {"collation": {"local": "fr"} })
```

###### Renommer une collection 
[RenameColelction](https://www.mongodb.com/docs/manual/reference/command/renameCollection/)
```js
db.runCommand(
   {
     renameCollection: "<source_namespace>",
     to: "<target_namespace>",
     dropTarget: <true|false>,
     writeConcern: <document>,
     comment: <any>
   }
)
```

#### Les documents
###### Insertion d'un document

```js
db.maCollection.insertOne(< /*documents ou tableau de documents*/ >)

db.personnes.insertOne([
	{"nom": "Bertrand", "prenom": "Julien"},
	{"nom": "l'éponde", "prenom": "Bob", "age": 8}						
])
```

###### Modification d'un document 
```js
//filtre = permet de cibler les champs souhaité
//modification = permet de réaliser les modifications sur les champs filtrés
db.collection.updateOne(<filtre>, <modifications>)

db.personnes.update(
	{"nom": "Bertrand"},
	{
		$set : { "nom": "bertrand" }
	}
)
```

Cas de l'upsert : la combinaison d'update et d'insert.
```js
db.personnes.updateOne(
	{"nom": "Bertrand"},
	{
		$set : { "nom": "bertrand" }
	},
	{
		"upsert": true
	}
)
```

#### Effectuer des recherches
###### La méthode 'findAndModify'
[findAndModify](https://www.mongodb.com/docs/manual/reference/method/db.collection.findAndModify/)
Modifie et renvoie un seul document. Par défaut, le document retourné n'inclut pas les modifications apportées à la mise à jour. Pour retourner le document avec les modifications apportées à la mise à jour, utilisez l' `new`option.
```js
db.collection.findAndModify({
    query: <document>,
    sort: <document>,
    remove: <boolean>,
    update: <document or aggregation pipeline>, // Changed in MongoDB 4.2
    new: <boolean>,
    fields: <document>,
    upsert: <boolean>,
    bypassDocumentValidation: <boolean>,
    writeConcern: <document>,
    collation: <document>,
    arrayFilters: [ <filterdocument1>, ... ],
    let: <document> // Added in MongoDB 5.0
});
```

#### Validation des documents
```js
var atheles = [
	{
		"nom": "Eclair",
		"prenom": "Jean-Michel",
		"discipline": "Course"
	},
	{
		"nom": "Cavalera",
		"prenom": "Max",
		"discipline": "Saut de haies"
	},
	{
		"nom": "Hammer",
		"prenom": "Ski"
	}
]

db.athletes.insertMany(athletes)

var proprietes = {
	"nom" : {
		"bsonType": "string",
		"pattern": "^[A-Z]",
		"description" : "Chaine de caractères + expr régulière - obligatoire" },
	"prenom": {
		"bsonType": "string",
		"description": "Chaine de caractère -obligatoire" },
	"discipline": {
		"enum": ["Course", "Lancer de marteau", "Ski"],
		"description": "Enumeration - obligatoire"
	}
}
```

Maintenant que nos règles sont établies, nous allons modifier la collection à l'aide de la commande `colMod`

```js
db.runCommand(
	{
		"collMod": "athletes",
		"validaitor: 
		{
			$jsonSchema: 
			{
				"bsonType": "object",
				"required": ["nom", "prenom", "discipline"],
				"properties": proprietes
			}	
		}
	}
)
```

Les méthodes find et findOne ont la même signature et permettent d'effectuer des requestes Mongo.

```js
db.collection.find(<requete>, <projection>)
```

Chacun de ces paramètres sont tous deux des documents. (par defaud ça retourne 20 documents)

```js
DBQuery.shellBatchSize = 40
```

mongorc.js est un fichier de config qui se retrouve à la racine du répertoire utilisateur.

/home/userName
```js
db.maCollection.find().limite(12)
db.maCollection.find().limite(12).pretty()
```

On peut utiliser des operateurs afin d'affiner la recherche :

```js
db.maCollection.find({"age": {$eq: 76} }) // retourne toutes les personnes qui ont 76 ans.
db.maCollection.find({"age": 76}, {"prenom": true}) // ne retourne que les prenoms de ceux qui ont 76 ans.
```

On retrouve d'autres opérateurs tels que :
- $ne : différent de
- $gt : supérieur à
- $gte : supérieur ou egal à
- $lt: inférieur à
- $lte: inférieur ou égal à 
- $nin : absence
- $in: presence

On peut combiner ces opérateurs pour effectuer des recherches sur des intervalles :
```js
db.maCollection.find({"age": { $lt: 50, $gt: 20 }})
db.maCollection.find({"age": { $nin: [23, 45, 78] }})
db.maCollection.find({"age": { $exists: true }})
```

#### Les opérateurs logiques :

```js
db.personnes.find(
	{
		$and: [
			{
				"age": {
					$exists: true || 1
				}
			},
			{
				"age": {
					$nin: [23, 45, 234, 100]
				}
			}
		]
	},
	{
		"_id": false || 0,
		"nom": "1",
		"prenom": "1"
	}
)
```

##### L'opérateur `$expr`

[Lien vers la doc officiel](https://www.mongodb.com/docs/manual/reference/operator/query/expr/)

Il permet d'utiliser des expressions dans nos requêtes. Ces expressions pourront contenir des opérateurs, des objets ou encore des chemins de champ. (field path // $NomDeLaVariable)

On va chercher à afficher les documents dont la longueur du nom multipliée parn 12 est supérieur à l'âge.

```js
	db.maCollection.find({
		"nom": { $exists: 1 },
		"age": { $exists: 1 },
		$expr: { $gt: [ { $multiply: [ { $strLenCP: "$nom" }, 12] }, "$age" ] }
	},
	{
		"_id":0
		"nom":1
	}
)
```

Afficher les comptes dont la sommes des opérations de débit est supérieur au montant du credit :

```js
	db.banque.find({
		"credit": { $exists: 1 },
		"debit": { $exists: 1 },
		$expr: { $gt: [ { $sum: "$debit" }, "$credit" ] }
	}
)
```

##### L'opérateur `$type`

[Lien vers la doc officiel](https://www.mongodb.com/docs/manual/reference/operator/query/type/)

```js
{ champ :  {  $type: < type BSON > } }
 
{ champ :  {  $type: [< type BSON >, < type BSON >] } }
```

```js
db.personnes.insertOne({
	"nom": "Zidane", "prenom": "Z", "age": numberInt(50)
})
```

##### L'opérateur `$mod`

[Lien vers la doc officiel](https://www.mongodb.com/docs/manual/reference/operator/query/mod/)

```js
{ champ: {$mod: [diviseur, reste] } }
```

##### L'opérateur `$Where`

[Lien vers la doc officiel](https://www.mongodb.com/docs/manual/reference/operator/query/where/)

L'opérateur `$Where` permet d'éxécuter directement du javascript.

Exemple :
```js
db.personnes.find({ $where: "this.nom.lenght > 6" })

// Equivalent avec une fonction
db.personnes.find({ $where: function() {
	return obj.nom.lenght > 6
} })
```

##### Les opérateurs de tableau
[Lien vers la doc $push](https://www.mongodb.com/docs/manual/reference/operator/update/push/)
[Lien vers la doc $pull](https://www.mongodb.com/docs/manual/reference/operator/update/pull/)

```js
{ $push: { <champ>: <valeur>, ... } }
```

```js
{ $pull: { <field1>: <value|condition>... } }
```

	Exemple pour `$push` (Ajoute un champ)
```js
db.hobbies.updateOne( {_id:1}, {$push: {passions: "streetFighter"}})
db.hobbies.updateMany( {}, {$push: {passions: "streetFighter"}})
```

Exeple pour `$pull` (Retire un champ)
```js
db.hobbies.updateOne( {_id:3}, {$pull: {passions: "paracute"}})
```

##### L'opérateur `$addToSet`
[Lien vers la doc officiel](https://www.mongodb.com/docs/manual/reference/operator/update/addToSet/)

Permet d'éviter l'ajout de doublon, a privilégier à `$push`.

Exemple :
```js
db.hobbies.updateOne( {_id:1}, {$addToSet: {passions: "streetFighter"}})
```

##### Question :
```js
db.personnes.find({interets:"jardinage"})

db.personnes.find({interets:["jardinage", "bridge"]})

db.personnes.find({interets:["bridge", "jardinage"]})

// permet de chercher peut importe l'ordre.
db.personnes.find({interets: {$all: ["bridge", "jardinage"]}}) 

// rehcreche le spersonne dont l'interet 1 est jardiange
db.personnes.find({ "interets.1": "jardinage" }) 

// Recherche les personnes qui ont deux interêts
db.personnes.find({ "interets": {$size: 2} }) 

// Recherche les personnes qui ont au moins 2 interets. (les tableaux commence à 0 donc 0 =1, 1=2 ....)
db.personnes.find({ "interets.1": {$exists:1} })
```

##### L'opérateur `$elemMatch`

[Lien vers la doc officiel](https://www.mongodb.com/docs/manual/reference/operator/query/elemMatch/)

```js
db.eleves.find({ "notes": { $elemMatch :  { $gt: 0, $lt: 10} }})

db.eleves.find({ "notes" : { $all : [5, 7.50] } }) // Ne retourne que les élèves qui ont eu 5 et 7.5. Ca agit comme un && logique.

```

#### Les tableaux de documents

Renvoyer les documents dont les élèves ont au moins une note à 10.
```js
db.eleves.find({"notes.note": 10})
```

Renvoyer les documents dont les élèves ont au moins une note entre 10 et 15 dans une matière quelqconque :

```js
// Retourne si les deux conditions sont remplis.
db.eleves.find(
	{
		"notes": 
		{
			$elemMatch: 
			{
				"note": { $gt: 10, $lte: 15 }
			}
		}
	}
)

// Retourne si au moin une condition est remplit.
db.eleves.find(
	{
		"notes.note": {$gt: 10, $lte: 15}
	}
)

//Affiche les élèves qui ont eu moins de 10 en Mathémtique.
db.eleves.find(
	{
		"notes.0.note": {$lt: 10}
	}
)
```

##### Le trie

[Lien de la doc officiel](https://www.mongodb.com/docs/manual/reference/method/cursor.sort/)

```js
curseur.sort(<tri>)

```




#### Les index
[Lien de la doc officiel](https://www.mongodb.com/docs/manual/indexes/)

Les index prennent en charge l'exécution efficace des requêtes dans MongoDB. Sans index, MongoDB doit effectuer une _analyse de collection_ , c'est-à-dire analyser chaque document d'une collection, pour sélectionner les documents qui correspondent à l'instruction de requête. Si un index approprié existe pour une requête, MongoDB peut utiliser l'index pour limiter le nombre de documents qu'il doit inspecter.

Les index sont des structures de données spéciales qui stockent une petite partie de l'ensemble de données de la collection sous une forme facile à parcourir. L'index stocke la valeur d'un champ spécifique ou d'un ensemble de champs, triés par la valeur du champ. L'ordre des entrées d'index prend en charge des correspondances d'égalité efficaces et des opérations de requête basées sur des plages. De plus, MongoDB peut renvoyer des résultats triés en utilisant l'ordre dans l'index.

Le schéma suivant illustre une requête qui sélectionne et ordonne les documents correspondants à l'aide d'un index :

![Schéma d'une requête qui utilise un index pour sélectionner et renvoyer des résultats triés.  L'index stocke les valeurs de ``score`` dans l'ordre croissant.  MongoDB peut parcourir l'index dans l'ordre croissant ou décroissant pour renvoyer des résultats triés.](https://www.mongodb.com/docs/manual/images/index-for-sort.bakedsvg.svg)
***Avantage*** : Le temps de recherche est réduit.
***Inconvénient*** : Augmente le temps d'écriture.

On indexe en priorité les champs courrant utilisé dans notre application.

La nature de votre application devra impacter votre logique d'indexation : 
- Est-elle orienté écriture (write-heavy) ?
- Est-elle orienté lecture (read-heavy) ?

Montrer deux requetes : une sans indexation et une avec et comment les mettre en place.
[doc getIndex](https://www.mongodb.com/docs/manual/reference/method/db.collection.getIndexes/)
[doc createIndex](https://www.geeksforgeeks.org/mongodb-db-collection-createindex-method/#:~:text=The%20indexes%20are%20ordered%20by,index%2C%202d%20index%2C%20etc.)
[doc dropIndex()](https://www.mongodb.com/docs/manual/reference/method/db.collection.dropIndex/)

```js
db.collection.createIndex(<champ + type>, <option?>)

db.personnes.createIndex({"age": -1})

{
	"createdCollectionAutomatically": false,
	"numIndexesBefore": 1,
	..
	ok:1
}

// Affiche un tableau avec les détails d'un indexe
db.personnes.getIndexes()

// Suprime un index
db.personnes.dropIndex("age_-1")

// Crée un index avec un nom et une collation
db.personnes.createIndex({"age": -1}, {"name": "index_age", "background": true}, {"collation": {"locale": "fr"}})




```

Un indexe peut porter sur plusieurs champs, cela s'apelle un ***indexe composé***.

L'ordre dans lequels les champs sont énuméré est très important.

La suppression d'index et la création d'index peut avoir un impacte très négatif. Une procédure de création/suppression doit se faire lors d'une maintenance.

```js
// Exemple d'une requête avec indexe composé (age et nom)
db.personnes.createIndex(
	{
		"age": -1, "nom":1
	},
	{
		"name": "idx_age_nom",
		collation: {local:"fr"} 
	}
)
```

MongoDB permet l'utilisation de deux types d'index qui permettent de gérer les requêtes geospatiales :
- Les index de type `2dsphere` sont utilisés par des des requêtes geospatiales intervenant sur une surface spherique.
- Le sindex `2d` concernent des requêtes intervenant sur un plan Euclidien.

Pour un champ nommé `donneesSpatiales` d'une collection `cartographie` vous pouvez par exemple créer un index de tpe `2d` avec la commande :

```js
db.cartographie.createIndex({ "donneesSpatiales": "2d" })
```

Pour la création d'un index `2dsphere` on utilisera plûtot :
```js
db.cartographie.createIndex({ "donneesSpatiales": "2dsphere" })
```

Les index `2d` font intervenir des coordonnées de type `legacy`  permettent de travailler avec des absysses et des ordonnées.

*A RETENIR l'ordre : longétude/latitude*

```js
db.plan.insertIne({ "nom": "firstPoint", "geodata": [1,1] })

db.plan.insertOne({ "nom": "firstPoint_bis", "geodata": [4.7, 44.5] })

db.plan.insertOne({ "nom": "firstPoint_bis", "geodata": "lon": 4.7, "lat": 44.5 })
```

#### geoJson

[Lien de la doc officiel](https://www.mongodb.com/docs/manual/reference/geojson/)

```js
{ type: <type d'objet>, coordinates: <coordonnees>}
```

##### Le type Point

```js
"type": "Point",
"coordinates": [ 14.0, 1.0 ]
```

##### Le type MultiPoint

```js
"type": "Point",
"coordinates": [ [ 14.0, 1.0 ], [ 13.0, 1.0 ] ]
```

##### Le type LineString:

```js
//Permet de matérialiser une ligne
"type": LineString,
"coordinates": [
	[ 14.0, 1.0 ], [ 13.0, 1.0 ]
]
```

##### Le type  Polyon

[Lien de la doc officiel](https://www.mongodb.com/docs/manual/reference/operator/query/polygon/)

```js
//Permet de matérialiser un polygone (c'est un tableau de tableau de tableau)
"type": Polygon,
"coordinates": [
	[
		[ 14.0, 1.0 ], [ 13.0, 1.0 ]
	],
	[
		[ 14.0, 1.0 ], [ 13.0, 1.0 ]
	]
]
```


```js
db.avignon.createIndex({"localisation": "2dsphere"})
db.avignon2s.createIndex({"localisation": "2d"})
```

##### L'opérateur $nearSphere

[Lien de la doc officiel](https://www.mongodb.com/docs/manual/reference/operator/query/nearSphere/)

```js
	$nearSphere: {
		$geometry: {
				type: "Point",
				coordinates: [<longitude>, <latitude>]
			},
			$minDistance: <distance en metres>, //optionnel
			$maxDistance: <distance en metres> // optionnel
		}
	}
	
	{
		$nearSphere: [ <x>, <y>],
		$minDistance: <distance en radian>,
		$maxDistance: <distance en radian>
	}
```

Nativement, `$nearSphere` réalise un tri.

###### Exercice du cours
```js
var operaAvignon = { type: "Point", coordinates: [4.81, 43.95] }
```

Effectuer une requête sur la collection avignon

***Entrée***
```js

//La requête va retourner les localisation présente du plus proche au plus loin par rapport à notre variable.
db.avignon.find(
	{
		"localisation": {
			$nearSphere: {
				$geometry: operaAvignon
			}
		}
	}, {"_id": 0, "nom": 1}
)
```

***Sortie***
```js
{  nom: 'Collection Lambert'}

{  nom: 'Palais des Papes'}

{  nom: 'Pont Saint-Bénézet'}
```

##### L'opérateur `$geoWithin`
[Liende la doc officiel](https://www.mongodb.com/docs/manual/reference/operator/query/geoWithin/)
Permet de capturer les documents des coordonnées geospatiales sont ......
Cet opérateur n'effectue aucun tri et ne necessite pas la création d'un index geospatiale, on l'utilise de la manière suivante :

```js
{
	<champ des documents contenant les coordonnées> : {
		$geoWithin: {
			<opérateur de forme>: <coordonnées>	
		}
	}
}
```

Création d'un polygone pour notre exemple :
```js
var polygone = [
	[43.9548, 4.80143],
	[43.95475, 4.80779],
	[43.95045, 4.81097],
	[43.4657, 4.80449],
]
```

La requête suivante utilise ce polygone :
```js
db.avignon2d.find(
	{
		"localisation": 
		{
			$geoWithin: 
			{
				$polygon: polygone
			}
		}
	}, {"_id": 0, "nom": 1}
)
```

Signature pour le cas d'utilisation d'objet GeoJSON
```js
{
	<champ des documents contenant les coordonnées> : {
		$geoWithin: {
			type: < "Polygon" ou bien "MultiPolygon">,
			coordinates: [<coordonnees>]
		}
	}
}
```

```js
var polygone = [
	[43.9548, 4.80143],
	[43.95475, 4.80779],
	[43.95045, 4.81097],
	[43.4657, 4.80449],
	[43.9548, 4.80143]
]

db.avignon.find(
{
	"localisation": 
	{
		$geoWithin: 
		{
			$geometry: 
			{
				type: "Polygon",
				coordinates: [polygone]
			}
		}
	}
}, {"_id": 0, "nom": 1}
)
```

#### FrameWork d'agregation

MongoDB met a disposition un puissant outil d'analyse et de traitement d'information: le pipeline d'agregation (ou framework).
Metaphore du tapis roulant d'usine

Méthode utilisée:
```js
db.collection.aggregate(pipeline, option)
```

- Pipeline: Designe un tableau d'étapes
- options: Désigne un document

Parmis les options, nous retiendrons:
- collation, permet d'affecter une collation à l'opération d'aggregation
- bypassDocumentValidation: Fonctionne avec un opérateur appelé `$out` et permet de passer au travers de la validation des documents.
- allowDiskUse: Donne la possibilité de faire déborder les opérations d'écriture sur le disque.

Vous pouvez appeler aggregate sans argument:
```js
db.personnes.aggregate()
```

Au sein du shell, nous allons créer une variable pipeline :
```js
var pipeline = []
db.personnes.aggregate(pipeline)
db.personne.aggregate(
	pipeline,
	{
		"collation": {
			"local": "fr"
		}
	}
)
```

Le filtrage avec `$match`
[Liend e la doc](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/)

Cela permet d'obtenir des pipelines performants avec des temps d'éxécution courts. Normalement, `$match` doit intervenir le plus en amont possible dans le pipeline car `$match` agit comme un filtre en réduisant le nombre de documents à traiter plus en aval dans le pipeline. (Dans l'ideal on devrait le trouver comme premier opérateur)

La syntaxe est la suivante : 
```js
{ $match : {<requete>} }
```

Commençons par la première étape :
```js
var pipeline = [{
	$match : {
		"interets": "jardinage"
	},
	$match: {
		"nom": /^L/,
		"age": {$gt:70}
	}
}]

db.personnes.aggregate(pipeline)
```

Sélection/modification de champs : `$project`

[Lien de la doc](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/)

Syntaxe :
```js
{ $project: { <spec> } }
```

```js
var pipeline = [
	{
		$match : {
			"interets": "jardinage"
			}
	},
	{
		$project: {
			"_id": 0,
			"nom": 1,
			"prenom": 1,
			"super_senior": { $gte: ["$age", 70] }
		}
	},
	{ // retourne ceux qui ont la condition juste après avoir créer la donnée
		$match: {
			"super_senior": false
		}
	}
]

db.personnes.aggregate(pipeline)
```

```js
var pipeline = [
	{
		$match : {
			"interets": "jardinage"
			}
	},
	{
		$project: {
			"_id": 0,
			"nom": 1,
			"prenom": 1,
			"ville": "$adresse.ville"
		}
	},
	{
		$match: {
			"ville": {$exists: true}
		}
	}
]

db.personnes.aggregate(pipeline)
```

##### L'opérateur $addFields

[Lien de la doc](https://www.mongodb.com/docs/manual/reference/operator/aggregation/addFields/)

```js
 $addField: { <nouveau champ> : <expression>, ...} }

db.personnes.aggregate([
	{
		$addFields : {
			"numero_secu_s" : ""
		}
	}
])
```

###### Exercice du cours
```js
db.achats.insertMany(
	[
		{
			"nom": "Pascal",
			"prenom": "Léo",
			"achats": [112.29, 88.36, 72.01],
			"reductions": [12.30, 2.01]  
		},
		{
			"nom": "Perez",
			"prenom": "Alex",
			"achats": [20.01, 296.35],
			"reductions": [9.91, 0.87]  
		}
	]
)    

db.achats.aggregate(
	[
		{
			$addFields: 
				{
					"total_achats": { $sum: "$achats" },
					"total_reduc": { $sum: "$reductions" }
				}
		},
		{
			$addFields: {
				"total_final": { $subtract: [ "$total_achats",  "$total_reduc" ] }
			}
		},
		{
			$project: {
				"_id": 0,
				"nom": 1,
				"prenom":1,
				"Total payé": "$total_final"
			}
		}
	]
)
```

##### L'opérateur `$group`

```js
{
	$group: {
		"_id": <expression>,
		<champ>: { <operateur d'accumulation>}
	}
}
```

Opérateur d'accumulation: `$push`, `$sum`, `$avg`, `$min`, `$max`

Exemple d'utilisation :
```js
var pipeline = [
	{
	//group par age et affiche le nombre de personne par section d'age
		$group: {
			"_id": "$age",
			"nombre_personne": { $sum: 1}
		}
	},
	{
	//permet de trier
		$sort: {
			"nombre_personne": 1
		}
	}

]

db.personnes.aggregate(pipeline)

//Fait la même chose que plus haut
db.personnes.aggregate([
{
	$sortByCount: "$age"
}
])

//Affiche le nombre de personne
var pipeline = [{
	$group: {
		"_id": null,
		"nombre_personnes": { $sum: 1}}
}]
db.personnes.aggregate(pipeline)


var pipeline = [{
	$match: {
		"age": { $exists: true}
	},
	},
{
	$group: {
		"_id": null,
		"avg": { $avg: "$age"}
	}
},
	{
		$project: {
			"_id": 0,
			"Age_moyen"{
				$ceil: '$avg'
			}
		}
	}
}]
```
