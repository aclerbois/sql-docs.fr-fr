---
title: Sys.dm_pdw_hadoop_operations (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
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
ms.assetid: 5d2337d4-e2c7-48de-9c26-cdc7e6eb5d55
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: a9158bacb9959691118c356e4c742dc62e862346
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmpdwhadoopoperations-transact-sql"></a>Sys.dm_pdw_hadoop_operations (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Contient une ligne pour chaque tâche Map/reduce qui est envoyée vers le bas dans Hadoop dans le cadre de l’exécution une [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] requête sur une table Hadoop externe. Chaque tâche Map/reduce représente un des prédicats de la requête. Cela est utilisé uniquement lors de l’insertion d’un prédicat est activée pour les requêtes sur des tables externes Hadoop.  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|ID pour cette opération Hadoop externe.|Identique à l’ID dans [sys.dm_pdw_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Index de l’étape de requête qui fait référence à cette opération Hadoop.|Identique à step_index dans [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|operation_type|**nvarchar(255)**|Décrit le type d’opération externe.|'Opération externe Hadoop'|  
|nom_opération|**nvarchar(4000)**|L’ID de tâche pour une tâche Map/reduce. Ceci est retourné par Hadoop après [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] soumet le travail.||  
|map_progress|**float**|Le pourcentage de données d’entrée qui a été consommés jusque-là par le travail de la carte.|Nombre à virgule flottante compris entre et y compris 0 et 100.|  
|reduce_progress|**int**|Le pourcentage de la tâche de réduction est terminée...|Nombre à virgule flottante compris entre et y compris 0 et 100.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90)  
  
  
