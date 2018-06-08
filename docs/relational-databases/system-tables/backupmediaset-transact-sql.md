---
title: backupmediaset (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- backupmediaset
- backupmediaset_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], backupmediaset system table
- backupmediaset system table
ms.assetid: d9c18a93-cab9-4db8-ae09-c6bd8145ab8f
caps.latest.revision: 39
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 21e327f7c630b106a52a0bd720cbf94f8b4a23fd
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="backupmediaset-transact-sql"></a>backupmediaset (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque support de sauvegarde. Cette table est stockée dans le **msdb** base de données.  
 
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**media_set_id**|**int**|Numéro d'identification unique du support de sauvegarde. Identité, clé primaire.|  
|**media_uuid**|**uniqueidentifier**|Identificateur UUID du support de sauvegarde. Tous les [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] jeux de supports ont un identificateur UUID.<br /><br /> Pour les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], toutefois, si un support de sauvegarde contient uniquement une famille de supports, le **media_uuid** colonne peut être NULL (**media_family_count** est 1).|  
|**media_family_count**|**tinyint**|Nombre de familles de supports dans le support de sauvegarde. Sa valeur peut être NULL.|  
|**nom**|**nvarchar(128)**|Nom du support de sauvegarde. Sa valeur peut être NULL.<br /><br /> Pour plus d’informations, consultez MEDIANAME et MEDIADESCRIPTION dans [sauvegarde &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).|  
|**description**|**nvarchar(255)**|Description textuelle du support de sauvegarde. Sa valeur peut être NULL.<br /><br /> Pour plus d’informations, consultez MEDIANAME et MEDIADESCRIPTION dans [sauvegarde &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).|  
|**software_name**|**nvarchar(128)**|Nom du logiciel de sauvegarde qui a écrit l’étiquette du support. Sa valeur peut être NULL.|  
|**software_vendor_id**|**int**|Numéro d'identification du fournisseur du logiciel qui a écrit l'étiquette du support de sauvegarde. Sa valeur peut être NULL.<br /><br /> La valeur de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est 0 x 1200 en hexadécimal.|  
|**MTF_major_version**|**tinyint**|Numéro de la version principale de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Tape Format utilisée pour créer ce support de sauvegarde. Sa valeur peut être NULL.|  
|**mirror_count**|**tinyint**|Nombre de miroirs dans le support de sauvegarde.|  
|**is_password_protected**|**bit**|Mot de passe protégé du support de sauvegarde :<br /><br /> 0 = Non protégé<br /><br /> 1 = Protégé|  
|**is_compressed**|**bit**|Indique si la sauvegarde est compressée :<br /><br /> 0 = Non compressée<br /><br /> 1 = Compressée<br /><br /> Pendant une **msdb** mise à niveau, cette valeur est définie sur NULL. ce qui indique une sauvegarde non compressée.|  
|**is_encrypted**|**Bit**|Indique si la sauvegarde est chiffrée :<br /><br /> 0 = Non chiffrée<br /><br /> 1 = Chiffrée|  
  
## <a name="remarks"></a>Notes  
 RESTORE VERIFYONLY FROM *backup_device* WITH LOADHISTORY remplit les colonnes de la **backupmediaset** table avec les valeurs appropriées de l’en-tête du support de sauvegarde.  
  
 Pour réduire le nombre de lignes dans cette table et dans d’autres tables d’historique de sauvegarde et, exécutez le [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) procédure stockée.  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde et restauration des Tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
