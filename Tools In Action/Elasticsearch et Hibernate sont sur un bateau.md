# Elasticsearch et Hibernate sont sur un bateau...

Présenté par : Emmanuel Bernard (de Red Hat) et David Pilato (d'Elastic)

On essaie de bénéficier des fonctionnalités d'[ElasticSearch](https://www.elastic.co/) (ES) avec un modèle persistent [Hibernate ORM](http://hibernate.org/orm/).

## Hier

On part d'un test unitaire contenant des tests avec des requêtes sur des entités Hibernate.

On ajoute un client ElasticSearch qui se connecte à un serveur local.

A chaque modification d'une entité, on doit ajouter ou mettre à jour la version JSON de l'objet Java dans ES. On utilise notamment [Jackson](https://github.com/FasterXML/jackson) pour sérialiser l'objet persistant en JSON (on a d'ailleurs dû personnaliser les annotations Jackson pour éviter les cycles dans le modèle).

On peut alors utiliser le client pour requêter ES et récupérer un objet Java (en le désérialisant avec Jackson). On aura besoin d'un `getObjectById` pour reconnecter l'objet à un `EntityManager` Hibernate.

Cela fait beaucoup de "plomberie"... Et encore, on aura davantage de problème en essayant de gérer des transactions conjointes Hibernate et ES !

## Aujourd'hui

[Hibernate Search](http://hibernate.org/search/) (HS) remplace le client ES : il se connecte au serveur ES (par Web Service REST) et met automatiquement les index ES en fonction des modifications sur les entités Hibernate.

Il faut pour cela avoir, au préalable, demandé l'indexation avec des annotations sur les entités. HS gère nativement les cycles dans le modèle lors de la sérialisation JSON.

Il faut également déclarer les références vers d'autres entités à réindexer quand un type d'entité est modifiée.

HS fournit un DSL spécifique pour le requêtage de ES.

La requête HS récupère directement les objets persistants (pas besoin d'un appel supplémentaire de `getObjectById`. Bien sûr, il est possible de désactiver ce comportement pour améliorer les performances.

De plus, HS gère nativement les transactions et donne la possibilité de gérer les erreurs d'indexation ES.

## Et après ?

En plus des fonctionnalités natives d'ES, on peut bénéficier de tout son écosystème : un *dashboard* [Kibana](https://www.elastic.co/products/kibana) sur les entités persistantes, par exemple.
