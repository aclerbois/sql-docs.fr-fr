---
title: Sys.dm_pdw_request_steps (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/01/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: cc563e88-0d34-436e-b914-b60d6ee0d50b
caps.latest.revision: 10
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 421d8db6ec62035b8605fde1293f727f7a6dc917
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmpdwrequeststeps-transact-sql"></a>Sys.dm_pdw_request_steps (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Contient des informations sur toutes les étapes qui composent une demande donnée ou une requête dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Il répertorie une ligne pour chaque étape de requête.  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|request_id et step_index composent la clé pour cette vue.<br /><br /> Id numérique unique associé à la demande.|Consultez request_id dans [sys.dm_pdw_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|request_id et step_index composent la clé pour cette vue.<br /><br /> La position de cette étape dans la séquence d’étapes qui composent la demande.|0 pour (n-1) pour une demande avec les n étapes suivantes.|  
|operation_type|**nvarchar(35)**|Type de l’opération représentée par cette étape.|**Opérations de plan de requête de DMS :** 'ReturnOperation', 'PartitionMoveOperation', 'MoveOperation', 'BroadcastMoveOperation', 'ShuffleMoveOperation', 'TrimMoveOperation', 'CopyOperation', 'DistributeReplicatedTableMoveOperation'<br /><br /> **Opérations de plan de requête SQL :** 'OnOperation', 'RemoteOperation'<br /><br /> **Autres opérations de plan de requête :** 'MetaDataCreateOperation', 'RandomIDOperation'<br /><br /> **Les opérations externes pour les lectures :** 'HadoopShuffleOperation', 'HadoopRoundRobinOperation', 'HadoopBroadcastOperation'<br /><br /> **Les opérations externes pour MapReduce :** 'HadoopJobOperation', 'HdfsDeleteOperation'<br /><br /> **Les opérations externes pour les écritures :** 'ExternalExportDistributedOperation', 'ExternalExportReplicatedOperation', 'ExternalExportControlOperation'<br /><br /> Pour plus d’informations, consultez « Présentation des Plans de requête » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].|  
|distribution_type|**nvarchar(32)**|Type de cette étape est soumises à distribution.|'AllNodes', 'AllDistributions', 'AllComputeNodes', 'ComputeNode', 'Distribution', 'SubsetNodes', 'SubsetDistributions', 'Unspecified'|  
|élément location_type|**nvarchar(32)**|Où l’étape est exécutée.|'Compute', 'Control', 'DMS'|  
|status|**nvarchar(32)**|État de cette étape.|En attente, en cours, terminé, échec, UndoFailed, PendingCancel, annulé, annulé, abandonnée|  
|error_id|**nvarchar(36)**|Id unique de l’erreur associée à cette étape, le cas échéant.|Consultez ID_erreur de [sys.dm_pdw_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md). NULL si aucune erreur ne s’est produite.|  
|start_time|**datetime**|Heure de début de l’exécution de l’étape.|Inférieure ou égale à l’heure actuelle et supérieurs ou égaux à end_compile_time de la requête à laquelle appartient cette étape. Pour plus d’informations sur les requêtes, consultez [sys.dm_pdw_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|end_time|**datetime**|Heure à laquelle cette étape exécution s’est terminée, a été annulée ou a échoué.|Inférieure ou égale à l’heure actuelle et supérieurs ou égaux à heure_début. La valeur NULL pour les étapes d’exécution ou en file d’attente.|  
|total_elapsed_time|**int**|Durée totale de l’étape de la requête s’exécute, en millisecondes.|Comprise entre 0 et la différence entre heure_fin et heure_début. 0 pour les étapes en attente.<br /><br /> Si total_elapsed_time dépasse la valeur maximale pour un entier, total_elapsed_time continueront à être la valeur maximale. Cette condition génère l’avertissement « la valeur maximale a été dépassée. »<br /><br /> La valeur maximale, en millisecondes est équivalente à 24.8 jours.|  
|row_count|**bigint**|Nombre total de lignes modifiées ou retournée par cette demande.|0 pour les étapes qui ne pas de modification ou de retourner des données. Sinon, d ' nombre de lignes affectées.|  
|commande|**nvarchar(4000)**|Contient le texte intégral de la commande de cette étape.|Toute chaîne de requête valide pour une étape. NULL lorsque l’opération est de type MetaDataCreateOperation. Tronquée à 4000 caractères.|  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez la section de valeurs de la vue système Maximum dans le « Minimum et Maximum valeurs » dans le [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Entrepôt de données SQL et les données en parallèle de l’entrepôt de vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
