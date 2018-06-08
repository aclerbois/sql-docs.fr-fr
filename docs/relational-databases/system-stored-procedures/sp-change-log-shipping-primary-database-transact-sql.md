---
title: sp_change_log_shipping_primary_database (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_change_log_shipping_primary_database
- sp_change_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_primary_database
ms.assetid: 8c9dce6b-d2a3-4ca7-a832-8f59a5adb214
caps.latest.revision: 27
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 0fd020ff499dfb230478434e70cee94edb45ba92
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spchangelogshippingprimarydatabase-transact-sql"></a>sp_change_log_shipping_primary_database (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les paramètres de la base de données primaire.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_change_log_shipping_primary_database [ @database = ] 'database'  
[, [ @backup_directory = ] 'backup_directory']   
[, [ @backup_share = ] 'backup_share']   
[, [ @backup_retention_period = ] 'backup_retention_period']  
[, [ @monitor_server_security_mode = ] 'monitor_server_security_mode']  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @backup_threshold = ] 'backup_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
[, [ @backup_compression = ] backup_compression_option ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@database =** ] '*base de données*'  
 Nom de la base de données sur le serveur principal. *primary_database* est **sysname**, sans valeur par défaut.  
  
 [  **@backup_directory =** ] '*Répertoire_Sauvegarde*'  
 Chemin d'accès au dossier de sauvegarde sur le serveur principal. *Répertoire_Sauvegarde* est **nvarchar (500)**, sans valeur par défaut, et ne peut pas être NULL.  
  
 [  **@backup_share =** ] '*backup_share*'  
 Chemin d'accès réseau au répertoire de sauvegarde sur le serveur principal. *backup_share* est **nvarchar (500)**, sans valeur par défaut, et ne peut pas être NULL.  
  
 [  **@backup_retention_period =** ] '*backup_retention_period*'  
 Durée, en minutes, de conservation du fichier de sauvegarde de fichier journal dans le répertoire de sauvegarde sur le serveur principal. *backup_retention_period* est **int**, sans valeur par défaut, et ne peut pas être NULL.  
  
 [  **@monitor_server_security_mode =** ] '*monitor_server_security_mode*'  
 Mode de sécurité utilisé pour la connexion au serveur moniteur.  
  
 1 = Authentification Windows.  
  
 0 = Authentification SQL Server.  
  
 *monitor_server_security_mode* est **bits** et ne peut pas être NULL.  
  
 [  **@monitor_server_login =** ] '*monitor_server_login*'  
 Nom d'utilisateur du compte utilisé pour accéder au serveur moniteur.  
  
 [  **@monitor_server_password =** ] '*monitor_server_password*'  
 Mot de passe du compte qui permet d'accéder au serveur moniteur.  
  
 [  **@backup_threshold =** ] '*backup_threshold*'  
 Est la longueur de la durée, en minutes, après la dernière sauvegarde avant qu’un *threshold_alert ne* erreur est générée. *backup_threshold* est **int**, avec une valeur par défaut de 60 minutes.  
  
 [  **@threshold_alert =** ] '*threshold_alert ne*'  
 Alerte à déclencher lorsque le seuil de sauvegarde est dépassé. *l’argument threshold_alert* est **int** et ne peut pas être NULL.  
  
 [  **@threshold_alert_enabled =** ] '*threshold_alert_enabled*'  
 Indique si une alerte est générée lorsque *backup_threshold* est dépassé.  
  
 1 = Activé.  
  
 0 = Désactivé.  
  
 *threshold_alert_enabled* est **bits** et ne peut pas être NULL.  
  
 [  **@history_retention_period =** ] '*history_retention_period*'  
 Est la durée en minutes pendant laquelle l’historique est conservé. *history_retention_period* est **int**. La valeur 14 420 est utilisée si aucune autre valeur n'est spécifiée.  
  
 [ **@backup_compression**=] *backup_compression_option*  
 Spécifie si une configuration de copie des journaux utilise [compression de la sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md). Ce paramètre est pris en charge uniquement dans le [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (ou une version ultérieure).  
  
 0 = Désactivées. Ne jamais compresser des sauvegardes de journal.  
  
 1 = Activé. Toujours compresser des sauvegardes de journal.  
  
 2 = utiliser le paramètre de la [afficher ou configurer l’Option de Configuration de serveur par défaut de compression de la sauvegarde](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Ceci est la valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_change_log_shipping_primary_database** doit être exécuté à partir de la **master** base de données sur le serveur principal. Elle effectue les actions suivantes :  
  
1.  Modifie les paramètres de la **log_shipping_primary_database** enregistrer, si nécessaire.  
  
2.  Modifie l’enregistrement local dans **log_shipping_monitor_primary** sur le serveur principal à l’aide des arguments fournis, si nécessaire.  
  
3.  Si le serveur moniteur est différent du serveur principal, modification de l’enregistrement dans **log_shipping_monitor_primary** sur le moniteur de serveur à l’aide des arguments fournis, si nécessaire.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe peut exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de **sp_change_log_shipping_primary_database** pour mettre à jour les paramètres associés à la base de données principal [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
EXEC master.dbo.sp_change_log_shipping_primary_database   
 @database = N'AdventureWorks'   
, @backup_directory = N'c:\LogShipping'   
, @backup_share = N'\\tribeca\LogShipping'   
, @backup_retention_period = 1440   
, @backup_threshold = 60   
, @threshold_alert = 0   
, @threshold_alert_enabled = 1   
, @history_retention_period = 1440   
,@monitor_server_security_mode = 1  
,@backup_compression = 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de journaux de transaction & #40 ; SQL Server & #41 ;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [log_shipping_primary_databases &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)  
  
  
