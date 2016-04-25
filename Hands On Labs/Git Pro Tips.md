# Git Pro Tips

Présenté par : [Christophe Porteneuve](http://www.git-attitude.fr/), présentation disponible [en ligne](http://bit.ly/devoxx-git).

## Interfaces utilisateur ?

Le Git Bash, évidemment. Pour les interfaces graphiques : [Atlassian SourceTree](https://www.sourcetreeapp.com/)

## Objectifs & philo

Surtout, ne pas appliquer les recettes SVN/CVS !

Il existe deux types de configs Git :
* Niveau *projet*
* Niveau *poste de travail*

Il faut évidemment privilégier le premier niveau pour obtenir une bonne cohérence au sein de l'équipe.

### Commits

#### Améliorer le *diff*
Quelques options :
* `mnemonixPrefix` : ajoute un préfixe `i/w/c` pour identifier l'emplacement de la copie du fichier (*stage*, *working dir*, *commit*)
* avec `wordRegex` : indiquer l'unité à analyser lors du diff
* avec `-w` : pour ignorer les espaces
* `diff --stat HEAD~200`
* `diff --dirstat HEAD~200`

#### Ignorer des fichiers

Ne conserver qu'un seul fichier `.gitignore` à la racine du projet pour qu'il soit partagé par tous les développeurs.

On peut le générer avec [GitIgnore.io](https://gitignore.io).

#### La commande `git add`

Ne fonctionne pas du tout comme le `svn add`: on ne fait pas qu'ajouter un fichier à l'ensemble des fichiers à committer. En fait, on inclut une différence au prochain commit (on la met en *staging*).

Le *stage* contient tous les fichiers indexés. Le `git status` ne montre que les différences entre les fichiers à committer et les fichiers committés précédemment.

* `git add` --> mise en *staging*
* `git add -p` --> staging "partiel" d'un sous-ensemble des modifs du fichier, à suivre par un commit
    * permet de faire plusieurs modifs dans un fichier sans les committer toutes en même temps.
       * par défaut, séparation par ligne. possibilité par caractère.
       * disponible dans SourceTree

### Utiliser `git log`

L'option `follow` permet de suivre les renommages/déplacements.

`git blame` permet de connaître le dernier committeur d'une ligne, mais cela n'est souvent pas suffisant : car on recherche parfois celui qui a supprimé une ligne.

Préférer donc `git log -S` ou `git log -G` ou surtout `git log -L` (historique d'une portion de code, identifiée par un nom de fonction ou par des numéros de lignes !).

A noter : `git reflog` : historique des opérations git **locales**, ce qui n'est pas l'historique d'une branche. On y retrouvera tout, y compris les erreurs de commits qui ont été corrigées.

### Nettoyer une branche avec `rebase`

Objectif : conserver des branches lisibles, compréhensibles.

Cela peut être fait au fil de l'eau. Au pire, cela **doit** être avant un *push*.

Pour cela, on fera *rebase* interactif :
* `pick` : Conserver un commit
* `reword` : Modifier le message d'un commit
* `edit` : Découper un commit en plusieurs
    * Utiliser le `reset -p` qui modifie le *stage* sans toucher au *working dir*
* `squash/fixup` : Fusionner des commits adjacents
* `drop` : Supprimer un commit
* Réordonner les commits

Forcément, tout n'est pas possible (ex : supprimer le commit de création d'un fichier mais conserver un commit de modification du même fichier).

Ce *rebase* est aussi possible après le *push*, mais plus délicat (ex : supprimer un commit à partir duquel un collaborateur a créé une branche).

### Push

Par défaut, cette commande ne *pushe* que la branche courante **si** il y a une branche distante de même nom. Et elle ne *pushe* pas les tags.

Les options suivantes permettent d'éviter ces inconvénients :
* `push.default upstream` : *pusher* la branche même si elle n'existe pas
* `push.followTags true` : *pusher* les tags

### Pull

L'option `pull.rebase preserve` permet d'éviter la création de *fausses* branches issues du travail collaboratif sur une même branche. Cela nuirait en effet à la lisibilité des branches.

L'option `fetch.prune false` désactive le nettoyage auto des branches supprimées par erreur sur le dépôt distant. N'étant pas à l'abri d'erreurs, mieux vaut conserver localement les branches distantes qui auraient pu être supprimées.
