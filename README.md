# backup_cms

Pour sauvegarder un site web de type CMS (wordpress, drupal, joomla, etc.)

Il y a 2 types de données à récupérer : la base de données et les fichirs utilisateurs



Pour sauvegarder la base de données
Les méthodes de sauvegarde de base de données dépendent du système de gestion de base de données (SGBD). L’exemple qui suit étaye une procédure pour « MySQL », pour les serveurs utilisant « phpMyAdmin » il est possible d’effectuer des sauvegardes manuelles via l’interface GUI.

$ cd /mon/repertoire/de/sauvegarde

#Commande permettant d’extraire la BDD
$ mysqldump -h example.com -u myusername --password=mypassword --default-character-set=utf8  -C -Q -e --create-options mydatabasename > wordpress-database.sql

#Compression de la BDD
$ gzip wordpress-database.sql

Une fois la première sauvegarde réalisée il est possible d’automatiser cette tâche avec un script/cron :
    • Créer un fichier texte contenant : 

#!/bin/bash
cd /mon/repertoire/de/sauvegarde
mv wordpress-database.sql.gz wordpress-database-old.sql.gz
mysqldump -h example.com -u myusername --password=mypassword --default-character-set=utf8 -C -Q -e --create-options mydatabasename > wordpress-database.sql
gzip wordpress-database.sql
Enregistrer sous le nom « backupbdd.sh » dans le répertoire « home » par exemple.
La fréquence des sauvegardes peut se faire par heure/jour/semaine/mois (cron.hourly, cron.daily, cron.weekly, cron.monthly) 

Ici est traité l’exemple des sauvegardes hebdomadaires.
cd ~ 
mv backupbdd /etc/cron.weekly
chmod 755 backupbdd.sh

Attention à l’encodage des caractères, il est indispensable de s’assurer que la sauvegarde utilise la norme UTF-8.


Outil permettant de sauvegarder une base de données en interface graphique : phmyadmin
