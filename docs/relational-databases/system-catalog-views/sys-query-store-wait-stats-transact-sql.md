---
title: Sys.query_store_wait_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 04/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.query_store_wait_stats
- query_store_wait_stats
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_wait_stats catalog view
- sys.query_store_wait_stats catalog view
ms.assetid: ccf7a57c-314b-450c-bd34-70749a02784a
caps.latest.revision: 18
author: AndrejsAnt
ms.author: AndrejsAnt
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2017 || = sqlallproducts-allversions
ms.openlocfilehash: d4a7afdca88de97188577e726d203040e33c8c71
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "33182015"
---
# <a name="sysquerystorewaitstats-transact-sql"></a>Sys.query_store_wait_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2017-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-asdb-xxxx-xxx-md.md)]

  Contient des informations sur les informations de l’attente de la requête.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**wait_stats_id**|**bigint**|Identificateur de la ligne représentant les statistiques d’attente de plan_id, runtime_stats_interval_id, execution_type et wait_category. Il est unique seulement pour les derniers intervalles de statistiques de runtime. Intervalle actuellement actif peut avoir plusieurs lignes représentant des statistiques d’attente pour le plan référencé par plan_id, avec le type d’exécution représenté par execution_type et la catégorie d’attente représentées par wait_category. En règle générale, une ligne représente les statistiques d’attente sont vidées sur le disque, tandis que les autres (s) représentent l’état en mémoire. Par conséquent, pour obtenir l’état réel de chaque intervalle vous devez les métriques d’agrégation, le regroupement par plan_id, runtime_stats_interval_id, execution_type et wait_category. |  
|**plan_id**|**bigint**|Clé étrangère. Joint à [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md).|  
|**runtime_stats_interval_id**|**bigint**|Clé étrangère. Joint à [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md).|  
|**wait_category**|**tinyint**|Types d’attente sont classés à l’aide de la table ci-dessous, et, le temps d’attente sont agrégé sur ces attendre des catégories. Différentes catégories d’attente nécessitent une analyse de suivi différente pour résoudre le problème, mais les types d’attente d’une même catégorie entraînent des expériences de résolution de problèmes très similaires à condition que la requête affectée au dessus des attentes soit l’élément manquant de la plupart de ces expériences.|
|**wait_category_desc**|**nvarchar(128)**|Pour une description textuelle du champ de catégorie d’attente, veuillez consulter le tableau ci-dessous.|
|**execution_type**|**tinyint**|Détermine le type d’exécution de requête :<br /><br /> 0 – exécution normal (terminée avec succès)<br /><br /> 3 – initié par le client abandon de l’exécution<br /><br /> 4 - exception abandonnée de l’exécution|  
|**execution_type_desc**|**nvarchar(128)**|Description textuelle du champ de type d’exécution :<br /><br /> 0 – standard<br /><br /> 3 – abandonnée<br /><br /> 4 - exception|  
|**total_query_wait_time_ms**|**bigint**|Processeur total délai d’attente pour le plan de requête dans l’intervalle d’agrégation et catégorie (signalée en millisecondes) d’attente.|
|**avg_query_wait_time_ms**|**float**|Durée du plan de requête par l’exécution dans la catégorie d’attente et l’intervalle d’agrégation (signalée en millisecondes) d’attente moyen.|
|**last_query_wait_time_ms**|**bigint**|Dernière durée du plan de requête dans l’intervalle d’agrégation d’attente et catégorie (signalée en millisecondes) d’attente.|
|**min_query_wait_time_ms**|**bigint**|Processeur minimal, temps d’attente pour le plan de requête dans l’intervalle d’agrégation et catégorie (signalée en millisecondes) d’attente.|
|**max_query_wait_time_ms**|**bigint**|Temps d’attente pour le plan de requête dans l’intervalle d’agrégation processeur maximal et la catégorie (signalée en millisecondes) d’attente.|
|**stdev_query_wait_time_ms**|**float**|Attendez l’écart de la durée du plan de requête dans l’intervalle d’agrégation de requêtes et de catégorie (signalée en millisecondes) d’attente.|

 ## <a name="wait-categories-mapping-table"></a>Attendez la table de mappage de catégories  
  *« % » est utilisé comme un caractère générique*
  
