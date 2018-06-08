---
title: Sys.dm_os_memory_cache_counters (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_os_memory_cache_counters_TSQL
- dm_os_memory_cache_counters_TSQL
- sys.dm_os_memory_cache_counters
- dm_os_memory_cache_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_counters dynamic management view
ms.assetid: ca7bd036-d661-4c17-b00a-e1a975bd8932
caps.latest.revision: 36
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 39796490aad883b0efc9c7eb8eba8a313e5772d8
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmosmemorycachecounters-transact-sql"></a>sys.dm_os_memory_cache_counters (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne un instantané de l'état d'un cache dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **Sys.dm_os_memory_cache_counters** fournit des informations d’exécution sur les entrées du cache allouées, leur utilisation et la source de la mémoire pour les entrées du cache.  
  
> **Remarque :** à appeler à partir [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_os_memory_cache_counters**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|Indique l'adresse (clé primaire) des compteurs associés à un cache en particulier. N'accepte pas la valeur NULL.|  
|**nom**|**nvarchar (256)**|Spécifie le nom du cache. N'accepte pas la valeur NULL.|  
|**type**|**nvarchar(60)**|Indique le type de cache associé à cette entrée. N'accepte pas la valeur NULL.|  
|**single_pages_kb**|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Taille, en kilo-octets, de la mémoire monopage allouée. Il s'agit de la quantité de mémoire allouée au moyen de l'allocateur monopage. Cela fait référence aux pages de 8 kilo-octets prélevées directement dans le pool de mémoires tampons de ce cache. N'accepte pas la valeur NULL.|  
|**pages_kb**|**bigint**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Spécifie la taille, en kilo-octets, de la mémoire allouée dans le cache. N'accepte pas la valeur NULL.|  
|**multi_pages_kb**|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Taille, en kilo-octets, de la mémoire multipage allouée. Il s'agit de la quantité de mémoire allouée à l'aide de l'allocateur de pages multiples du nœud de mémoire. Cette mémoire est allouée en dehors du pool de mémoires tampons ; elle tire parti de l'allocateur virtuel des nœuds mémoire. N'accepte pas la valeur NULL.|  
|**pages_in_use_kb**|**bigint**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Spécifie la taille, en kilo-octets, de la mémoire allouée et en cours d'utilisation dans le cache. Autorise la valeur NULL.  Les valeurs pour les objets de type `USERSTORE_<*>` ne font pas l'objet d'un suivi.  NULL est retourné pour chacune d'entre elles.|  
|**single_pages_in_use_kb**|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Taille, en kilo-octets, de la mémoire monopage utilisée. Autorise la valeur NULL. Ces informations ne sont pas suivies pour les objets de type USERSTORE_\<* > et ces valeurs sont NULL.|  
|**multi_pages_in_use_kb**|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Taille, en kilo-octets, de la mémoire multipage utilisée. Accepte la valeur NULL. Ces informations ne sont pas suivies pour les objets de type USERSTORE_\<* >, et ces valeurs sont NULL.|  
|**entries_count**|**bigint**|Indique le nombre d'entrées dans le cache. N'accepte pas la valeur NULL.|  
|**entries_in_use_count**|**bigint**|Indique le nombre d'entrées dans le cache en cours d'utilisation. N'accepte pas la valeur NULL.|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations 

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="see-also"></a>Voir aussi  
  [Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


