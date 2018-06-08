---
title: DBCC SHRINKFILE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SHRINKFILE
- DBCC_SHRINKFILE_TSQL
- DBCC SHRINKFILE
- SHRINKFILE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data shrinking [SQL Server]
- TRUNCATEONLY option
- shrinking files
- NOTRUNCATE option
- shrinking databases
- decreasing database size
- file shrinking [SQL Server]
- database shrinking [SQL Server]
- logs [SQL Server], shrinking
- reducing database size
- DBCC SHRINKFILE statement
ms.assetid: e02b2318-bee9-4d84-a61f-2fddcf268c9f
caps.latest.revision: 87
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: a771f30b82a81fa05ea65409bce9a132cbb42dad
ms.sourcegitcommit: b3bb41424249de198f22d9c6d40df4996f083aa6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="dbcc-shrinkfile-transact-sql"></a>DBCC SHRINKFILE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Réduit la taille du fichier journal ou de données spécifié pour la base de données actuelle, ou vide un fichier en déplaçant les données depuis le fichier spécifié vers d'autres fichiers du même groupe de fichiers, permettant ainsi de supprimer le fichier de la base de données. Vous pouvez réduire un fichier à une taille inférieure à la taille spécifiée lors de sa création. Dans ce cas, la nouvelle valeur correspond à la taille de fichier minimale.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
  
