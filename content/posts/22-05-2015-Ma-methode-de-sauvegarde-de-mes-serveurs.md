+++
author = "Maxence Maireaux"
title = "Ma méthode de sauvegarde de mes serveurs"
date = "2015-05-22"
tags = [
"french",
]
+++

![Photo backup](/images/backup-1.jpg)

Il y a quelque semaine j’ai présenté le service DataCube.io qui vous permet de faire des sauvegardes de votre serveur et de les envoyer sur un cloud storage, cependant la solution a un inconvénient de taille pour moi. Elle n’est pas gratuite et pas libre. Ainsi je vous propose une autre solution qui réalisera une sauvegarde

alors j’ai commencé à vouloir utiliser le service de [Scaleway](https://www.scaleway.com/) pour stocker mes backups quotidiens dans le cloud en plus du stockage sur le serveur en local.

Le script a un fonctionnement assez basique, il vous faudra cependant installer s3cmd pour pouvoir envoyer les backups directement sur Storage cloud.

L’installation de S3cmd est vraiment très simple vous pouvez suivre la procédure d’installation sur [la documentation de scaleway](https://www.scaleway.com/docs/object-storage-with-s3cmd/).

Vous trouverez mon script en bas de cet article.

Hésiter pas a me faire un retour au moindre problème, je vous aiderais avec grand plaisir 🙂

{% highlight bash %}
#!/bin/bash
# Auteur : Maxence Maireaux
# Date : 28/04/2015
# Version : 1.2

#############
# Variables #
#############

# Binaire
mkdir_bin="/usr/bin/mkdir"
mysqldump_bin="/usr/bin/mysqldump"
mysql_bin="/usr/bin/mysql"
gzip_bin="/usr/bin/gzip"
tar_bin="/usr/bin/tar"

format_date='%d%m%Y'
date=`date +${format_date}`

dir_backup_base="/backup"
dir_backup="/backup/${date}"
dir_backup_databases="/backup/${date}/databases"
dir_backup_site="/backup/${date}/sites"
dir_sites="/home/sites_web"


##########
# Script #
##########

# Création des dossiers de backups + site
${mkdir_bin} -p ${dir_backup_databases}
${mkdir_bin} -p ${dir_backup_site}

# Backup des bases de données
for database_list in `${mysql_bin} -ss -e "SHOW DATABASES" | egrep -vi "^(information_schema|performance_schema)$"`; do
 ${mysqldump_bin} --events --opt -Q -B ${database_list} | ${gzip_bin} -c > ${dir_backup_databases}/backup_${database_list}.sql.gz
done

# Backup des fichiers du sites

cd ${dir_sites}
for site_list in `ls`; do
 tar czf ${dir_backup_site}/backup_${site_list}.tar.gz ${site_list};
done

# Réalisation d'un seul fichiers
cd ${dir_backup_base}
${tar_bin} czf ${dir_backup_base}/backup_${date}.tar.gz ${dir_backup}

# Envoie du backup sur Scaleway
s3cmd put /backup/backup_${date}.tar.gz s3://backup-flemzord-fx
{% endhighlight %}
