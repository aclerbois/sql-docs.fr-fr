---
title: Sys.dm_os_dispatcher_pools (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_os_dispatcher_pools_TSQL
- dm_os_dispatcher_pools
- sys.dm_os_dispatcher_pools
- sys.dm_os_dispatcher_pools_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], views
- sys.dm_os_dispatcher_pools DMV
ms.assetid: b9edbc83-c6bc-4753-9bb5-a454cfe7d6bf
caps.latest.revision: 25
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 65f149f5fc1478fabaff8735c55f16fc8e32f421
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmosdispatcherpools-transact-sql"></a>sys.dm_os_dispatcher_pools (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur les pools de répartiteurs de la session. Les pools de répartiteurs sont des pools de threads utilisés par les composants système pour effectuer  un traitement en arrière-plan.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_os_dispatcher_pools**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|dispatcher_pool_address|**varbinary(8)**|Adresse du pool de répartiteurs. dispatcher_pool_address est unique. N'accepte pas la valeur NULL.|  
|Type|**nvarchar (256)**|Type du pool de répartiteurs. N'accepte pas la valeur NULL. Il existe deux types de pools de répartiteurs :<br /><br /> DISP_POOL_XE_ENGINE<br /><br /> DISP_POOL_XE_SESSION<br /><br /> La vue de gestion dynamique pour obtenir la liste complète de la requête|  
|name|**nvarchar (256)**|Nom du pool de répartiteurs. N'accepte pas la valeur NULL.|  
|dispatcher_count|**int**|Nombre de threads de répartiteurs actifs. N'accepte pas la valeur NULL.|  
|dispatcher_ideal_count|**int**|Nombre de threads de répartiteurs que le pool de répartiteurs peut être amené à utiliser. N'accepte pas la valeur NULL.|  
|dispatcher_timeout_ms|**int**|Durée, en millisecondes, pendant laquelle un répartiteur attend une nouvelle tâche avant de se fermer. N'accepte pas la valeur NULL.|  
|dispatcher_waiting_count|**int**|Nombre de threads de répartiteurs inactifs. N'accepte pas la valeur NULL.|  
|queue_length|**int**|Nombre d'éléments de travail attendant d'être gérés par le pool de répartiteurs. N'accepte pas la valeur NULL.|  
|pdw_node_id|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="see-also"></a>Voir aussi  
  
  


