# Hibernate tu connais... mais en fait tu connais pas

Présenté par : Emmanuel Bernard (de Red Hat)

Il s'agit d'une présentation des dernières nouveautés d'Hibernate.

Se reporter également au blog de l'équipe Hibernate : [In Relation To](http://in.relation.to/)

## Hibernate ORM 5

L'architecture d'ORM a été modularisée, ce qui implique que la phase d'initialisation d'Hibernate a été changée. Ceci est masqué pour les projets qui utilisent l'API JPA.

Une réflexion ironique du *speaker* laisse penser que le fichier XML décrivant le *mapping* objet-relationnel pourrait finir par devenir déprécié, au profit des annotations Java.

Les types `Date` et `Time` de Java 8 sont supportés nativement grâce au composant `hibernate-java8`.

Les ressources de l'API Hibernate sont désormais des `java.lang.AutoClosable` (ex : `Session`, etc.).

Les performances ont été améliorées, notamment pour la détection de modification des entités. L'algorithme utilisé est le même que celui employé par [React](https://facebook.github.io/react/) pour détecer les modifications à appliquer au DOM.

### Propriétés *lazy*

En version 4, toutes les propriétés marquées *lazy* étaient chargées depuis la BD au même moment, dès que l'une d'entre elle devait être chargée.

Pour permettre une optimisation de son modèle par le développeur, Hibernate 5 introduit la notion de groupe de propriétés *lazy* : toutes les propriétés d'un groupe sont chargées dès que l'une d'elles doit être chargée (les propriétés *lazy* des autres groupes ne sont pas chargées).

### Associations bidirectionnelles

Ces associations devaient être gérées manuellement avec Hibernate 4 : modifier le pointeur d'une des extrémités de l'association nécessitait la mise à jour du pointeur de l'autre extrémité.

Cette modification de mise en cohérence est gérée automatiquement par Hibernate 5.

### Cache de niveau 2

L'empreinte mémoire du cache de niveau 2 a été optimisée en partageant (et ainsi évitant) la duplication de certaines instances d'objets :
* Les instances d'identifiants d'entités (*object IDs*)
* Les objets immuables qui sont mis en cache par référence

### Divers

Hibernate ORM 5 introduit la gestion automatique d'identifiants de type UUID.

La documentation a été grandement améliorée.

## Hibernate Search

Hibernate Search propose, avec un *back-end* [ElasticSearch](https://www.elastic.co/) de faire des recherches plain-texte sur les entités Hibernate.

Il est possible d'exprimer les requêtes en utilisant la syntaxe Lucene native ou en utilisant l'API `QueryBuilder`.

Les recherches sont possibles aussi sur des valeurs "spatiales" (latitude et longitude).

Le support du [*faceting*](https://fr.wikipedia.org/wiki/Classification_%C3%A0_facettes) est aussi possible. On peut ainsi raffiner le résultat d'une requête avec des critères complémentaires (un peu comme Amazon permet de filtrer le produit recherché en fonction de certaines de ses caractéristiques).

Note : Voir également la session [Elasticsearch et Hibernate sont sur un bateau](../Tools In Action/Elasticsearch et Hibernate sont sur un bateau.md).

## Hibernate OGM

Cette API permet d'utiliser une couche de persistance JPA pour une base de données NoSQL.

Le support est imparfait aujourd'hui, mais cela peut être utilisé pour des tests sur NoSQL d'une application existante...

## Hibernate Validator

C'est l'implémentation Hibernate de la spécification des *bean validators*. La nouveauté principale est la possibilité de déclarer des contraintes par programmation. Cela permet par exemple de supporter des contraintes qui dépendent d'un contexte (ex : multi-tenant).

## Autres liens

* [Debezium](https://github.com/debezium/debezium) est un outil qui permet le *streaming* des données d'une application en interceptant toutes les modifications des objets persistants pour en faire des évènements.