+++
author = "Maxence Maireaux"
title = "Utiliser Amazon Drive en ligne de commande"
date = "2016-09-06"
tags = [
    "french",
]
+++

Avec la présentation de Amazon Drive et surtout leur "Stockage illimité".
Je me suis demandé pourquoi ne pas envoyer mes backups dessus ? Et ainsi pouvoir les archivers sans avoir un coût excessif.

J'ai trouvé un outil ([acd_cli](https://github.com/yadayada/acd_cli)) qui me permetterais d'envoyer mes backups en CLI sans devoir synchroniser l'ensemble des données (comme un RSYNC ou SCP).
acd_cli permet aussi de monter notre Amazon Drive comme un filesystem via fuse.

## Configuration

Vous devez avoir Python 3 et pip 3 sur votre serveur / machine, puis faire l'installation via pip3.

```
pip3 install --upgrade --pre acdcli
```

Une fois que celui-ci est installé vous devez initialiser la connexion via oauth.

```
acd_cli init
```

Une fois le fichier oauth_data récupérer vous devez le mettre a la racine de votre $HOME dans mon cas :

```
/home/flemzord/.cache/acd_cli/oauth_data
```

Il ne vous reste plus cas faire une synchronisation avec la commande suivante :
```
acd_cli sync
```

 Et voila, vous pouvez utiliser la ligne de commande pour uploader des fichiers etc

## Envoyer des fichiers sur Amazon Drive en ligne de commande

```
acd_cli upload VOTRE_FICHIER /
```

## Utiliser Amazon Drive comme filesystem

Si vous souhaitez utiliser votre Amazon Drive en filesystem sur votre serveur ou autre, vous devez au préalable avoir fuse d'installer , puis exéctuer la commande suivante :

```
mkdir /data/AmazonDrive && acd_cli mount /data/AmazonDrive
```

Et voila, vous pouvez accéder a votre dossier /data/AmazonDrive directement et ainsi pouvoir faire des cp/mv etc.
Les performances de celui-ci ne sont pas trop mauvaise, cependant attention a la consommation CPU (fuse oblige ...).
![Photo ligne de commande](/images/amazon-drive-acd_cli-fuse.png)
