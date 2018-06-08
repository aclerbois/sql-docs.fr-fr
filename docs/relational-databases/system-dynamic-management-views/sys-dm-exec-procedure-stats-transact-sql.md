---
title: Sys.dm_exec_procedure_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/10/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats
- sys.dm_exec_procedure_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_procedure_stats dynamic management view
ms.assetid: ab8ddde8-1cea-4b41-a7e4-697e6ddd785a
caps.latest.revision: 24
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 0a2aadbc591400d899791759d25e8174104fd1d1
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecprocedurestats-transact-sql"></a>sys.dm_exec_procedure_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne les statistiques sur les performances des agrégats pour les procédures stockées mises en cache. La vue retourne une ligne pour chaque plan de procédure stockée mise en cache et la durée de vie de la ligne correspond à celle pendant laquelle la procédure stockée reste mise en cache. Lorsqu'une procédure stockée est supprimée du cache, la ligne correspondante est éliminée de cette vue. À ce stade, un événement de trace SQL de statistiques de performances est déclenché comme **sys.dm_exec_query_stats**.  
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d'exposer ces informations, chaque ligne contenant des données qui n'appartient pas au locataire connecté est filtrée.  
  
> [!NOTE]
> Une requête initiale de **sys.dm_exec_procedure_stats** peut produire des résultats inexacts s’il existe une charge de travail en cours d’exécution sur le serveur. Des résultats plus précis peuvent être déterminés en réexécutant la requête.  
  
> [!NOTE]
> Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_exec_procedure_stats**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de base de données dans lequel réside la procédure stockée.|  
|**object_id**|**int**|Numéro d'identification d'objet de la procédure stockée.|  
|**type**|**char(2)**|Type de l'objet :<br /><br /> P = Procédure stockée SQL<br /><br /> PC = Procédure stockée d'assembly (CLR)<br /><br /> X = Procédure stockée étendue|  
|**type_desc**|**nvarchar(60)**|Description du type d'objet :<br /><br /> SQL_STORED_PROCEDURE<br /><br /> CLR_STORED_PROCEDURE<br /><br /> EXTENDED_STORED_PROCEDURE|  
|**sql_handle**|**varbinary(64)**|Il peut servir à mettre en corrélation avec des requêtes de **sys.dm_exec_query_stats** qui ont été exécutées à partir de cette procédure stockée.|  
|**plan_handle**|**varbinary(64)**|Identificateur du plan en mémoire. Cet identificateur est temporaire et il reste constant uniquement tant que le plan est dans le cache. Cette valeur peut être utilisée avec la **sys.dm_exec_cached_plans** vue de gestion dynamique.<br /><br /> Sa valeur est toujours 0x000 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**cached_time**|**datetime**|Heure à laquelle la procédure stockée a été ajoutée au cache.|  
|**last_execution_time**|**datetime**|Heure de dernière exécution de la procédure stockée.|  
|**execution_count**|**bigint**|Le nombre de fois que la procédure stockée a été exécutée depuis sa dernière compilation.|  
|**total_worker_time**|**bigint**|La quantité totale de temps processeur, en microsecondes, consommé par les exécutions de cette procédure stockée depuis sa compilation.<br /><br /> Pour les procédures stockées compilées en mode natif, **total_worker_time** peut être inexact si plusieurs exécutions sont réalisées en moins d’une milliseconde.|  
|**last_worker_time**|**bigint**|Temps processeur, en microsecondes, consommé lors de la dernière exécution de la procédure stockée. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Le temps processeur minimal, en microsecondes, cette procédure stockée consommé une seule exécution. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Temps processeur maximal, en microsecondes, que cette procédure stockée a consommé une seule exécution. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Nombre total de lectures physiques effectuées par les exécutions de cette procédure stockée depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_physical_reads**|**bigint**|Nombre de lectures physiques effectuées à la dernière exécution de la procédure stockée.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_physical_reads**|**bigint**|Le nombre minimal de lectures physiques par cette procédure stockée effectuées lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_physical_reads**|**bigint**|Le nombre maximal de lectures physiques par cette procédure stockée effectuées lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_writes**|**bigint**|Nombre total d’écritures logiques effectuées par les exécutions de cette procédure stockée depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_writes**|**bigint**|Le nombre de pages de pool de mémoires tampons modifiées lors de la dernière exécution du plan. Si une page est déjà modifiée, aucune écriture n'est comptée.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_logical_writes**|**bigint**|Le nombre minimal d’écritures logiques par cette procédure stockée effectuées lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_writes**|**bigint**|Le nombre maximal d’écritures logiques par cette procédure stockée effectuées lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_reads**|**bigint**|Nombre total de lectures logiques effectuées par les exécutions de cette procédure stockée depuis sa compilation.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_reads**|**bigint**|Nombre de lectures logiques effectuées à la dernière exécution de la procédure stockée.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_logical_reads**|**bigint**|Le nombre minimal de lectures logiques par cette procédure stockée effectuées lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_reads**|**bigint**|Le nombre maximal de lectures logiques par cette procédure stockée effectuées lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_elapsed_time**|**bigint**|Le temps total écoulé, en microsecondes, pour les exécutions de cette procédure stockée.|  
|**last_elapsed_time**|**bigint**|Le temps écoulé, en microsecondes, pour la dernière exécution de cette procédure stockée.|  
|**min_elapsed_time**|**bigint**|Le temps minimal écoulé, en microsecondes, pour toutes les exécutions de cette procédure stockée.|  
|**max_elapsed_time**|**bigint**|Le temps maximal écoulé, en microsecondes, pour toutes les exécutions de cette procédure stockée.|  
|**total_spills**|**bigint**|Le nombre total de pages répandues par l’exécution de cette procédure stockée depuis sa compilation.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Le nombre de pages répandues la dernière exécution de la procédure stockée.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Le nombre minimal de pages par cette procédure stockée a jamais répandues lors d’une seule exécution.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Le nombre maximal de pages par cette procédure stockée a jamais répandues lors d’une seule exécution.<br /><br /> **S’applique aux**: compter [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**pdw_node_id**|**int**|L’identificateur du nœud qui se trouve sur cette distribution.<br /><br />**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
  
 <sup>1</sup> des procédures stockées compilées en mode natif lors de la collecte de statistiques est activée, des temps de travail est collecté en millisecondes. Si la requête s'exécute en moins d'une milliseconde, la valeur est 0.  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
   
## <a name="remarks"></a>Notes  
 Les statistiques de la vue sont mises à jour lorsqu'une exécution de procédure stockée se termine.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur les dix principales procédures stockées identifiées d'après le temps moyen écoulé.  
  
```sql  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'proc name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_procedure_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Voir aussi  
[Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)    
[sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)    
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)    
  
  


