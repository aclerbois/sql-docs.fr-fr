---
title: Sys.dm_os_tasks (Transact-SQL) | Documents Microsoft
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
- sys.dm_os_tasks
- sys.dm_os_tasks_TSQL
- dm_os_tasks_TSQL
- dm_os_tasks
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_tasks dynamic management view
ms.assetid: 180a3c41-e71b-4670-819d-85ea7ef98bac
caps.latest.revision: 34
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: b800dc110baa7279edb2ce788befb433b7afd77a
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmostasks-transact-sql"></a>sys.dm_os_tasks (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une ligne pour chaque tâche active dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_os_tasks**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**task_address**|**varbinary(8)**|Adresse mémoire de l'objet.|  
|**task_state**|**nvarchar(60)**|État de la tâche. Il peut s'agir de l'un des états suivants :<br /><br /> PENDING : En attente d'un thread de travail.<br /><br /> RUNNABLE : Exécutable, mais en attente d'un quantum.<br /><br /> RUNNING : En cours d'exécution sur le planificateur.<br /><br /> SUSPENDED : Doté d'un processus de travail, mais en attente d'un événement.<br /><br /> DONE : Terminé.<br /><br /> SPINLOOP : Bloqué dans une boucle.|  
|**context_switches_count**|**int**|Nombre de commutateurs de contexte de planificateur exécutés par cette tâche.|  
|**pending_io_count**|**int**|Nombre d'E/S physiques qui sont effectuées par cette tâche.|  
|**pending_io_byte_count**|**bigint**|Nombre total d'octets d'E/S qui sont traités par cette tâche.|  
|**pending_io_byte_average**|**int**|Nombre moyen d'octets d'E/S qui sont traités par cette tâche.|  
|**scheduler_id**|**int**|ID du planificateur parent. Handle pointant vers les informations de planification associées à cette tâche. Pour plus d’informations, consultez [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|**session_id**|**smallint**|ID de la session qui est associée à la tâche.|  
|**exec_context_id**|**int**|ID du contexte d'exécution qui est associé à la tâche.|  
|**request_id**|**int**|ID de la demande de la tâche. Pour plus d’informations, consultez [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|**worker_address**|**varbinary(8)**|Adresse mémoire du processus de travail qui exécute la tâche.<br /><br /> NULL = La tâche est en attente d'un processus de travail pour s'exécuter ou son exécution vient de se terminer.<br /><br /> Pour plus d’informations, consultez [sys.dm_os_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|**host_address**|**varbinary(8)**|Adresse mémoire de l'hôte.<br /><br /> 0 = Aucun hôte n'a été utilisé pour créer la tâche. Cela permet d'identifier l'hôte utilisé pour créer cette tâche.<br /><br /> Pour plus d’informations, consultez [sys.dm_os_hosts &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md).|  
|**parent_task_address**|**varbinary(8)**|Adresse mémoire de la tâche qui est le parent de l'objet.|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="examples"></a>Exemples  
  
### <a name="a-monitoring-parallel-requests"></a>A. Surveillance des demandes parallèles  
 Pour les demandes qui sont exécutées en parallèle, vous verrez plusieurs lignes pour la même combinaison de (\<**session_id**>, \< **request_id**>). Utilisez la requête suivante pour rechercher la [configurer le degré maximal de parallélisme Server Configuration Option](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) pour toutes les demandes actives.  
  
> [!NOTE]  
>  A **request_id** est unique dans une session.  
  
```  
SELECT  
    task_address,  
    task_state,  
    context_switches_count,  
    pending_io_count,  
    pending_io_byte_count,  
    pending_io_byte_average,  
    scheduler_id,  
    session_id,  
    exec_context_id,  
    request_id,  
    worker_address,  
    host_address  
  FROM sys.dm_os_tasks  
  ORDER BY session_id, request_id;  
```  
  
### <a name="b-associating-session-ids-with-windows-threads"></a>B. Association d'ID de session à des threads Windows  
 Vous pouvez utiliser la requête suivante pour associer une valeur d'ID de session à un ID de thread Windows. Vous pouvez ensuite surveiller les performances du thread dans l'Analyseur de performances Windows. La requête suivante ne renvoie pas d'informations pour les sessions en veille.  
  
```  
SELECT STasks.session_id, SThreads.os_thread_id  
  FROM sys.dm_os_tasks AS STasks  
  INNER JOIN sys.dm_os_threads AS SThreads  
    ON STasks.worker_address = SThreads.worker_address  
  WHERE STasks.session_id IS NOT NULL  
  ORDER BY STasks.session_id;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
  [Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


