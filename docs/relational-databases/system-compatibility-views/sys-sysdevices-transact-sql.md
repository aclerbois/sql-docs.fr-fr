---
title: Sys.sysdevices (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-compatibility-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysdevices
- sysdevices_TSQL
- sys.sysdevices
- sys.sysdevices_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysdevices compatibility view
- sysdevices system table
ms.assetid: ac5bcaf4-8fb6-4855-8856-d7643f469361
caps.latest.revision: 24
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: bf1a22fae1c4c8f69edf843c1a126844abdd1190
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syssysdevices-transact-sql"></a>sys.sysdevices (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque fichier de sauvegarde sur disque, chaque fichier de sauvegarde sur bande et chaque fichier de base de données.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Nom logique du fichier de sauvegarde ou du fichier de base de données.|  
|**size**|**int**|Taille du fichier en pages de 2 kilo-octets (Ko).|  
|**Faible**|**int**|Conservé pour compatibilité descendante uniquement.|  
|**Haute**|**int**|Conservé pour compatibilité descendante uniquement.|  
|**status**|**smallint**|Bitmap indiquant le type de périphérique :<br /><br /> 1 = Disque par défaut<br /><br /> 2 = Disque physique<br /><br /> 4 = Disque logique<br /><br /> 8 = Omettre en-tête<br /><br /> 16 = Fichier de sauvegarde<br /><br /> 32 = Écritures en série<br /><br /> 4096 = Lecture seule|  
|**cntrltype**|**smallint**|Type de contrôleur :<br /><br /> 0 = Fichier de base de données non CD-ROM<br /><br /> 2 = Fichier de sauvegarde sur disque<br /><br /> 3 - 4 = Fichier de sauvegarde sur disquette<br /><br /> 5 = Fichier de sauvegarde sur bande<br /><br /> 6 = Fichier de canal nommé|  
|**phyname**|**nvarchar(260)**|Nom du fichier physique.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage des Tables système avec les vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
