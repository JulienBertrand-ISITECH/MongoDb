# Exercice 1 
Modifiez la collection salle afin que soient dorénavant validés les documents destinés à y être insérés ; cette validation aura lieu en mode « strict » et portera sur les champs suivants :

nom sera obligatoire et devra être de type chaîne de caractères.

capacite sera obligatoire et devra être de type entier (int).

Dans le champ adresse, les champs codePostal et ville, tous deux de type chaîne de caractères, seront obligatoires.

***En m'aidant du cours, j'ai pu réaliser le validator suivant :***

```js
var proprietes = {
	"nom": {
		"bsonType": "string",
		"description": "Chaine de caractères + expr régulière - obligatoire" },
	"capacite": {
		"bsonType": "int",
		"description": "Integer - obligatoire" },
	"adresse.codePostal": {
		"bsonType": "string",
		"description": "Chaine de caractères + expr régulière - obligatoire"},
	"adresse.ville": {
		"bsonType": "string",
		"description": "Chaine de caractères + expr régulière - obligatoire"}
}
```

```js
db.runCommand( 
	{ 
		"collMod": "salles", 
		"validator": 
			{ "$jsonSchema": 
				{ 
					"bsonType": "object",
					"required": ["nom", "capacite", "adresse.codePostal", "adresse.ville", ],
					"properties": proprietes
				}
			}
	}
)
```

## Que constatez-vous lors de la tentative d’insertion suivante, et quelle en est la cause ?

```
db.salles.insertOne( 
{"nom": "Super salle", "capacite": 1500, "adresse": {"ville": "Musiqueville"}} 
) 
```

### Réponse
Lorsque j'ai tenté d'insérer la requête, le `MONGOSH` a retourné une erreur :

```js
[MongoServerError:](#) Document failed validation
Additional information:

{
	failingDocumentId: {},
	details: {
		operatorName: '$jsonSchema',
		schemaRulesNotSatisfied:
		[
			{
				operatorName: 'required',
				specifiedAs: 
				{
					required: ['nom', 'capacite', 'adresse.codePostal', 'adresse.ville']
				},
				missingProperties: [ 'adresse.codePostal' ]
			}
		]
	}
}

    at C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3269030
    at C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3108796
    at C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3307258
    at C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3306213
    at e.monitorCommands.i.cb (C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3101908)
    at Connection.onMessage (C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3099534)
    at MessageStream.<anonymous> (C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3096954)
    at MessageStream.emit (node:events:394:28)
    at c (C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3118818)
    at MessageStream._write (C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3117466)
```

Cela est dû à l'oublie d'insertion du champ `adresse.codePostal` comme l'indique la `console` :
```js
missingProperties: [ 'adresse.codePostal' ]
```

## Que proposez-vous pour régulariser la situation ?

### Reponse
Afin de pouvoir réguler la situation, je propose la requête suivante :

```js
db.salles.insertOne
( 
	{
		"nom": "Super salle",
		"capacite": 1500,
		"adresse": {"ville": "Musiqueville", "codePostal": "84350"}
	} 
) 
```

J'ai ajouté à la propriété manquante de l'objet `adresse` : `"codePostal": "84350"`

# Exercice 2

Rajoutez à vos critères de validation existants un critère supplémentaire : le champ _id devra dorénavant être de type entier (int) ou ObjectId.

```js
var proprietes = {
	"_id": { 
		"bsonType": ["int", "objectId"],
		"description": "Integer ou objectId - obligatoire" },
	"nom": { 
		"bsonType": "string",
		"description": "Chaine de caractères - obligatoire" },
	"capacite": { 
		"bsonType": "int",
		"description": "Integer - obligatoire" },
	"adresse.codePostal": {
		"bsonType": "string",
		"description": "Chaine de caractères - obligatoire" },
	"adresse.ville": {
		"bsonType": "string",
		"description": "Chaine de caractères - obligatoire" }
}
```

