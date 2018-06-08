---
title: Sys.dm_os_job_object (de base de données SQL Azure) | Documents Microsoft
ms.custom: ''
ms.date: 04/17/2018
ms.prod: ''
ms.prod_service: sql-database
ms.reviewer: ''
ms.service: sql-database
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_resource_stats
- sys.dm_db_resource_stats_TSQL
- dm_db_resource_stats
- dm_db_resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_resource_stats
- dm_db_resource_stats
ms.assetid: 6e76b39f-236e-4bbf-b0b5-38be190d81e8
caps.latest.revision: 11
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: 8ab408179388ca10821ad79e855e39fd3ec7eb01
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmosjobobject-azure-sql-database"></a>Sys.dm_os_job_object (de base de données SQL Azure)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

Retourne une ligne unique décrivant la configuration de l’objet de tâche qui gère le processus SQL Server, ainsi que certaines statistiques de consommation de ressources au niveau de l’objet de tâche. Retourne un jeu vide, si SQL Server n’est pas en cours d’exécution dans un objet de tâche. 

Un objet de tâche est une construction Windows qui implémente la gouvernance des ressources processeur, mémoire et e/s au niveau du système d’exploitation. Pour plus d’informations sur les objets travail, consultez [objets travail](https://msdn.microsoft.com/library/windows/desktop/ms684161.aspx). 

> [!NOTE]
> Le sys.dm_os_job_object DMV peut actuellement être sys.dm_job_object. Il s’agit temporaire : `sys.dm_os_job_object` sera le nom permanent de cette vue de gestion dynamique. 
  
|Columns|Type de données| Description|  
|-------------|---------------|-----------------|  
|cpu_rate|**int**|Spécifie la partie de cycles de processeur que les threads SQL Server peuvent utiliser au cours de chaque intervalle de planification. La valeur est signalée comme un pourcentage de cycles disponibles dans un intervalle de planification du cycle de 10000. Par exemple, la valeur 100 signifie que les threads peuvent utiliser des cœurs de processeur est à leur capacité maximale.|
|cpu_affinity_mask|**bigint**|Un masque de bits qui décrit les processeurs logiques, le processus SQL Server peut utiliser dans le groupe de processeurs. Par exemple, cpu_affinity_mask 255 (1111 1111 en binaire) signifie que les huit premiers processeurs logiques peuvent être utilisés.|
|cpu_affinity_group|**int**|Le numéro du groupe de processeur qui est utilisé par SQL Server.|
|memory_limit_mb|**bigint**|La quantité maximale de mémoire allouée, en Mo, utilisable par tous les processus dans l’objet de traitement, y compris SQL Server, depuis le début.| 
|process_memory_limit_mb |**bigint**|La quantité maximale de mémoire allouée, en Mo, ce qui permet de l’objet de traitement, telles que SQL Server, un seul processus.|
|workingset_limit_mb |**bigint**|La quantité maximale de mémoire, en Mo, ce qui permet de la plage de travail de SQL Server.|
|non_sos_mem_gap_mb|**bigint**|La quantité de mémoire, en Mo, définie une plage pour les piles de threads, des DLL et des autres allocations de mémoire non-SOS. Mémoire cible de SOS est la différence entre `process_memory_limit_mb` et `non_sos_mem_gap_mb`.| 
|low_mem_signal_threshold_mb|**bigint**|Seuil de mémoire, en Mo. Lorsque la quantité de mémoire disponible pour l’objet de traitement est inférieur à ce seuil, un signal de notification de mémoire insuffisante est envoyé au processus SQL Server. |
|total_user_time|**bigint**|Le nombre total de graduations de 100 ns que les threads dans l’objet de traitement passé en mode utilisateur, étant donné que l’objet de traitement a été créé. |
|total_kernel_time |**bigint**|Le nombre total de graduations de 100 ns que les threads dans l’objet de traitement passé en mode noyau, étant donné que l’objet de traitement a été créé. |
|write_operation_count |**bigint**|Le nombre total d’opérations d’écriture e/s sur les disques locaux émis par SQL Server depuis la création de l’objet de traitement. |
|read_operation_count |**bigint**|Nombre total d’e/s des opérations de lecture sur les disques locaux émis par SQL Server depuis la création de l’objet de traitement. |
|peak_process_memory_used_mb|**bigint**|La quantité maximale de mémoire, en Mo, ce qui a un seul processus de l’objet de travail, telles que SQL Server, a utilisé depuis la création de l’objet de traitement.| 
|peak_job_memory_used_mb|**bigint**|La quantité maximale de mémoire, en méga-octets, tous les processus dans l’objet de traitement ont utilisé cumulativement depuis l’objet de traitement a été créée.|
  
## <a name="permissions"></a>Autorisations  
Sur l’Instance gérée de base de données SQL, requiert `VIEW SERVER STATE` autorisation. Sur la base de données SQL, requiert le `VIEW DATABASE STATE` autorisation dans la base de données.  
 
## <a name="see-also"></a>Voir aussi  

Pour plus d’informations sur les Instances gérées, consultez [une Instance de base de données gérée SQL](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance).
  
