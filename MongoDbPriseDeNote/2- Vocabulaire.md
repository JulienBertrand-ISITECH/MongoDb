## **SGBD** : 
Systeme de Gestion de Base de Données

## **Collection** : 
Groupe de documents MongoDB. C'ets l'équivalent d'une table dans un système de SGBD. Les collections n'imposent pas de schémas précis.

## **Les documents** :
Ensemble de données clé-valeur. Le schémas des documents est dit **dynamique**.

## **Un schéma** : 
Un schéma est un objet JSON qui définit la structure et le contenu de vos données. Vous pouvez utiliser les schémas BSON d'Atlas App Services, qui étendent[Schéma JSON](https://json-schema.org/)standard, pour définir le modèle de données de votre application et valider les documents chaque fois qu'ils sont créés, modifiés ou supprimés.

Les schémas représentent des _types_ de données plutôt que des valeurs spécifiques. App Services prend en charge de nombreux [types de schémas](https://www.mongodb.com/docs/atlas/app-services/schemas/types/#std-label-schema-types) intégrés . Ceux-ci incluent des primitives, telles que des chaînes et des nombres, ainsi que des types structurels, tels que des objets et des tableaux, que vous pouvez combiner pour créer des schémas qui représentent des _types d'objets_ personnalisés .

``` MongoDB
{
	"title": "car",
	"required": [
		"_id",
		"year",
		"make",
		"model",
		"miles"
	],
	"properties": {
		"_id": { "bsonType": "objectId" },
		"year": { "bsonType": "string" },
		"make": { "bsonType": "string" },
		"model": { "bsonType": "string" },
		"miles": { "bsonType": "number" }
	}
}
```

## **UTF-8** 
(abréviation de l'anglais _[Universal Character Set](https://fr.wikipedia.org/wiki/ISO/CEI_10646 "ISO/CEI 10646") Transformation Format_[1](https://fr.wikipedia.org/wiki/UTF-8#cite_note-1) - 8 [bits](https://fr.wikipedia.org/wiki/Bit "Bit")) est un [codage de caractères](https://fr.wikipedia.org/wiki/Codage_des_caract%C3%A8res "Codage des caractères") [informatiques](https://fr.wikipedia.org/wiki/Caract%C3%A8re_(informatique) "Caractère (informatique)") conçu pour coder l’ensemble des caractères du « répertoire universel de caractères codés », initialement développé par l’[ISO](https://fr.wikipedia.org/wiki/Organisation_internationale_de_normalisation "Organisation internationale de normalisation") dans la norme internationale [ISO/CEI 10646](https://fr.wikipedia.org/wiki/ISO/CEI_10646 "ISO/CEI 10646"), aujourd’hui totalement compatible avec le standard [Unicode](https://fr.wikipedia.org/wiki/Unicode "Unicode"), en restant compatible avec la norme [ASCII](https://fr.wikipedia.org/wiki/American_Standard_Code_for_Information_Interchange "American Standard Code for Information Interchange") limitée à l'anglais de base, mais très largement répandue depuis des décennies.

**L'UTF-8** est utilisé par 82,2 % des sites web en décembre 2014[2](https://fr.wikipedia.org/wiki/UTF-8#cite_note-2), 87,6 % en 2016[3](https://fr.wikipedia.org/wiki/UTF-8#cite_note-W3Techs-3), 90,5 % en 2017[4](https://fr.wikipedia.org/wiki/UTF-8#cite_note-4), 93,1 % en février 2019[5](https://fr.wikipedia.org/wiki/UTF-8#cite_note-5) et près de 95,2 % en octobre 2020. Par sa nature, UTF-8 est d'un usage de plus en plus courant sur Internet, et dans les systèmes devant échanger de l'information. Il s'agit également du codage le plus utilisé dans les systèmes [GNU/Linux](https://fr.wikipedia.org/wiki/Gnu/Linux "Gnu/Linux") et compatibles pour gérer le plus simplement possible des textes et leurs traductions dans tous les systèmes d'écritures et tous les alphabets du monde.

## **BSON**
Est un format d'échange de données [informatiques](https://fr.wikipedia.org/wiki/Informatique "Informatique") utilisé principalement comme stockage de données et format de transfert de données par le réseau dans la base de données [MongoDB](https://fr.wikipedia.org/wiki/MongoDB "MongoDB"). C'est un format binaire permettant de représenter des [structures de données](https://fr.wikipedia.org/wiki/Structure_de_donn%C3%A9es "Structure de données") simples et des [tableaux associatifs](https://fr.wikipedia.org/wiki/Tableau_associatif "Tableau associatif") (appelées objets ou des documents dans MongoDB). Le nom _BSON_ est basé sur le terme [JSON](https://fr.wikipedia.org/wiki/JavaScript_Object_Notation "JavaScript Object Notation") et signifie _Binary JSON.

## Collation : 
C'est un ensemble de règle spécifique d'une langue qui permet de manipuler une chaîne de caractère en fonction de la langue.

## **GeoJSON** 
(de l'anglais Geographic JSON, signifiant littéralement JSON géographique) est un [format ouvert](https://fr.wikipedia.org/wiki/Format_ouvert "Format ouvert") d'encodage d'ensemble de données géospatiales simples utilisant la norme [JSON](https://fr.wikipedia.org/wiki/JavaScript_Object_Notation "JavaScript Object Notation") (JavaScript Object Notation).

Il permet de décrire des données de type [point](https://fr.wikipedia.org/wiki/Point_(g%C3%A9om%C3%A9trie) "Point (géométrie)"), [ligne](https://fr.wikipedia.org/wiki/Ligne "Ligne"), [chaîne de caractères](https://fr.wikipedia.org/wiki/Cha%C3%AEne_de_caract%C3%A8res "Chaîne de caractères"), [polygone](https://fr.wikipedia.org/wiki/Polygone "Polygone"), ainsi que des ensembles et sous-ensembles de ces types de données et d'y ajouter des attributs d'information qui ne sont pas spatiales.