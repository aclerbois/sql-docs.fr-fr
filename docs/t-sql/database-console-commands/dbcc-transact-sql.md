---
title: DBCC (Transact-SQL) | Microsoft Docs
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
- DBCC
- DBCC_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transactional consistency
- Database Console Command statements
- status information [SQL Server], DBCC statements
- snapshots [SQL Server database snapshots], DBCC statements
- emergency database state [SQL Server]
- TABLOCK
- internal database snapshots [SQL Server]
- reporting DBCC statement progress
- logical consistency of data [SQL Server]
- DBCC statements
- validation statements [SQL Server]
- miscellaneous statements [SQL Server]
- statements [SQL Server], DBCC statements
- DBCC commands [SQL Server]
- result sets [SQL Server], DBCC statements
- maintenance statements [SQL Server]
- physical consistency of data [SQL Server]
- current DBCC statement status
- progress reporting [DBCC statements]
- informational statements [SQL Server]
ms.assetid: c6da8c04-5b6b-459a-9f76-110c92ca8b29
caps.latest.revision: 50
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: 6dbf227ec42614e77cb4129c273ec55609781b2f
ms.sourcegitcommit: 38f8824abb6760a9dc6953f10a6c91f97fa48432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dbcc-transact-sql"></a>DBCC (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Le langage de programmation [!INCLUDE[tsql](../../includes/tsql-md.md)] fournit des instructions DBCC qui jouent le rôle d'instructions de console de base de données pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
Les instructions de la console de base de données sont regroupées selon les catégories suivantes :
  
|Catégorie de commande|Action|  
|---|---|
|Maintenance|Les tâches de maintenance sur une base de données, un index ou un groupe de fichiers.|  
|Divers|Les tâches diverses, telles que l'activation des indicateurs de trace ou la suppression d'une DLL de la mémoire.|  
|Information|Les tâches qui recueillent et affichent différents types d'informations.|  
|Validation|Valide une base de données, une table, un index, un catalogue, un groupe de fichiers ou l'allocation de pages de base de données|  
  
Les commandes DBCC prennent des paramètres d'entrée et renvoient des valeurs. Tous les paramètres des commandes DBCC acceptent les littéraux de type Unicode et DBCS.
  
## <a name="dbcc-internal-database-snapshot-usage"></a>Utilisation de l'instantané de base de données interne DBCC  
Les commandes DBCC suivantes fonctionnent sur un instantané de base de données en lecture seule interne créé par le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Ceci évite les problèmes de blocage et d'accès simultané lors de l'exécution de ces commandes. Pour plus d’informations, consultez [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).
- DBCC CHECKALLOC
- DBCC CHECKCATALOG
- DBCC CHECKDB
- DBCC CHECKFILEGROUP
- DBCC CHECKTABLE

Quand vous exécutez l’une de ces commandes DBCC, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée un instantané de base de données dont l’état est cohérent d’un point de vue transactionnel. La commande DBCC exécute alors les vérifications sur cet instantané. Lorsque la commande DBCC a terminé, cet instantané est supprimé.
  
Parfois, l'instantané de base de données interne n'est pas nécessaire ou n'est pas possible. Dans ce cas, la commande DBCC s'exécute sur la base de données réelle. Si la base de données est en ligne, la commande DBCC a recours au verrouillage des tables pour garantir la cohérence des objets qu'elle est en train de vérifier. Ce comportement serait identique si l'option WITH TABLOCK était spécifiée.
  
Aucun instantané de base de données interne n'est créé lors de l'exécution d'une commande DBCC :
-   Sur une base de données **master**, si l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute en mode mono-utilisateur.  
-   Sur une base de données autre que **master**, si cette base de données a été placée en mode mono-utilisateur à l’aide de l’instruction ALTER DATABASE.  
-   Sur une base de données en lecture seule.  
-   Sur une base de données qui a été placée en mode urgence à l'aide de la commande ALTER DATABASE.  
-   Sur une base de données **tempdb**. Dans ce cas, l'instantané de base de données ne peut pas être créé, en raison de restrictions internes.  
-   Si l'option WITH TABLOCK est utilisée. Dans ce cas, DBCC satisfait la demande en ne créant pas d'instantané de la base de données.  
  
