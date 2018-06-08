---
title: sp_add_log_shipping_secondary_primary (Transact-SQL) | Documents Microsoft
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
- sp_add_log_shipping_secondary_primary_TSQL
- sp_add_log_shipping_secondary_primary
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_secondary_primary
ms.assetid: bfbbbee2-c255-4a59-a963-47d6e980a8e2
caps.latest.revision: 19
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 0f4f5ef8c72155b83595f04c92cdeb2c1cf16a34
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddlogshippingsecondaryprimary-transact-sql"></a>sp_add_log_shipping_secondary_primary (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Initialise les informations liées au serveur principal, ajoute des liens de surveillance local et distant, et crée des travaux de copie et de restauration sur le serveur secondaire pour la base de données primaire spécifiée.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_log_shipping_secondary_primary  
 [ @primary_server = ] 'primary_server',   
[ @primary_database = ] 'primary_database',  
[ @backup_source_directory = ] 'backup_source_directory' ,   
[ @backup_destination_directory = ] 'backup_destination_directory'  
[ @copy_job_name = ] 'copy_job_name'  
[ @restore_job_name = ] 'restore_job_name'  
[, [ @file_retention_period = ] 'file_retention_period']  
[, [ @monitor_server = ] 'monitor_server']  
[, [ @monitor_server_security_mode = ] 'monitor_server_security_mode']  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @copy_job_id = ] 'copy_job_id' OUTPUT ]  
[, [ @restore_job_id = ] 'restore_job_id' OUTPUT ]  
[, [ @secondary_id = ] 'secondary_id' OUTPUT]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@primary_server** =] '*primary_server*'  
 Le nom de l’instance principale de la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration d’envoi de journaux. *primary_server* est **sysname** et ne peut pas être NULL.  
  
 [ **@primary_database** =] '*primary_database*'  
 Nom de la base de données sur le serveur principal. *primary_database* est **sysname**, sans valeur par défaut.  
  
 [ **@backup_source_directory** =] '*backup_source_directory*'  
 Répertoire où les fichiers de sauvegarde des journaux de transactions du serveur principal sont stockés. *backup_source_directory* est **nvarchar (500)** et ne peut pas être NULL.  
  
 [ **@backup_destination_directory** =] '*backup_destination_directory*'  
 Répertoire sur le serveur secondaire où sont copiés les fichiers de sauvegarde. *backup_destination_directory* est **nvarchar (500)** et ne peut pas être NULL.  
  
 [ **@copy_job_name** =] '*copy_job_name*'  
 Nom à utiliser pour le travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] créé pour copier les sauvegardes du journal de transactions vers le serveur secondaire. *copy_job_name* est **sysname** et ne peut pas être NULL.  
  
 [ **@restore_job_name** =] '*restore_job_name*'  
 Nom de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] travail de l’Agent sur le serveur secondaire qui restaure les sauvegardes de base de données secondaire. *restore_job_name* est **sysname** et ne peut pas être NULL.  
  
 [ **@file_retention_period** =] '*file_retention_period*'  
 La longueur de la durée, en minutes, pendant laquelle un fichier de sauvegarde est conservé sur le serveur secondaire dans le chemin d’accès spécifié par le @backup_destination_directory paramètre avant d’être supprimé. *history_retention_period* est **int**, avec NULL comme valeur par défaut. Une valeur de 14420 sera utilisée en l'absence de toute autre spécification.  
  
 [ **@monitor_server** =] '*monitor_server*'  
 Nom du serveur moniteur. *Monitor_server* est **sysname**, sans valeur par défaut, et ne peut pas être NULL.  
  
 [ **@monitor_server_security_mode** =] '*monitor_server_security_mode*'  
 Mode de sécurité utilisé pour la connexion au serveur moniteur.  
  
 1 = Authentification Windows.  
  
 0 = Authentication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *monitor_server_security_mode* est **bits** et ne peut pas être NULL.  
  
 [ **@monitor_server_login** =] '*monitor_server_login*'  
 Nom d'utilisateur du compte utilisé pour accéder au serveur moniteur.  
  
 [ **@monitor_server_password** =] '*monitor_server_password*'  
 Mot de passe du compte qui permet d'accéder au serveur moniteur.  
  
 [ **@copy_job_id** =] '*copy_job_id*' sortie  
 ID associé au travail de copie sur le serveur secondaire. *copy_job_id* est **uniqueidentifier** et ne peut pas être NULL.  
  
 [ **@restore_job_id** =] '*restore_job_id*' sortie  
 ID associé au travail de restauration sur le serveur secondaire. *restore_job_id* est **uniqueidentifier** et ne peut pas être NULL.  
  
 [ **@secondary_id** =] '*secondary_id*' sortie  
 ID du serveur secondaire dans la configuration d'envoi de journaux. *secondary_id* est **uniqueidentifier** et ne peut pas être NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_add_log_shipping_secondary_primary** doit être exécuté à partir de la **master** base de données sur le serveur secondaire. Elle effectue les actions suivantes :  
  
1.  Génère un ID secondaire pour le serveur principal et la base de données primaire spécifiés.  
  
2.  Se charge :  
  
    1.  Ajoute une entrée pour l’ID secondaire dans **log_shipping_secondary** à l’aide des arguments fournis.  
  
    2.  de créer un travail de copie pour l'ID secondaire qui est désactivé ;  
  
    3.  Définit l’ID de tâche de copie dans le **log_shipping_secondary** entrée à l’ID de tâche de la copie.  
  
    4.  de créer un travail de restauration pour l'ID secondaire qui est désactivé ;  
  
    5.  Définir l’ID de tâche de restauration dans le **log_shipping_secondary** entrée à l’ID de tâche du travail de restauration.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe peut exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de la **sp_add_log_shipping_secondary_primary** procédure stockée pour définir les informations de la base de données primaire [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] sur le serveur secondaire.  
  
```  
EXEC master.dbo.sp_add_log_shipping_secondary_primary   
@primary_server = N'TRIBECA'   
,@primary_database = N'AdventureWorks'   
,@backup_source_directory = N'\\tribeca\LogShipping'   
,@backup_destination_directory = N''   
,@copy_job_name = N''   
,@restore_job_name = N''   
,@file_retention_period = 1440   
,@monitor_server = N'ROCKAWAY'   
,@monitor_server_security_mode = 1   
,@copy_job_id = @LS_Secondary__CopyJobId OUTPUT   
,@restore_job_id = @LS_Secondary__RestoreJobId OUTPUT   
,@secondary_id = @LS_Secondary__SecondaryId OUTPUT ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de journaux de transaction & #40 ; SQL Server & #41 ;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
