---
title: Sys.dm_os_memory_brokers (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_os_memory_brokers
- dm_os_memory_brokers_TSQL
- sys.dm_os_memory_brokers_TSQL
- dm_os_memory_brokers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_brokers dynamic management view
ms.assetid: 48dd6ad9-0d36-4370-8a12-4921d0df4b86
caps.latest.revision: 20
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: c0d32ef0b368c0e3c5a0de1cbb9c0a2f60110050
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmosmemorybrokers-transact-sql"></a>sys.dm_os_memory_brokers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Les allocations qui sont internes à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent le gestionnaire de mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La différence entre les compteurs de mémoire de processus à partir de suivi **sys.dm_os_process_memory** et compteurs internes peuvent indiquer l’utilisation de la mémoire à partir des composants externes dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espace mémoire.  
  
 Les gestionnaires d'allocation mémoire répartissent équitablement les allocations de mémoire entre les différents composants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en fonction de l'utilisation actuelle et prévue. Ils n'effectuent pas d'allocations. Ils n'effectuent que le suivi des allocations pour calculer la distribution.  
  
 Le tableau suivant fournit des informations sur les gestionnaires d'allocation mémoire.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_os_memory_brokers**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**pool_id**|**int**|ID du pool de ressources s'il est associé à un pool du gouverneur de ressources.|  
|**memory_broker_type**|**nvarchar(60)**|Type de gestionnaire d'allocation mémoire. Il existe actuellement trois types de gestionnaires d’allocation de mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], répertoriées ci-dessous, avec leurs descriptions.<br /><br /> **MEMORYBROKER_FOR_CACHE** : quantité de mémoire allouée pour une utilisation par les objets mis en cache.<br /><br /> **MEMORYBROKER_FOR_STEAL** : mémoire occultée du pool de mémoires tampons. Cette mémoire ne peut pas être réutilisée par d'autres composants tant qu'elle n'est pas libérée par le propriétaire actuel.<br /><br /> **MEMORYBROKER_FOR_RESERVE** : mémoire réservée pour une utilisation future par les demandes en cours d’exécution.|  
|**allocations_kb**|**bigint**|Quantité de mémoire, en kilo-octets (Ko), allouée à ce type de gestionnaire d'allocation mémoire.|  
|**allocations_kb_per_sec**|**bigint**|Taux d'allocations de mémoire, en kilo-octets (Ko) par seconde. Cette valeur peut être négative pour les désallocations de mémoire.|  
|**predicted_allocations_kb**|**bigint**|Quantité prédite de mémoire allouée par le gestionnaire d'allocation mémoire. Cette valeur est basée sur le modèle d'utilisation de la mémoire.|  
|**target_allocations_kb**|**bigint**|Quantité recommandée de mémoire allouée, en kilo-octet (Ko), basée sur les paramètres actuels et le modèle d'utilisation de la mémoire. Ce gestionnaire d'allocation mémoire doit augmenter ou diminuer la quantité pour obtenir cette valeur.|  
|**future_allocations_kb**|**bigint**|Nombre projeté d'allocations, en kilo-octet (Ko), qui seront effectuées dans les prochaines secondes.|  
|**overall_limit_kb**|**bigint**|Quantité maximale de mémoire, en kilo-octets (Ko), que le gestionnaire d'allocation mémoire peut allouer.|  
|**last_notification**|**nvarchar(60)**|Recommandation relative à l'utilisation de la mémoire basée sur les paramètres actuels et le modèle d'utilisation. Les valeurs valides sont les suivantes :<br /><br /> grow<br /><br /> shrink<br /><br /> stable|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
  
## <a name="see-also"></a>Voir aussi  

  [Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


