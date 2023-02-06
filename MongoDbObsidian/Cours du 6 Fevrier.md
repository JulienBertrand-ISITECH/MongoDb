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

