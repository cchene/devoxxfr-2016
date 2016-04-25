# Intégration et déploiement en continu chez GitHub

Présenté par : Alain Hélaïli (de GitHub)

[GitHub](https://github.com/), c'est environ 40 mises en production par jour. Comment ?

## Démarrage du développement d'une fonctionnalité

Au début du développement d'une fonctionnalité, le développeur crée une *feature branch*. Très rapidement, il initie une *pull request* pour amorcer une discussion avec les autres développeurs sur cette fonctionnalité (son contenu, la manière de la réaliser, etc.).

Grâce à cette *pull request*, tous les développeurs peuvent faire des revues de code et proposer des améliorations.

Au final, il ne doit pas y avoir une personne qui soit le propriétaire exclusif de la fonctionnalité.

Étant donnée l'hétérogénéité des utilisateurs de GitHub, il est impossible de vouloir tout tester (trop cher). Une bonne partie des tests d'intégration sont donc effectués par les utilisateurs finaux eux-mêmes, sans le savoir.

Du coup, on peut déployer la *feature branch* directement en production. Il y a évidemment des environnements dédiés aux tests pour les développements les plus sensibles.

## Aparté sur l'outillage : Hubot & Slack

[Hubot](https://hubot.github.com/) est un robot-logiciel configurable. Les développeurs de GitHub communiquent avec lui via [Slack](https://slack.com/). Du coup, la communication est visible par tous les développeurs qui peuvent aider ou faire des remarques.

Par exemple, on peut demander à Hubot de déployer (en production ou dans un environnement de test). Hubot trace le déploiement dans la *pull request* associée à la fonctionnalité.

En fait, Hubot centralise une bonne partie de la connaissance du projet, notamment les procédures (de déploiement, de gestion d'anomalies, etc.). C'est lui également qui connaît les identifiants de connexion aux serveurs, les URL des outils, etc.

## Après la mise en production

Le développeur doit surveiller la production. Il peut être averti par Hubot d'évènements se produisant sur la production (trop d'exceptions, temps de réponse mauvais, etc.). En effet, les serveurs sont *monitorés* et c'est Hubot qui reçoit les notifications. Pour chaque type de notification, Hubot crée un *issue* avec les *logs* utiles (récupérés automatiquement car connus pour aider à résoudre ce type de problème). Il y évidemment une notification envoyée dans Slack.

Si on est pas satisfait du résultat en production, on peut revenir en arrière et demander à Hubot de remettre en production la version précédente. Et quand on est satisfait du résultat en production, on peut *merger* dans le *master*.

## Sécuriser l'environnement de production

Faire une mise en production par Slack, c'est bien joli, mais comment peut-on alors sécuriser cet environnement de production ?

Il y a deux manières de sécuriser les opérations déclenchées via Slack :
* Certaines opérations ne peuvent être démarrées que par certaines *chatrooms* (et donc par un de leurs membres)
* Les opérations les plus sensibles passent par une *two-factor authentication* (confirmation par un code aléatoire envoyé par SMS).