Les commandes DBCC utilisent des verrous de table au lieu d'instantanés internes de base de données lorsque la commande est exécutée sur les bases de données suivantes :
-   un groupe de fichier en lecture seule ;  
-   un système de fichiers FAT ;  
-   un volume qui ne prend pas en charge les « flux nommés » ;  
-   un volume qui ne prend pas en charge les « flux de remplacement ».  
  
> [!NOTE]  
>  Pour tenter d'exécuter DBCC CHECKALLOC, ou la partie équivalente de DBCC CHECKDB, à l'aide de l'option WITH TABLOCK, il faut utiliser un verrou X (exclusif) de base de données. Ce verrou de base de données ne peut pas être défini sur des bases de données **tempdb** ni **master** et risque d’échouer sur toutes les autres bases de données.  
  
> [!NOTE]  
>  DBCC CHECKDB échoue quand il est exécuté sur une base de données **master** s’il n’est pas possible de créer d’instantané de base de données interne.  
  
## <a name="progress-reporting-for-dbcc-commands"></a>Rapport de progression pour les commandes DBCC  
La vue de catalogue **sys.dm_exec_requests** contient des informations sur la progression et la phase d’exécution en cours des commandes DBCC CHECKDB, CHECKFILEGROUP et CHECKTABLE. La colonne **percent_complete** indique le pourcentage d’exécution de la commande, et la colonne **command** indique la phase d’exécution en cours de cette commande.
  
La définition d'une unité de progression dépend de la phase en cours d'exécution de la commande DBCC. La progression est parfois indiquée avec un niveau de granularité correspondant à une page de base de données, alors que pour d'autres phases elle est indiquée avec un niveau de granularité correspondant à une seule réparation de base de données ou d'allocation. Le tableau qui suit décrit chaque phase de l'exécution, et le niveau de granularité utilisé par la commande pour indiquer la progression.
  
|Phase d'exécution|Description|Granularité du rapport de progression|  
|---------------------|-----------------|------------------------------------|  
|DBCC TABLE CHECK|Durant cette phase, la cohérence logique et physique des objets de la base de données est vérifiée.|La progression est indiquée au niveau de la page de base de données.<br /><br /> La valeur de rapport de progression est actualisée toutes les 1 000 pages de base de données vérifiées.|  
|DBCC TABLE REPAIR|Durant cette phase, les réparations de base de données sont exécutées si REPAIR_FAST, REPAIR_REBUILD ou REPAIR_ALLOW_DATA_LOSS est spécifié, et s'il existe des erreurs à réparer sur les objets.|La progression est indiquée au niveau de la réparation.<br /><br /> Le compteur est mis à jour pour chaque réparation terminée.|  
|DBCC ALLOC CHECK|Durant cette phase, les structures d'allocation de la base de données sont vérifiées.<br /><br /> Remarque : DBCC CHECKALLOC exécute les mêmes vérifications.|La progression n’est pas indiquée|  
|DBCC ALLOC REPAIR|Durant cette phase, les réparations de base de données sont exécutées si REPAIR_FAST, REPAIR_REBUILD ou REPAIR_ALLOW_DATA_LOSS est spécifié, et s'il existe des erreurs d'allocation à réparer.|La progression n'est pas indiquée.|  
|DBCC SYS CHECK|Durant cette phase, les tables système de la base de données sont vérifiées.|La progression est indiquée au niveau de la page de base de données.<br /><br /> La valeur de progression est actualisée toutes les 1 000 pages de base de données vérifiées.|  
|DBCC SYS REPAIR|Durant cette phase, les réparations de base de données sont exécutées si REPAIR_FAST, REPAIR_REBUILD ou REPAIR_ALLOW_DATA_LOSS est spécifié, et s'il existe des erreurs de tables système à réparer.|La progression est indiquée au niveau de la réparation.<br /><br /> Le compteur est mis à jour pour chaque réparation terminée.|  
|DBCC SSB CHECK|Durant cette phase, les objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Broker sont vérifiés.<br /><br /> Remarque : Cette phase n’est pas réalisée lors de l’exécution de DBCC CHECKTABLE.|La progression n'est pas indiquée.|  
|DBCC CHECKCATALOG|Durant cette phase, la cohérence des catalogues de la base de données est vérifiée.<br /><br /> Remarque : Cette phase n’est pas réalisée lors de l’exécution de DBCC CHECKTABLE.|La progression n'est pas indiquée.|  
|DBCC IVIEW CHECK|Durant cette phase, la cohérence logique des vues indexées présentes dans la base de données est vérifiée.|La progression est indiquée au niveau de chaque vue de base de données vérifiée.|  
  
