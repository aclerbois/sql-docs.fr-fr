---
title: Sys.dm_db_xtp_nonclustered_index_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_nonclustered_index_stats_TSQL
- dm_db_xtp_nonclustered_index_stats
- sys.dm_db_xtp_nonclustered_index_stats_TSQL
- sys.dm_db_xtp_nonclustered_index_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_nonclustered_index_stats
ms.assetid: d55ba31c-296c-419b-9c4b-c126e0a3d156
caps.latest.revision: 10
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7bec37b0223f2384ebdfc2898717bd937bb0b046
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbxtpnonclusteredindexstats-transact-sql"></a>sys.dm_db_xtp_nonclustered_index_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]

  sys.dm_db_xtp_nonclustered_index_stats inclut des statistiques sur les opérations effectuées sur index non-cluster dans les tables mémoire optimisées. sys.dm_db_xtp_nonclustered_index_stats contient une ligne pour chaque index non-cluster sur une table mémoire optimisée dans la base de données active.  
  
 Les statistiques reflétées dans sys.dm_db_xtp_nonclustered_index_stats sont collectées lorsque la structure de l'index en mémoire est créée. Les structures d'index en mémoire sont recréées lors du redémarrage de la base de données.  
  
 Utilisez sys.dm_db_xtp_nonclustered_index_stats pour comprendre et surveiller l'activité de l'index pendant des opérations DML et lorsqu'une base de données est mise en ligne. Lorsqu'une base de données avec une table mémoire optimisée est redémarrée, l'index est construit en insérant une ligne à la fois dans la mémoire. Le nombre de fractionnements, fusions et consolidations de pages peut vous aider à comprendre le travail effectué pour construire l'index lorsqu'une base de données est mise en ligne. Vous pouvez également consulter ces informations avant et après une série d'opérations DML.  
  
 Un grand nombre de nouvelles tentatives indique des problèmes de concurrence ; appelez le support [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
 Pour plus d’informations sur les index non ordonnés en clusters, mémoire optimisées, consultez [présentation mécanismes internes de SQL Server en mémoire OLTP](http://t.co/T6zToWc6y6), page 17.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID de l'objet.|  
|xtp_object_id|**bigint**|ID de la table optimisée en mémoire.|  
|index_id|**int**|Identificateur de l'index.|  
|delta_pages|**bigint**|Nombre total de pages delta pour cet index dans l'arborescence.|  
|internal_pages|**bigint**|À usage interne uniquement. Nombre total de pages internes pour cet index dans l'arborescence.|  
|leaf_pages|**bigint**|Nombre total de pages feuilles pour cet index dans l'arborescence.|  
|outstanding_retired_nodes|**bigint**|À usage interne uniquement. Nombre total de nœuds pour cet index dans les structures internes.|  
|page_update_count|**bigint**|Nombre cumulé d'opérations de mise à jour d'une page dans l'index.|  
|page_update_retry_count|**bigint**|Nombre cumulé de nouvelles tentatives d'une opération de mise à jour de page dans l'index.|  
|page_consolidation_count|**bigint**|Nombre cumulé de consolidations de page dans l'index.|  
|page_consolidation_retry_count|**bigint**|Nombre cumulé de nouvelles tentatives de consolidation de page.|  
|page_split_count|**bigint**|Nombre cumulé d'opérations de fractionnement de page dans l'index.|  
|page_split_retry_count|**bigint**|Nombre cumulé de nouvelles tentatives de fractionnement de page.|  
|key_split_count|**bigint**|Nombre cumulé de fractionnements de clé dans l'index.|  
|key_split_retry_count|**bigint**|Nombre cumulé de nouvelles tentatives de fractionnement de clé.|  
|page_merge_count|**bigint**|Nombre cumulé d'opérations de fusion de page dans l'index.|  
|page_merge_retry_count|**bigint**|Nombre cumulé de nouvelles tentatives de fusion de page.|  
|key_merge_count|**bigint**|Nombre cumulé d'opérations de fusion de clé dans l'index.|  
|key_merge_retry_count|**bigint**|Nombre cumulé de nouvelles tentatives de fusion de clé.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DATABASE STATE sur la base de données active.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables optimisées en mémoire &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