DBCC SHRINKFILE   
(  
    { file_name | file_id }   
    { [ , EMPTYFILE ]   
    | [ [ , target_size ] [ , { NOTRUNCATE | TRUNCATEONLY } ] ]  
    }  
)  
[ WITH NO_INFOMSGS ]  
```  
  
## <a name="arguments"></a>Arguments  
*file_name*  
Nom logique du fichier dont la taille doit être réduite.
  
*file_id*  
Numéro d'identification (ID) du fichier à réduire. Pour obtenir un ID de fichier, utilisez la fonction système [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md) ou interrogez l’affichage catalogue [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) dans la base de données actuelle.
  
*target_size*  
Taille en octets du fichier, exprimée sous la forme d'un entier. Si ce paramètre n'est pas spécifié, DBCC SHRINKFILE réduit la taille à la taille de fichier par défaut. La taille par défaut est la taille spécifiée lors de la création du fichier.
  
> [!NOTE]  
>  Vous pouvez réduire la taille par défaut d’un fichier vide en utilisant DBCC SHRINKFILE *target_size*. Par exemple, si vous créez un fichier de 5 Mo, puis que vous le réduisez à 3 Mo pendant que le fichier est encore vide, la taille de fichier par défaut est fixée à 3 Mo. Cela s'applique uniquement aux fichiers vides qui n'ont jamais contenu des données.  
  
Cette option n'est pas prise en charge pour les conteneurs de groupe de fichiers FILESTREAM.  
Si *target_size* est spécifié, DBCC SHRINKFILE tente de réduire le fichier à la taille spécifiée. Les pages utilisées dans la partie du fichier à libérer sont transférées vers un espace libre disponible dans la partie du fichier conservée. Par exemple, s’il s’agit d’un fichier de données de 10 Mo et que vous affectez la valeur 8 à *target_size* pour les opérations DBCC SHRINKFILE, toutes les pages utilisées dans les 2 derniers Mo du fichier sont transférées dans les pages non allouées des 8 premiers Mo du fichier. DBCC SHRINKFILE ne réduit pas un fichier au-delà de la taille nécessaire pour stocker les données dans le fichier. De fait, s’il s’agit d’un fichier de 10 Mo et que 7 Mo sont utilisés, l’instruction DBCC SHRINKFILE avec une valeur de 6 pour *target_size* réduit la taille à 7 Mo et non pas à 6 Mo.
  
EMPTYFILE  
Permet la migration de toutes les données du fichier spécifié vers d’autres fichiers dans le **même groupe de fichiers**. En d’autres mots, EmptyFile effectue la migration des données du fichier spécifié vers d’autres fichiers dans le même groupe de fichiers. Emptyfile vous garantit qu’aucune nouvelle donnée n’est ajoutée au fichier, même si le fichier n’est pas marqué en lecture seule. Le fichier peut être supprimé à l’aide de l’instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md). Si la taille de fichier est modifiée à l’aide de l’instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md), l’indicateur de lecture seule est réinitialisé et les données peuvent être ajoutées.

Pour les conteneurs de groupe de fichiers FILESTREAM, le fichier ne peut pas être supprimé à l'aide d'ALTER DATABASE tant que le Garbage collector FILESTREAM n'a pas été exécuté pour supprimer tous les fichiers conteneurs de groupe de fichiers inutiles qu'EMPTYFILE a copiés vers un autre conteneur. Pour plus d’informations, consultez [sp_filestream_force_garbage_collection &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md)
  
> [!NOTE]  
>  Pour plus d’informations sur la suppression d’un conteneur FILESTREAM, consultez la section correspondante dans [Options de fichiers et de groupes de fichiers ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)  
  
NOTRUNCATE  
Déplace des pages allouées depuis la fin d’un fichier de données vers les pages non allouées du début du fichier, avec ou sans spécification de *target_percent*. L'espace libre à la fin du fichier n'est pas restitué au système d'exploitation et la taille physique du fichier ne change pas. Ainsi, lorsque l'option NOTRUNCATE est spécifiée, le fichier ne paraît pas être réduit.
NOTRUNCATE n'est applicable qu'aux fichiers de données. Les fichiers journaux ne sont pas affectés.   Cette option n'est pas prise en charge pour les conteneurs de groupe de fichiers FILESTREAM.
  
TRUNCATEONLY  
Libère pour le système d'exploitation tout l'espace libre à la fin du fichier, mais n'effectue aucun déplacement de page au sein du fichier. Le fichier de données est réduit seulement jusqu'à la dernière extension allouée.
*target_size* est ignoré si spécifié avec TRUNCATEONLY.  
L'option TRUNCATEONLY ne déplace pas d'informations dans le journal, mais supprime les fichiers journaux virtuels inactifs de la fin du fichier journal. Cette option n'est pas prise en charge pour les conteneurs de groupe de fichiers FILESTREAM.
  
WITH NO_INFOMSGS  
Supprime tous les messages d'information.
  
## <a name="result-sets"></a>Jeux de résultats  
Le tableau suivant décrit les colonnes du jeu de résultats.
  
|Nom de colonne|Description|  
|---|---|
|**DbId**|Numéro d'identification de base de données du fichier que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] tente de réduire.|  
|**FileId**|Numéro d’identification du fichier que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a tenté de réduire.|  
|**CurrentSize**|Nombre de pages de 8 Ko que le fichier occupe actuellement.|  
|**MinimumSize**|Nombre de pages de 8 Ko que le fichier pourrait occuper au minimum. Ceci correspond à la taille minimale ou à la taille de création d'un fichier.|  
|**UsedPages**|Nombre de pages de 8 Ko que le fichier utilise actuellement.|  
|**EstimatedPages**|Nombre de pages de 8 Ko estimé par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] auquel la taille du fichier peut être ramenée.|  
  
## <a name="remarks"></a>Notes   
DBCC SHRINKFILE s'applique aux fichiers de la base de données active. Pour plus d’informations sur le changement de base de données active, consultez [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md).
  
Les opérations DBCC SHRINKFILE peuvent être arrêtées à n'importe quel stade du processus, chaque travail terminé étant conservé. Si le paramètre EMPTYFILE est utilisé sur un fichier et que l’opération est annulée, le fichier n’est pas marqué pour empêcher l’ajout de données supplémentaires.
  
Une erreur est générée quand une opération DBCC SHRINKFILE échoue.
  
 Il n'est pas nécessaire que la base de données à réduire soit en mode mono-utilisateur ; d'autres utilisateurs peuvent travailler dans la base de données lorsque la taille du fichier est réduite. Il n'est pas nécessaire d'exécuter l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur pour réduire les bases de données système.  
  
## <a name="shrinking-a-log-file"></a>Réduction d'un fichier journal  
Pour les fichiers journaux, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise *target_size* pour calculer la taille cible du journal complet. *target_size* correspond donc à la quantité d’espace libre dans le journal après l’opération de réduction. La taille cible de l'ensemble du journal est ensuite convertie dans la taille cible de chaque fichier journal. DBCC SHRINKFILE tente de ramener immédiatement la taille de chaque fichier journal physique à la taille cible. Toutefois, si une partie du journal logique se trouve dans les journaux virtuels au-delà de la taille cible, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] libère autant d'espace que possible, puis envoie un message d'information. Le message décrit les actions à effectuer pour déplacer le journal logique à partir des journaux virtuels à la fin du fichier. Lorsque les actions sont effectuées, DBCC SHRINKFILE peut être utilisé pour libérer l'espace restant.
  
Comme un fichier journal ne peut être réduit que jusqu'à une limite virtuelle, il arrive qu'il ne soit pas possible de réduire un fichier journal à une taille inférieure à celle d'un fichier journal virtuel, même s'il n'est pas utilisé. La taille du fichier journal virtuel est choisie dynamiquement par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] au moment de la création ou de l'extension des fichiers journaux.
  
## <a name="best-practices"></a>Bonnes pratiques  
Prenez en compte les informations suivantes lorsque vous envisagez de réduire un fichier :
-   Une opération de réduction de taille de fichier est plus efficace après l'exécution d'une opération qui crée une grande quantité d'espace inutilisé, telle qu'une troncature de table ou une suppression de table.  
-   Un certain espace libre doit exister pour les opérations quotidiennes courantes pour la plupart des bases de données. Si vous réduisez plusieurs fois la taille d'une base de données et que vous constatez que la taille augmente de nouveau, cela indique que l'espace qui a été réduit est nécessaire pour les opérations courantes. Dans ce cas, la réduction de la taille de la base de données ne sert à rien.  
-   Une opération de réduction ne conserve pas l'état de fragmentation des index de la base de données, et augmente généralement la fragmentation. Il s'agit là d'une raison supplémentaire pour ne pas réduire la taille de la base de données de manière répétitive.  
-   Réduisez plusieurs fichiers dans la même base de données de manière séquentielle plutôt que simultanément. La contention sur les tables système peut entraîner des délais en raison de blocage.  
  
## <a name="troubleshooting"></a>Dépannage  
Cette section décrit comment diagnostiquer et corriger les problèmes qui peuvent se produire lors de l'exécution de la commande DBCC SHRINKFILE.
  
### <a name="the-file-does-not-shrink"></a>La taille du fichier n'est pas réduite  
Si l'opération de réduction s'exécute sans erreur, mais que la taille du fichier ne semble pas avoir changé, vérifiez que le fichier dispose d'un espace adéquat à supprimer, en effectuant l'une des opérations suivantes :
- Exécutez la requête suivante.  
  
```sql
SELECT name ,size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0 AS AvailableSpaceInMB
FROM sys.database_files;
```

-   Exécutez la commande [DBCC SQLPERF](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md) pour restituer l’espace utilisé dans le journal des transactions.  
Si l'espace disponible est insuffisant, l'opération de réduction ne peut pas réduire davantage la taille du fichier.
  
En règle générale, c'est la taille du fichier journal qui semble impossible à réduire. Ceci est généralement dû à un fichier journal qui n'a pas été tronqué. Vous pouvez tronquer le journal en attribuant la valeur SIMPLE au mode de récupération de base de données, ou en sauvegardant le journal et en réexécutant l'opération DBCC SHRINKFILE.
  
### <a name="the-shrink-operation-is-blocked"></a>L'opération de réduction est bloquée  
Les opérations de réduction peuvent être bloquées par une transaction en cours d'exécution sous un [niveau d'isolation basé sur le contrôle de version de ligne](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md). Par exemple, si une importante opération de suppression exécutée sous un niveau d'isolation basé sur le contrôle de version de ligne se déroule parallèlement à une opération DBCC SHRINK DATABASE, l'opération de réduction attendra la fin de l'opération de suppression pour réduire la taille des fichiers. Dans ce cas, les opérations DBCC SHRINKFILE et DBCC SHRINKDATABASE envoient un message d'information (5202 pour SHRINKDATABASE et 5203 pour SHRINKFILE) dans le journal des erreurs de SQL Server toutes les cinq minutes au cours de la première heure, puis toutes les heures. Par exemple, si le journal des erreurs contient le message d'erreur suivant, l'erreur suivante se produit :
  
```sql
DBCC SHRINKFILE for file ID 1 is waiting for the snapshot   
transaction with timestamp 15 and other snapshot transactions linked to   
timestamp 15 or with timestamps older than 109 to finish.  
```  
  
cela signifie que l'opération de réduction est bloquée par des transactions d'instantané ayant des valeurs d'horodateur plus anciennes que 109, qui est le numéro de la dernière transaction que l'opération de réduction a effectuée. Cela indique également que les colonnes **transaction_sequence_num** ou **first_snapshot_sequence_num** dans la vue de gestion dynamique [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) contiennent la valeur 15. Si les colonnes **transaction_sequence_num** ou **first_snapshot_sequence_num** dans la vue contiennent un numéro inférieur à la dernière transaction effectuée par une opération de réduction (109), l'opération de réduction attend que ces transactions soient terminées.
  
Pour résoudre ce problème, vous pouvez effectuer l'une des opérations suivantes :
-   Achevez la transaction qui bloque l'opération de réduction.
-   Achevez l'opération de réduction. Si vous achevez l'opération de réduction, tout travail accompli est conservé.  
-   Laissez simplement l'opération de réduction attendre que la transaction bloquante s'achève.  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’appartenance au rôle de serveur fixe **sysadmin** ou au rôle de base de données fixe **db_owner** .
  
## <a name="examples"></a>Exemples  
  
### <a name="a-shrinking-a-data-file-to-a-specified-target-size"></a>A. Réduction d'un fichier de données à une taille cible spécifiée  
L’exemple suivant ramène la taille d’un fichier de données nommé `DataFile1` de la base de données utilisateur `UserDB` à 7 Mo.
  
```sql  
USE UserDB;  
GO  
DBCC SHRINKFILE (DataFile1, 7);  
GO  
```  
  
### <a name="b-shrinking-a-log-file-to-a-specified-target-size"></a>B. Réduction d'un fichier journal à une taille cible spécifiée  
L'exemple suivant ramène la taille du fichier journal de la base de données `AdventureWorks` à 1 Mo. Pour que la commande DBCC SHRINKFILE puisse réduire la taille du fichier, le système tronque d'abord celui-ci en attribuant la valeur SIMPLE au mode de récupération de base de données.
  
```sql  
USE AdventureWorks2012;  
GO  
-- Truncate the log by changing the database recovery model to SIMPLE.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY SIMPLE;  
GO  
-- Shrink the truncated log file to 1 MB.  
DBCC SHRINKFILE (AdventureWorks2012_Log, 1);  
GO  
-- Reset the database recovery model.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY FULL;  
GO  
```  
  
### <a name="c-truncating-a-data-file"></a>C. Troncation d'un fichier de données  
L'exemple suivant tronque le fichier de données primaire dans la base de données `AdventureWorks`. Le système interroge l'affichage catalogue `sys.database_files` afin d'obtenir la valeur `file_id` du fichier de données.
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT file_id, name  
FROM sys.database_files;  
GO  
DBCC SHRINKFILE (1, TRUNCATEONLY);  
```  
  
### <a name="d-emptying-a-file"></a>D. Vidage d'un fichier  
L'exemple suivant illustre la procédure qui permet de vider un fichier de manière à ce qu'il puisse être supprimé de la base de données. Pour que cet exemple fonctionne, un fichier de données est d'abord créé et celui-ci est censé contenir des données.
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create a data file and assume it contains data.  
ALTER DATABASE AdventureWorks2012   
ADD FILE (  
    NAME = Test1data,  
    FILENAME = 'C:\t1data.ndf',  
    SIZE = 5MB  
    );  
GO  
-- Empty the data file.  
DBCC SHRINKFILE (Test1data, EMPTYFILE);  
GO  
-- Remove the data file from the database.  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE Test1data;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC SHRINKDATABASE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md)  
[FILE_ID &#40;Transact-SQL&#41;](../../t-sql/functions/file-id-transact-sql.md)  
[sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[Réduire un fichier](../../relational-databases/databases/shrink-a-file.md)
  
  
