---
title: Sys.dm_os_hosts (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_os_hosts_TSQL
- dm_os_hosts
- dm_os_hosts_TSQL
- sys.dm_os_hosts
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_hosts dynamic management view
ms.assetid: a313ff3b-1fe9-421e-b94b-cea19c43b0e5
caps.latest.revision: 35
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 85a739c740cb16b89d728085cbc43c386cf03654
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmoshosts-transact-sql"></a>sys.dm_os_hosts (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie tous les hôtes actuellement inscrits dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette vue renvoie également les ressources qu'ils utilisent.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_os_hosts**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**host_address**|**varbinary(8)**|Adresse mémoire interne de l'objet hôte.|  
|**type**|**nvarchar(60)**|Type de composant hébergé. Par exemple :<br /><br /> SOSHOST_CLIENTID_SERVERSNI = interface SQL Server Native<br /><br /> SOSHOST_CLIENTID_SQLOLEDB = fournisseur OLE DB SQL Server Native Client<br /><br /> SOSHOST_CLIENTID_MSDART = temps d'exécution de Microsoft Data Access|  
|**nom**|**nvarchar(32)**|Nom de l'hôte.|  
|**enqueued_tasks_count**|**int**|Nombre total de tâches que cet hôte a placées dans des files d'attente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**active_tasks_count**|**int**|Nombre de tâches en cours d'exécution que cet hôte a placées dans des files d'attente.|  
|**completed_ios_count**|**int**|Nombre total d'entrées/sorties sollicitées et réalisées dans cet hôte.|  
|**completed_ios_in_bytes**|**bigint**|Nombre total d'octets transférés par les entrées/sorties de cet hôte.|  
|**active_ios_count**|**int**|Nombre total de demandes d'entrées/sorties relatives à cet hôte qui sont en attente d'exécution.|  
|**default_memory_clerk_address**|**varbinary(8)**|Adresse mémoire de l'objet régisseur de mémoire associé à cet hôte. Pour plus d’informations, consultez [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="remarks"></a>Notes  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet à des composants, tels qu'un fournisseur OLE DB, qui ne font pas partie de l'exécutable de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'allouer de la mémoire et de participer à des planifications non préemptives. Ces composants sont hébergés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et toutes les ressources allouées par ces composants sont suivies. L'hébergement permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de mieux rendre compte des ressources utilisées par des composants externes à l'exécutable de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|From|Pour|Relation|  
|----------|--------|------------------|  
|sys.dm_os_hosts. default_memory_clerk_address|sys.dm_os_memory_clerks. memory_clerk_address|un-à-un|  
|sys.dm_os_hosts. host_address|sys.dm_os_memory_clerks. host_address|un-à-un|  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant détermine la quantité totale de mémoire allouée par un composant hébergé.  
  
||  
|-|  
|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
  
```  
SELECT h.type, SUM(mc.pages_kb) AS commited_memory  
FROM sys.dm_os_memory_clerks AS mc   
INNER JOIN sys.dm_os_hosts AS h   
    ON mc.memory_clerk_address = h.default_memory_clerk_address  
GROUP BY h.type;  
```  
  
## <a name="see-also"></a>Voir aussi  

 [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)   
 [Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