|Valeur entière|Catégorie d’attente|Incluent dans la catégorie des types d’attente|  
|-----------------|---------------|-----------------|  
|**0**|**Unknown**|Unknown |  
|**1**|**Unité centrale**|SOS_SCHEDULER_YIELD|
|**2**|**Thread de travail**|THREADPOOL|
|**3**|**verrou**|LCK_M_ %|
|**4**|**Verrou**|LATCH_ %|
|**5**|**Verrou de mémoire tampon**|PAGELATCH_ %|
|**6**|**Mémoire tampon d’e/s**|PAGEIOLATCH_ %|
|**7**|**Compilation***|RESOURCE_SEMAPHORE_QUERY_COMPILE|
|**8**|**CLR SQL**|CLR, SQLCLR %|
|**9**|**Mise en miroir**|DBMIRROR %|
|**10**|**Transaction**|XACT %, DTC %, TRAN_MARKLATCH_ %, MSQL_XACT_ %, TRANSACTION_MUTEX|
|**11**|**Des temps d’inactivité**|% SLEEP_, LAZYWRITER_SLEEP, SQLTRACE_BUFFER_FLUSH, SQLTRACE_INCREMENTAL_FLUSH_SLEEP, SQLTRACE_WAIT_ENTRIES, FT_IFTS_SCHEDULER_IDLE_WAIT, XE_DISPATCHER_WAIT, REQUEST_FOR_DEADLOCK_SEARCH, LOGMGR_QUEUE, ONDEMAND_TASK_QUEUE, CHECKPOINT_QUEUE, XE_TIMER_EVENT|
|**12**|**PreEmptive**|PREEMPTIVE_ %|
|**13**|**Service Broker**|BROKER_ % **(mais pas BROKER_RECEIVE_WAITFOR)**|
|**14**|**Tran journal e/s**|GESTIONNAIRE DE FICHIERS JOURNAUX, LOGBUFFER, LOGMGR_RESERVE_APPEND, LOGMGR_FLUSH, LOGMGR_PMM_LOG, CHKPT, WRITELOGF|
|**15**|**E/s réseau**|ASYNC_NETWORK_IO, NET_WAITFOR_PACKET, PROXY_NETWORK_IO, EXTERNAL_SCRIPT_NETWORK_IOF|
|**16**|**Parallelism**|CXPACKET, EXCHANGE|
|**17**|**Mémoire**|RESOURCE_SEMAPHORE, CMEMTHREAD, CMEMPARTITIONED, EE_PMOLOCK, MEMORY_ALLOCATION_EXT, RESERVED_MEMORY_ALLOCATION_EXT, MEMORY_GRANT_UPDATE|
|**18**|**Attente de l’utilisateur**|WAITFOR, WAIT_FOR_RESULTS, BROKER_RECEIVE_WAITFOR|
|**19**|**Suivi**|TRACEWRITE, SQLTRACE_LOCK, SQLTRACE_FILE_BUFFER, SQLTRACE_FILE_WRITE_IO_COMPLETION, SQLTRACE_FILE_READ_IO_COMPLETION, SQLTRACE_PENDING_BUFFER_WRITERS, SQLTRACE_SHUTDOWN, QUERY_TRACEOUT, TRACE_EVTNOTIFF|
|**20**|**Recherche en texte intégral**|FT_RESTART_CRAWL, OUTIL D’EXTRACTION DE TEXTE INTÉGRAL, MSSEARCH, FT_METADATA_MUTEX, FT_IFTSHC_MUTEX, FT_IFTSISM_MUTEX, FT_IFTS_RWLOCK, FT_COMPROWSET_RWLOCK, FT_MASTER_MERGE, FT_PROPERTYLIST_CACHE, FT_MASTER_MERGE_COORDINATOR, PWAIT_RESOURCE_SEMAPHORE_FT_PARALLEL_QUERY_SYNC|
|**21**|**Autres e/s de disque**|ASYNC_IO_COMPLETION, IO_COMPLETION, BACKUPIO, WRITE_COMPLETION, IO_QUEUE_LIMIT, IO_RETRY|
|**22**|**Réplication**|SE_REPL_ %, REPL_ %, HADR_ % **(mais pas HADR_THROTTLE_LOG_RATE_GOVERNOR)**, PWAIT_HADR_ %, REPLICA_WRITES, FCB_REPLICA_WRITE, FCB_REPLICA_READ, PWAIT_HADRSIM|
|**23**|**Journal Rate Governor.**|LOG_RATE_GOVERNOR, POOL_LOG_RATE_GOVERNOR, HADR_THROTTLE_LOG_RATE_GOVERNOR, INSTANCE_LOG_RATE_GOVERNOR|

***Compilation** catégorie d’attente n’est actuellement pas pris en charge. 

  
## <a name="permissions"></a>Autorisations  
 Requiert le **VIEW DATABASE STATE** autorisation.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [Sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Procédures stockées du magasin de requête &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
  
  
