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
				"properties": properties
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

#### Question :
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

#### L'opérateur `$elemMatch`

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

#### Le trie

[Lien de la doc officiel](https://www.mongodb.com/docs/manual/reference/method/cursor.sort/)

```js
curseur.sort(<tri>)

```