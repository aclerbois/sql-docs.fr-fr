---
title: Sys.dm_os_memory_cache_entries (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_os_memory_cache_entries
- sys.dm_os_memory_cache_entries
- dm_os_memory_cache_entries_TSQL
- sys.dm_os_memory_cache_entries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_entries dynamic management view
ms.assetid: dd32be6b-10d1-4059-b4fd-0bf817f40d54
caps.latest.revision: 39
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: cb6c3adbf021eb08b8e76892217cf04c9caeafe0
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmosmemorycacheentries-transact-sql"></a>sys.dm_os_memory_cache_entries (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie des informations sur toutes les entrées en mémoire cache dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez cette vue pour suivre les entrées en mémoire cache en fonction des objets qui leur sont associés. Vous pouvez également utiliser cette vue pour obtenir des statistiques sur les entrées en mémoire cache.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_os_memory_cache_entries**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|Adresse du cache. N'accepte pas la valeur NULL.|  
|**nom**|**nvarchar (256)**|Nom du cache. N'accepte pas la valeur NULL.|  
|**type**|**varchar(60)**|Type de cache. N'accepte pas la valeur NULL.|  
|**entry_address**|**varbinary(8)**|Adresse du descripteur de l'entrée en mémoire cache. N'accepte pas la valeur NULL.|  
|**entry_data_address**|**varbinary(8)**|Adresse des données utilisateur dans l'entrée en mémoire cache.<br /><br /> 0x00000000 = L'adresse des données d'entrée n'est pas disponible.<br /><br /> N'accepte pas la valeur NULL.|  
|**in_use_count**|**int**|Nombre d'utilisateurs simultanés de cette entrée en mémoire cache. N'accepte pas la valeur NULL.|  
|**is_dirty**|**bit**|Indique si cette entrée du cache est marquée en vue d'une suppression. 1 = marquée pour la suppression. N'accepte pas la valeur NULL.|  
|**disk_ios_count**|**int**|Nombre d'E/S qui ont eu lieu pendant la création de cette entrée. N'accepte pas la valeur NULL.|  
|**context_switches_count**|**int**|Nombre de changements de contexte subis pendant la création de cette entrée. N'accepte pas la valeur NULL.|  
|**original_cost**|**int**|Coût initial de l'entrée. Cette valeur est une approximation du nombre d'entrées/sorties engagées, du coût des instructions processeur et de la quantité de mémoire utilisée par l'entrée Plus le coût est élevé, moins il y a de probabilités que l'élément soit supprimé de la mémoire cache. N'accepte pas la valeur NULL.|  
|**current_cost**|**int**|Coût actuel de l'entrée en mémoire cache. Cette valeur est mise à jour lors de la purge des entrées. Le coût actuel est réinitialisé à sa valeur d'origine lors de la réutilisation de l'entrée. N'accepte pas la valeur NULL.|  
|**memory_object_address**|**varbinary(8)**|Adresse de l'objet mémoire associé. Autorise la valeur NULL.|  
|**pages_allocated_count**|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Nombre de pages de 8 Ko pour stocker cette entrée en mémoire cache. N'accepte pas la valeur NULL.|  
|**pages_kb**|**bigint**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Quantité de mémoire, en kilo-octets (Ko), utilisée par cette entrée du cache.  N'accepte pas la valeur NULL.|  
|**entry_data**|**nvarchar(2048)**|Représentation en série de l'entrée en cache. Ces informations sont dépendantes du magasin du cache. Autorise la valeur NULL.|  
|**pool_id**|**int**|**S'applique à**: [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> ID de pool de ressources associé à l'entrée. Autorise la valeur NULL.<br /><br /> not katmai|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations 

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="see-also"></a>Voir aussi  
 
  [Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


