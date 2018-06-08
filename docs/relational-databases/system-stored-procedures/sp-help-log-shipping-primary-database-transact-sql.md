---
title: sp_help_log_shipping_primary_database (Transact-SQL) | Documents Microsoft
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
- sp_help_log_shipping_primary_database_TSQL
- sp_help_log_shipping_primary_database
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_primary_database
ms.assetid: e711b01c-ef29-4eb6-a016-0e647e337818
caps.latest.revision: 28
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 56f3955c179e73d30172f0ed2126a600b1c1771a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelplogshippingprimarydatabase-transact-sql"></a>sp_help_log_shipping_primary_database (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Extrait les paramètres de la base de données primaire.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_log_shipping_primary_database  
[ @database = ] 'database' OR  
[ @primary_id = ] 'primary_id'  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@database =** ] '*base de données*'  
 Nom de la base de données primaire pour la copie des journaux de transaction. *base de données* est **sysname**, sans valeur par défaut, et ne peut pas être NULL.  
  
 [  **@primary_id =** ] '*primary_id*'  
 ID de la base de données primaire pour la configuration de la copie des journaux de transaction. *primary_id* est **uniqueidentifier** et ne peut pas être NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne| Description|  
|-----------------|-----------------|  
|**primary_id**|ID de la base de données primaire pour la configuration de la copie des journaux de transaction.|  
|**primary_database**|Nom de la base de données primaire dans la configuration d'envoi de journaux.|  
|**backup_directory**|Répertoire où les fichiers de sauvegarde des journaux de transactions du serveur principal sont stockés.|  
|**backup_share**|Réseau ou chemin d'accès UNC au répertoire de sauvegarde.|  
|**backup_retention_period**|Durée de conservation (en minutes) avant la suppression d'un fichier de sauvegarde de journal dans le répertoire de sauvegarde.|  
|**backup_compression**|Indique si la configuration de copie des journaux utilise [compression de la sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md).<br /><br /> **0** = désactivé. Ne jamais compresser des sauvegardes de journal.<br /><br /> **1** = activé. Toujours compresser des sauvegardes de journal.<br /><br /> **2** = utiliser le paramètre de la [afficher ou configurer l’Option de Configuration de serveur par défaut de compression de la sauvegarde](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Ceci est la valeur par défaut.<br /><br /> La compression de la sauvegarde est prise en charge uniquement dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (ou les versions ultérieures). Dans les autres éditions, la valeur est toujours 2.|  
|**backup_job_id**|Le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ID de tâche de l’Agent associé à la tâche de sauvegarde sur le serveur principal.|  
|**monitor_server**|Le nom de l’instance de la [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilisé en tant que serveur moniteur dans la configuration d’envoi de journaux.|  
|**monitor_server_security_mode**|Mode de sécurité utilisé pour la connexion au serveur moniteur.<br /><br /> 1 = Authentification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.|  
|**backup_threshold**|Le nombre de minutes qui peuvent s'écouler entre les opérations de sauvegarde avant le déclenchement d'une alerte.|  
|**threshold_alert**|Alerte à déclencher lorsque le seuil de sauvegarde est dépassé.|  
|**threshold_alert_enabled**|Détermine si les alertes du seuil de sauvegarde sont activées.<br /><br /> **1** = activé.<br /><br /> **0** = désactivé.|  
|**last_backup_file**|Chemin d'accès absolu de la sauvegarde la plus récente des journaux de transactions.|  
|**last_backup_date**|Date et heure de la dernière opération de sauvegarde des journaux.|  
|**last_backup_date_utc**|Date et heure de la dernière opération de sauvegarde des journaux de transactions sur la base de données primaire, exprimée en temps universel coordonné (UTC).|  
|**history_retention_period**|Durée de conservation (en minutes) avant suppression des enregistrements historiques de copie des journaux de transaction pour une base de données primaire donnée.|  
  
## <a name="remarks"></a>Notes  
 **sp_help_log_shipping_primary_database** doit être exécuté à partir de la **master** base de données sur le serveur principal.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe peut exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de **sp_help_log_shipping_primary_database** pour récupérer les paramètres de base de données primaire pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
EXEC master.dbo.sp_help_log_shipping_primary_database @database=N'AdventureWorks2012';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de journaux de transaction & #40 ; SQL Server & #41 ;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
