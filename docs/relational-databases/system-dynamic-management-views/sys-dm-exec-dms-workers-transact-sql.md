---
title: Sys.dm_exec_dms_workers (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SYS.DM_EXEC_DMS_WORKERS_TSQL
- DM_EXEC_DMS_WORKERS_TSQL
- DM_EXEC_DMS_WORKERS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_dms_workers management view
- sys.dm_exec_dms_workers management view
ms.assetid: f468da29-78c3-4f10-8a3c-17905bbf46f2
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 940857ee9b723eed5adf1626d8db8c7868d30e7c
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecdmsworkers-transact-sql"></a>Sys.dm_exec_dms_workers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md)]

  Contient des informations sur tous les traitements étapes en DMS.  
  
 Cette vue affiche les données pour les requêtes de 1000 derniers et les demandes actives ; demandes actives ont toujours des données présentes dans cette vue.  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**nvarchar(32)**|Les requêtes que ce processus de travail DMS est la partie of.request_id, step_index, et dms_step_index forment la clé pour cette vue.||  
|step_index|**int**|Étape de que ce processus de travail DMS fait partie de la requête.|Consultez les index d’étape dans [sys.dm_exec_distributed_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql.md).|  
|dms_step_index|**int**|L’étape dans le plan DMS ce processus de travail est en cours d’exécution.|Consultez [sys.dm_exec_dms_workers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md)|  
|compute_node_id|**int**|Nœud qui le processus de travail est en cours d’exécution.|Consultez [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|distribution_id|**int**|||  
|Type|**nvarcha(32)**|||  
|status|**nvarchar(32)**|État de cette étape.|« Suspendu », « Running », « Complète », 'Échec', 'UndoFailed', 'PendingCancel', 'annulée', 'Annuler', 'Abandonné'|  
|bytes_per_sec|**bigint**|||  
|bytes_processed|**bigint**|||  
|rows_processed|**bigint**|||  
|start_time|**datetime**|Heure de début de l’exécution de l’étape|Inférieure ou égale à l’heure actuelle et supérieurs ou égaux à end_compile_time de la requête à laquelle appartient cette étape.|  
|end_time|**datetime**|Heure à laquelle cette étape exécution s’est terminée, a été annulée ou a échoué.|Inférieure ou égale à l’heure actuelle et supérieurs ou égaux à heure_début, la valeur NULL pour les étapes d’exécution ou en file d’attente.|  
|total_elapsed_time|**int**|Quantité totale de temps à qu'exécuter l’étape de requête, en millisecondes|Comprise entre 0 et la différence entre heure_fin et heure_début. 0 pour les étapes en attente.|  
|cpu_time|**bigint**|||  
|query_time|**int**|||  
|buffers_available|**int**|||  
|dms_cpid|**int**|||  
|sql_spid|**int**|||  
|error_id|**nvarchar(36)**|||  
|source_info|**nvarchar(4000)**|||  
|destination_info|**nvarchar(4000)**|||  
|commande|**nvarchar(4000)**|||  
  
## <a name="see-also"></a>Voir aussi  
 [PolyBase, résolution des problèmes avec les vues de gestion dynamique](http://msdn.microsoft.com/library/ce9078b7-a750-4f47-b23e-90b83b783d80)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
  
