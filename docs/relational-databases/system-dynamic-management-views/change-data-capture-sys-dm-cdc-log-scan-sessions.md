---
title: Sys.dm_cdc_log_scan_sessions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_cdc_log_scan_sessions
- dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], log scan reporting
- sys.dm_cdc_log_scan_sessions dynamic management view
ms.assetid: d337e9d0-78b1-4a07-8820-2027d0b9f87c
caps.latest.revision: 17
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 74a99cc08a030327c4f0b70f11c64f6c2a952dec
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="change-data-capture---sysdmcdclogscansessions"></a>Capture de données modifiées - sys.dm_cdc_log_scan_sessions
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque session d'analyse du journal dans la base de données actuelle. La dernière ligne retournée représente la session active. Vous pouvez utiliser cette vue pour retourner des informations d'état sur la session d'analyse du journal actuelle, ou des informations de synthèse sur toutes les sessions depuis le dernier démarrage de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
   
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID de la session.<br /><br /> 0 = les données retournées dans cette ligne sont un agrégat de toutes les sessions depuis le dernier démarrage de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**start_time**|**datetime**|Heure de début de la session.<br /><br /> Lorsque **session_id** = 0, l’heure de début de la collecte des données agrégées.|  
|**end_time**|**datetime**|Heure à laquelle la session est terminée.<br /><br /> NULL = la session est active.<br /><br /> Lorsque **session_id** = 0, heure de la dernière session s’est terminée.|  
|**duration**|**bigint**|Durée (en secondes) de la session.<br /><br /> 0 = la session ne contient pas de transactions de capture des données modifiées.<br /><br /> Lorsque **session_id** = 0, la somme de la durée (en secondes) de toutes les sessions avec les transactions de capture de données modifiées.|  
|**scan_phase**|**nvarchar(200)**|Phase actuelle de la session. Voici les valeurs possibles et leurs descriptions :<br /><br /> 1 : configuration de la lecture<br />2 : première analyse, création de table de hachage<br />3 : deuxième analyse<br />4 : deuxième analyse<br />5 : deuxième analyse<br />6 : contrôle de version de schéma<br />7 : la dernière analyse<br />8 : terminé<br /><br /> Lorsque **session_id** = 0, cette valeur est toujours « Agrégat ».|  
|**error_count**|**int**|Nombre d'erreurs rencontrées.<br /><br /> Lorsque **session_id** = 0, le nombre total d’erreurs dans toutes les sessions.|  
|**start_lsn**|**nvarchar(23)**|Numéro séquentiel dans le journal de démarrage pour la session.<br /><br /> Lorsque **session_id** = 0, le LSN de début pour la dernière session.|  
|**current_lsn**|**nvarchar(23)**|Numéro séquentiel dans le journal actuel qui est analysé.<br /><br /> Lorsque **session_id** = 0, le numéro LSN actuel est 0.|  
|**end_lsn**|**nvarchar(23)**|Numéro séquentiel dans le journal de fin pour la session.<br /><br /> NULL = la session est active.<br /><br /> Lorsque **session_id** = 0, le LSN de fin pour la dernière session.|  
|**tran_count**|**bigint**|Nombre de transactions de capture des données modifiées traitées. Ce compteur est rempli en phase 2.<br /><br /> Lorsque **session_id** = 0, le nombre de transactions traitées dans toutes les sessions.|  
|**last_commit_lsn**|**nvarchar(23)**|Numéro séquentiel dans le journal du dernier enregistrement du journal de validation traité.<br /><br /> Lorsque **session_id** = 0, la dernière validation journal numéro LSN d’enregistrement pour toute session.|  
|**last_commit_time**|**datetime**|Heure de traitement du dernier enregistrement du journal de validation.<br /><br /> Lorsque **session_id** = 0, heure de la validation du dernier enregistrement du journal pour n’importe quelle session.|  
|**log_record_count**|**bigint**|Nombre d'enregistrements du journal analysés.<br /><br /> Lorsque **session_id** = 0, nombre d’enregistrements analysés pour toutes les sessions.|  
|**schema_change_count**|**int**|Nombre d'opérations de langage de définition de données (DDL) détectées. Ce compteur est rempli lors de la phase 6.<br /><br /> Lorsque **session_id** = 0, le nombre d’opérations DDL traitées dans toutes les sessions.|  
|**command_count**|**bigint**|Nombre de commandes traitées.<br /><br /> Lorsque **session_id** = 0, le nombre de commandes traitées dans toutes les sessions.|  
|**first_begin_cdc_lsn**|**nvarchar(23)**|Premier numéro séquentiel dans le journal qui contenait des transactions de capture des données modifiées.<br /><br /> Lorsque **session_id** = 0, le premier numéro séquentiel qui contenait des transactions de capture de données modifiées.|  
|**last_commit_cdc_lsn**|**nvarchar(23)**|Numéro séquentiel dans le journal du dernier enregistrement du journal de validation qui contenait des transactions de capture des données modifiées.<br /><br /> Lorsque **session_id** = 0, le dernier enregistrement du journal LSN de validation pour toute session qui contenait des transactions de capture de données de modification|  
|**last_commit_cdc_time**|**datetime**|Heure de traitement du dernier enregistrement du journal de validation qui contenait des transactions de capture des données modifiées.<br /><br /> Lorsque **session_id** = 0, l’heure du dernier journal de validation enregistrer pour n’importe quelle session qui contenait transactions de capture de données modifiées.|  
|**Temps de latence**|**int**|La différence, en secondes, entre **end_time** et **last_commit_cdc_time** dans la session. Ce compteur est rempli à la fin de la phase 7.<br /><br /> Lorsque **session_id** = 0, la dernière valeur de latence différente de zéro enregistrée par une session.|  
|**empty_scan_count**|**int**|Nombre de sessions consécutives qui ne contenaient aucune transaction de capture des données modifiées.|  
|**failed_sessions_count**|**int**|Nombre de sessions qui ont échoué.|  
  
## <a name="remarks"></a>Notes  
 Les valeurs dans cette vue de gestion dynamique sont réinitialisées à chaque démarrage de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’autorisation VIEW DATABASE STATE pour interroger le **sys.dm_cdc_log_scan_sessions** vue de gestion dynamique. Pour plus d’informations sur les autorisations sur les vues de gestion dynamique, consultez [fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations pour la session la plus active.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT session_id, start_time, end_time, duration, scan_phase  
    error_count, start_lsn, current_lsn, end_lsn, tran_count  
    last_commit_lsn, last_commit_time, log_record_count, schema_change_count  
    command_count, first_begin_cdc_lsn, last_commit_cdc_lsn,   
    last_commit_cdc_time, latency, empty_scan_count, failed_sessions_count  
FROM sys.dm_cdc_log_scan_sessions  
WHERE session_id = (SELECT MAX(b.session_id) FROM sys.dm_cdc_log_scan_sessions AS b);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.dm_cdc_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md)  
  
  