## <a name="informational-statements"></a>Instructions d’information  
  
|||  
|-|-|  
|[DBCC INPUTBUFFER](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)|[DBCC SHOWCONTIG](../../t-sql/database-console-commands/dbcc-showcontig-transact-sql.md)|  
|[DBCC OPENTRAN](../../t-sql/database-console-commands/dbcc-opentran-transact-sql.md)|[DBCC SQLPERF](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)|  
|[DBCC OUTPUTBUFFER](../../t-sql/database-console-commands/dbcc-outputbuffer-transact-sql.md)|[DBCC TRACESTATUS](../../t-sql/database-console-commands/dbcc-tracestatus-transact-sql.md)|  
|[DBCC PROCCACHE](../../t-sql/database-console-commands/dbcc-proccache-transact-sql.md)|[DBCC USEROPTIONS](../../t-sql/database-console-commands/dbcc-useroptions-transact-sql.md)|  
|[DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)||  
  
## <a name="validation-statements"></a>Instructions de validation  
  
|||  
|-|-|  
|[DBCC CHECKALLOC](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md)|[DBCC CHECKFILEGROUP](../../t-sql/database-console-commands/dbcc-checkfilegroup-transact-sql.md)|  
|[DBCC CHECKCATALOG](../../t-sql/database-console-commands/dbcc-checkcatalog-transact-sql.md)|[DBCC CHECKIDENT](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)|  
|[DBCC CHECKCONSTRAINTS](../../t-sql/database-console-commands/dbcc-checkconstraints-transact-sql.md)|[DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)|  
|[DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)||  
  
## <a name="maintenance-statements"></a>Instructions de maintenance  
  
|||  
|-|-|  
|[DBCC CLEANTABLE](../../t-sql/database-console-commands/dbcc-cleantable-transact-sql.md)|[DBCC INDEXDEFRAG](../../t-sql/database-console-commands/dbcc-indexdefrag-transact-sql.md)|  
|[DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md)|[DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md)|  
|[DBCC DROPCLEANBUFFERS](../../t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql.md)|[DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)|  
|[DBCC FREEPROCCACHE](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)|[DBCC UPDATEUSAGE](../../t-sql/database-console-commands/dbcc-updateusage-transact-sql.md)|  
  
## <a name="miscellaneous-statements"></a>Instructions diverses  
  
|||  
|-|-|  
|[DBCC dllname (FREE)](../../t-sql/database-console-commands/dbcc-dllname-free-transact-sql.md)|[DBCC HELP](../../t-sql/database-console-commands/dbcc-help-transact-sql.md)|  
|[DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md)|[DBCC TRACEOFF](../../t-sql/database-console-commands/dbcc-traceoff-transact-sql.md)|  
|[DBCC FREESESSIONCACHE](../../t-sql/database-console-commands/dbcc-freesessioncache-transact-sql.md)|[DBCC TRACEON](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)|  
|[DBCC FREESYSTEMCACHE](../../t-sql/database-console-commands/dbcc-freesystemcache-transact-sql.md)|[DBCC CLONEDATABASE](../../t-sql/database-console-commands/dbcc-clonedatabase-transact-sql.md) <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
  
  
