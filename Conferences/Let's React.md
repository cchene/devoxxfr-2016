# Let's React

Présenté par : Mathieu Ancelin

React est une bibliothèque JS développée par Facebook. Elle ne s'occupe que de la vue, pas de l'architecture globale de l'application.

## Qu'est-ce qu'un composant React ?

Un composant a :
* une méthode de rendu qui produira le DOM
* des propriétés (normalement avec des valeurs immuables, une sorte de configuration)
* des enfants (composants "enfants" dans la structure de la page)
* un état (des valeurs qui vont changer dans le temps)
* un cycle de vie (ex: avant le "montage" dans la vue, après le "montage", avant le "démontage", etc.)
* des règles de validations (équivalent d'assertions activées en développement pour aider au débogage)

Il est décrit avec une syntaxe JSX, mélange de JS et de XML. Quand l'état change, React calcule un *virtual DOM* qui est ensuite comparé avec le DOM réel. A partir des différences, React calcule et applique l'ensemble minimal des changements à apporter au DOM.

Un composant est testable avec une API fournie "de base" qui a le défaut d'être assez technique. Il existe des API de plus haut niveau, telles que [Enzyme par Airbnb](https://github.com/airbnb/enzyme).

## Autour de React

L'écosystème React inclut également des API telles que [React Router](https://github.com/reactjs/react-router), [React Redux](https://github.com/reactjs/react-redux), etc.

Se reporter à [Awesome React](https://github.com/enaqx/awesome-react) pour davantage d'API.

## React Native

React Native remplace la partie HTML de React pour la remplacer par une partie "interface mobile native" (pour les plate-formes Android, iOS et Windows Mobile).

Le principe fondamental est un *bridge* qui convertit les composants React en composants natifs.

Avantages :
* Des compétences JS suffisent pour développer une application mobile native
* On a du *hot reload* en débogage

Inconvénients :
* Chaque plate-forme a ses spécificités et on doit donc de toute façon recoder une partie de l'application pour chaque plate-forme. Cependant, il y a des exemples où 80% de l'appli mobile est commune à toutes les plate-formes.
* On peut pas mettre en commun les composants de React (Web) et de React Native
