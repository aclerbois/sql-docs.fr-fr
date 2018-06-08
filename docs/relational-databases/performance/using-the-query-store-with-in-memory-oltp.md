---
title: Utilisation du magasin de requêtes avec OLTP en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Query Store, in-memory
ms.assetid: aae5ae6d-7c90-4661-a1c5-df704319888a
caps.latest.revision: 10
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ee44687081e26301eca66c04aed67d74417cfd1c
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="using-the-query-store-with-in-memory-oltp"></a>Utilisation du magasin de requêtes avec l'OLTP en mémoire
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Le magasin de requêtes vous permet de surveiller les performances du code compilé en mode natif pour les charges de travail exécutant l’OLTP en mémoire.  
Des statistiques de compilation et d’exécution sont collectées et exposées de la même manière que pour les charges de travail sur disque.   
Lorsque vous migrez vers l’OLTP en mémoire, vous pouvez continuer à utiliser les affichages du magasin de requêtes dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ainsi que les scripts personnalisés que vous avez développés pour les charges de travail sur disque avant la migration. Cela vous permet de réutiliser vos investissements d’apprentissage de la technologie du magasin de requêtes et les rend plus généralement utilisables pour la résolution des problèmes de tous les types de charges de travail.  
Pour obtenir des informations générales sur l'utilisation du magasin de requêtes, consultez [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).  
  
 L’utilisation du magasin de requêtes avec l'OLTP en mémoire ne nécessite pas de configuration de fonctionnalités supplémentaires. Lorsque vous l'activez sur votre base de données, il fonctionne pour tous les types de charges de travail.   
Toutefois, il existe quelques aspects spécifiques que les utilisateurs doivent connaître lors de l'utilisation du magasin de requêtes avec l'OLTP en mémoire :  
  
-   Lorsque le magasin de requêtes est activé, les requêtes, les plans et les statistiques de compilation sont collectées par défaut. Toutefois, la collecte de statistiques d’exécution n’est pas activée, sauf si vous l’activez explicitement avec [sys.sp_xtp_control_query_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).  
  
-   Lorsque vous attribuez la valeur 0 à *@new_collection_value* , le magasin de requêtes arrête de collecter les statistiques d’exécution pour la procédure concernée ou pour l’intégralité de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   La valeur configurée avec [sys.sp_xtp_control_query_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md) n’est pas rendue persistante. Veillez à vérifier et configurer à nouveau la collecte de statistiques après le redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Comme dans le cas de la collecte de statistiques de requête standard, les performances peuvent diminuer lorsque vous utilisez le magasin de requêtes pour effectuer le suivi de l'exécution des charges de travail. Il est possible de n’activer la collecte de statistiques que pour un sous-ensemble important de procédures stockées compilées en mode natif.  
  
-   Les requêtes et les plans sont capturés et stockés sur la première compilation native et mis à jour à chaque recompilation.  
  
