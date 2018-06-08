---
title: Sys.dm_os_memory_pools (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_os_memory_pools_TSQL
- dm_os_memory_pools
- dm_os_memory_pools_TSQL
- sys.dm_os_memory_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_pools dynamic management view
ms.assetid: 1ef053f3-c6f3-456e-82b6-26e4bd630d46
caps.latest.revision: 25
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 0de67bd53b568f77a16fd10903a5d0c3fddfce4c
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmosmemorypools-transact-sql"></a>sys.dm_os_memory_pools (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une ligne pour chaque objet stocké dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez utiliser cet vue pour surveiller l'utilisation de la mémoire cache et pour identifier les comportements de mise en cache incorrects.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_os_memory_pools**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**memory_pool_address**|**varbinary(8)**|Adresse mémoire de l'entrée qui représente le pool de mémoire. N'accepte pas la valeur NULL.|  
|**pool_id**|**int**|Identificateur d'un pool spécifique au sein d'un ensemble de pools. N'accepte pas la valeur NULL.|  
|**type**|**nvarchar(60)**|Type de pool d'objets. N'accepte pas la valeur NULL. Pour plus d’informations, consultez [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**nom**|**nvarchar (256)**|Nom affecté par le système à cet objet de mémoire. N'accepte pas la valeur NULL.|  
|**max_free_entries_count**|**bigint**|Nombre maximum d'entrées libres possibles dans un pool. N'accepte pas la valeur NULL.|  
|**free_entries_count**|**bigint**|Nombre d'entrées actuellement stockées dans le pool. N'accepte pas la valeur NULL.|  
|**removed_in_all_rounds_count**|**bigint**|Nombre d'entrées supprimées du pool depuis le démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. N'accepte pas la valeur NULL.|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="remarks"></a>Notes  
 Les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent parfois une structure de pools commune pour mettre en mémoire cache des types de données homogènes sans état (stateless). La structure des pools de mémoire est plus simple que celle des mémoires cache. Toutes les entrées des pools sont considérées égales. En interne, les pools sont des régisseurs de mémoire et peuvent être utilisés dans les mêmes situations.  
  
## <a name="see-also"></a>Voir aussi  
 
  [Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


