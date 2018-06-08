---
title: dbo.cdc_jobs (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- cdc_jobs
- cdc_jobs_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dbo.cdc_jobs
ms.assetid: 85e2d580-1c54-4b81-b7e6-2e12997199fd
caps.latest.revision: 17
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 1ea10a7d9959fa29367e69cfc2d5831ae44dd292
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbocdcjobs-transact-sql"></a>dbo.cdc_jobs (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Stocke les paramètres de configuration de capture de données modifiées pour les travaux de capture et de nettoyage. Cette table est stockée dans **msdb**.  
  
 
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de la base de données dans laquelle le travail est exécuté.|  
|**type_du_travail**|**nvarchar(20)**|Type de travail, capture ou nettoyage.|  
|**job_id**|**uniqueidentifier**|ID unique associé au travail.|  
|**maxtrans**|**int**|Le nombre maximal de transactions à traiter dans chaque cycle d’analyse.<br /><br /> **maxtrans** est valide uniquement pour les travaux de capture.|  
|**maxscans**|**int**|Le nombre maximal de cycles d’analyse à exécuter afin d’extraire toutes les lignes du journal.<br /><br /> **maxscans** est valide uniquement pour les travaux de capture.|  
|**Continue**|**bit**|Indicateur qui précise si le travail de capture doit être exécuté en continu (1) ou ponctuellement (0). Pour plus d’informations, consultez [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md).<br /><br /> **continue** est valide uniquement pour les travaux de capture.|  
|**PollingInterval**|**bigint**|Nombre de secondes entre les cycles d'analyse du journal.<br /><br /> **PollingInterval** est valide uniquement pour les travaux de capture.|  
|**retention**|**bigint**|Nombre de minutes pendant lesquelles les lignes modifiées doivent être conservées dans les tables de modifications.<br /><br /> **rétention** est valide uniquement pour les travaux de nettoyage.|  
|**Seuil**|**bigint**|Nombre maximal d'entrées qui peuvent être supprimées à l'aide d'une instruction unique lors du nettoyage.|  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)   
 [Sys.sp_cdc_change_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md)   
 [Sys.sp_cdc_help_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md)   
 [Sys.sp_cdc_drop_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-drop-job-transact-sql.md)  
  
  
