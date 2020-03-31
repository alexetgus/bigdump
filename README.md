# BigDump

## Importateur de dump MySQL

**Description** : Importation de gros et très gros dumps MySQL (comme les dumps phpMyAdmin) même à travers les serveurs web avec limite d'exécution en dur et ceux en mode sécurisé. Le script n'importe qu'une petite partie de l'énorme dump et redémarre lui-même. La session suivante commence là où la précédente a été arrêtée.

## Crédits

Ce code a été écrit à l'origine par Alexey Ozerov.
Puis, il a été forké par Alex B. et traduit en français.

## Utilisation

1. Téléchargez et dézippez _bigdump.zip_ sur votre PC.
2. Ouvrez _bigdump.php_ dans un éditeur de texte, ajustez la configuration de la base de données et spécifiez l'encodage des fichiers.
3. Retirez les anciennes tables sur la base de données cible si votre dump ne contient pas "DROP TABLE" (utilisez phpMyAdmin).
4. Créez le répertoire de travail (par exemple /dump) sur votre serveur web
5. Téléchargez _bigdump.php_ et les fichiers dump (*.sql ou *.gz) par FTP dans le répertoire de travail (attention au mode **TEXT** pour _bigdump.php_ et _dump.sql_ mais au mode **BINARY** pour _dump.gz_ si vous téléchargez depuis MS Windows).
6. Exécutez le fichier _bigdump.php_ à partir de votre navigateur web via une URL comme _http://www.yourdomain.com/dump/bigdump.php_.
7. Vous pouvez maintenant sélectionner le fichier à importer dans la liste de votre répertoire de travail. Cliquez sur "Démarrer l'importation" pour commencer.
8. BigDump démarrera automatiquement chaque session d'importation suivante si JavaScript est activé dans votre navigateur.
9. Détendez-vous et attendez que le script se termine. Ne fermez PAS la fenêtre du navigateur !
10. **IMPORTANT** : Supprimez _bigdump.php_ et vos fichiers de dump de votre serveur web.

## Notes avancées

**Note 1 :** BigDump ne pourra pas traiter de grands tableaux contenant des insertions étendues. Un insert étendu contient toutes les entrées de la table dans une seule requête SQL. BigDump n'est pas capable de fractionner de telles requêtes SQL. Dans la plupart des cas, BigDump s'arrêtera si une requête comprend trop de lignes. Mais si PHP se plaint que la taille mémoire autorisée est épuisée ou que le serveur MySQL a disparu, votre dump contient probablement aussi des insertions étendues. **Veuillez désactiver les insertions étendues lorsque vous exportez une base de données depuis phpMyAdmin**.

**Note 2 :** Si vous souhaitez télécharger les fichiers de dump via un navigateur web, donnez aux scripts les droits d'écriture sur le répertoire de travail (par exemple chmod 777 sur un système basé sur Linux). Vous pouvez télécharger les fichiers de dump depuis le navigateur jusqu'à la taille limite fixée par la configuration PHP actuelle du serveur web. Vous pouvez également télécharger tous les fichiers par FTP. Certains serveurs web interdisent l'exécution de scripts dans les répertoire avec des droits d'écriture pour des raisons de sécurité. Si vous avez modifié les permissions sur le répertoire de travail et que vous obtenez une erreur de serveur lors de l'exécution du script, rétablissez les permissions à leur état normal (chmod 755) pour les répertoires.

**Note 3 :** Si des erreurs de Timeout se produisent encore, vous devrez peut-être ajuster le paramètre **$linespersession** dans _bigdump.php_.

**Note 4 :** Si le dépassement du serveur MySQL se produit en raison de la restriction **max_requests**, vous pouvez utiliser le paramètre **$delaypersession** pour laisser le script en veille quelques millisecondes ou plus avant de démarrer la session suivante. **_Ce paramètre ne fonctionnera que si JavaScript est activé_**. L'importation de fichiers de dump sur de tels serveurs peut prendre beaucoup de temps.

**Note 5 :** BigDump n'est actuellement pas capable de restaurer un seul fichier de dump avec plusieurs bases de données à l'intérieur (commutées par la déclaration USE). BigDump n'est pas non plus en mesure de restaurer une seule base de données spécifique à partir du fichier de dump contenant plusieurs bases de données.

**Note 6 :** Si vous rencontrez des problèmes avec des caractères non-latins lors de l'utilisation de BigDump, vous devez ajuster la variable de configuration **$db_connection_char_set** dans _bigdump.php_ pour qu'elle corresponde à l'encodage de votre fichier de dump.

**Note 7 :** Le support de GZip n'est disponible qu'avec PHP 4.3.0 et plus. L'utilisation d'un énorme fichier de dump compressé avec GZip peut entraîner le dépassement de la limite de mémoire/temps d'exécution de PHP, car le fichier de dump doit être décompressé dès le début à chaque fois que la session démarre. Si cela se produit, utilisez le fichier de dump non compressé. C'est votre seule chance.

**Note 8 :** Ce n'est pas une très bonne idée, mais vous pouvez également importer un fichier CSV dans une table mySQL en utilisant Bigdump. Vous devez spécifier le nom de la table dans **$csv_insert_table**. Veuillez également vérifier les autres paramètres CSV dans la configuration de Bigdump.

Cette contribution a-t-elle été utile ? Recommandez-la à un ami !
