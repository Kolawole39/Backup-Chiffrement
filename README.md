# Backup-Chiffrement

Ce script permet de réaliser des sauvegardes incrémentales d’un système de fichiers Unix local ou distant. Les fichiers sont identiques d’une sauvegarde à l’autre ne sont pas dupliqués. Ils sont liées entre eux par leur inodes minimisant ainsi l’espace disque nécessaire.
Il permet également de sauvegarder des bases de données MySQL.
Il s’appuie sur rsync pour la copie des fichiers et sur ssh pour réaliser la sauvegarde des bases de données.
La configuration est contenue dans le fichier configuration.txt qui défini les variables suivantes :
oREMOTE_HOST : si vous souhaitez faire une sauvegarde sur un site distant, préciser l’adresse sous la forme login@host, sinon pour faire une sauvegarde locale, mettez localhost,
oWHAT_TO_SAVE : la liste des répertoires ou des fichiers à sauvegarder, séparés par des espaces,
oDESTINATION : le répertoire où seront copiés les fichiers,
oEXCLUSION : un fichier contenant une liste de fichiers à exclures,
oDATABASE_OPTIONS : les options nécessaires pour sauvegarder vos bases MySQL (à minima, probablement -u login -p password) ou NONE si vous ne souhaitez pas de sauvegarde.
Attention au format, il ne doit pas y avoir d’espaces entre le nom de variable, sa valeur et le signe =. Voici un exemple de fichier de configuration :
REMOTE_HOST=root@xael.org
WHAT_TO_SAVE=/usr/local/ /etc/ /zBackups/ /home/
DESTINATION=/BigDisk/Backups/xael.org/vps
EXCLUSION=/BigDisk/Backups/xael.org/vps/exclusions.txt
DATABASE_OPTIONS=NONE
Utilisation
Pour utiliser le script, il suffit de lancer la commande suivante :
make -f Makefile_4_Backup backup
Le script va alors créer un répertoire backup nommé selon la date et l’heure de lancement (exemple : backup_20151106-224537). Une fois la sauvegarde terminée, il va également créer un lien symbolique current vers la dernière sauvegarde utilisée.
drwxr-xr-x 6 root root 4096 nov.   6 22:46 backup_20151106-224537
lrwxrwxrwx 1 root root   52 nov.   6 22:47 current -> /BigDisk/Backups/xael.org/vps/backup_2015110
