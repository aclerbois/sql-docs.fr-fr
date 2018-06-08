---
title: restorehistory (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- restorehistory
- restorehistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- restorehistory system table
ms.assetid: 9140ecc1-d912-4d76-ae70-e2a857da6d44
caps.latest.revision: 28
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 72f788cf3248ba87bfa4ded7efed219ef3043cf5
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="restorehistory-transact-sql"></a>restorehistory (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne par opération de restauration. Cette table est stockée dans le **msdb** base de données.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**restore_history_id**|**int**|Numéro d'identification unique de chaque opération de restauration Identité, clé primaire.|  
|**restore_date**|**datetime**|Date et heure de fin de l'opération de restauration. Sa valeur peut être NULL.|  
|**destination_database_name**|**nvarchar(128)**|Nom de la base de données de destination à utiliser pour l'opération de restauration. Sa valeur peut être NULL.|  
|**user_name**|**nvarchar(128)**|Nom de l'utilisateur qui a exécuté l'opération de restauration. Sa valeur peut être NULL.|  
|**backup_set_id**|**int**|Numéro d'identification unique identifiant le jeu de sauvegarde en cours de restauration. Références **backupset (backup_set_id)**.|  
|**restore_type**|**char(1)**|Type d'opération de restauration :<br /><br /> D = Base de données<br /><br /> F = Fichier<br /><br /> G = Groupe de fichiers<br /><br /> I = Différentielle<br /><br /> L = Journal<br /><br /> V = Vérification uniquement<br /><br /> Sa valeur peut être NULL.|  
|**replace**|**bit**|Indique si l'option REPLACE a été spécifiée pour l'opération de restauration :<br /><br /> 1 = Spécifiée<br /><br /> 0 = Non spécifiée<br /><br /> Sa valeur peut être NULL.<br /><br /> Lorsqu'un instantané de base de données est rétabli, 0 est la seule option disponible.|  
|**recovery**|**bit**|Indique si l'option RECOVERY ou NORECOVERY a été spécifiée pour l'opération de restauration :<br /><br /> 1 = RECOVERY<br /><br /> Sa valeur peut être NULL.<br /><br /> Lorsqu’une base de données est rétabli à un instantané de base de données, 1 est la seule option.<br /><br /> 0 = NORECOVERY|  
|**restart**|**bit**|Indique si l'option RESTART a été spécifiée pour l'opération de restauration :<br /><br /> 1 = Spécifiée<br /><br /> 0 = Non spécifiée<br /><br /> Sa valeur peut être NULL.<br /><br /> Lorsqu'un instantané de base de données est rétabli, 0 est la seule option disponible.|  
|**stop_at**|**datetime**|Limite dans le temps de la restauration de la base de données Sa valeur peut être NULL.|  
|**device_count**|**tinyint**|Nombre de périphériques concernés par l'opération de restauration. Ce nombre peut être inférieur au nombre de familles de supports utilisées pour la sauvegarde. Sa valeur peut être NULL.<br /><br /> Lorsqu'un instantané de base de données est rétabli, la valeur est toujours 1.|  
|**stop_at_mark_name**|**nvarchar(128)**|Indique la récupération au point de la transaction contenant la marque nommée. Sa valeur peut être NULL.<br /><br /> Lorsqu'un instantané de base de données est rétabli, cette valeur est NULL.|  
|**stop_before**|**bit**|Indique si la transaction contenant la marque nommée est incluse dans la récupération :<br /><br /> 0 = Récupération arrêtée avant la transaction marquée<br /><br /> 1 = Transaction marquée incluse dans la récupération<br /><br /> Sa valeur peut être NULL.<br /><br /> Lorsqu'un instantané de base de données est rétabli, cette valeur est NULL.|  
  
## <a name="remarks"></a>Notes  
 Pour réduire le nombre de lignes dans cette table et dans d’autres tables d’historique de sauvegarde et, exécutez le [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) procédure stockée.  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde et restauration des Tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [restorefile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/restorefile-transact-sql.md)   
 [restorefilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
