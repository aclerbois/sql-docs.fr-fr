---
title: Sauvegarde et restauration des bases de données SQL Server sur Linux | Documents Microsoft
description: Découvrez comment sauvegarder et restaurer des bases de données SQL Server sur Linux.
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 11/14/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: d30090fb-889f-466e-b793-5f284fccc4e6
ms.openlocfilehash: 6e4699fb2ffadc3aaad73c4032073ff8567c856c
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="backup-and-restore-sql-server-databases-on-linux"></a>Sauvegarde et restauration de bases de données SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Pour effectuer des sauvegardes de bases de données à partir de 2017 du serveur SQL sur Linux avec les mêmes outils que les autres plates-formes. Sur un serveur Linux, vous pouvez utiliser **sqlcmd** pour vous connecter à SQL Server et effectuer des sauvegardes. À partir de Windows, vous pouvez vous connecter à SQL Server sur Linux et effectuer des sauvegardes avec l’interface utilisateur. La fonctionnalité de sauvegarde est le même sur plusieurs plateformes. Par exemple, vous pouvez sauvegarder les bases de données localement, aux lecteurs à distance ou aux [service de stockage d’objets Blob Microsoft Azure](../relational-databases/backup-restore/sql-server-backup-to-url.md).

## <a name="backup-a-database"></a>Sauvegarde une base de données

Dans l’exemple suivant **sqlcmd** se connecte à l’instance locale de SQL Server et prend un complet sauvegarde de base de données utilisateur appelée `demodb`.

```bash
sqlcmd -S localhost -U SA -Q "BACKUP DATABASE [demodb] TO DISK = N'/var/opt/mssql/data/demodb.bak' WITH NOFORMAT, NOINIT, NAME = 'demodb-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
```

Lorsque vous exécutez la commande, SQL Server demande un mot de passe. Une fois que vous entrez le mot de passe, l’interpréteur de commandes retournera les résultats de la sauvegarde en cours. Par exemple :

```
Password:
10 percent processed.
21 percent processed.
32 percent processed.
40 percent processed.
51 percent processed.
61 percent processed.
72 percent processed.
80 percent processed.
91 percent processed.
Processed 296 pages for database 'demodb', file 'demodb' on file 1.
100 percent processed.
Processed 2 pages for database 'demodb', file 'demodb_log' on file 1.
BACKUP DATABASE successfully processed 298 pages in 0.064 seconds (36.376 MB/sec).
```

### <a name="backup-the-transaction-log"></a>Sauvegarde le journal des transactions

Si votre base de données est en mode de récupération complète, vous pouvez également rendre les sauvegardes de journaux de transactions pour les options de restauration plus granulaires. Dans l’exemple suivant, **sqlcmd** se connecte à l’instance locale de SQL Server et prend la sauvegarde de journal de transactions.

```bash
sqlcmd -S localhost -U SA -Q "BACKUP LOG [demodb] TO DISK = N'/var/opt/mssql/data/demodb_LogBackup.bak' WITH NOFORMAT, NOINIT, NAME = N'demodb_LogBackup', NOSKIP, NOREWIND, NOUNLOAD, STATS = 5"
```

## <a name="restore-a-database"></a>Restaurer une base de données

Dans l’exemple suivant **sqlcmd** se connecte à l’instance locale de SQL Server et restaure la base de données demodb. Notez que la `NORECOVERY` option est utilisée pour permettre aux restaurations supplémentaires de sauvegardes du fichier journal. Si vous ne souhaitez pas restaurer les fichiers journaux supplémentaires, supprimez le `NORECOVERY` option.

```bash
sqlcmd -S localhost -U SA -Q "RESTORE DATABASE [demodb] FROM DISK = N'/var/opt/mssql/data/demodb.bak' WITH FILE = 1, NOUNLOAD, REPLACE, NORECOVERY, STATS = 5"
```

> [!TIP]
> Si vous accidentellement WITH NORECOVERY, mais n’avez pas de sauvegardes de fichiers journaux supplémentaires, exécutez la commande `RESTORE DATABASE demodb` avec des paramètres supplémentaires. Cela termine la restauration et laisse la base de données opérationnelle.

### <a name="restore-the-transaction-log"></a>Restaurer le journal des transactions

La commande ci-dessous restaure la sauvegarde de journal de transaction précédente.

```bash
sqlcmd -S localhost -U SA -Q "RESTORE LOG demodb FROM DISK = N'/var/opt/mssql/data/demodb_LogBackup.bak'"
```

## <a name="backup-and-restore-with-sql-server-management-studio-ssms"></a>Sauvegarde et restauration avec SQL Server Management Studio (SSMS)

Vous pouvez utiliser SSMS à partir d’un ordinateur Windows pour vous connecter à une base de données de Linux et effectuez une sauvegarde via l’interface utilisateur.

>[!NOTE] 
> Utiliser la dernière version de SSMS pour vous connecter à SQL Server. Pour télécharger et installer la version la plus récente, consultez [télécharger SSMS](../ssms/download-sql-server-management-studio-ssms.md). Pour plus d’informations sur l’utilisation de SSMS, consultez [utilisez SSMS pour gérer SQL Server sur Linux](sql-server-linux-manage-ssms.md).

Les étapes suivantes en effectuant une sauvegarde avec SSMS. 

1. Démarrez SSMS et connectez-vous à votre serveur dans 2017 du serveur SQL sur Linux.

1. Dans l’Explorateur d’objets, cliquez sur votre base de données, cliquez sur **tâches**, puis cliquez sur **sauvegarder...** .

1. Dans le **sauvegarde la base de données** boîte de dialogue, vérifiez les paramètres et options, puis cliquez sur **OK**.
 
SQL Server termine la sauvegarde de base de données.

### <a name="restore-with-sql-server-management-studio-ssms"></a>Restauration avec SQL Server Management Studio (SSMS) 

Les étapes suivantes vous guident tout au long de la restauration d’une base de données avec SSMS.

1. Dans SSMS droit **bases de données** et cliquez sur **restaurer les bases de données...** . 

1. Sous **Source** cliquez sur **périphérique :** puis cliquez sur le bouton de sélection (...).

1. Recherchez votre fichier de sauvegarde de base de données et cliquez sur **OK**. 

1. Sous **du plan de restauration**, vérifiez le fichier de sauvegarde et les paramètres. Cliquez sur **OK**. 

1. SQL Server restaure la base de données. 

## <a name="see-also"></a>Voir aussi

* [Créer une sauvegarde complète de la base de données (SQL Server)](../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)
* [Sauvegarder un journal des transactions (SQL Server)](../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)
* [BACKUP (Transact-SQL)](../t-sql/statements/backup-transact-sql.md)
* [Sauvegarde SQL Server vers une URL](../relational-databases/backup-restore/sql-server-backup-to-url.md)