```js
db.runCommand( 
	{ 
		"collMod": "salles", 
		"validator": 
			{ "$jsonSchema": 
				{ 
					"bsonType": "object",
					"required": ["nom", "capacite", "adresse.codePostal", "adresse.ville", ],
					"properties": proprietes
				}
			}
	}
)
```

## Que se passe-t-il si vous tentez de mettre à jour l’ensemble des documents existants dans la collection à l’aide de la requête suivante :

```
db.salles.updateMany({}, {$set: {"verifie": true}}) 
```

### Reponse
Lorsque je tente de mettre à jour l'ensemble des documents existant dans la collection à l'aide de la requête, il se passe deux choses :
- Premièrement, je constate qu'un nouveau champ est apparu sur les documents respectant le schéma de validation créé.

```js
verifie: true
```

Ce champ confirme la validation du respect du schéma de validation.

- Deuxièmement, j'ai un message d'erreur qui est apparut dans la `MONGOSH`. Ce message a été généré, car la requête à rencontrer un document ne respectant pas le schéma de validation.

```js

[MongoServerError:](#) Document failed validation
Additional information:

{
	failingDocumentId: {},
	details: 
	{
		operatorName: '$jsonSchema',
		schemaRulesNotSatisfied: [
			{
				operatorName: 'required',
				specifiedAs: 
				{
					required: [ 'nom','capacite','adresse.codePostal','adresse.ville' ]
				},        
				missingProperties: [ 'adresse.codePostal' ] 
			}    
		]
	}
}

    at C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3284288
    at C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3108796
    at C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3307258
    at C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3306213
    at e.monitorCommands.i.cb (C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3101908)
    at Connection.onMessage (C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3099534)
    at MessageStream.<anonymous> (C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3096954)
    at MessageStream.emit (node:events:394:28)
    at c (C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3118818)
    at MessageStream._write (C:\Users\jbert\AppData\Local\MongoDBCompass\app-1.35.0\resources\app.asar.unpacked\node_modules\@mongosh\node-runtime-worker-thread\dist\worker-runtime.js:1917:3117466)

```

## Supprimez les critères rajoutés à l’aide de la méthode delete en JavaScript

### Reponse
***Entrée***
```js
db.salles.updateMany(
	{},
	{
		$unset: {"verifie": ""}
	}
)
```

# Exercice 3

## Rajoutez aux critères de validation existants le critère suivant :

Le champ smac doit être présent OU les styles musicaux doivent figurer parmi les suivants : "jazz", "soul", "funk" et "blues".

```js
var proprietes = {
	"nom": {
		"bsonType": "string",
		"description": "Chaine de caractères + expr régulière - obligatoire" },
		"capacite": { 
			"bsonType": "int",
			"description": "Integer - obligatoire" },
			"smac": { 
				"bsonType": [ "string", "null"],
				"enum": [ "jazz", "soul", "funk", "blues"],
				"description": "Doit être présent ou parmi les styles musicaux suivants : jazz, soul, funk, blues - facultatif" },
			"adresse.codePostal": {
				"bsonType": "string",
				"description": "Chaine de caractères + expr régulière - obligatoire"},
			"adresse.ville": { 
				"bsonType": "string",
				"description": "Chaine de caractères + expr régulière - obligatoire"}
	}
```

```js
db.runCommand( 
	{ 
		"collMod": "salles", 
		"validator": 
			{ "$jsonSchema": 
				{ 
					"bsonType": "object",
					"required": ["nom", "capacite", "adresse.codePostal", "adresse.ville", "smac"],
					"properties": proprietes
				}
			}
	}
)
```

## Que se passe-t-il lorsque nous exécutons la mise à jour suivante ?

db.salles.update({"_id": 3}, {$set: {"verifie": false}}) 

### Reponse

Lorsque nous exécutons la requête, nous mettons à jour le document possédant l'id 3 de la collection salles.

Le schéma de validation étant validé, le champ `verifie` avec sa valeur `false` sont ajouté.
