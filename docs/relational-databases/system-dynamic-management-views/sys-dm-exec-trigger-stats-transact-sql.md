---
title: Sys.dm_exec_trigger_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/10/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_exec_trigger_stats
- dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_trigger_stats dynamic management function
ms.assetid: 863498b4-849c-434d-b748-837411458738
caps.latest.revision: 14
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 8dace7d8dccee89fadac0ff88fe7d1400b28b023
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexectriggerstats-transact-sql"></a>sys.dm_exec_trigger_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne les statistiques sur les performances des agrégats pour les déclencheurs mis en cache. La vue contient une ligne par déclencheur, et la durée de vie de la ligne correspond à celle pendant laquelle le déclencheur reste mis en cache. Lorsqu'un déclencheur est supprimé du cache, la ligne correspondante est éliminée de cette vue. À ce stade, un événement de trace SQL de statistiques de performances est déclenché comme **sys.dm_exec_query_stats**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de base de données dans lequel réside le déclencheur.|  
|**object_id**|**int**|Numéro d'identification d'objet du déclencheur.|  
|**type**|**char(2)**|Type de l'objet :<br /><br /> TA = Déclencheur assembly (CLR)<br /><br /> TR = Déclencheur SQL|  
|**Type_desc**|**nvarchar(60)**|Description du type d'objet :<br /><br /> CLR_TRIGGER<br /><br /> SQL_TRIGGER|  
|**sql_handle**|**varbinary(64)**|Il peut servir à mettre en corrélation avec des requêtes de **sys.dm_exec_query_stats** qui ont été exécutées à partir de ce déclencheur.|  
|**plan_handle**|**varbinary(64)**|Identificateur du plan en mémoire. Cet identificateur est temporaire et il reste constant uniquement tant que le plan est dans le cache. Cette valeur peut être utilisée avec la **sys.dm_exec_cached_plans** vue de gestion dynamique.|  
|**cached_time**|**datetime**|Heure à laquelle le déclencheur a été ajouté au cache.|  
|**last_execution_time**|**datetime**|Heure de dernière exécution du déclencheur.|  
|**execution_count**|**bigint**|Le nombre de fois que le déclencheur a été exécuté depuis sa dernière compilation.|  
|**total_worker_time**|**bigint**|La quantité totale de temps processeur, en microsecondes, consommé par les exécutions de ce déclencheur depuis sa compilation.|  
|**last_worker_time**|**bigint**|Temps processeur, en microsecondes, consommé lors de la dernière exécution du déclencheur.|  
|**min_worker_time**|**bigint**|Le temps processeur maximal, en microsecondes, consommé ce déclencheur lors d’une seule exécution.|  
|**max_worker_time**|**bigint**|Le temps processeur maximal, en microsecondes, consommé ce déclencheur lors d’une seule exécution.|  
|**total_physical_reads**|**bigint**|Nombre total de lectures physiques effectuées par les exécutions de ce déclencheur depuis sa compilation.|  
|**last_physical_reads**|**bigint**|Nombre de lectures physiques effectuées la dernière exécution du déclencheur.|  
|**min_physical_reads**|**bigint**|Le nombre minimal de lectures physiques par ce déclencheur effectuées lors d’une seule exécution.|  
|**max_physical_reads**|**bigint**|Le nombre maximal de lectures physiques par ce déclencheur effectuées lors d’une seule exécution.|  
|**total_logical_writes**|**bigint**|Nombre total d’écritures logiques effectuées par les exécutions de ce déclencheur depuis sa compilation.|  
|**last_logical_writes**|**bigint**|Nombre d’écritures logiques effectuées la dernière exécution du déclencheur.|  
|**min_logical_writes**|**bigint**|Le nombre minimal d’écritures logiques par ce déclencheur effectuées lors d’une seule exécution.|  
|**max_logical_writes**|**bigint**|Le nombre maximal d’écritures logiques par ce déclencheur effectuées lors d’une seule exécution.|  
|**total_logical_reads**|**bigint**|Nombre total de lectures logiques effectuées par les exécutions de ce déclencheur depuis sa compilation.|  
|**last_logical_reads**|**bigint**|Nombre de lectures logiques effectuées la dernière exécution du déclencheur.|  
|**min_logical_reads**|**bigint**|Le nombre minimal de lectures logiques effectuées ce déclencheur lors d’une seule exécution.|  
|**max_logical_reads**|**bigint**|Le nombre maximal de lectures logiques effectuées ce déclencheur lors d’une seule exécution.|  
|**total_elapsed_time**|**bigint**|Le temps écoulé total, en microsecondes, pour les exécutions de ce déclencheur.|  
|**last_elapsed_time**|**bigint**|Temps écoulé, en microsecondes, pour la dernière exécution de ce déclencheur.|  
|**min_elapsed_time**|**bigint**|Temps écoulé minimal, en microsecondes, pour toutes les exécutions de ce déclencheur.|  
|**max_elapsed_time**|**bigint**|Le temps écoulé maximal, en microsecondes, pour toutes les exécutions de ce déclencheur.| 
|**total_spills**|**bigint**|Le nombre total de pages répandues par l’exécution de ce déclencheur depuis sa compilation.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Le nombre de pages répandues la dernière exécution du déclencheur.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Le nombre minimal de pages de ce déclencheur a répandues jamais une seule exécution.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Le nombre maximal de pages que ce déclencheur a répandues jamais une seule exécution.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
  
## <a name="remarks"></a>Notes  
 Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d'exposer ces informations, chaque ligne contenant des données qui n'appartient pas au locataire connecté est filtrée.  

Les statistiques de la vue sont actualisées lorsqu'une requête est terminée.  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur les cinq principaux déclencheurs identifiés d'après le temps moyen écoulé.  
  
```sql  
SELECT TOP 5 d.object_id, d.database_id, DB_NAME(database_id) AS 'database_name',   
    OBJECT_NAME(object_id, database_id) AS 'trigger_name', d.cached_time,  
    d.last_execution_time, d.total_elapsed_time,   
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],   
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_trigger_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Voir aussi  
[Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
[Sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)   
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
  
