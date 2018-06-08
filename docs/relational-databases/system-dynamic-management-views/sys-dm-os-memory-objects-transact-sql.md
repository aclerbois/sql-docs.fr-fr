---
title: Sys.dm_os_memory_objects (Transact-SQL) | Documents Microsoft
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
- dm_os_memory_objects
- sys.dm_os_memory_objects
- sys.dm_os_memory_objects_TSQL
- dm_os_memory_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_objects dynamic management view
ms.assetid: 5688bcf8-5da9-4ff9-960b-742b671d7096
caps.latest.revision: 40
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d2cfed528dcbc58e4abed89ae1b76d0532d6bf6f
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmosmemoryobjects-transact-sql"></a>sys.dm_os_memory_objects (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne des objets mémoire qui sont actuellement alloués par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez utiliser **sys.dm_os_memory_objects** pour analyser l’utilisation de la mémoire et identifier les possibles fuites.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**memory_object_address**|**varbinary(8)**|Adresse de l'objet mémoire. N'accepte pas la valeur NULL.|  
|**parent_address**|**varbinary(8)**|Adresse de l'objet mémoire parent. Autorise la valeur NULL.|  
|**pages_allocated_count**|**int**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Nombre de pages allouées par cet objet. N'accepte pas la valeur NULL.|  
|**pages_in_bytes**|**bigint**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Quantité de mémoire, en octets, allouée par cette instance de l'objet mémoire. N'accepte pas la valeur NULL.|  
|**creation_options**|**int**|À usage interne uniquement Autorise la valeur NULL.|  
|**bytes_used**|**bigint**|À usage interne uniquement Autorise la valeur NULL.|  
|**type**|**nvarchar(60)**|Type d'objet mémoire.<br /><br /> Indique le composant auquel cet objet mémoire appartient ou la fonction de l'objet mémoire. Autorise la valeur NULL.|  
|**nom**|**varchar(128)**|À usage interne uniquement Autorise la valeur Null.|  
|**memory_node_id**|**smallint**|ID d'un nœud de mémoire utilisé par cet objet mémoire. N'accepte pas la valeur NULL.|  
|**creation_time**|**datetime**|À usage interne uniquement Autorise la valeur NULL.|  
|**max_pages_allocated_count**|**int**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Nombre maximum de pages allouées par cet objet mémoire. N'accepte pas la valeur NULL.|  
|**page_size_in_bytes**|**int**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Taille des pages, en octets, allouée par cet objet. N'accepte pas la valeur NULL.|  
|**max_pages_in_bytes**|**bigint**|Quantité de mémoire maximale utilisée par cet objet mémoire. N'accepte pas la valeur NULL.|  
|**page_allocator_address**|**varbinary(8)**|Adresse mémoire de l'allocateur de page. N'accepte pas la valeur NULL. Pour plus d’informations, consultez [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**creation_stack_address**|**varbinary(8)**|À usage interne uniquement Autorise la valeur NULL.|  
|**sequence_num**|**int**|À usage interne uniquement Autorise la valeur NULL.|  
|**partition_type**|**int**|Le type de partition :<br /><br /> 0 - objet de mémoire non-configurable en partition<br /><br /> 1 - objet de mémoire avec partitions, actuellement ne pas partitionnée<br /><br /> 2 - objet de mémoire avec partitions, partitionnée par nœud NUMA. Dans un environnement avec un seul nœud NUMA, cela équivaut à 1.<br /><br /> 3 - objet configurable en partition de mémoire partitionné par processeur.|  
|**contention_factor**|**real**|Valeur spécifiant la contention sur cet objet mémoire, avec 0, ce qui signifie aucune contention. La valeur est mise à jour chaque fois qu’un nombre spécifié d’allocations de mémoire ont été apportée à une contention réfléchissante pendant cette période. S’applique uniquement aux objets de mémoire thread-safe.|  
|**waiting_tasks_count**|**bigint**|Nombre d’attentes sur cet objet mémoire. Ce compteur est incrémenté chaque fois que la mémoire est allouée à partir de cet objet mémoire. L’incrément est le nombre de tâches en attente pour l’accès à cet objet mémoire. S’applique uniquement aux objets de mémoire thread-safe. Il s’agit d’une meilleure valeur effort sans garantie est correcte.|  
|**exclusive_access_count**|**bigint**|Spécifie la fréquence à laquelle cet objet mémoire a été accéder de façon exclusive. S’applique uniquement aux objets de mémoire thread-safe.  Il s’agit d’une meilleure valeur effort sans garantie est correcte.|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
 **partition_type**, **contention_factor**, **waiting_tasks_count**, et **exclusive_access_count** ne sont pas encore implémentées dans [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="remarks"></a>Notes  
 Les objets mémoire sont des segments. Ils fournissent des allocations qui ont une granularité plus fine que celles fournies par les régisseurs de mémoire. Les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent des objets mémoire au lieu de régisseurs de mémoire. Les objets mémoire utilisent l'interface d'allocation de page du régisseur de mémoire pour allouer les pages. Ils n'utilisent pas les interfaces de mémoire virtuelle ou partagée. Selon les modèles d'allocation, les composants peuvent créer différents types d'objets mémoire pour allouer des régions de taille arbitraire.  
  
 La taille de page type pour un objet mémoire est de 8 Ko. Toutefois, les objets mémoire incrémentiels peuvent avoir des tailles de page allant de 512 octets à 8 Ko.  
  
> [!NOTE]  
>  La taille de page n'est pas une allocation maximale. Il s'agit en fait de granularité d'allocation prise en charge par un allocateur de page et mise en œuvre par un régisseur de mémoire. Vous pouvez demander des allocations supérieures à 8 Ko d'objets mémoire.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne la quantité de mémoire allouée par chaque type d'objet mémoire.  
  
```  
SELECT SUM (pages_in_bytes) as 'Bytes Used', type   
FROM sys.dm_os_memory_objects  
GROUP BY type   
ORDER BY 'Bytes Used' DESC;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
  [Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)  
  
  


