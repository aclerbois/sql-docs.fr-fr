---
title: Sys.dm_exec_background_job_queue_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_exec_background_job_queue_stats
- sys.dm_exec_background_job_queue_stats
- dm_exec_background_job_queue_stats_TSQL
- sys.dm_exec_background_job_queue_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_background_job_queue_stats dynamic management view
ms.assetid: 27f62ab5-46c4-417e-814d-8d6437034d1c
caps.latest.revision: 27
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: fed35b861dfe919d4b7f49b11f98f205ea16b71b
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecbackgroundjobqueuestats-transact-sql"></a>sys.dm_exec_background_job_queue_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une ligne qui fournit des statistiques agrégées pour chaque travail du processeur de requêtes soumis pour une exécution asynchrone (en arrière-plan).  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_exec_background_job_queue_stats**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**queue_max_len**|**int**|Longueur maximale de la file d'attente.|  
|**enqueued_count**|**int**|Nombre de demandes placées dans la file d'attente.|  
|**started_count**|**int**|Nombre de demandes dont l'exécution a commencé.|  
|**ended_count**|**int**|Nombre de demandes dont l'exécution s'est terminée sur un succès ou un échec.|  
|**failed_lock_count**|**int**|Nombre de demandes ayant échoué à cause d'un problème de contention de verrouillage ou de blocage.|  
|**failed_other_count**|**int**|Nombre de demandes ayant échoué pour d'autres raisons.|  
|**failed_giveup_count**|**int**|Nombre de demandes ayant échoué parce que le nombre limite de tentatives était atteint.|  
|**enqueue_failed_full_count**|**int**|Nombre de tentatives d'empilement ayant échoué parce que la file d'attente était saturée.|  
|**enqueue_failed_duplicate_count**|**int**|Nombre de tentatives d'empilement en double.|  
|**elapsed_avg_ms**|**int**|Temps moyen écoulé par demande en millisecondes.|  
|**elapsed_max_ms**|**int**|Temps écoulé pour la demande la plus longue, en millisecondes.|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="remarks"></a>Notes  
 Seules les informations pour les travaux de mise à jour des statistiques asynchrone apparaissent dans cette vue. Pour plus d’informations sur les statistiques de mise à jour asynchrone, consultez [statistiques](../../relational-databases/statistics/statistics.md).  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="examples"></a>Exemples  
  
### <a name="a-determining-the-percentage-of-failed-background-jobs"></a>A. Détermination du pourcentage des travaux d'arrière-plan ayant échoué  
 L'exemple suivant retourne le pourcentage de travaux d'arrière-plan ayant échoué pour toutes les requêtes exécutées.  
  
```  
SELECT   
        CASE ended_count WHEN 0   
                THEN 'No jobs ended'   
                ELSE CAST((failed_lock_count + failed_giveup_count + failed_other_count) / CAST(ended_count AS float) * 100 AS varchar(20))   
        END AS [Percent Failed]  
FROM sys.dm_exec_background_job_queue_stats;  
GO  
```  
  
### <a name="b-determining-the-percentage-of-failed-enqueue-attempts"></a>B. Détermination du pourcentage des tentatives d'empilement ayant échoué  
 L'exemple suivant retourne le pourcentage des tentatives d'empilement ayant échoué pour toutes les requêtes exécutées.  
  
```  
SELECT   
        CASE enqueued_count WHEN 0   
                THEN 'No jobs posted'   
                ELSE CAST((enqueue_failed_full_count + enqueue_failed_duplicate_count) / CAST(enqueued_count AS float) * 100 AS varchar(20))   
        END AS [Percent Enqueue Failed]  
FROM sys.dm_exec_background_job_queue_stats;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


