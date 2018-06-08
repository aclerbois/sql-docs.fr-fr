---
title: Sys.dm_pdw_sql_requests (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
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
ms.assetid: 44e19609-902c-46cf-acdf-19ea75011365
caps.latest.revision: 10
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: aa8d0384eee7ab74c17797c9bd11ac65042d79c5
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmpdwsqlrequests-transact-sql"></a>Sys.dm_pdw_sql_requests (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Contient des informations sur toutes les distributions de requête de SQL Server dans le cadre d’une étape SQL dans la requête.  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Identificateur unique de la requête à laquelle appartient cette distribution de la requête SQL.<br /><br /> request_id, step_index et distribution_id forment la clé pour cette vue.|Consultez request_id dans [sys.dm_pdw_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Index de cette distribution est la partie de l’étape de requête.<br /><br /> request_id, step_index et distribution_id forment la clé pour cette vue.|Consultez step_index dans [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|pdw_node_id|**int**|Identificateur unique du nœud sur lequel cette distribution de la requête est exécutée.|Consultez node_id dans [sys.dm_pdw_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|distribution_id|**int**|Identificateur unique de la distribution sur lequel cette distribution de la requête est exécutée.<br /><br /> request_id, step_index et distribution_id forment la clé pour cette vue.|Consultez distribution_id dans [sys.pdw_distributions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-distributions-transact-sql.md). La valeur -1 pour les demandes qui s’exécutent dans l’étendue du nœud, pas l’étendue de la distribution.|  
|status|**nvarchar(32)**|État actuel de la distribution de la requête.|En attente, en cours d’exécution, échec, annulé, terminé, abandonné, CancelSubmitted|  
|error_id|**nvarchar(36)**|Identificateur unique de l’erreur associée à cette distribution requête, le cas échéant.|Consultez ID_erreur dans [sys.dm_pdw_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md). La valeur NULL si aucune erreur ne s’est produite.|  
|start_time|**datetime**|Heure de début de l’exécution de requête distribution.|Inférieure ou égale à l’heure actuelle et supérieure ou égale à start_time de l’étape de requête appartient cette distribution de la requête|  
|end_time|**datetime**|Heure à laquelle cette distribution requête exécution s’est terminée, a été annulée ou a échoué.|Supérieur ou égal à l’heure de début, ou la valeur NULL si la distribution de la requête est en cours ou en file d’attente.|  
|total_elapsed_time|**int**|Représente l’heure de que la distribution de la requête s’exécute, en millisecondes.|Supérieur ou égal à 0. Égal au delta de start_time et heure_fin terminé, échec ou annulé les distributions de requête.<br /><br /> Si total_elapsed_time dépasse la valeur maximale pour un entier, total_elapsed_time continueront à être la valeur maximale. Cette condition génère l’avertissement « la valeur maximale a été dépassée. »<br /><br /> La valeur maximale, en millisecondes est équivalente à 24.8 jours.|  
|row_count|**bigint**|Nombre de lignes modifiées ou lues par cette distribution de la requête.|-1 pour les opérations qui ne les modifiez pas ou renvoient des données, telles que CREATE TABLE et DROP TABLE.|  
|spid|**int**|Id de session sur l’instance de SQL Server en cours d’exécution la distribution de la requête.||  
|commande|**nvarchar(4000)**|Texte complet de la commande pour la distribution de cette requête.|Toute chaîne de requête ou de demande valide.|  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez la section valeurs de la vue système Maximum dans la [valeurs minimale et maximale (SQL Server PDW)](http://msdn.microsoft.com/en-us/5243f018-2713-45e3-9b61-39b2a57401b9) rubrique.  
  
## <a name="see-also"></a>Voir aussi  
 [Entrepôt de données SQL et les données en parallèle de l’entrepôt de vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
