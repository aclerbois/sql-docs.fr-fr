---
title: Sys.traces (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- traces
- sys.traces_TSQL
- sys.traces
- traces_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.traces catalog view
ms.assetid: 4a03be22-b7da-4e2a-97ff-94bed890a620
caps.latest.revision: 23
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 8cabef6e979c613c977d7e5636f46a10b447fbbe
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="systraces-transact-sql"></a>sys.traces (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **sys.traces** affichage catalogue contient les traces en cours d’exécution en cours sur le système. Cette vue est destinée à remplacer pour les **fn_trace_getinfo** (fonction).  
  
 Pour obtenir une liste complète des événements de trace pris en charge, consultez [SQL Server Event Class Reference](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt les affichages catalogue des événements étendus.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de la trace.|  
|**status**|**int**|État de la trace :<br /><br /> 0 = arrêtée<br /><br /> 1 = en cours d’exécution|  
|**path**|**nvarchar(260)**|Chemin du fichier de trace Cette valeur est NULL lorsque la trace représente une trace d'ensemble de lignes.|  
|**max_size**|**bigint**|Limite de la taille de fichier de trace maximale en mégaoctets (Mo). Cette valeur est NULL lorsque la trace représente une trace d'ensemble de lignes.|  
|**stop_time**|**datetime**|Heure d'arrêt de trace d'exécution.|  
|**max_files**|**int**|Nombre maximal de fichiers de substitution. Cette valeur est NULL lorsque le nombre maximal n'est pas défini.|  
|**is_rowset**|**bit**|1 = trace d'ensemble de lignes.|  
|**is_rollover**|**bit**|1 = l'option de substitution est activée.|  
|**is_shutdown**|**bit**|1 = l'option d'arrêt est activée.|  
|**is_default**|**bit**|1 = trace par défaut.|  
|**buffer_count**|**int**|Nombre de tampons en mémoire utilisés par la trace.|  
|**buffer_size**|**int**|Taille de chaque tampon (Ko).|  
|**file_position**|**bigint**|Dernière position d'un fichier de trace. Cette valeur est NULL lorsque la trace représente une trace d'ensemble de lignes.|  
|**reader_spid**|**int**|ID de session de lecteur de trace d'ensemble de lignes. Cette valeur est NULL lorsque la trace est un fichier de trace.|  
|**start_time**|**datetime**|Heure de début de trace.|  
|**last_event_time**|**datetime**|Heure du dernier déclenchement d'événement.|  
|**event_count**|**bigint**|Nombre total d'événements qui se sont produits.|  
|**dropped_event_count**|**int**|Nombre total d'événements supprimés.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Sys.trace_categories &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trace-categories-transact-sql.md)   
 [sys.trace_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trace-columns-transact-sql.md)   
 [Sys.trace_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trace-events-transact-sql.md)   
 [Sys.trace_event_bindings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trace-event-bindings-transact-sql.md)   
 [sys.trace_subclass_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trace-subclass-values-transact-sql.md)  
  
  
