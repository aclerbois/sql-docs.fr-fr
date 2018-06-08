---
title: Sys.dm_pdw_wait_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
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
ms.assetid: cfb8d905-c34f-44de-9574-dde81e170916
caps.latest.revision: 7
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: b0998939599ba5f793eeb8a4784246a75b68198a
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmpdwwaitstats-transact-sql"></a>Sys.dm_pdw_wait_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Contient des informations relatives à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] état du système d’exploitation associées aux instances en cours d’exécution sur les différents nœuds. Pour obtenir la liste des types d’attentes et leur description, consultez [sys.dm_os_wait_stats](http://msdn.microsoft.com/en-us/library/ms179984\(v=sql.120\).aspx).  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|**pdw_node_id**|**int**|ID du nœud que cette entrée fait référence.||  
|**wait_name**|**nvarchar(255)**|Nom du type d'attente.||  
|**max_wait_time**|**bigint**|Délai d’attente maximal de ce type d’attente.||  
|**request_count**|**bigint**|Nombre d’attentes de cette attente en attente de type.||  
|**signal_time**|**bigint**|Différence entre le moment où le thread qui attend a été signalé et le moment où il a commencé à s'exécuter.||  
|**completed_count**|**bigint**|Nombre d’attentes de ce type effectuées depuis le dernier serveur de redémarrer.||  
|**wait_time**|**bigint**|Temps d’attente total pour ce type d’attente dans millisecons. Temps inclusif de signal_time.||  
  
## <a name="see-also"></a>Voir aussi  
 [Entrepôt de données SQL et les données en parallèle de l’entrepôt de vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)   
 [Sys.dm_pdw_waits &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql.md)  
  
  
