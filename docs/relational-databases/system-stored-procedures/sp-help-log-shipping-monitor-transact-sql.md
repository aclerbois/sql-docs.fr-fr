---
title: sp_help_log_shipping_monitor (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_help_log_shipping_monitor_TSQL
- sp_help_log_shipping_monitor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_monitor
ms.assetid: a4e96c45-6dcd-471a-a494-b5c619459855
caps.latest.revision: 23
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: e6259e0947794bef3bbfe2a6c2172be14295dc70
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelplogshippingmonitor-transact-sql"></a>sp_help_log_shipping_monitor (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne un jeu de résultats contenant l'état et d'autres informations pour les bases de données principale et secondaire d'un serveur principal, secondaire ou moniteur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_log_shipping_monitor  
```  
  
## <a name="arguments"></a>Arguments  
 Aucun.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**status**|**bit**|État collectif des agents de la base de données d'envoi des journaux :<br /><br /> **0** = sains et pas d’agent échecs.<br /><br /> **1** = dans le cas contraire.|  
|**is_primary**|**bit**|Indique si cette ligne est destinée à une base de données principale :<br /><br /> **1** = la ligne est une base de données principale.<br /><br /> **0** = la ligne est destinée à une base de données secondaire.|  
|**server**|**sysname**|Nom du serveur principal ou secondaire où réside cette base de données.|  
|**database_name**|**sysname**|Nom de la base de données.|  
|**time_since_last_backup**|**int**|Temps écoulé, en minutes, depuis la dernière sauvegarde du journal.<br /><br /> NULL = les informations ne sont pas disponibles ou ne sont pas appropriées.|  
|**last_backup_file**|**nvarchar(500)**|Nom du dernier fichier de sauvegarde valide du journal.<br /><br /> NULL = les informations ne sont pas disponibles ou ne sont pas appropriées.|  
|**backup_threshold**|**int**|Durée écoulée (en minutes) depuis la dernière sauvegarde avant qu'une erreur threshold_alert ne soit générée. **backup_threshold** est **int**, avec une valeur par défaut **60 minutes**.<br /><br /> NULL = les informations ne sont pas disponibles ou ne sont pas appropriées.<br /><br /> Cette valeur peut être modifiée à l’aide de [sp_add_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md).|  
|**is_backup_alert_enabled**|**bit**|Spécifie si une alerte est déclenchée lorsque **backup_threshold** est dépassé. La valeur d’une (**1**), la valeur par défaut, signifie que l’alerte sera déclenchée.<br /><br /> NULL = les informations ne sont pas disponibles ou ne sont pas appropriées.<br /><br /> Cette valeur peut être modifiée à l’aide de [sp_add_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md).|  
|**time_since_last_copy**|**int**|Temps écoulé, en minutes, depuis la dernière copie de la sauvegarde du journal.<br /><br /> NULL = les informations ne sont pas disponibles ou ne sont pas appropriées.|  
|**last_copied_file**|**nvarchar(500)**|Nom du dernier fichier de sauvegarde du journal copié avec succès.<br /><br /> NULL = les informations ne sont pas disponibles ou ne sont pas appropriées.|  
|**time_since_last_restore**|**int**|Temps écoulé, en minutes, depuis la dernière restauration de la sauvegarde du journal.<br /><br /> NULL = les informations ne sont pas disponibles ou ne sont pas appropriées.|  
|**last_restored_file**|**nvarchar (500).**|Nom du dernier fichier de sauvegarde du journal restauré avec succès.<br /><br /> NULL = les informations ne sont pas disponibles ou ne sont pas appropriées.|  
|**last_restored_latency**|**int**|Temps écoulé, en minutes, entre la création de la dernière sauvegarde et la restauration de cette sauvegarde.<br /><br /> NULL = les informations ne sont pas disponibles ou ne sont pas appropriées.|  
|**restore_threshold**|**int**|Nombre de minutes pouvant s'écouler entre les opérations de restauration avant qu'une alerte ne soit générée. **restore_threshold** ne peut pas être NULL.|  
|**is_restore_alert_enabled**|**bit**|Indique si une alerte est générée lorsque **restore_threshold** est dépassé. La valeur d’une (**1**), la valeur par défaut, signifie que l’alerte est déclenchée.<br /><br /> NULL = les informations ne sont pas disponibles ou ne sont pas appropriées.<br /><br /> Pour définir le seuil de restauration, utilisez [sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md).|  
  
## <a name="remarks"></a>Notes  
 **sp_help_log_shipping_monitor** doit être exécuté à partir de la **master** base de données sur le serveur moniteur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de journaux de transaction & #40 ; SQL Server & #41 ;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
