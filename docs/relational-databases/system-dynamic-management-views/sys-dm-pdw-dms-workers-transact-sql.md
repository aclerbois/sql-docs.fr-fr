---
title: Sys.dm_pdw_dms_workers (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/07/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 0a284d18-3c46-4ffa-bcc9-689e660ee8b4
caps.latest.revision: 7
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 68d4b12395172f8ea5c820e745abf2a3f5f4c7c8
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmpdwdmsworkers-transact-sql"></a>Sys.dm_pdw_dms_workers (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Contient des informations sur tous les traitements étapes en DMS.  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Que ce processus de travail DMS fait partie d’une requête.<br /><br /> request_id, step_index et dms_step_index forment la clé pour cette vue.|Consultez request_id dans [sys.dm_pdw_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Étape de que ce processus de travail DMS fait partie de la requête.<br /><br /> request_id, step_index et dms_step_index forment la clé pour cette vue.|Consultez step_index dans [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|dms_step_index|**int**|L’étape dans le plan DMS ce processus de travail est en cours d’exécution.<br /><br /> request_id, step_index et dms_step_index forment la clé pour cette vue.||  
|pdw_node_id|**int**|Nœud qui le processus de travail est en cours d’exécution.|Consultez node_id dans [sys.dm_pdw_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|distribution_id|**Int**|Distribution du travail s’exécute, le cas échéant.|Consultez distribution_id dans [sys.pdw_distributions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-distributions-transact-sql.md).|  
|Type|**nvarchar(32)**|Type de thread de travail DMS que cette entrée représente.|'DIRECT_CONVERTER', 'DIRECT_READER', 'FILE_READER', 'HASH_CONVERTER', 'HASH_READER', 'ROUNDROBIN_CONVERTER', 'EXPORT_READER', 'EXTERNAL_READER', 'EXTERNAL_WRITER', 'PARALLEL_COPY_READER', « REJECT_WRITER », « WRITER »|  
|status|**nvarchar(32)**|État du processus de travail DMS.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|bytes_per_sec|**bigint**|Débit de lecture ou d’écriture de la dernière seconde.|Supérieur ou égal à 0. NULL si la requête a été annulée ou a échoué avant d’exécuter le processus de travail.|  
|bytes_processed|**bigint**|Nombre total d’octets traité par ce processus de travail.|Supérieur ou égal à 0. NULL si la requête a été annulée ou a échoué avant d’exécuter le processus de travail.|  
|rows_processed|**bigint**|Nombre de lignes lues ou écrites pour ce processus de travail.|Supérieur ou égal à 0. NULL si la requête a été annulée ou a échoué avant d’exécuter le processus de travail.|  
|start_time|**datetime**|Heure de début de l’exécution de ce processus de travail.|Supérieur ou égal à l’heure de début de l’étape de requête qu'auquel appartient ce processus de travail. Consultez [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|end_time|**datetime**|Heure à laquelle l’exécution s’est terminée, a échoué ou a été annulée.|NULL pour les traitements en cours ou en file d’attente. Sinon, supérieur à heure_début.|  
|total_elapsed_time|**int**|Temps total passé à l’exécution, en millisecondes.|Supérieur ou égal à 0.<br /><br /> Temps total écoulé depuis le système de démarrer ou redémarrer. Si total_elapsed_time dépasse la valeur maximale pour un entier (24.8 jours en millisecondes), cela entraînera l’échec de matérialisation en raison d’un dépassement de capacité.<br /><br /> La valeur maximale, en millisecondes est équivalente à 24.8 jours.|  
|cpu_time|**bigint**|Temps processeur consommé par ce processus de travail, en millisecondes.|Supérieur ou égal à 0.|  
|query_time|**int**|Période de temps avant SQL démarre renvoyant des lignes au thread, en millisecondes.|Supérieur ou égal à 0.|  
|buffers_available|**int**|Nombre de tampons inutilisés.| NULL si la requête a été annulée ou a échoué avant d’exécuter le processus de travail.|  
|sql_spid|**int**|Id de session sur l’instance de SQL Server effectuer le travail de ce travail DMS.||  
|dms_cpid|**int**|ID du processus du thread réel en cours d’exécution.||  
|error_id|**nvarchar(36)**|Identificateur unique de l’erreur qui s’est produite lors de l’exécution de ce processus de travail, le cas échéant.|Consultez ID_erreur dans [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|source_info|**nvarchar(4000)**|Pour un lecteur, la spécification des tables sources et des colonnes.||  
|destination_info|**nvarchar(4000)**|Pour un writer, la spécification de tables de destination.||  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez [valeurs de la vue système Maximum](http://msdn.microsoft.com/en-us/5243f018-2713-45e3-9b61-39b2a57401b9).  
  
## <a name="see-also"></a>Voir aussi  
 [Entrepôt de données SQL et les données en parallèle de l’entrepôt de vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
