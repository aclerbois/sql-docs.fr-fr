---
title: Sys.dm_db_column_store_row_group_operational_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 31b71c68-50a0-4fd8-a7fe-2d2292be1163
caps.latest.revision: 6
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 655f5838456fd72566405c453cecfd8858d7d6dd
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbcolumnstorerowgroupoperationalstats-transact-sql"></a>sys.dm_db_column_store_row_group_operational_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  E/s, verrouillage, au niveau des lignes retourne actuel et la méthode d’accès pour les rowgroups compressés dans un index columnstore. Utilisez **sys.dm_db_column_store_row_group_operational_stats** pour effectuer le suivi de la durée pendant laquelle une requête de l’utilisateur doit attendre pour lire ou écrire dans un rowgroup compressé ou une partition d’un index columnstore et identifier les rowgroups qui connaissent une activité d’e/s importante ou des zones réactives.  
  
 Les index columnstore en mémoire n’apparaissent pas dans cette vue DMV.  
 
 
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de la table contenant l’index columnstore.|  
|**index_id**|**int**|ID de l'index columnstore.|  
|**partition_number**|**int**|Numéro de partition (basé sur la valeur 1) au sein de l'index ou du segment de mémoire.|  
|**row_group_id**|**int**|ID du rowgroup dans l’index columnstore. Il est unique au sein d’une partition.|  
|**scan_count**|**int**|Nombre d’analyses par le biais du groupe de lignes depuis le dernier redémarrage SQL.|  
|**delete_buffer_scan_count**|**int**|Nombre de fois où que la mémoire tampon de suppression a été utilisé pour déterminer les lignes supprimées dans ce groupe de lignes. Cela inclut l’accès à la table de hachage en mémoire et le btree sous-jacent.|  
|**index_scan_count**|**int**|Nombre de fois que la partition d’index columnstore a été analysée. Cela est identique pour tous les groupes de lignes dans la partition.|  
|**rowgroup_lock_count**|**bigint**|Nombre cumulatif de demandes de verrouillage pour ce groupe de lignes depuis le dernier redémarrage SQL.|  
|**rowgroup_lock_wait_count**|**bigint**|Nombre cumulatif de fois où le moteur de base de données a attendu sur ce verrou de groupe de lignes depuis le dernier redémarrage SQL.|  
|**rowgroup_lock_wait_in_ms**|**bigint**|Nombre cumulatif de millisecondes attendues dans le moteur de base de données sur ce verrou de groupe de lignes depuis le dernier redémarrage SQL.|  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations suivantes sont nécessaires :  
  
-   Autorisation CONTROL sur la table spécifiée par object_id.  
  
-   Autorisation VIEW DATABASE STATE pour retourner des informations sur tous les objets dans la base de données à l’aide de l’objet générique @*object_id* = NULL  
  
 L'octroi de l'autorisation VIEW DATABASE STATE autorise le renvoi de tous les objets de la base de données, quelles que soient les autorisations CONTROL refusées sur des objets spécifiques.  
  
 Le refus de l'autorisation VIEW DATABASE STATE interdit le retour de tous les objets de la base de données, quelles que soient les autorisations CONTROL accordées sur des objets spécifiques. Également, lorsque la base de données générique @*database_id*= NULL est spécifiée, la base de données est omis.  
  
 Pour plus d’informations, consultez [fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux index &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Surveiller et optimiser les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [Sys.dm_db_index_physical_stats &#40; Transact-SQL &#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_usage_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)   
 [sys.dm_os_latch_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)   
 [sys.dm_db_partition_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)   
 [Sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  

