# Flexbox, et le CSS redevient fun !

Présenté par : Hubert Sablonnière

Le `display: flex` (*a.k.a.* Flexbox) est une nouveauté de CSS qui permet de résoudre les problèmes classiques de mise en page. Ceux qui se résolvent aujourd'hui avec des `float` et des `clear`, alors que ces derniers n'ont pas été prévus pour cela.

Pour appliquer la *flexbox*, on définit un conteneur (ex : un `div`) dont les enfants seront agencés les uns par rapport aux autres :
* Hauteur harmonisée
* Largeur du conteneur répartie entre les enfants selon des ratios
* Alignement horizontal des enfants par rapport au conteneur
* Alignement vertical des enfants par rapport au conteneur
* Retour à la ligne des enfants quand le conteneur est trop petit
    * Les enfants de chaque ligne sont agencés entre eux selon les mêmes règles

Une propriété supplémentaire `flex-direction` permet de sélectionner un axe selon lequel arranger les enfants. Par défaut, c'est `row`, i.e. de gauche à droite. Mais cela peut être `column` (de haut en bas), et même `row-reverse` (de droite à gauche) et `column-reverse` (de bas en haut).

Le support par les navigateurs est assez récent : les dernières versions supportent entièrement cette syntaxe, sauf IE11.

Liens :
* [Flexbugs](https://github.com/philipwalton/flexbugs)
* [Autoprefixer CSS online](https://autoprefixer.github.io/)
