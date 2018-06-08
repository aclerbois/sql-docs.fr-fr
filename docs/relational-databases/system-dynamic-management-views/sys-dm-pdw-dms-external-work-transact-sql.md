---
title: Sys.dm_pdw_dms_external_work (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 47345015-f861-451e-97c4-6e1cb81d1922
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: d980034474bc08e57044ecdcf555877b50a0c1f2
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmpdwdmsexternalwork-transact-sql"></a>Sys.dm_pdw_dms_external_work (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] vue système qui conserve des informations sur toutes les étapes de Service de déplacement des données (DMS) pour les opérations externes.  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Requête qui utilise ce processus de travail DMS.<br /><br /> request_id, step_index et dms_step_index forment la clé pour cette vue.|Identique à request_id dans [sys.dm_pdw_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Étape de requête qui appelle ce processus de travail DMS.<br /><br /> request_id, step_index et dms_step_index forment la clé pour cette vue.|Identique à step_index dans [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|dms_step_index|**int**|Étape actuelle dans le plan DMS.<br /><br /> request_id, step_index et dms_step_index forment la clé pour cette vue.|Identique à dms___step_index dans [sys.dm_pdw_dms_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql.md).|  
|pdw_node_id|**int**|Nœud qui exécute le processus de travail DMS.|Identique à node_id dans [sys.dm_pdw_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|Type|**nvarchar(60)**|Type d’opération externe de que ce nœud est en cours d’exécution.<br /><br /> FICHIER fractionné est une opération sur un fichier Hadoop externe qui a été fractionné en plusieurs concernent la plage inférieure.|« FRACTIONNEMENT DE FICHIER »|  
|work_id|**int**|Le fichier fractionné ID.|Supérieur ou égal à 0.<br /><br /> Unique pour chaque nœud de calcul.|  
|input_name|**nvarchar(60)**|Nom de l’entrée en cours de lecture de la chaîne.|Pour un fichier Hadoop, ceci est le nom de fichier Hadoop.|  
|read_location|**bigint**|Décalage de l’emplacement de lecture.||  
|estimated_bytes_processed|**bigint**|Nombre d’octets traités par ce processus de travail.|Supérieur ou égal à 0.|  
|length|**bigint**|Nombre d’octets dans le fichier de fractionnement.<br /><br /> Pour Hadoop, il s’agit de la taille du bloc HDFS.|Défini par l’utilisateur. La valeur par défaut est de 64 Mo.|  
|status|**nvarchar(32)**|État du processus de travail.|En attente, traitement, terminé, échec, abandonnée|  
|start_time|**datetime**|Heure de début de l’exécution de ce processus de travail.|Supérieur ou égal à l’heure de début de l’étape de requête qu'auquel appartient ce processus de travail. Consultez [sys.dm_pdw_request_steps &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|end_time|**datetime**|Heure à laquelle l’exécution s’est terminée, a échoué ou a été annulée.|NULL pour les traitements en cours ou en file d’attente. Sinon, supérieur à heure_début.|  
|total_elapsed_time|**int**|Temps total passé à l’exécution, en millisecondes.|Supérieur ou égal à 0.<br /><br /> Si total_elapsed_time dépasse la valeur maximale pour un entier, total_elapsed_time continueront à être la valeur maximale. Cette condition génère l’avertissement « la valeur maximale a été dépassée. »<br /><br /> La valeur maximale, en millisecondes est équivalente à 24.8 jours.|  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez [valeurs de la vue système Maximum](http://msdn.microsoft.com/en-us/5243f018-2713-45e3-9b61-39b2a57401b9).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90)  
  
  
