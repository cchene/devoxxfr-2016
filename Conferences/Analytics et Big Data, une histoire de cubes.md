# Analytics et Big Data, une histoire de cubes...

Présenté par : Mathias Kluba et Mehdi Ben Haj Abbes (SGCIB)

Ce retour d'expérience de la [Société Générale](https://www.societegenerale.fr/) a pour objet de montrer l'évolution de l'architecture adoptée pour la mise en place d'[analytique](https://fr.wikipedia.org/wiki/Analytique_%28recherche%29) sur du [*big data*](https://fr.wikipedia.org/wiki/Big_data).

## La version basique

On commence par faire du simple SQL sur une base de données comme [PostgreSQL](http://www.postgresql.org/).

## Quand le volume augmente...

Avec le volume, les performances de la version basique se dégradent. Il faut alors adapter le stockage de la base de données.

En s'appuyant sur [Apache Hadoop Distributed File System](http://hadoop.apache.org/), [Apache Hive](http://hive.apache.org/) permet de distribuer l'exécution des requêtes SQL.

On améliore encore les performances en adoptant un autre format de base de données : le [stockage orienté colonne](https://fr.wikipedia.org/wiki/Base_de_donn%C3%A9es_orient%C3%A9e_colonnes). En effet, contrairement aux usages classiques d'une base de données (on récupère toutes les colonnes de quelques lignes), l'analytique récupère plus souvent quelques colonnes de toutes les lignes.

On complète cela avec un pré-calcul (en mode *batch*) de cubes [OLAP](https://fr.wikipedia.org/wiki/Traitement_analytique_en_ligne).

Les solutions [Oracle](https://www.oracle.com/) et [Microsoft](http://www.microsoft.com/) s'appuient sur *Hive*, mais les performances se dégradent quand les volumes deviennent vraiment trop importants.

On peut aussi choisir de stocker les cubes OLAP dans une base de données [NoSQL](https://fr.wikipedia.org/wiki/NoSQL) [Apache HBase](http://hbase.apache.org/), alimentée par [Apache Spark](http://spark.apache.org/).

Les performances sont correctes tant que le nombre de dimension des cubes reste limité (moins de 20). Au-delà, le temps de calcul et le volume de données du cube deviennent trop importants.

## Quand le volume augmente encore...

On reste avec une base *Hadoop*+*HBase* et avec des traitements de type *batch*. Mais on s'appuie sur [Apache Kylin](http://kylin.apache.org/) qui optimise le calcul des cubes.

*Kylin* propose également une IHM Web d'administration des cubes, ce qui permet au personnel métier de concevoir lui-même les requêtes.

## Quand le calcul *batch* n'est pas adapté...

Le cube doit être calculé en temps réel, c'est la version *full streaming*. Pour cela, on utilise une architecture [Kappa](http://milinda.pathirage.org/kappa-architecture.com/).

C'est alors un [Apache Kafka](http://kafka.apache.org/) qui alimente un *Spark* qui alimente à son tour un *HBase* (qui sera requêté par l'utilisateur final).

Cela ne fonctionne que pour un nombre limité de dimensions.

## L'alliance du *batch* et du temps réel

Parce que les calculs *batch* et temps réel répondent chacun à des besoins différents, on cherche à les utiliser tous les deux. On utilise [Druid](http://druid.io) pour allier les deux.

Les données récentes calculées en temps réel restent "en mémoire" (pour un accès immédiat), les données anciennes sont stockées dans un *Hadoop DFS*. Un *broker* est là pour orienter les requêtes de l'utilisateur entre ces deux stockages, en fonction de l'âge des données requêtées.

Les données calculées en *batch* (parce que trop lourdes pour le temps réel) sont directement stockées dans *Hadoop FS*.

Il n'y a pas de *dashboard* fourni avec *Druid*, mais il en existe des tiers : [Grafana](http://grafana.org/), [Implydata Pivot](https://github.com/implydata/pivot), [Pulsar](http://gopulsar.io/).

## Conclusion personnelle

L'architecture à utiliser dépend tout autant des volumes de données que de ce qu'on veut en faire, et donc des cas d'utilisations, des besoins des utilisateurs finaux de la solution.

Une analyse très fine de ces points est absolument nécessaire.
