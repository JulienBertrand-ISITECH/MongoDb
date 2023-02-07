# Créez une base de données sample nommée "sample_db" et une collection appelée "employees".

**Etape 1** : 
***Entrée*** : use sample_db

**Etape 2**
```js
db.createCollection("employees")  
```

Je viens de créer ma collection "employees" dans ma db "sample_db"

**Etape 3** :
J'initialise mes documents dans une variable pour ensuite l'utiliser dans ma requête "insertMany()".

***Entrée*** : 
```js
var employees = [
   {
   name: "John Doe",
   age: 35,
   job: "Manager",
   salary: 80000
   },
   {
   name: "Jane Doe",
   age: 32,
   job: "Developer",
   salary: 75000
   },
   {
   name: "Jim Smith",
   age: 40,
   job: "Manager",
   salary: 85000
   }
]
```

***Entrée*** : 
```js
db.employees.insertMany(employees)  
```

***Sortie*** : Mes documents ont bien été insérés.

```js
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("63e11092dab2f2f8e0d01ef0"),
    '1': ObjectId("63e11092dab2f2f8e0d01ef1"),
    '2': ObjectId("63e11092dab2f2f8e0d01ef2")
  }
}
```

# Écrivez une requête MongoDB pour trouver tous les documents dans la collection "employees".

***Entrée*** :
```js
db.employees.find() 
```  

***Sortie*** : Retourne tous les documents de la collection "employees" grâce à la fonction "find()"

# Écrivez une requête pour trouver tous les documents où l'âge est supérieur à 33.  

***Entrée*** :  Dans ma fonction find(), j'indique avec l'opérateur "$gt" que je souhaite retourner uniquement les documents dont l'âge est supérieur à 33.

```js
db.employees.find({"age": {$gt:33}}) 
```
   
***Sortie*** : Retourne 2 documents dont l'âge est supérieur à 33 ans.

# Écrivez une requête pour trier les documents dans la collection "employees" par salaire décroissant.

***Entrée*** : Afin de pouvoir trier les documents de ma collection, j'effectue successivement la fonction 'find()' et 'sort()'
```js
db.employees.find().sort({"salary": 1})
```

***Sortie*** Retourne la liste des employées dans l'ordre croissant de leur salaire.

```js
{
  _id: ObjectId("63e11092dab2f2f8e0d01ef1"),
  name: 'Jane Doe',
  age: 32,
  job: 'Developer',
  salary: 75000
}
{
  _id: ObjectId("63e11092dab2f2f8e0d01ef0"),
  name: 'John Doe',
  age: 35,
  job: 'Manager',
  salary: 80000
}
{
  _id: ObjectId("63e11092dab2f2f8e0d01ef2"),
  name: 'Jim Smith',
  age: 40,
  job: 'Manager',
  salary: 85000
}
```

# Écrivez une requête pour sélectionner uniquement le nom et le job de chaque document.

***Entrée*** : Je recherche dans ma collection "employée" où l'id existe tous les documents, et je retourne uniquement les noms et les jobs.

```js
db.employees.find({"_id": { $exists: true }}, {"_id":0, "name":1, "job": 1})   
```

***Sortie*** :
```js
{
  name: 'John Doe',
  job: 'Manager'
}
{
  name: 'Jane Doe',
  job: 'Developer'
}
{
  name: 'Jim Smith',
  job: 'Manager'
}
```

# Écrivez une requête pour compter le nombre d'employés par poste.

***Entrée*** : En utilisant la fonction aggregate() et l'opérateur $group nous pouvons séparer les documents en groupes selon une clé. Dans notre cas, la clé est le "job". Ensuite, nous comptons chaque document présent par clé avec la fonction count().

```js
db.employees.aggregate(
	[
		{
			$group:
			{
				_id:"$job",NombreEmployé:
					{ 
						$count: {}
					}
			}
		}
	]
)
```

# Écrivez une requête pour mettre à jour le salaire de tous les développeurs à 80000.

***entrée*** : Pour pouvoir mettre à jour le salaire de tous les développeurs, je réalise un 'updateMany()' en passant en paramètre le job "Developeur".
```js
db.employees.updateMany(
	{
		"job": "Developer"
	}, 
	{
		$set: {"salary": 80000}
	}
)  
```

***Sortie*** : 
```js
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```


