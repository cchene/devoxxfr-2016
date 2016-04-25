# Gradle : harder, better, faster, stronger

Présenté par : [Andres Almiray](http://jroller.com/aalmiray)

Gradle, c'est comme Maven mais :
* Exécution du *build* plus rapide
* un DSL basé sur Groovy et donc moins verbeux que XML
* Pas besoin d'installer Gradle dans le serveur d'intégration grâce au *gradle wrapper*)
* Plus souple pour l'organisation du projet et du *build*
* Plus récent, donc communauté moins importante et moins de *plugins*
* Plus de documentation

Ant+Ivy en forte perte de vitesse. Maven en progression continue. Gradle en progression forte.

## Quelques bons points pour Gradle

Avant de démarrer une tâche, Gradle vérifie que son *input* a été modifié (*up-to-date check*).

Le cycle de vie du projet est configurable/modifiable.

Le *Gradle Daemon* qui accélère le *build* en conservant la JVM démarrée entre deux *builds*.

Le développement de nouveaux *plugins* est plus facile.

Le *build* de projets qui dépendent les uns des autres est plus simple : pas besoin de passer une étape de publication des dépendances dans un dépôt local (avec `mvn install`).

## Quelques astuces

Utiliser [SdkMan.io](http://sdkman.io/) pour installer Gradle.

Utiliser [Lazybones](https://github.com/pledbrook/lazybones) pour remplacer les *archetypes* de Maven.