---
title: Sys.query_store_query (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- QUERY_STORE_QUERY
- SYS.QUERY_STORE_QUERY_TSQL
- SYS.QUERY_STORE_QUERY
- QUERY_STORE_QUERY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_query catalog view
- sys.query_store_query catalog view
ms.assetid: bdee149e-7556-4fc3-8242-925dd4b7b6ac
caps.latest.revision: 15
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d9d53bc6cd0219502698ba8a02b6ba19eaf5f34f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysquerystorequery-transact-sql"></a>Sys.query_store_query (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Contient des informations sur la requête et ses statistiques d’exécution runtime agrégées globale associé.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**query_id**|**bigint**|Clé primaire.|  
|**query_text_id**|**bigint**|Clé étrangère. Joint à [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)|  
|**context_settings_id**|**bigint**|Clé étrangère. Joint à [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md).|  
|**object_id**|**bigint**|ID de l’objet de base de données qui fait partie de la requête (procédure stockée, déclencheur, UDAgg/CLR UDF, etc..). 0 si la requête n’est pas exécutée en tant que partie d’un objet de base de données (requête ad-hoc).|  
|**batch_sql_handle**|**varbinary(64)**|ID du lot instruction la requête fait partie. Remplie uniquement si la requête fait référence à des tables temporaires ou des variables de table.|  
|**query_hash**|**binary(8)**|Hachage MD5 de la requête individuelle, en fonction de l’arborescence logique de requête. Inclut des indicateurs d’optimiseur.|  
|**is_internal_query**|**bit**|La requête a été générée en interne.|  
|**query_parameterization_type**|**tinyint**|Type de paramétrage :<br /><br /> 0 – aucun<br /><br /> 1 = utilisateur<br /><br /> 2 – simple<br /><br /> 3 – forcé|  
|**query_parameterization_type_desc**|**nvarchar(60)**|Texte de description pour le type de paramétrage.|  
|**initial_compile_start_time**|**datetimeoffset**|Heure de début de la compilation.|  
|**last_compile_start_time**|**datetimeoffset**|Heure de début de la compilation.|  
|**last_execution_time**|**datetimeoffset**|Dernière heure d’exécution fait référence à la dernière heure de fin de la requête/plan.|  
|**last_compile_batch_sql_handle**|**varbinary(64)**|Handle du dernier lot SQL dans laquelle requête était utilisée heure de la dernière. Elle peut être fournie comme entrée pour [sys.dm_exec_sql_text &#40;Transact-SQL&#41; ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) pour obtenir le texte complet du lot.|  
|**last_compile_batch_offset_start**|**bigint**|Informations qui peuvent être fournies à sys.dm_exec_sql_text avec last_compile_batch_sql_handle.|  
|**last_compile_batch_offset_end**|**bigint**|Informations qui peuvent être fournies à sys.dm_exec_sql_text avec last_compile_batch_sql_handle.|  
|**count_compiles**|**bigint**|Statistiques de compilation.|  
|**avg_compile_duration**|**float**|Statistiques de compilation en microsecondes.|  
|**last_compile_duration**|**bigint**|Statistiques de compilation en microsecondes.|  
|**avg_bind_duration**|**float**|Statistiques de liaison en microsecondes.|  
|**last_bind_duration**|**bigint**|Statistiques de la liaison.|  
|**avg_bind_cpu_time**|**float**|Statistiques de la liaison.|  
|**last_bind_cpu_time**|**bigint**|Statistiques de la liaison.|  
|**avg_optimize_duration**|**float**|Statistiques d’optimisation en microsecondes.|  
|**last_optimize_duration**|**bigint**|Statistiques d’optimisation.|  
|**avg_optimize_cpu_time**|**float**|Statistiques d’optimisation en microsecondes.|  
|**last_optimize_cpu_time**|**bigint**|Statistiques d’optimisation.|  
|**avg_compile_memory_kb**|**float**|Compiler des statistiques de la mémoire.|  
|**last_compile_memory_kb**|**bigint**|Compiler des statistiques de la mémoire.|  
|**max_compile_memory_kb**|**bigint**|Compiler des statistiques de la mémoire.|  
|**is_clouddb_internal_query**|**bit**|Toujours 0 dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] local.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert le **VIEW DATABASE STATE** autorisation.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Procédures stockées du magasin de requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  
