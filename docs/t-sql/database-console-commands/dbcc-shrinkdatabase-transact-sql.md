---
title: DBCC SHRINKDATABASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DBCC_SHRINKDATABASE_TSQL
- DBCC SHRINKDATABASE
- SHRINKDATABASE_TSQL
- SHRINKDATABASE
dev_langs:
- TSQL
helpviewer_keywords:
- data shrinking [SQL Server]
- shrinking files
- shrinking databases
- DBCC SHRINKDATABASE statement
- decreasing database size
- file shrinking [SQL Server]
- database shrinking [SQL Server]
- logs [SQL Server], shrinking
- reducing database size
ms.assetid: fc976afd-1edb-4341-bf41-c4a42a69772b
caps.latest.revision: 62
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: e6022b5609c2d4b4d362f90088bee4e84ad874c7
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="dbcc-shrinkdatabase-transact-sql"></a>DBCC SHRINKDATABASE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Réduit la taille des fichiers de données et journaux dans la base de données spécifiée.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DBCC SHRINKDATABASE   
( database_name | database_id | 0   
     [ , target_percent ]   
     [ , { NOTRUNCATE | TRUNCATEONLY } ]   
)  
[ WITH NO_INFOMSGS ]  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name* | *database_id* | 0  
 Nom ou identificateur de la base de données à réduire. Si 0 est spécifié, la base de données active est utilisée.  
  
 *target_percent*  
 Pourcentage d'espace que vous voulez laisser disponible dans le fichier de la base de données après sa réduction.  
  
 NOTRUNCATE  
 Compacte les données des fichiers de données en déplaçant les pages allouées de la fin du fichier vers les pages non allouées du début du fichier. *target_percent* est facultatif. Cette option n’est pas prise en charge avec Azure SQL Data Warehouse. 
  
 L'espace libre à la fin du fichier n'est pas restitué au système d'exploitation et la taille physique du fichier ne change pas. Ainsi, lorsque l'option NOTRUNCATE est spécifiée, la base de données ne paraît pas être réduite.  
  
 NOTRUNCATE n'est applicable qu'aux fichiers de données. Le fichier journal n'est pas affecté.  
  
 TRUNCATEONLY  
 Libère pour le système d'exploitation tout l'espace libre à la fin du fichier, mais n'effectue aucun déplacement de page au sein du fichier. Le fichier de données est réduit seulement jusqu'à la dernière extension allouée. *target_percent* est ignoré si spécifié avec TRUNCATEONLY. Cette option n’est pas prise en charge avec Azure SQL Data Warehouse.
  
 TRUNCATEONLY affecte le fichier journal. Pour tronquer uniquement le fichier de données, utilisez DBCC SHRINKFILE.  
  
 WITH NO_INFOMSGS  
 Supprime tous les messages d'information dont les niveaux de gravité sont compris entre 0 et 10.  
  
## <a name="result-sets"></a>Jeux de résultats  
Le tableau suivant décrit les colonnes du jeu de résultats.
  
|Nom de colonne|Description|  
|-----------------|-----------------|  
|**DbId**|Numéro d'identification de base de données du fichier que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] tente de réduire.|  
|**FileId**|Numéro d'identification du fichier que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] tente de réduire.|  
|**CurrentSize**|Nombre de pages de 8 Ko que le fichier occupe actuellement.|  
|**MinimumSize**|Nombre de pages de 8 Ko que le fichier pourrait occuper au minimum. Ceci correspond à la taille minimale ou à la taille de création d'un fichier.|  
|**UsedPages**|Nombre de pages de 8 Ko que le fichier utilise actuellement.|  
|**EstimatedPages**|Nombre de pages de 8 Ko estimé par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] auquel la taille du fichier peut être ramenée.|  
  
>[!NOTE]
> Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'affiche pas de ligne pour les fichiers qui ne sont pas réduits.  
  
## <a name="remarks"></a>Notes   

>[!NOTE]
> Actuellement, Azure SQL Data Warehouse ne prend pas en charge DBCC SHRINKDATABASE. L’exécution de cette commande n’est pas recommandée, car c’est une opération qui consomme beaucoup d’E/S et qui peut déconnecter votre entrepôt de données. Par ailleurs, l’exécution de cette commande entraîne de coûteuses implications sur vos instantanés d’entrepôt de données. 

Pour réduire tous les fichiers de données et fichiers journaux d'une base de données particulière, exécutez la commande DBCC SHRINKDATABASE. Pour réduire un fichier de données ou un fichier journal d'une base de données particulière, exécutez la commande [DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md).
  
