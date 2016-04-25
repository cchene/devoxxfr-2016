# How to stop wasting your time and start performing useful code reviews

Présenté par : Maria Khalusova (de JetBrains)

La *speakerine* présente ici un ensemble de bonnes pratiques pour introduire les revues de code dans la méthode de travail, et éviter les pièges inhérents à cette pratique.

Note : Je n'ai pas repris tous les éléments de sa présentation.

## Bénéfices attendus

* Trouver des anomalies dont la détection ne peut pas être automatisée.
* Partager la connaissance du projet et des pratiques de l'équipe.
* Rendre le code plus maintenable.

Dans tous les cas, les revues de code ne remplace par la QA habituelle (tests, audits statiques, etc.) : elle ne fait que la compléter.

## Par où commencer ?

Il faut commencer par l'accompagnement au changement : expliquer le pourquoi et le comment à tous les acteurs, c'est-à-dire l'équipe.

Parce qu'on ne peut pas toujours tout relire, il faut déterminer ce qui doit l'être en priorité. Par exemple, on pourra se concentrer sur les nouveaux commits.

Il faut que chaque relecteur ait du temps réservé pour les revues. Par exemple, on pourra réserver un créneau hebdomadaire pour cette tâche.

Le processus de la revue doit rester simple :
* il est itératif (c'est une discussion entre l'auteur et le relecteur)
* il ne doit pas impliquer trop de personnes (mais on pourra aussi combiner les expériences en affectant deux relecteurs, un jeune et un plus expérimenté).

Il faut s'outiller de manière adaptée (c'est-à-dire trouver l'outil qui ne gênera pas le travail habituel).

## Comment optimiser les revues de code ?

### Le manager

Il faut s'assurer que le style de code est partagé par tous. On évitera ainsi les querelles de type : "on avait dit trois espaces, pas des tabulations, pour l'indentation".

### Le codeur

Avant de soumettre son code à la relecture d'un autre, la première chose à faire est... de le relire !

Il est utile de simplifier la tâche du relecteur en :
* limitant l'ampleur des portions de code à relire
* indiquant un message de commit pertinent
* documentant le code autant que nécessaire/utile

### Le relecteur

Il doit s'astreindre à :
* ne pas toujours repousser la relecture
* ne pas y passer trop de temps (par exemple : une heure car il est rarement utile d'y passer plus de temps)

Il faut qu'il se concentre sur :
* les objectifs du projet
  * les performances
  * la sécurité
  * l'ergonomie
* son propre domaine d'expertise
  * la logique métier
  * l'architecture

## Et pendant la revue...

La rédaction d'une revue (par le relecteur) et la réception d'une revue (par le codeur) sont des sources potentielles de conflits. Il est important de porter une attention toute particulière au vocabulaire, à la formulation, etc.