-   Si vous avez activé le magasin de requêtes ou effacé son contenu une fois toutes les procédures stockées natives compilées, vous devez les recompiler manuellement afin qu’elles soient capturées par le magasin de requêtes. Ceci est également valable si vous avez supprimé manuellement des requêtes à l’aide de [sp_query_store_remove_query &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md) ou de [sp_query_store_remove_plan &#40;Transct-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md). Pour forcer la recompilation de la procédure, utilisez [sp_recompile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md).  
  
-   Le magasin de requêtes tire parti des mécanismes de génération de plan de l'OLTP en mémoire pour capturer le plan d'exécution de requête lors de la compilation. Le plan stocké est sémantiquement équivalent à celui que vous pourriez obtenir avec `SET SHOWPLAN_XML ON` , à une différence près : les plans du magasin de requêtes sont fractionnés et stockés par instruction individuelle.  
    
-   Lorsque vous exécutez le magasin de requêtes dans une base de données avec une charge de travail mixte, vous pouvez utiliser le champ **is_natively_compiled** à partir de [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md) pour trouver rapidement les plans de requête générés par la compilation de code en mode natif.  
  
-   Le magasin de requêtes (paramètre *QUERY_CAPTURE_MODE* dans l’instruction **ALTER TABLE**) n’affecte pas les requêtes à partir des modules compilés en mode natif, car elles sont toujours capturées, quelle que soit la valeur configurée. Cela comprend la définition de `QUERY_CAPTURE_MODE = NONE`.  
  
-   La durée de compilation des requêtes capturée par le magasin de requêtes comprend uniquement le temps passé à l'optimisation des requêtes, avant la génération du code natif. Plus précisément, elle n'inclut pas le temps de compilation du code C et de la génération des structures internes nécessaires à la génération du code C.  
  
-   Les métriques d’allocation de mémoire au sein de [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md) ne sont pas renseignées pour les requêtes compilées en mode natif. Elles ont toujours la valeur 0. Les colonnes d’allocation de mémoire sont les suivantes : avg_query_max_used_memory, last_query_max_used_memory, min_query_max_used_memory, max_query_max_used_memory et stdev_query_max_used_memory.  
  
## <a name="enabling-and-using-query-store-with-in-memory-oltp"></a>Activation et utilisation du magasin de requêtes avec l'OLTP en mémoire  
 L'exemple simple suivant illustre l'utilisation du magasin de requêtes avec l'OLTP en mémoire dans un scénario utilisateur de bout en bout. Dans cet exemple, nous supposons qu’une base de données (`MemoryOLTP`) est activée pour l’OLTP en mémoire.  
    Pour plus d’informations sur la configuration requise des tables optimisées en mémoire, consultez [Création d’une table optimisée en mémoire et d’une procédure stockée compilée en mode natif](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).  
  
```  
USE MemoryOLTP;  
GO  
  
-- Create a simple memory-optimized table   
CREATE TABLE dbo.Ord  
   (OrdNo INTEGER not null PRIMARY KEY NONCLUSTERED,   
    OrdDate DATETIME not null,   
    CustCode NVARCHAR(5) not null)   
WITH (MEMORY_OPTIMIZED=ON);  
GO  
  
-- Enable Query Store before native module compilation  
ALTER DATABASE MemoryOLTP SET QUERY_STORE = ON;  
GO  
  
-- Create natively compiled stored procedure  
CREATE PROCEDURE dbo.OrderInsert(@OrdNo integer, @CustCode nvarchar(5))  
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS   
    BEGIN ATOMIC WITH  
    (TRANSACTION ISOLATION LEVEL = SNAPSHOT,  
    LANGUAGE = N'English')  
  
    DECLARE @OrdDate DATETIME = GETDATE();  
    INSERT INTO dbo.Ord (OrdNo, CustCode, OrdDate)   
        VALUES (@OrdNo, @CustCode, @OrdDate);  
END;  
GO  
  
-- Enable runtime stats collection for queries from dbo.OrderInsert stored procedure  
DECLARE @db_id INT = DB_ID()  
DECLARE @proc_id INT = OBJECT_ID('dbo.OrderInsert');  
DECLARE @collection_enabled BIT;  
  
EXEC [sys].[sp_xtp_control_query_exec_stats] @new_collection_value = 1,   
    @database_id = @db_id, @xtp_object_id = @proc_id;  
  
-- Check the state of the collection flag  
EXEC sp_xtp_control_query_exec_stats @database_id = @db_id,   
    @xtp_object_id = @proc_id,   
    @old_collection_value= @collection_enabled output;  
SELECT @collection_enabled AS 'collection status';  
  
-- Execute natively compiled workload  
EXEC dbo.OrderInsert 1, 'A';  
EXEC dbo.OrderInsert 2, 'B';  
EXEC dbo.OrderInsert 3, 'C';  
EXEC dbo.OrderInsert 4, 'D';  
EXEC dbo.OrderInsert 5, 'E';  
  
-- Check Query Store Data  
-- Compile time data  
SELECT q.query_id, plan_id, object_id, query_hash, p.query_plan,  
    p.initial_compile_start_time, p.last_compile_start_time,   
    p.last_execution_time, p.avg_compile_duration,  
    p.last_force_failure_reason, p.force_failure_count  
FROM sys.query_store_query AS q  
JOIN sys.query_store_plan AS p   
    ON q.query_id = p.plan_id  
WHERE q.object_id = OBJECT_ID('dbo.OrderInsert');  
  
-- Get runtime stats  
-- Check count_executions field to verify that runtime statistics   
-- have been collected by the Query Store  
SELECT q.query_id, p.plan_id, object_id, rsi.start_time, rsi.end_time,    
    p.last_force_failure_reason, p.force_failure_count, rs.*  
FROM sys.query_store_query AS q  
JOIN sys.query_store_plan AS p   
    ON q.query_id = p.plan_id  
JOIN sys.query_store_runtime_stats AS rs   
    ON rs.plan_id = p.plan_id  
JOIN sys.query_store_runtime_stats_interval AS rsi   
    ON rs.runtime_stats_interval_id = rsi.runtime_stats_interval_id  
WHERE q.object_id = OBJECT_ID('dbo.OrderInsert');  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Création d’une table optimisée en mémoire et d’une procédure stockée compilée en mode natif](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md)   
 [Bonnes pratiques relatives au magasin de requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md)   
 [Procédures stockées du magasin de requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [Affichages catalogue du magasin de requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)  
  
  
