---
title: Sys.dm_broker_queue_monitors (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_broker_queue_monitors
- sys.dm_broker_queue_monitors_TSQL
- dm_broker_queue_monitors_TSQL
- sys.dm_broker_queue_monitors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_queue_monitors dynamic management view
ms.assetid: 401207dc-ef4a-4a3f-879c-76dcbb52d6bc
caps.latest.revision: 29
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 1cef9da48e964164ca13b80de6f69c00d889bd08
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmbrokerqueuemonitors-transact-sql"></a>sys.dm_broker_queue_monitors (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque moniteur de file d'attente de l'instance. Un moniteur de file d'attente gère l'activation pour une file d'attente.  
  

|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Identificateur d'objet de la base de données qui contient la file d'attente surveillée par le moniteur. Accepte la valeur NULL.|  
|**queue_id**|**int**|Identificateur d'objet de la file d'attente surveillée par le moniteur. Accepte la valeur NULL.|  
|**state**|**nvarchar(32)**|État de l’analyse. Accepte la valeur NULL. Il s’agit d’une des opérations suivantes :<br /><br /> **INACTIF**<br /><br /> **NOTIFICATION**<br /><br /> **RECEIVES_OCCURRING**|  
|**last_empty_rowset_time**|**datetime**|Horodatage de la dernière opération RECEIVE appliquée à la file d'attente qui a retourné un jeu de résultats vide. Accepte la valeur NULL.|  
|**last_activated_time**|**datetime**|Horodatage de la dernière activation d'une procédure stockée par ce moniteur de file d'attente. Accepte la valeur NULL.|  
|**tasks_waiting**|**int**|Nombre de sessions actuellement en attente dans une instruction RECEIVE relative à la file d'attente considérée. Accepte la valeur NULL.<br /><br /> Remarque : Ce nombre inclut toutes les sessions exécutant une instruction receive, indépendamment de si le moniteur de file d’attente a démarré la session. C'est pourquoi vous utilisez WAITFOR avec RECEIVE. À la base, ces tâches attendent toutes les deux l'arrivée de messages dans la file d'attente.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-current-status-queue-monitor"></a>A. Moniteur d'état des files d'attente  
 Ce scénario fournit le statut actuel de toutes les files d'attente de messages.  
  
```  
SELECT t1.name AS [Service_Name],  t3.name AS [Schema_Name],  t2.name AS [Queue_Name],    
CASE WHEN t4.state IS NULL THEN 'Not available'   
ELSE t4.state   
END AS [Queue_State],    
CASE WHEN t4.tasks_waiting IS NULL THEN '--'   
ELSE CONVERT(VARCHAR, t4.tasks_waiting)   
END AS tasks_waiting,   
CASE WHEN t4.last_activated_time IS NULL THEN '--'   
ELSE CONVERT(varchar, t4.last_activated_time)   
END AS last_activated_time ,    
CASE WHEN t4.last_empty_rowset_time IS NULL THEN '--'   
ELSE CONVERT(varchar,t4.last_empty_rowset_time)   
END AS last_empty_rowset_time,   
(   
SELECT COUNT(*)   
FROM sys.transmission_queue t6   
WHERE (t6.from_service_name = t1.name) ) AS [Tran_Message_Count]   
FROM sys.services t1    INNER JOIN sys.service_queues t2   
ON ( t1.service_queue_id = t2.object_id )     
INNER JOIN sys.schemas t3 ON ( t2.schema_id = t3.schema_id )    
LEFT OUTER JOIN sys.dm_broker_queue_monitors t4   
ON ( t2.object_id = t4.queue_id  AND t4.database_id = DB_ID() )    
INNER JOIN sys.databases t5 ON ( t5.database_id = DB_ID() );  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique & #40 ; liées à Service Broker Transact-SQL & #41 ;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

