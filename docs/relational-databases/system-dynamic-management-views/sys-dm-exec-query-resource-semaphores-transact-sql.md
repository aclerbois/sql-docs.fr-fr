---
title: Sys.dm_exec_query_resource_semaphores (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_query_resource_semaphores_TSQL
- dm_exec_query_resource_semaphores_TSQL
- sys.dm_exec_query_resource_semaphores
- dm_exec_query_resource_semaphores
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_resource_semaphores dynamic management view
ms.assetid: e43a2aa9-dd52-4c89-911e-1a7d05f7ffbb
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 4eb917c4aded6443a6a7d09e6c62f71dfa416261
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecqueryresourcesemaphores-transact-sql"></a>sys.dm_exec_query_resource_semaphores (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne les informations relatives à l'état actuel du sémaphore de ressource de requête dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **Sys.dm_exec_query_resource_semaphores** fournit l’état de la mémoire d’exécution de requête général et vous permet de déterminer si le système peut accéder à suffisamment de mémoire. Cette vue vient s’ajouter aux informations de mémoire obtenues à partir de [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md) pour fournir une image complète de l’état de la mémoire. **Sys.dm_exec_query_resource_semaphores** renvoie une ligne pour le sémaphore de ressource ordinaire et une autre ligne pour le sémaphore de ressource de petites requêtes. Il existe deux spécifications d’un sémaphore de petites requêtes :  
  
-   L’allocation de mémoire demandée doit être inférieure à 5 Mo  
  
-   Le coût de requête doit être inférieure à 3 unités de coût  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_exec_query_resource_semaphores**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**resource_semaphore_id**|**smallint**|ID non unique du sémaphore de ressource. 0 pour le sémaphore de ressource ordinaire et 1 pour le sémaphore de ressource de petites requêtes.|  
|**target_memory_kb**|**bigint**|Cible d'allocation d'utilisation en kilo-octets.|  
|**max_target_memory_kb**|**bigint**|Cible maximale potentielle en kilo-octets. NULL pour le sémaphore de ressource de petites requêtes.|  
|**total_memory_kb**|**bigint**|Mémoire détenue par le sémaphore de ressource, en kilo-octets. Si le système est soumis à une sollicitation de la mémoire ou si minimale forcée mémoire est fréquemment allouée, cette valeur peut être supérieure à la **target_memory_kb** ou **max_target_memory_kb** valeurs. La mémoire totale est la somme de la mémoire disponible et de la mémoire allouée.|  
|**available_memory_kb**|**bigint**|Mémoire disponible pour une nouvelle allocation, en kilo-octets.|  
|**granted_memory_kb**|**bigint**|Mémoire totale allouée, en kilo-octets.|  
|**used_memory_kb**|**bigint**|Partie de la mémoire allouée utilisée physiquement, en kilo-octets.|  
|**grantee_count**|**int**|Nombre de requêtes actives dont l'allocation est satisfaite.|  
|**waiter_count**|**int**|Nombre de requêtes attendant que leur allocation soit satisfaite.|  
|**timeout_error_count**|**bigint**|Nombre total d'erreurs de dépassement de délai d'attente depuis le démarrage du serveur. NULL pour le sémaphore de ressource de petites requêtes.|  
|**forced_grant_count**|**bigint**|Nombre total d'allocations de mémoire minimale forcées depuis le démarrage du serveur. NULL pour le sémaphore de ressource de petites requêtes.|  
|**pool_id**|**int**|ID du pool de ressources auquel ce sémaphore de ressource appartient.|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
  
## <a name="remarks"></a>Notes  
 Les requêtes qui utilisent des vues de gestion dynamiques qui incluent ORDER BY ou des fonctions d'agrégation peuvent accroître la consommation de mémoire et par conséquent contribuer au problème qu'elles tentent de résoudre.  
  
 Utilisez **sys.dm_exec_query_resource_semaphores** pour la résolution des problèmes, mais ne l’incluez pas dans les applications qui utiliseront les versions futures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La fonctionnalité Gouverneur de ressources permet à un administrateur de base de données de répartir des ressources serveur entre plusieurs pools de ressources (64 pools au maximum). Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et les versions ultérieures, chaque pool se comporte comme une petite instance de serveur indépendante et requiert 2 sémaphores.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
  
  


