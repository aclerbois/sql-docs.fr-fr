---
title: Sys.dm_exec_query_profiles (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_exec_query_profiles_TSQL
- sys.dm_exec_query_profiles_TSQL
- dm_exec_query_profiles
- sys.dm_exec_query_profiles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_profiles dynamic management view
ms.assetid: 54efc6cb-eea8-4f6d-a4d0-aa05eeb54081
caps.latest.revision: 19
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4b3acec798d858f31aac79231060d0533a3499b3
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecqueryprofiles-transact-sql"></a>sys.dm_exec_query_profiles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]

  Contrôle la progression en temps réel lorsqu'une requête est en cours d'exécution. Par exemple, utilisez cette vue de gestion dynamique pour déterminer la partie de la requête qui est lente. Joignez cette vue de gestion dynamique à d'autres vues de gestion dynamique système identifiées dans le champ de description. Ou bien, joignez cette vue de gestion dynamique à d'autres compteurs de performances (tels que l'analyseur de performances, xperf) à l'aide de colonnes timestamp.  
  
## <a name="table-returned"></a>Table retournée  
 Les compteurs retournés sont par opérateur par thread. Les résultats sont dynamiques et ne correspondent pas aux résultats des options existantes telles que SET STATISTICS XML ON qui crée uniquement une sortie quand la requête est terminée.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Identifie la session dans laquelle cette requête s'exécute. Référence dm_exec_sessions.session_id.|  
|request_id|**int**|Identifie la demande cible. Référence dm_exec_sessions.request_id.|  
|sql_handle|**varbinary(64)**|Identifie la requête cible. Référence dm_exec_query_stats.sql_handle.|  
|plan_handle|**varbinary(64)**|Identifie la requête cible (références dm_exec_query_stats.plan_handle).|  
|physical_operator_name|**nvarchar (256)**|Nom de l'opérateur physique.|  
|node_id|**int**|Identifie un nœud d'opérateur dans l'arborescence de requête.|  
|thread_id|**int**|Fait la distinction entre les threads (pour une requête parallèle) qui appartiennent au même nœud d'opérateur de requête.|  
|task_address|**varbinary(8)**|Identifie la tâche SQLOS utilisée par ce thread. Référence dm_os_tasks.task_address.|  
|row_count|**bigint**|Nombre de lignes retournées par l'opérateur jusqu'à présent.|  
|rewind_count|**bigint**|Nombre de rembobinages jusqu'à présent.|  
|rebind_count|**bigint**|Nombre de reliaisons jusqu'à présent.|  
|end_of_scan_count|**bigint**|Nombre de fins d'analyses jusqu'à présent.|  
|estimate_row_count|**bigint**|Nombre de lignes estimé. Il peut être utile pour comparer estimated_row_count à actual row_count réel.|  
|first_active_time|**bigint**|Heure du premier appel de l'opérateur en millisecondes.|  
|last_active_time|**bigint**|Heure du dernier appel de l'opérateur en millisecondes.|  
|open_time|**bigint**|Horodatage lors de l'ouverture (en millisecondes).|  
|first_row_time|**bigint**|Horodatage lors de l'ouverture de la première ligne (en millisecondes).|  
|last_row_time|**bigint**|Horodatage lors de l'ouverture de la dernière ligne (en millisecondes).|  
|close_time|**bigint**|Horodatage lors de la fermeture (en millisecondes).|  
|elapsed_time_ms|**bigint**|Temps total écoulé (en millisecondes) utilisé par les opérations du nœud cible jusqu'à présent.|  
|cpu_time_ms|**bigint**|Temps processeur total écoulé (en millisecondes) utilisé par les opérations du nœud cible jusqu'à présent.|  
|database_id|**smallint**|ID de la base de données qui contient l'objet sur lequel les opérations de lecture et d'écriture sont effectuées.|  
|object_id|**int**|Identificateur de l'objet sur lequel les opérations de lecture et écriture sont effectuées. Fait référence à sys.objects.object_id.|  
|index_id|**int**|Index (le cas échéant) dans lequel l'ensemble de lignes est ouvert.|  
|scan_count|**bigint**|Nombre d'analyses de tables ou d'index jusqu'à présent.|  
|logical_read_count|**bigint**|Nombre de lectures logiques jusqu'à présent.|  
|physical_read_count|**bigint**|Nombre de lectures physiques jusqu'à présent.|  
|read_ahead_count|**bigint**|Nombre de lectures anticipées jusqu'à présent.|  
|write_page_count|**bigint**|Nombre d'écritures de page jusqu'à présent en raison de débordement.|  
|lob_logical_read_count|**bigint**|Nombre de lectures logiques LOB jusqu'à présent.|  
|lob_physical_read_count|**bigint**|Nombre de lectures physiques LOB jusqu'à présent.|  
|lob_read_ahead_count|**bigint**|Nombre de lectures anticipées LOB jusqu'à présent.|  
|segment_read_count|**int**|Nombre de lectures anticipées de segment jusqu'à présent.|  
|segment_skip_count|**int**|Nombre de segments ignorés jusqu'à présent.| 
|actual_read_row_count|**bigint**|Nombre de lignes lues par un opérateur avant que le prédicat résiduel a été appliqué.| 
|estimated_read_row_count|**bigint**|**S’applique à :** compter [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] SP1. <br/>Nombre de lignes estimé pour être lus par un opérateur avant que le prédicat résiduel a été appliqué.|  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Si le nœud du plan de requête n'a pas d'E/S, tous les compteurs d'E/S sont définis sur NULL.  
  
 Les compteurs d'E/S indiqués par cette vue de gestion dynamique sont plus précis que ceux signalés par SET STATISTICS IO, notamment :  
  
-   SET STATISTICS IO regroupe les compteurs pour toutes les E/S dans une table donnée. Avec cette vue de gestion dynamique, vous obtenez des compteurs séparés pour chaque nœud du plan de requête qui effectue des E/S dans la table.  
  
-   En cas d'analyse parallèle, cette vue de gestion dynamique indique des compteurs pour chaque threads parallèles de l'analyse.
 
 En commençant par [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1, les statistiques d’exécution de requête standard infrastructure de profilage existe côte à côte avec une statistiques d’exécution de requête léger infrastructure de profilage. La nouvelle requête l’exécution du profilage infrastructure de statistiques réduit considérablement la charge de travail de collecte de statistiques de l’exécution de requête par l’opérateur, telles que nombre réel de lignes. Cette fonctionnalité peut être activée à l’aide global démarrage [indicateur de trace 7412](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md), ou est automatiquement activé lorsque des événements étendus query_thread_profile est utilisé.

>[!NOTE]
> Processeur et le temps écoulé ne sont pas pris en charge sous l’infrastructure de profilage de statistiques d’exécution de requête léger pour réduire l’impact sur les performances.

 SET STATISTICS XML ON et SET STATISTICS PROFILE ON toujours utiliser les statistiques d’exécution de requête hérité infrastructure de profilage.
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
   
## <a name="examples"></a>Exemples  
 Étape 1 : Connexion à une session dans laquelle vous prévoyez d’exécuter la requête vous analyserez avec sys.dm_exec_query_profiles. Pour configurer la requête pour le profilage utiliser SET STATISTICS PROFILE sur. Exécutez votre requête dans la même session.  
  
```  
--Configure query for profiling with sys.dm_exec_query_profiles  
SET STATISTICS PROFILE ON;  
GO  

--Or enable query profiling globally under SQL Server 2016 SP1 or above  
DBCC TRACEON (7412, -1);  
GO 
  
--Next, run your query in this session, or in any other session if query profiling has been enabled globally 
```  
  
 Étape 2 : Se connecter à une deuxième session différente de la session dans laquelle votre requête est en cours d’exécution.  
  
 L'instruction suivante résume l'avancement de la requête en cours d'exécution dans la session 54. Pour ce faire, elle calcule le nombre total de lignes de sortie de tous les threads pour chaque nœud, et compare ce nombre au nombre estimé de lignes de sortie pour ce nœud.  
  
```  
--Run this in a different session than the session in which your query is running. 
--Note that you may need to change session id 54 below with the session id you want to monitor.
SELECT node_id,physical_operator_name, SUM(row_count) row_count, 
  SUM(estimate_row_count) AS estimate_row_count, 
  CAST(SUM(row_count)*100 AS float)/SUM(estimate_row_count)  
FROM sys.dm_exec_query_profiles   
WHERE session_id=54
GROUP BY node_id,physical_operator_name  
ORDER BY node_id;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  

