---
title: managed_backup.sp_get_backup_diagnostics (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_get_backup_diagnostics_TSQL
- sp_get_backup_diagnostics
- smart_admin.sp_get_backup_diagnostics_TSQL
- smart_admin.sp_get_backup_diagnostics
dev_langs:
- TSQL
helpviewer_keywords:
- sp_get_backup_diagnostics
- smart_admin.sp_get_backup_diagnostics
ms.assetid: 2266a233-6354-464b-91ec-824ca4eb9ceb
caps.latest.revision: 12
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: e2b2f8c78b1802ff177040352cd7342b51b035c6
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="managedbackupspgetbackupdiagnostics-transact-sql"></a>managed_backup.sp_get_backup_diagnostics (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Retourne les événements étendus enregistrés par Smart Admin.  
  
 Utilisez cette procédure stockée pour surveiller les événements étendus enregistrés par Smart Admin. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] événements sont enregistrés dans le système et peuvent être vérifiées et analysées à l’aide de cette procédure stockée.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
managed_backup.sp_get_backup_diagnostics [@xevent_channel = ] 'event type' [, [@begin_time = ] 'time1' ] [, [@end_time = ] 'time2'VARCHAR(255) = 'Xevent',@begin_time DATETIME = NULL,@end_time DATETIME = NULL  
```  
  
##  <a name="Arguments"></a> Arguments  
 @xevent_channel  
 Type d'événement étendu. La valeur par défaut retourne tous les événements enregistrés au cours des 30 dernières minutes. Les événements enregistrés dépendent des types d'événements étendus activés. Vous pouvez utiliser ce paramètre pour filtrer la procédure stockée et afficher uniquement les événements d'un type spécifique. Vous pouvez spécifier le nom d’événement complet ou spécifier une sous-chaîne comme : **'Admin'**, **'Analytic'**, **'Operational'**, et **'Debug'**. Le @event_channel est **VARCHAR (255)**.  
  
 Pour obtenir une liste d’utilisation de types actuellement activées événement la **managed_backup.fn_get_current_xevent_settings** (fonction).  
  
 [@begin_time  
 Début de la période à partir de laquelle les événements doivent être affichés. Le @begin_time paramètre est de type DATETIME avec NULL comme valeur par défaut. S'il n'est pas spécifié, les événements correspondant aux dernières 30 minutes sont affichés.  
  
 @end_time  
 Fin de la période jusqu'à laquelle les événements doivent être affichés. Le @end_time paramètre est DateTime avec NULL comme valeur par défaut.  S'il n'est pas spécifié, les événements qui se sont produits jusqu'à l'heure actuelle sont affichés.  
  
## <a name="table-returned"></a>Table retournée  
 Cette procédure stockée retourne une table contenant les informations suivantes :  
  
||||  
|-|-|-|  
|Nom de la colonne|Type de données| Description|  
|event_type|NVARCHAR (512)|Type d’événement étendu.|  
|Événement|NVARCHAR (512)|Récapitulatif des journaux d'événements.|  
|Horodateur|TIMESTAMP|Horodateur de l'événement indiquant à quel moment l'événement s'est produit.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert **EXECUTE** autorisations sur la procédure stockée. Elle requiert également **VIEW SERVER STATE** autorisations, car elle appelle en interne autres objets système qui nécessitent cette autorisation.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne tous les événements enregistrés au cours des 30 dernières minutes.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics  
  
```  
  
 L'exemple suivant retourne tous les événements enregistrés au cours d'une plage de temps spécifique.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics @xevent_channel = 'Admin',  
  @begin_time = '2013-06-01', @end_time = '2013-06-10'  
  
```  
  
 L'exemple suivant retourne tous les événements analytiques enregistrés au cours des 30 dernières minutes.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics @xevent_channel = 'Analytic'  
  
```  
  
  
