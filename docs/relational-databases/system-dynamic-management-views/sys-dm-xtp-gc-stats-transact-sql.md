---
title: Sys.dm_xtp_gc_stats (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_xtp_gc_stats
- dm_xtp_gc_stats_TSQL
- sys.dm_xtp_gc_stats_TSQL
- sys.dm_xtp_gc_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xtp_gc_stats dynamic management view
ms.assetid: 8287d611-50e3-43e1-ba8d-3e3793d3ba0e
caps.latest.revision: 20
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: a66e199d232ed96fd194d42e340f3468fee51b6b
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmxtpgcstats-transact-sql"></a>sys.dm_xtp_gc_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]

  Fournit des informations (statistiques globales) sur le comportement actuel du processus de garbage collection de [!INCLUDE[hek_2](../../includes/hek-2-md.md)].  
  
 Les lignes sont récupérées par le garbage collector dans le cadre du traitement transactionnel normal de la transaction, ou par le thread principal du garbage collection, désigné comme le processus de travail inactif. Lorsqu’une transaction utilisateur est validée, elle enlève un élément de travail à partir de la file d’attente de garbage collection ([sys.dm_xtp_gc_queue_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-gc-queue-stats-transact-sql.md)). Toutes les lignes qui peuvent être récupérées par le garbage collection mais auxquelles la transaction utilisateur principale n'a pas eu accès, sont récupérées par le garbage collection via le processus de travail inactif, au cours d'une analyse d'angles inutilisés (une analyse des zones de l'index auxquelles on accède moins).  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|rows_examined|**bigint**|Nombre de lignes examinées par le sous-système de garbage collection depuis le démarrage du serveur.|  
|rows_no_sweep_needed|**bigint**|Nombre de lignes ayant été supprimées sans analyse d'angles inutilisés.|  
|rows_first_in_bucket|**bigint**|Nombre de lignes examinées par le garbage collection qui figuraient en premier dans le compartiment de hachage.|  
|rows_first_in_bucket_removed|**bigint**|Nombre de lignes examinées par garbage collection qui ont été les premières à être supprimées dans le compartiment de hachage.|  
|rows_marked_for_unlink|**bigint**|Nombre de lignes examinées par garbage collection qui étaient déjà marquées comme étant dissociées dans leurs index avec ref count =0.|  
|parallel_assist_count|**bigint**|Nombre de lignes traitées par les transactions utilisateur.|  
|idle_worker_count|**bigint**|Nombre de lignes à nettoyer traitées par le processus de travail inactif.|  
|sweep_scans_started|**bigint**|Nombre d'analyses d'angles inutilisés effectuées par le sous-système du garbage collection.|  
|sweep_scans_retries|**bigint**|Nombre d'analyses d'angles inutilisés effectuées par le sous-système du garbage collection.|  
|sweep_rows_touched|**bigint**|Lignes lues par le processus d'analyse d'angles inutilisés.|  
|sweep_rows_expiring|**bigint**|Lignes en cours d'expiration lues par le processus d'analyse d'angles inutilisés.|  
|sweep_rows_expired|**bigint**|Lignes expirées lues par le processus d'analyse d'angles inutilisés.|  
|sweep_rows_expired_removed|**bigint**|Lignes expirées supprimées par le processus d'analyse d'angles inutilisés.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW SERVER STATE sur l'instance.  
  
## <a name="usage-scenario"></a>Scénario d'utilisation  
 Voici un exemple de sortie :  
  
```  
rows_examined        rows_no_sweep_needed rows_first_in_bucket rows_first_in_bucket_removed  
280085               209512               69905  
rows_first_in_bucket_removed rows_marked_for_unlink parallel_assist_count idle_worker_count  
69905                        0                      8953  
  
idle_worker_count    sweep_scans_started  sweep_scan_retries   sweep_rows_touched  
10306473             670                  0                    1343  
  
sweep_rows_expiring  sweep_rows_expired   sweep_rows_expired_removed  
               0                 673673  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables optimisées en mémoire &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
