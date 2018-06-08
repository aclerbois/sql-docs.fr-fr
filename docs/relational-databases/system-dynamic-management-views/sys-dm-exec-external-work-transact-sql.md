---
title: Sys.dm_exec_external_work (Transact-SQL) | Documents Microsoft
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
- DM_EXEC_EXTERNAL_WORK
- DM_EXEC_EXTERNAL_WORK_TSQL
- SYS.DM_EXEC_EXTERNAL_WORK_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_external_work management view
- dm_exec_external_work management view
- PolyBase,views
- PolyBase
ms.assetid: 7597d97b-1fde-4135-ac35-4af12968f300
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a8500beb0be1e231e2beae4cd56e719ad8951c72
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecexternalwork-transact-sql"></a>Sys.dm_exec_external_work (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md)]

  Retourne des informations sur la charge de travail par le processus de travail, sur chaque nœud de calcul.  
  
 Sys.dm_exec_external_work de requête pour identifier le travail utilisée pour communiquer avec la source de données externe (par exemple, Hadoop ou externes SQL Server).  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**nvarchar(32)**|Identificateur unique pour les requêtes PolyBase associé.|Consultez *request_ID* dans [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|step_index|**int**|La demande de que l’exécution de ce processus de travail.|Consultez *step_index* dans [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|dms_step_index|**int**|L’étape dans le plan DMS ce processus de travail est en cours d’exécution.|Consultez [sys.dm_exec_dms_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md).|  
|compute_node_id|**int**|Le nœud le processus de travail est en cours d’exécution.|Consultez [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|Type|**nvarchar(60)**|Le type de travail externe.|« Fractionnement de fichiers »|  
|work_id|**int**|ID de la division réelle.|Supérieur ou égal à 0.|  
|input_name|**nvarchar(4000)**|Nom de l’entrée à lire|Nom de fichier lors de l’utilisation de Hadoop.|  
|read_location|**bigint**|Décalage ou de lire l’emplacement.|Offset du fichier à lire.|  
|bytes_processed|**bigint**|Nombre total d’octets traité par ce processus de travail.|Supérieur ou égal à 0.|  
|length|**bigint**|Longueur de la division ou un bloc HDFS en cas de Hadoop|Définis par l’utilisateur. La valeur par défaut est 64M|  
|status|**nvarchar(32)**|État du processus de travail|En attente, traitement, terminé, échec, abandonnée|  
|start_time|**datetime**|Début du travail||  
|end_time|**datetime**|Fin du travail||  
|total_elapsed_time|**int**|Durée totale en millisecondes||  
  
## <a name="see-also"></a>Voir aussi  
 [PolyBase, résolution des problèmes avec les vues de gestion dynamique](http://msdn.microsoft.com/library/ce9078b7-a750-4f47-b23e-90b83b783d80)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
  
