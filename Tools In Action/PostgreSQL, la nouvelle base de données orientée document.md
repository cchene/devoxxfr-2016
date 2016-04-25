# Postgresql : la nouvelle base de données orientée document

Présenté par : Yan Bonnel (de Cegedim), présentation disponible [en ligne](https://github.com/ybonnel/devoxx-postgresql)

## Versions
* 9.3 : type JSON, non indexable et donc utilisable sur des volumes de données modérés
* 9.4 : type JSONB, indexable et donc utilisable sur de gros volumes de données

## Requêtage
JSONB est un nouveau type de colonne à utiliser dans une table, comme n'importe quel autre type.

Une valeur de ce type est un objet structuré JSON, qui possède donc des propriétés (dont la valeur peut elle-même être un objet JSON).

Une propriété issue d'une valeur de type JSONB peut être utilisée à n'importe quel clause d'une requête de sélection :

* `SELECT`
* `WHERE`
* `GROUP BY`
* `ORDER BY`

La syntaxe est assez "déroutante" : il y a de nouveaux opérateurs, notamment pour naviguer dans les propriétés d'un objet JSON (ex: `->` et `->>`).

Pour utiliser les opérateurs "classiques" (comparaison, etc.), les valeurs JSON doivent être *castées* en un type légal PostgreSQL (ex: `...::INTEGER` ou `...::DATE`).

Le type JSONB permet la création d'index sur des propriétés à l'intérieur d'une valeur JSON. Un index peut même être créé sur les éléments d'un tableau JSON.

Une démonstration a été faite avec une base de données de 8 Go :

* Requête avec un `WHERE` sur une propriété indexée en moins de 0,1 ms
* Requête avec un `WHERE` sur une propriété des éléments dans un tableau en quelques ms.

A noter également, PostgreSQL propose la possibilité de formatter en JSON le résultat d'une requête SELECT (permet d'écrire rapidement une source pour un WS REST).
