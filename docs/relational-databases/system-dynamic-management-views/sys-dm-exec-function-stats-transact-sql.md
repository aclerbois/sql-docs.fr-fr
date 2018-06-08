---
title: Sys.dm_exec_function_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016 Preview
f1_keywords:
- sys.dm_exec_function_stats
- sys.dm_exec_function_stats_tsql
- dm_exec_function_stats
- dm_exec_function_stats_tsql
helpviewer_keywords:
- sys.dm_exec_function_stats dynamic management view
ms.assetid: 4c3d6a02-08e4-414b-90be-36b89a0e5a3a
caps.latest.revision: 9
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ad955c58adf7a77c1fab429657d2d8331602fb21
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecfunctionstats-transact-sql"></a>Sys.dm_exec_function_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

  Retourne les statistiques de performances pour les fonctions de mise en cache des agrégats. La vue retourne une ligne pour chaque plan de la fonction mise en cache, et la durée de vie de la ligne est aussi longtemps que la fonction est mis en cache. Lorsqu’une fonction est supprimée du cache, la ligne correspondante est éliminée de cette vue. À ce stade, un événement de trace SQL de statistiques de performances est déclenché comme **sys.dm_exec_query_stats**. Retourne des informations sur les fonctions scalaires, notamment les fonctions de la mémoire et de fonctions scalaires CLR. Ne retourne pas d’informations sur les fonctions table.  
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d'exposer ces informations, chaque ligne contenant des données qui n'appartient pas au locataire connecté est filtrée.  
  
> [!NOTE]
> Une requête initiale de **sys.dm_exec_function_stats** peut produire des résultats inexacts s’il existe une charge de travail en cours d’exécution sur le serveur. Des résultats plus précis peuvent être déterminés en réexécutant la requête.  
  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de base de données dans laquelle la fonction réside.|  
|**object_id**|**int**|Numéro d’identification de la fonction.|  
|**type**|**char(2)**|Type de l’objet : FN = les fonctions scalaires|  
|**type_desc**|**nvarchar(60)**|Description du type d’objet : SQL_SCALAR_FUNCTION|  
|**sql_handle**|**varbinary(64)**|Il peut servir à mettre en corrélation avec des requêtes de **sys.dm_exec_query_stats** qui ont été exécutées à partir de cette fonction.|  
|**plan_handle**|**varbinary(64)**|Identificateur du plan en mémoire. Cet identificateur est temporaire et il reste constant uniquement tant que le plan est dans le cache. Cette valeur peut être utilisée avec la **sys.dm_exec_cached_plans** vue de gestion dynamique.<br /><br /> Sera toujours égal à 0 x 000 lorsqu’une table d’une mémoire optimisée de requêtes de fonction compilée en mode natif.|  
|**cached_time**|**datetime**|Heure à laquelle la fonction a été ajoutée au cache.|  
|**last_execution_time**|**datetime**|Dernière heure à laquelle la fonction a été exécutée.|  
|**execution_count**|**bigint**|Nombre de fois que la fonction a été exécutée depuis sa dernière compilation.|  
|**total_worker_time**|**bigint**|Total de temps processeur, en microsecondes, consommé par les exécutions de cette fonction, car il a été compilé.<br /><br /> Pour les fonctions compilées en mode natif, **total_worker_time** peut être inexact si plusieurs exécutions inférieur à 1 milliseconde.|  
|**last_worker_time**|**bigint**|Temps processeur, en microsecondes, consommé lors de la dernière exécution de la fonction. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Temps processeur minimal, en microsecondes, consommé lors d’une seule exécution cette fonction. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Temps processeur maximal, en microsecondes, consommé lors d’une seule exécution cette fonction. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Nombre total de lectures physiques effectuées par les exécutions de cette fonction, car il a été compilé.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_physical_reads**|**bigint**|Nombre de lectures physiques effectuées lors de la dernière exécution de la fonction.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_physical_reads**|**bigint**|Nombre minimal de lectures physiques par cette fonction effectuées lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_physical_reads**|**bigint**|Nombre maximal de lectures physiques par cette fonction effectuées lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_writes**|**bigint**|Nombre total d’écritures logiques effectuées par les exécutions de cette fonction, car il a été compilé.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_writes**|**bigint**|Numéro du nombre de pages du pool de mémoires tampons modifiées lors de la dernière exécution du plan. Si une page est déjà modifiée, aucune écriture n'est comptée.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_logical_writes**|**bigint**|Nombre minimal d’écritures logiques par cette fonction effectuées lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_writes**|**bigint**|Nombre maximal d’écritures logiques par cette fonction effectuées lors d’une seule exécution.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_logical_reads**|**bigint**|Nombre total de lectures logiques effectuées par les exécutions de cette fonction, car il a été compilé.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**last_logical_reads**|**bigint**|Nombre de lectures logiques effectuées lors de la dernière exécution de la fonction.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**min_logical_reads**|**bigint**|Nombre minimal de lectures logiques effectuées lors d’une seule exécution cette fonction.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**max_logical_reads**|**bigint**|Nombre maximal de lectures logiques effectuées lors d’une seule exécution cette fonction.<br /><br /> Sa valeur est toujours 0 lors de l'interrogation d'une table optimisée en mémoire.|  
|**total_elapsed_time**|**bigint**|Temps total écoulé, en microsecondes, pour les exécutions de cette fonction.|  
|**last_elapsed_time**|**bigint**|Temps écoulé, en microsecondes, pour la dernière exécution de cette fonction.|  
|**min_elapsed_time**|**bigint**|Temps minimal écoulé, en microsecondes, pour une exécution de cette fonction.|  
|**max_elapsed_time**|**bigint**|Temps maximal écoulé, en microsecondes, pour une exécution de cette fonction.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne des informations sur les dix fonctions supérieure identifié par le temps moyen écoulé.  
  
```  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'function name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_function_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 
 [sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)   
 [Sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  
  
  
