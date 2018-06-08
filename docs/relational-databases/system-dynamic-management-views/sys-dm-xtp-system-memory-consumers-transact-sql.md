---
title: Sys.dm_xtp_system_memory_consumers (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_xtp_system_memory_consumers
- sys.dm_xtp_system_memory_consumers_TSQL
- sys.dm_xtp_system_memory_consumers
- dm_xtp_system_memory_consumers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xtp_system_memory_consumers dynamic management view
ms.assetid: 9eb0dd82-7920-42e0-9e50-7ce6e7ecee8b
caps.latest.revision: 22
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 3363fa2208f735c38ebd696b782fced80e5c49ce
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmxtpsystemmemoryconsumers-transact-sql"></a>sys.dm_xtp_system_memory_consumers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]

  Indique les consommateurs de mémoire au niveau système pour l'[!INCLUDE[hek_2](../../includes/hek-2-md.md)]. La mémoire pour ces consommateurs vient du pool par défaut (lorsque l'allocation est dans le contexte d'un thread d'utilisateur) ou du pool interne (si l'allocation est dans le contexte d'un thread système).  
  
```  
-- system memory consumers @ instance  
select * from sys.dm_xtp_system_memory_consumers  
```  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|memory_consumer_id|**bigint**|ID interne du consommateur de mémoire.|  
|memory_consumer_type|**int**|Entier qui représente le type de consommateur de mémoire avec l’une des valeurs suivantes :<br /><br /> 0 – Ne doit pas être affiché. Regroupe l'utilisation de la mémoire de deux consommateurs ou plus.<br /><br /> 1 – LOOKASIDE : Effectue le suivi de la consommation de mémoire pour un système lookaside.<br /><br /> 2 - VARHEAP : Effectue le suivi de la consommation de mémoire pour un segment de longueur variable.<br /><br /> 4 - pool de pages d’e/s : effectue le suivi de la consommation de mémoire pour un pool de pages système utilisé pour les opérations d’e/s.|  
|memory_consumer_type_desc|**nvarchar(16)**|Description du type de consommateur de mémoire :<br /><br /> 0 – Ne doit pas être affiché.<br /><br /> 1 – LOOKASIDE<br /><br /> 2 - VARHEAP<br /><br /> 4 - PGPOOL|  
|memory_consumer_desc|**nvarchar(64)**|Description de l'instance de consommateur de mémoire :<br /><br /> VARHEAP : <br />Segment du système. Usage universel. Actuellement utilisé uniquement pour allouer des éléments de travail de garbage collection.<br />-OU-<br />Segment lookaside. Utilisé par les looksides lorsque le nombre d'éléments contenus dans la liste de disponibilité atteint un atteint une extrémité de fin prédéfinie (en règle générale environ 5 000 éléments).<br /><br /> PGPOOL : Pour système d’e/s pools il sont trois pool de pages de 4 Ko de système de différentes tailles, pool de pages 64 Ko et pool de pages de 256 Ko.|  
|lookaside_id|**bigint**|ID du fournisseur de mémoire lookaside, ThreadLocal.|  
|pagepool_id|**bigint**|ID du fournisseur de mémoire de pool de pages, ThreadLocal.|  
|allocated_bytes|**bigint**|Nombre d'octets réservés pour ce consommateur.|  
|used_bytes|**bigint**|Octets utilisés par ce consommateur. S'applique uniquement aux consommateurs de mémoire varheap.|  
|allocation_count|**int**|Nombre d'allocations.|  
|partition_count|**int**|À usage interne uniquement|  
|sizeclass_count|**int**|À usage interne uniquement|  
|min_sizeclass|**int**|À usage interne uniquement|  
|max_sizeclass|**int**|À usage interne uniquement|  
|memory_consumer_address|**varbinary**|Adresse interne du consommateur.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite des autorisations VIEW SERVER STATE sur le serveur.  
  
## <a name="user-scenario"></a>Scénario d'utilisateur  
  
```  
-- system memory consumers @ instance  
selectmemory_consumer_type_desc,   
allocated_bytes/1024 as allocated_bytes_kb,   
used_bytes/1024 as used_bytes_kb, allocation_count  
from sys.dm_xtp_system_memory_consumers  
```  
  
 Le résultat montre tous les consommateurs de mémoire au niveau du système. Pour cet exemple, il existe des consommateurs pour la transaction disponible.  
  
```  
memory_consumer_type_name           memory_consumer_desc                           allocated_bytes_kb   used_bytes_kb        allocation_count  
-------------------------------          ---------------------                          -------------------  --------------        ----------------  
VARHEAP                                  Lookaside heap                                 0                    0                    0  
VARHEAP                                  System heap                                    768                  0                    2  
LOOKASIDE                                GC transaction map entry                       64                   64                   910  
LOOKASIDE                                Redo transaction map entry                     128                  128                  1260  
LOOKASIDE                                Recovery table cache entry                     448                  448                  8192  
LOOKASIDE                                Transaction recent rows                        3264                 3264                 4444  
LOOKASIDE                                Range cursor                                   0                    0                    0  
LOOKASIDE                                Hash cursor                                    3200                 3200                 11070  
LOOKASIDE                                Transaction save-point set entry               0                    0                    0  
LOOKASIDE                                Transaction partially-inserted rows set        704                  704                  1287  
LOOKASIDE                                Transaction constraint set                     576                  576                  1940  
LOOKASIDE                                Transaction save-point set                     0                    0                    0  
LOOKASIDE                                Transaction write set                          704                  704                  672  
LOOKASIDE                                Transaction scan set                           320                  320                  156  
LOOKASIDE                                Transaction read set                           704                  704                  343  
LOOKASIDE                                Transaction                                    4288                 4288                 1459  
PGPOOL                                   System 256K page pool                          5120                 5120                 20  
PGPOOL                                   System 64K page pool                           0                    0                    0  
PGPOOL                                   System 4K page pool                            24                   24                   6  
```  
  
 Pour voir la mémoire totale consommée par les allocateurs système :  
  
```  
select sum(allocated_bytes)/(1024*1024) as total_allocated_MB, sum(used_bytes)/(1024*1024) as total_used_MB   
from sys.dm_xtp_system_memory_consumers  
  
total_allocated_MB   total_used_MB  
-------------------- --------------------  
2                    2  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables optimisées en mémoire &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