Pour afficher la quantité d'espace actuellement libre (non allouée) dans la base de données, exécutez [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md).
  
Les opérations DBCC SHRINKDATABASE peuvent être arrêtées à n'importe quel stade du processus, chaque travail terminé étant conservé.
  
La base de données ne peut pas être réduite à une taille inférieure à la taille minimale de la base de données. La taille minimale correspond à la taille spécifiée lors de la création initiale de la base de données ou à la dernière taille explicitement spécifiée à l'aide d'une opération de modification de taille de fichier, notamment en utilisant l'instruction DBCC SHRINKFILE ou ALTER DATABASE. Par exemple, si une base de données est créée avec une taille de 10 Mo et atteint une taille de 100 Mo, la base de données ne peut pas être réduite à moins de 10 Mo, même si toutes les données de la base de données sont supprimées.
  
Exécuter DBCC SHRINKDATABASE sans spécifier l'option NOTRUNCATE ou TRUNCATEONLY revient à exécuter une opération DBCC SHRINKDATABASE avec NOTRUNCATE suivie d'une opération DBCC SHRINKDATABASE avec TRUNCATEONLY.
  
Il n'est pas nécessaire que la base de données réduite soit mono-utilisateur ; d'autres utilisateurs peuvent travailler sur cette base au cours de l'opération. Ceci inclut également les bases de données système.
  
Vous ne pouvez pas réduire la taille d'une base de données en cours de sauvegarde. Inversement, vous ne pouvez pas sauvegarder une base de données alors qu'elle fait l'objet d'une opération de réduction.
  
## <a name="how-dbcc-shrinkdatabase-works"></a>Fonctionnement de DBCC SHRINKDATABASE  
DBCC SHRINKDATABASE réduit les fichiers de données, fichier par fichier, mais réduit les fichiers journaux comme si tous les fichiers journaux existaient dans un groupe de journaux contigus. Les fichiers sont toujours réduits à partir de la fin.
  
Supposons que la base de données **mydb** a un fichier de données et deux fichiers journaux. La taille de chacun des fichiers est de 10 Mo et le fichier de données contient 6 Mo de données.
  
Pour chaque fichier, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] calcule une taille cible, qui est la taille à laquelle le fichier doit être réduit. Quand DBCC SHRINKDATABASE est spécifié avec *target_percent*, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] calcule la taille cible pour qu'elle corresponde à la quantité *target_percent* d'espace disponible dans le fichier après la réduction. Par exemple, si vous spécifiez un *target_percent* de 25 pour réduire **mydb**, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] calcule une taille cible de 8 Mo pour le fichier de données (6 Mo de données plus 2 Mo d'espace libre). Par conséquent, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] déplace toutes les données des 2 derniers Mo du fichier de données vers l'espace libre des 8 premiers Mo du fichier de données, puis réduit le fichier.
  
Supposons que le fichier de données de **mydb** contient 7 Mo de données. Si vous spécifiez un *target_percent* de 30, le fichier de données peut être réduit à 30 % d'espace libre. En revanche, si vous spécifiez un *target_percent* de 40, le fichier de données ne peut pas être réduit, car le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne peut pas réduire un fichier à une taille inférieure à celle qu’occupent les données actuellement. En d’autres termes : si vous ajoutez 40 % d’espace libre souhaité à 70 % d’espace occupé dans le fichier de données (7 Mo sur un total de 10 Mo), vous obtenez plus de 100 %. Comme la somme du pourcentage d'espace libre souhaité et du pourcentage actuel occupé par les données est supérieure à 100 % (de 10 %), toute valeur *target_size* supérieure à 30 n'entraîne pas la réduction du fichier de données.
  
Pour les fichiers journaux, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise *target_percent* pour calculer la taille cible du fichier journal complet. *target_percent* correspond donc à l'espace libre dans le journal après l'opération de réduction. La taille cible pour le journal complet est alors convertie en taille cible pour chaque fichier journal.
  
DBCC SHRINKDATABASE essaie immédiatement de réduire chaque fichier journal physique à sa taille cible. Si aucune partie du journal logique ne se trouve dans les journaux virtuels au-delà de la taille cible du fichier journal, le fichier est tronqué avec succès et DBCC SHRINKDATABASE s'exécute normalement sans émettre de messages. Toutefois, si une partie du journal logique se trouve dans les journaux virtuels au-delà de la taille cible, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] libère autant d'espace que possible, puis envoie un message d'information. Le message décrit les actions à effectuer pour déplacer le journal logique à partir des journaux virtuels à la fin du fichier. Lorsque les actions sont effectuées, DBCC SHRINKDATABASE peut être utilisé pour libérer l'espace restant.
  
Comme un fichier journal ne peut être réduit que jusqu'à une limite virtuelle, il arrive qu'il ne soit pas possible de réduire un fichier journal à une taille inférieure à celle d'un fichier journal virtuel, même s'il n'est pas utilisé. La taille du fichier journal virtuel est choisie dynamiquement par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] au moment de la création ou de l'extension des fichiers journaux.
  
## <a name="best-practices"></a>Bonnes pratiques  
Prenez en compte les informations suivantes lorsque vous envisagez de réduire une base de données :
-   Une opération de réduction de taille de fichier est plus efficace après l'exécution d'une opération qui crée une grande quantité d'espace inutilisé, telle qu'une troncature de table ou une suppression de table.  
-   Un certain espace libre doit exister pour les opérations quotidiennes courantes pour la plupart des bases de données. Si vous réduisez plusieurs fois la taille d'une base de données et que vous constatez que la taille augmente de nouveau, cela indique que l'espace qui a été réduit est nécessaire pour les opérations courantes. Dans ce cas, la réduction de la taille de la base de données ne sert à rien.  
-   Une opération de réduction ne conserve pas l'état de fragmentation des index de la base de données, et augmente généralement la fragmentation. Il s'agit là d'une raison supplémentaire pour ne pas réduire la taille de la base de données de manière répétitive.  
-   Sauf en cas de besoin précis, n'attribuez pas la valeur ON à l'option de base de données AUTO_SHRINK.  
  
## <a name="troubleshooting"></a>Dépannage  
 Les opérations de réduction peuvent être bloquées par une transaction en cours d'exécution sous un [niveau d'isolation basé sur le contrôle de version de ligne](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md). Par exemple, si une importante opération de suppression exécutée sous un niveau d'isolation basé sur le contrôle de version de ligne se déroule parallèlement à une opération DBCC SHRINK DATABASE, l'opération de réduction attendra la fin de l'opération de suppression pour réduire la taille des fichiers. Dans ce cas, les opérations DBCC SHRINKFILE et DBCC SHRINKDATABASE envoient un message d'information (5202 pour SHRINKDATABASE et 5203 pour SHRINKFILE) dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] toutes les cinq minutes au cours de la première heure, puis toutes les heures. Par exemple, si le journal des erreurs contient le message d'erreur :  
  
