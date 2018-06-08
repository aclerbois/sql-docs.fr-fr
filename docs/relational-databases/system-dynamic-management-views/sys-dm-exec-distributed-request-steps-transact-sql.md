---
title: Sys.dm_exec_distributed_request_steps (Transact-SQL) | Documents Microsoft
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
- SYS.DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS_TSQL
- DM_EXEC_DISTRIBUTED_REQUEST_STEPS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_distributed_request_steps
- sys.dm_exec_distributed_request_steps management view
ms.assetid: 1954541d-b716-4e03-8fcc-7022f428e01d
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f226ff8c5f18a0e812c6a457fe3382cb8f7b8d84
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecdistributedrequeststeps-transact-sql"></a>Sys.dm_exec_distributed_request_steps (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md)]

  Contient des informations sur toutes les étapes qui composent une demande PolyBase donnée ou une requête. Il répertorie une ligne pour chaque étape de requête.  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**int**|execution_id et step_index composent la clé pour cette vue. Id numérique unique associé à la demande.|Consultez le code dans [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|step_index|**int**|La position de cette étape dans la séquence d’étapes qui composent la demande.|0 pour (n-1) pour une demande avec les n étapes suivantes.|  
|operation_type|**nvarchar(128)**|Type de l’opération représentée par cette étape.|'MoveOperation','OnOperation','RandomIDOperation','RemoteOperation','ReturnOperation','ShuffleMoveOperation','TempTablePropertiesOperation','DropDiagnosticsNotifyOperation', ‘HadoopShuffleOperation', ‘HadoopBroadCastOperation', ‘HadoopRoundRobinOperation'|  
|distribution_type|**nvarchar(32)**|Où l’étape s’exécute.|« AllComputeNodes ',' AllDistributions', 'ComputeNode', 'Distribution', 'AllNodes', 'SubsetNodes', 'SubsetDistributions',' non définie ».|  
|élément location_type|**nvarchar(32)**|Où l’étape s’exécute.|« Compute', 'Head' ou « DMS ». Toutes les étapes de déplacement des données indiquent « DMS ».|  
|status|**nvarchar(32)**|État de cette étape.|« Suspendu », « Running », « Complète », 'Échec', 'UndoFailed', 'PendingCancel', 'annulée', 'Annuler', 'Abandonné'|  
|error_id|**nvarchar(36)**|Id unique de l’erreur associée à cette étape, le cas échéant|Consultez l’id de [sys.dm_exec_compute_node_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md), la valeur NULL si aucune erreur ne s’est produite.|  
|start_time|**datetime**|Heure de début de l’exécution de l’étape|Inférieure ou égale à l’heure actuelle et supérieurs ou égaux à end_compile_time de la requête à laquelle appartient cette étape.|  
|end_time|**datetime**|Heure à laquelle cette étape exécution s’est terminée, a été annulée ou a échoué.|Inférieure ou égale à l’heure actuelle et supérieurs ou égaux à heure_début, la valeur NULL pour les étapes d’exécution ou en file d’attente.|  
|total_elapsed_time|**int**|Quantité totale de temps à qu'exécuter l’étape de requête, en millisecondes|Comprise entre 0 et la différence entre heure_fin et heure_début. 0 pour les étapes en attente.|  
|row_count|**bigint**|Nombre total de lignes modifiées ou retournée par cette demande|0 pour les étapes qui ne pas de modification ou de retourner des données, le nombre de lignes affectées dans le cas contraire. La valeur -1 pour obtenir des instructions DMS.|  
|commande|nvarchar(4000)|Contient le texte intégral de la commande de cette étape.|Toute chaîne de requête valide pour une étape. Tronquée à 4000 caractères.|  
  
## <a name="see-also"></a>Voir aussi  
 [PolyBase, résolution des problèmes avec les vues de gestion dynamique](http://msdn.microsoft.com/library/ce9078b7-a750-4f47-b23e-90b83b783d80)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
  
