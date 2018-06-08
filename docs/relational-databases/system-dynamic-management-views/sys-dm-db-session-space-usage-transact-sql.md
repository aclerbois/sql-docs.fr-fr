---
title: Sys.dm_db_session_space_usage (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 11/16/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_db_session_space_usage_TSQL
- dm_db_session_space_usage
- sys.dm_db_session_space_usage
- sys.dm_db_session_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_session_space_usage dynamic management view
ms.assetid: a67a6045-8e14-460a-9fe3-912b846c08c1
caps.latest.revision: 34
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 61926a7bde695d1ca8af605373cf666653c1cee5
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbsessionspaceusage-transact-sql"></a>sys.dm_db_session_space_usage (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Renvoie le nombre de pages allouées et désallouées par chaque session de la base de données.  
  
> [!NOTE]  
>  Cette vue s’applique uniquement à la [base de données tempdb](../../relational-databases/databases/tempdb-database.md).  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_db_session_space_usage**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**smallint**|ID de la session.<br /><br /> **session_id** est mappé à **session_id** dans [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md).|  
|**database_id**|**smallint**|ID de la base de données.|  
|**user_objects_alloc_page_count**|**bigint**|Nombre de pages réservées ou allouées aux objets utilisateur par cette session.|  
|**user_objects_dealloc_page_count**|**bigint**|Nombre de pages désallouées et qui ne sont plus réservées aux objets utilisateur par cette session.|  
|**internal_objects_alloc_page_count**|**bigint**|Nombre de pages réservées ou allouées aux objets internes par cette session.|  
|**internal_objects_dealloc_page_count**|**bigint**|Nombre de pages désallouées et qui ne sont plus réservées aux objets internes par cette session.|  
|**user_objects_deferred_dealloc_page_count**|**bigint**|Nombre de pages qui ont été marqués pour une désallocation différée.<br /><br /> **Remarque :** introduites dans les service packs pour [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="remarks"></a>Notes  
 Les pages IAM ne sont pas incluses dans les nombres d'allocations ou de désallocations indiqués dans cette vue.  
  
 Les compteurs de pages sont initialisés à zéro (0) au début d'une session. Les compteurs suivent le nombre total de pages allouées ou désallouées pour des tâches déjà effectuées dans la session. Les compteurs sont mis à jour uniquement lorsqu'une tâche se termine ; ils ne reflètent pas les tâches en cours d'exécution.  
  
 Plusieurs demandes peuvent être simultanément actives dans une session. Une demande parallèle peut démarrer plusieurs threads et tâches.  
  
 Pour plus d’informations sur les sessions, les demandes et les tâches, consultez [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md), [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)et [sys.dm_os_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md).  
  
## <a name="user-objects"></a>Objets utilisateur  
 Les objets suivants sont compris dans les compteurs de pages des objets utilisateurs :  
  
-   les tables et les index définis par l'utilisateur ;  
  
-   les tables et les index système ;  
  
-   les tables temporaires globales et les index ;  
  
-   les tables temporaires locales et les index ;  
  
-   les variables de tables ;  
  
-   les tables renvoyées dans les fonctions table.  
  
## <a name="internal-objects"></a>Objets internes  
 Les objets internes se trouvent uniquement dans **tempdb**. Les objets suivants sont compris dans les compteurs de pages des objets internes :  
  
-   les tables de travail des opérations de curseur ou de mise en attente et le stockage temporaire d'objets LOB ;  
  
-   les fichiers de travail des opérations telles que les jointures de hachage ;  
  
-   Tris  
  
## <a name="physical-joins"></a>Jointures physiques  
 ![Jointures physiques pour sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/media/join-dm-db-session-space-usage-1.gif "de jointures physiques pour sys.dm_db_session_space_usage")  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|From|Pour|Relation|  
|----------|--------|------------------|  
|dm_db_session_space_usage.session_id|dm_exec_sessions.session_id|Un à un|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_os_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)   
 [sys.dm_db_task_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md)   
 [sys.dm_db_file_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md)  
  
  



