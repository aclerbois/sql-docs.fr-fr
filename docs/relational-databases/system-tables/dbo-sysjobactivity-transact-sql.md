---
title: dbo.sysjobactivity (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dbo.sysjobactivity_TSQL
- dbo.sysjobactivity
- sysjobactivity
- sysjobactivity_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobactivity system table
ms.assetid: fd17cac9-5d1f-4b44-b2dc-ee9346d8bf1e
caps.latest.revision: 17
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: a1d9e79856ac767d231993165b0c6d565d791464
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbosysjobactivity-transact-sql"></a>dbo.sysjobactivity (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Enregistre l'état et l'activité des travaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  Cette table est stockée dans le **msdb** base de données.
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID de la session stockée dans le **syssessions** de table dans le **msdb** base de données.|  
|**job_id**|**uniqueidentifier**|ID du travail.|  
|**run_requested_date**|**datetime**|Date et heure auxquelles le travail devait s'exécuter.|  
|**run_requested_source**|**sysname(nvarchar(128))**|Demandeur de l'exécution du travail.<br /><br /> **1** = SOURCE_SCHEDULER<br /><br /> **2** = SOURCE_ALERTER<br /><br /> **3** = SOURCE_BOOT<br /><br /> **4** = SOURCE_USER<br /><br /> **6** = SOURCE_ON_IDLE_SCHEDULE|  
|**queued_date**|**datetime**|Date et heure auxquelles ce travail a intégré une file d'attente. Si le travail est exécuté directement, cette colonne affiche la valeur NULL.|  
|**start_execution_date**|**datetime**|Date et heure auxquelles l'exécution du travail a été prévue.|  
|**last_executed_step_id**|**int**|ID de la dernière étape exécutée dans le travail.|  
|**last_executed_step_**<br /><br /> **date**|**datetime**|Date et heure auxquelles l'exécution de la dernière étape du travail a démarré.|  
|**stop_execution_date**|**datetime**|Date et heure auxquelles l'exécution du travail s'est terminée.|  
|**job_history_id**|**int**|Utilisé pour identifier une ligne dans le **sysjobhistory** table.|  
|**next_scheduled_run_date**|**datetime**|Prochaines date et heure prévues pour l'exécution du travail.|  

## <a name="example"></a>Exemple
Cet exemple retourne l’état d’exécution de tous les travaux de l’Agent SQL Server.  Exécutez l’instruction suivante [!INCLUDE[tsql](../../includes/tsql-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].
```sql
SELECT sj.Name, 
    CASE
        WHEN sja.start_execution_date IS NULL THEN 'Not running'
        WHEN sja.start_execution_date IS NOT NULL AND sja.stop_execution_date IS NULL THEN 'Running'
        WHEN sja.start_execution_date IS NOT NULL AND sja.stop_execution_date IS NOT NULL THEN 'Not running'
    END AS 'RunStatus'
FROM msdb.dbo.sysjobs sj
JOIN msdb.dbo.sysjobactivity sja
ON sj.job_id = sja.job_id
WHERE session_id = (
    SELECT MAX(session_id) FROM msdb.dbo.sysjobactivity); 
```
  
## <a name="see-also"></a>Voir aussi  
 [dbo.sysjobhistory &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
  
  
