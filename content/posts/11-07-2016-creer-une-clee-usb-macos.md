+++
author = "Maxence Maireaux"
title = "Créer une clée USB amorçable pour OS X"
date = "2016-07-11"
tags = [
"french",
]
+++

Quand on souhaite réinstaller un Mac, nous avons besoin de crée une clée USB bootable (amorçable). Nous pouvions le faire avec la commande dd dans le terminal, mais il n'est pas facile de se souvenir de chaque argument. Depuis Mavericks, Apple a sortie un utilitaire "createinstallmedia".

````
createinstallmedia --volume volumepath --applicationpath installerpath
````

# Exemple

## Etape 1
Il faut télécharger le programme d'installation d'OSX depuis le Mac App Store. Puis ouvrir le terminal.

## Etape 2
El Capitan:

````
sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/MonVolume --applicationpath /Applications/Install\ OS\ X\ El\ Capitan.app
````
