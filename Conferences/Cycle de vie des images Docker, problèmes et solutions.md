# Cycle de vie des images Docker : problèmes et solutions

Présenté par : Fred Simon (JFrog)

## Intro

Cette conférence aborde le problème de la distribution d'un conteneur Docker depuis le développement jusqu'à la production, le principal problème étant la confiance à accorder au conteneur.

Il s'agit notamment d'une expérience liée au produit Artifactory de JFrog. Il s'agit d'un dépôt de binaires, capable d'agréger des dépôts distants (github, npm, docker hub, etc.).

Il est notamment utilisé par Oracle pour distribuer WebLogic.

Docker n'intervient normalement qu'à partir des tests d'intégration. En développement, Docker est plus utilisé sur les outils d'intégration continue (pour contrôler l'environnement).

Aparté : Chez JFrog, ils ont noté un gain de temps pour l'intégration continue avec Docker par rapport aux VM.

Un point clé est qu'on cherchera à automatiser la promotion des binaires du développement à la production.

## Docker build à chaque étape

Une première solution envisageable est d'exécuter `docker build` (reconstruction du conteneur à partir de ses sources).

Mais ce n'est pas acceptable car le `Dockerfile` inclut généralement le téléchargement des versions les plus à jour de certains logiciels (ex: avec le `from`, avec les `agt-get install`, etc.). Du coup, on ne maîtrise plus les versions utilisées entre deux *builds*.

On pourrait essayer de forcer les versions de ces logiciels dans le `Dockerfile` mais ce n'est tout simplement pas possible :
* D'une part, le `from` référence un tag Docker qui n'est pas immuable (en effet, il inclut des mises à jour),
* D'autre part, le `Dockerfile` n'est plus lisible, ni maintenable proprement.

On ne peut donc pas reconstruire le conteneur à chaque étape du *pipeline* de mise en production. Ce sont les binaires de conteneurs qui doivent être promus entre les étapes, pas les `Dockerfile` !

## La promotion des conteneurs

Supposons que nous disposions de plusieurs dépôts Docker, un par plate-forme (développement, préproduction, production, etc.)

On utilise les commandes `push` et `pull` de Docker pour déplacer les conteneurs d'un dépôt à un autre.

C'est Jenkins qui s'occupe de la promotion des conteneurs, en fonction du résultat des audits de qualité.

La plus-value de JFrog Artifactory est alors de contrebalancer la limitation de Docker qui n'autorise qu'un seul dépôt par hôte. Si on ne dispose que d'un seul hôte, Artifactory peut simuler plusieurs dépôts (en utilisant un *proxy*). Il propose également la gestion de la visibilité des dépôts et des binaires ainsi que la traçabilité et les autorisations des promotions.
