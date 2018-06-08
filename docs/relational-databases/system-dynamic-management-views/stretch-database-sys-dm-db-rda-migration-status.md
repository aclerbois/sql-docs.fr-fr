---
title: Sys.dm_db_rda_migration_status (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-stretch
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_rda_migration_status
- sys.dm_db_rda_migration_status_TSQL
- dm_db_rda_migration_status
- dm_db_rda_migration_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_rda_migration_status dynamic management view
ms.assetid: faf3901c-a0e0-4e0c-8b1b-86d9f15f34dd
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7a8778c09acc143bc6384f9b82ff03fa25603334
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="stretch-database---sysdmdbrdamigrationstatus"></a>Stretch Database - sys.dm_db_rda_migration_status
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque lot de données migrées à partir de chaque table compatible Stretch sur l’instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les lots sont identifiés par leur heure de début et l’heure de fin.  
  
 **Sys.dm_db_rda_migration_status** est étendue pour le contexte actuel de la base de données. Vérifiez que vous êtes dans le contexte de base de données des tables activer Stretch pour lequel vous souhaitez voir l’état de la migration.  
  
 Dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], la sortie de **sys.dm_db_rda_migration_status** est limitée à 200 lignes.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**table_id**|**int**|L’ID de la table à partir de laquelle les lignes ont été migrés.|  
|**database_id**|**int**|ID de la base de données à partir de laquelle les lignes ont été migrés.|  
|**migrated_rows**|**bigint**|Le nombre de lignes migrées dans ce lot.|  
|**start_time_utc**|**datetime**|L’heure UTC à laquelle le lot a démarré.|  
|**end_time_utc**|**datetime**|L’heure UTC à laquelle le lot est terminé.|  
|**error_number**|**int**|Si le lot échoue, le numéro d’erreur de l’erreur qui s’est produite ; Sinon, null.|  
|**error_severity**|**int**|Si le lot échoue, la gravité de l’erreur qui s’est produite ; Sinon, null.|  
|**error_state**|**int**|Si le lot échoue, l’état de l’erreur qui s’est produite ; Sinon, null.<br /><br /> Le **error_state** indique la condition ou l’emplacement où l’erreur s’est produite.|  
  
## <a name="see-also"></a>Voir aussi  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
