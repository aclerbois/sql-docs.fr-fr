---
title: Sys.dm_clr_tasks (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_clr_tasks
- sys.dm_clr_tasks_TSQL
- dm_clr_tasks
- dm_clr_tasks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_tasks dynamic management view
ms.assetid: 462b9061-09fa-4858-9707-03d6cc19c769
caps.latest.revision: 21
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 581e89814828b9a5542d358abb5cb84025acfce4
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmclrtasks-transact-sql"></a>sys.dm_clr_tasks (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne une ligne pour toutes les tâches CLR (Common Language Runtime) en cours d'exécution. Un traitement [!INCLUDE[tsql](../../includes/tsql-md.md)] qui contient une référence à une routine CLR crée une tâche distincte pour exécuter l'ensemble du code managé de ce traitement. Les diverses instructions du traitement qui nécessitent l'exécution de code managé utilisent la même tâche CLR. Cette tâche CLR est chargée de tenir à jour les objets et les états liés à l'exécution du code managé, mais aussi les transitions entre l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le CLR.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**task_address**|**varbinary(8)**|Adresse de la tâche CLR.|  
|**sos_task_address**|**varbinary(8)**|Adresse de la tâche du traitement [!INCLUDE[tsql](../../includes/tsql-md.md)] sous-jacent.|  
|**appdomain_address**|**varbinary(8)**|Adresse du domaine d'application dans lequel cette tâche s'exécute.|  
|**state**|**nvarchar(128)**|État actuel de la tâche.|  
|**abort_state**|**nvarchar(128)**|État actuel de la procédure d'annulation (si la tâche a été annulée). L'abandon d'une tâche passe par plusieurs états.|  
|**type**|**nvarchar(128)**|Type de tâche.|  
|**affinity_count**|**int**|Affinité de la tâche.|  
|**forced_yield_count**|**int**|Nombre de fois où la tâche a dû être abandonnée.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  

