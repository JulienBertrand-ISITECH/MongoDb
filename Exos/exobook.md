# Créez une base de données sample nommée "sample_db" et une collection appelée "employees".

**Etape 1** : 
***Entrée*** : use sample_db

**Etape 2**
***Entrée*** : db.createCollection("employees")  
***Sortie*** : Je viens de créer ma collection "employees" dans ma db "sample_db"

**Etape 3** :
J'initialise mes documents dans une variable pour ensuite l'utiliser dans ma requête.

***Entrée*** : var employees = [
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

***Entrée*** : db.employees.insertMany(employees)  
***Sortie*** : 
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("63e11092dab2f2f8e0d01ef0"),
    '1': ObjectId("63e11092dab2f2f8e0d01ef1"),
    '2': ObjectId("63e11092dab2f2f8e0d01ef2")
  }
}

# Écrivez une requête MongoDB pour trouver tous les documents dans la collection "employees".
***Entrée*** : db.employees.find()   
***Sortie*** : Retourne tout les documents de la collection "employees"

# Écrivez une requête pour trouver tous les documents où l'âge est supérieur à 33.  
***Entrée*** : db.employees.find({"age": {$gt:33}})    
***Sortie*** : Retourne 2 documents dont l'age est supérieur à 33 ans.

# Écrivez une requête pour trier les documents dans la collection "employees" par salaire décroissant.
***Entrée*** : db.employees.find().sort({"salary": 1})  
***Sortie*** Retourne la liste des employées dans l'ordre croissant de leur salaire.

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

# Écrivez une requête pour sélectionner uniquement le nom et le job de chaque document.  
***Entrée*** : db.employees.find({"_id": { $exists: true }}, {"_id":0, "name":1, "job": 1})   
***Sortie*** :
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

Je recherche dans ma collection "employees" où l'id existe tout les documents, et je retourne uniquement les noms et les jobs.

# Écrivez une requête pour compter le nombre d'employés par poste.
***Entrée*** : db.employees.countDocuments()  
***Sortie*** : 3

En utilisant la fonction countDocuments(),la requête nous retourne le nombre de documents. Attention, la fonction ".count() est deprecated ! 

# Écrivez une requête pour mettre à jour le salaire de tous les développeurs à 80000.
***entrée*** : db.employees.updateMany({"job": "Developer"}, {$set: {"salary": 80000}})  
***Sortie*** : 
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}