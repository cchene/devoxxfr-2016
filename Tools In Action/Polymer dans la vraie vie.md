# Polymer dans la vraie vie

Présenté par : Horacio Gonzalez

Après le [Hands on Web Components with Polymer](../Hands On Labs/Hands on Web Components with Polymer.md), retour d'expérience sur un projet fait avec Polymer : [Warp 10 PLatform](http://www.warp10.io/).

Il faut d'abord noter Polymer permet de faire aussi bien des Web Components qu'une application complète.

Cette conférence a lever beaucoup d'interrogations sur la concurrence avec d'autres *frameworks* poussés par Google tels qu'AngularJS et GWT. La réponse du conférencier est que chacun de ces *frameworks* peut utiliser des Web Components. Mais cela ne lève pas le doute sur la partie de Polymer qui adresse l'architecture complète d'une application Web.

Ce qui suit est la liste des bons points relevés par le conférencier.

## Intégration d'une bibliothèque (JS) tierce

Cette intégration est largement simplifiée par les Web Components de Polymer.

## Portabilité

Grâce à Polyfill, le support des navigateurs est assez complet. Il est même natif dans Chrome, ce qui assure de très bonnes performances.

Cependant, il faut noter que le support dans les autres navigateurs (grâce à Polyfill) est assez lent. Et c'est surtout l'import de documents HTML qui pose problème. Notamment, Mozilla n'est pas encore convaincu que cet import soit la bonne solution à terme.

Pour limiter le ralentissement, il faut utiliser l'outil Vulcanize, qui permet de fusionner tous les imports dans le document HTML principal (c'est une sorte de minification). L'inconvénient, c'est que la page HTML devient alors énorme, et on perd la possibilité d'un chargement *lazy*. La solution proposée par Polymer, c'est alors le *web-component-shard*.

## Routage

Polymer propose désormais des briques techniques de routage (entre les pages d'une application). C'est là que se trouve le principal point de collision avec les autres *frameworks* d'application Web.

## Questions & conclusion

* Comment tester ? Polymer ne propose pas de solution pour faciliter les tests unitaires.
* La communauté autour de Polymer ? Même si elle n'inclut pas que des gens de Google, elle est assez réduite. Cependant, elle serait assez active.

A mon avis, en conclusion, la partie Web Components de Polymer est très intéressant et l'architecture orientée composant proposée semble pertinente. Cependant, le manque de support risque de freiner son adoption. Et la partie de Polymer qui s'attache à l'architecture complète d'une application Web, le mettant en concurrence avec d'autres *frameworks*, risque de le mettre en difficulté.
