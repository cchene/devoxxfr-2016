# Progressive Web Apps

Présenté par : Cyril Balit (SFEIR) et Florian Orpelière (SFEIR)

Les trois solutions connues pour proposer une application sur dispositif mobile sont : le natif, le web et l'hybride.

L'inconvénient d'une application mobile est, qu'entre le nombre de gens qui apprennent l'existence de l'application et le nombre de ceux qui l'installent réellement, le taux de conversion est assez faible (notamment à cause des nombreux intermédiaires, notamment le *store* de l'OS du téléphone).

Les *Progressive Web Apps* (PWA) se proposent de résoudre ce problème.

Une PWA, c'est une application Web qui est capable de se comporter comme une application quand l'utilisateur le souhaite. Il s'agit donc de, tout en conservant les fonctionnalités disponibles sur le Web, d'y ajouter les fonctionnalités qui en feront une application mobile.

Les caractéristiques d'une PWA sont :
* *Responsive*
* *Offline* si nécessaire
* Sécurisée
* Installable
* Fonctionne dans le navigateur
* Accessible par un lien Web classique

Ce qui suit est l'application des préceptes PWA à une application Web utilisant [AngularJS](https://angularjs.org/) et [Material Design](https://fr.wikipedia.org/wiki/Material_Design).

## *Responsive*

L'application d'origine était déjà *responsive*.

## Sécurité

L'application est déployée en HTTPS sur un fournisseur de *Cloud*. Pour un hébergement privé, il faudra envisager [Let's Encrypt](https://letsencrypt.org/).

## Performance

L'utilisateur n'a évidemment pas les mêmes attentes pour une application Web ou une application native. Il y a diverses recettes applicables pour limiter la dégradation des performances pour une application Web.

On peut par exemple charger "normalement" les CSS les plus importants (ceux qui permettent à l'utilisateur d'avoir un *feedback* visuel de l'application en train de se charger), et charger les CSS moins important en mode *lazy* (avec [LoadCSS](https://github.com/filamentgroup/loadCSS/) par exemple).

## *Offline*

HTML 5 propose l'"Application Cache", mais d'après les auteurs, cela fonctionne mal et est difficile à déboguer.

En fait, le mode déconnecté est plus que juste *online* et *offline* : il y a aussi le mode *online* dégradé (connexion lente). Une PWA doit être capable de gérer les trois situations en proposant un contenu à l'utilisateur dans tous les cas.

Pour cela, on utilisera le *Service Worker* (SW) pour exécuter un proxy Web côté client, dans le navigateur.

Support du *Service Worker* :
* Firefox : OK
* Chrome : OK
* IE : à venir ?
* Safari : pas près de venir !

Le SW est codé en JavaScript et utilise l'API `caches`.

Par exemple, lors de l'installation, on crée un cache statique qui pourra servir les ressources Web qui ne changent jamais et qui pourront servir quel que soit le niveau de connectivité. Au fur et à mesure des utilisations, le SW proxy recherche les URL demandées sur le réseau. Si la ressource est trouvée sur le réseau, alors elle est mise dans un cache dynamique et servie au client. Sinon, alors on recherche cette ressource dans le cache dynamique. Se reporter au livre [*Offline cookbook* de Jake Archibald](https://jakearchibald.com/2014/offline-cookbook/) pour davantage de détails.

Note : Aujourd'hui, pas de gestion de quotas de stockage dans les caches.

## Installable

Le W3C propose le standard [*Web App Manifest*](http://www.w3.org/TR/appmanifest/) qui décrit l'application et permet à l'utilisateur d'installer un site Web comme une application.

On peut ainsi, par exemple, proposer à l'utilisateur d'installer l'application si on détecte deux interactions en moins de 5 minutes.

## Outillage et APIs

Il y essentiellement les *Chrome dev tools*. On trouvera aussi des API JS pour simplifier les manipulations du SW et des caches : [Polymer](https://www.polymer-project.org/), [NPM IDB](https://www.npmjs.com/package/idb).
 
## Conclusion personnelle

Du fait de l'absence de support iOS, les PWA ne peuvent pas (encore) remplacer les applications natives, web, hybrides qu'on connaît aujourd'hui.
