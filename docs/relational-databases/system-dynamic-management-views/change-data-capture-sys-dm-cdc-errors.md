---
title: Sys.dm_cdc_errors (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_cdc_errors_TSQL
- dm_cdc_errors
- sys.dm_cdc_errors
- sys.dm_cdc_errors_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cdc_errors dynamic management view
- change data capture [SQL Server], error reporting
ms.assetid: 898f2d76-9e63-45ef-94da-8034e86004ab
caps.latest.revision: 14
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 488e9eb3695dea1eada4b57d092451d40131f419
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="change-data-capture---sysdmcdcerrors"></a>Capture de données modifiées - sys.dm_cdc_errors
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque erreur rencontrée pendant la session d'analyse du journal de la capture de données modifiées.  
 
 
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID de la session.<br /><br /> 0 = l'erreur ne s'est pas produite dans une session d'analyse du journal.|  
|**phase_number**|**int**|Nombre indiquant la phase de que la session a été au moment où l’erreur s’est produite. Pour obtenir une description de chaque phase, consultez [sys.dm_cdc_log_scan_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md).|  
|**entry_time**|**datetime**|Date et heure d'enregistrement de l'erreur. Cette valeur correspond à l'horodateur dans le journal des erreurs SQL.|  
|**error_number**|**int**|ID du message d'erreur.|  
|**error_severity**|**int**|Niveau de gravité du message, entre 1 et 25.|  
|**error_state**|**int**|Numéro d'état de l'erreur.|  
|**error_message**|**nvarchar(1024)**|Texte du message de l'erreur.|  
|**start_lsn**|**nvarchar(23)**|Valeur LSN de départ des lignes en cours de traitement lorsque l'erreur s'est produite.<br /><br /> 0 = l'erreur ne s'est pas produite dans une session d'analyse du journal.|  
|**begin_lsn**|**nvarchar(23)**|Valeur LSN de départ de la transaction en cours de traitement lorsque l'erreur s'est produite.<br /><br /> 0 = l'erreur ne s'est pas produite dans une session d'analyse du journal.|  
|**sequence_value**|**nvarchar(23)**|Valeur LSN des lignes en cours de traitement lorsque l'erreur s'est produite.<br /><br /> 0 = l'erreur ne s'est pas produite dans une session d'analyse du journal.|  
  
## <a name="remarks"></a>Notes  
 **Sys.dm_cdc_errors** contient des informations d’erreur pour les 32 sessions précédentes.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’autorisation VIEW DATABASE STATE pour interroger le **sys.dm_cdc_errors** vue de gestion dynamique. Pour plus d’informations sur les autorisations sur les vues de gestion dynamique, consultez [fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_cdc_log_scan_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md)   
 [sys.dm_repl_traninfo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-repl-traninfo-transact-sql.md)  
  
  