```sql
DBCC SHRINKDATABASE for database ID 9 is waiting for the snapshot   
transaction with timestamp 15 and other snapshot transactions linked to   
timestamp 15 or with timestamps older than 109 to finish.  
```  
  
cela signifie que l'opération de réduction est bloquée par des transactions d'instantané ayant des valeurs d'horodateur plus anciennes que 109, qui est le numéro de la dernière transaction que l'opération de réduction a effectuée. Cela indique également que les colonnes **transaction_sequence_num** ou **first_snapshot_sequence_num** dans la vue de gestion dynamique [sys.dm_tran_active_snapshot_database_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) contiennent une valeur de 15. Si les colonnes **transaction_sequence_num** ou **first_snapshot_sequence_num** dans la vue contiennent un numéro inférieur à la dernière transaction effectuée par une opération de réduction (109), l'opération de réduction attend que ces transactions soient terminées.
  
Pour résoudre ce problème, vous pouvez effectuer l'une des tâches suivantes :
-   Achevez la transaction qui bloque l'opération de réduction.  
-   Achevez l'opération de réduction. Tout travail achevé sera conservé.  
-   Laissez simplement l'opération de réduction attendre que la transaction bloquante s'achève.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance au rôle de serveur fixe **sysadmin** ou au rôle de base de données fixe **db_owner** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-shrinking-a-database-and-specifying-a-percentage-of-free-space"></a>A. Réduction de la taille d'une base de données et spécification d'un pourcentage d'espace libre  
 L'exemple suivant diminue la taille des fichiers de données et journaux de la base de données utilisateur `UserDB` pour obtenir 10 % d'espace libre dans la base de données.  
  
```sql  
DBCC SHRINKDATABASE (UserDB, 10);  
GO  
```  
  
### <a name="b-truncating-a-database"></a>B. Troncation d'une base de données  
 L'exemple suivant réduit la taille des fichiers de données et des fichiers journaux de l'exemple de base de données `AdventureWorks` jusqu'à la dernière extension allouée.  
  
```sql  
DBCC SHRINKDATABASE (AdventureWorks2012, TRUNCATEONLY);  
```  
  
## <a name="see-also"></a>Voir aussi  
[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC SHRINKFILE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)  
[Réduire une base de données](../../relational-databases/databases/shrink-a-database.md)
  
  
