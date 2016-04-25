# High-performance Hibernate

Présenté par : Vlad Mihalcea (de Red Hat)

Le temps pris par une requête vient des différents composants logiciels impliqués et des temps réseau, notamment :
* acquisition de la connexion
* envoi de la requête au serveur
* exécution de la requête
* récupération du résultat
* libération de la connexion

Les optimisations proposées ici ont été validées sur des *benchmarks*.

## Les connexions

On peut améliorer les performances en libérant la connexion à la fin de chaque transaction (et pas après chaque *statement* comme le fait JTA).

## Les identifiants des entités

La génération par table n'est pas la meilleure car pas assez optimisée par le SGBD.

Parmi les générateurs par table, le *hi-lo* est le moins bon (*pooled* et *pooled-lo* sont meilleurs).

## Les associations entre entités

Il est très important de bien configurer le *fetch* des associations : ainsi, on préférera le *lazy* au niveau modèle et une surcharge *eager* spécifiée association par association au moment de la requête.

Le type des attributs d'associations a également un impact sur les performances. Par exemple, pour une association `@ManyToMany`, le type `java.util.Set` est meilleur que le type `java.util.List`.

## Le *batching* JDBC

Il s'agit de comment Hibernate envoie par lot les ordres SQL au serveur.

Cela est configuré par l'option `hibernate.jdbc.batch_size`, et il y a une valeur seuil au-delà de laquelle il n'y a plus de gain sensible de performance.

Mettre à `true` les options `hibernate.order_inserts` et `....order_updates` semble également améliorer les performances.

## Le *fetching*

En général, c'est le *fetching* qui grève le plus les performances.

### Le JDBC *fetch size*

Le résultat d'une requête peut être récupéré depuis la BD en un ou plusieurs allers-retours serveur. C'est le *fetch size* (à ne pas confondre avec la taille du `ResultSet`).

C'est l'option `hibernate.jdbc.fetch_size` qui définit le nombre de lignes à récupérer à chaque aller-retour. La valeur par défaut dépend du *driver* JDBC utilisé. Par défaut, le *driver* PostgreSQL récupère toutes les lignes du résultat (taille du `ResultSet`), mais le *driver* Oracle récupère les lignes 10 par 10.

En terme de performances, la stratégie de PostgreSQL est bien meilleure et les utilisateurs d'Oracle ne devraient pas utiliser la valeur par défaut.

A noter : cette option est également modifiable pour chaque chaque requête (avec la méthode `setFetchSize`).

### La taille du `ResultSet`

On cherchera à limiter le nombre de lignes à récupérer depuis la BD en modifiant les options de pagination des requêtes (cf. méthodes `setFirstResult(int)` et `setMaxResult(int)`).

### Le nombre de colonnes récupérées

Il faut aussi limiter le nombre de colonnes récupérées et utiliser pour cela l'API [Projection](http://docs.jboss.org/hibernate/orm/5.1/javadocs/org/hibernate/criterion/Projection.html).

## Le cache

Il faut choisir la stratégie adaptée aux besoins, mais en général :
* *Transactional* (pour JTA) est assez mauvais
* *Read-Write* est assez bon

Dans tous les cas, un cache géré au niveau applicatif pour les entités qui ne changent jamais sera toujours plus performant que le cache de niveau 2 d'Hibernate.
