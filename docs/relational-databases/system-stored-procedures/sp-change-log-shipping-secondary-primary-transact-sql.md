---
title: sp_change_log_shipping_secondary_primary (Transact-SQL) | Documents Microsoft
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
- sp_change_log_shipping_secondary_primary
- sp_change_log_shipping_secondary_primary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_secondary_primary
ms.assetid: 5bcb4df7-6df3-4f2b-9207-b97b5addf2a6
caps.latest.revision: 17
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 499c95fc980bd6e8d996b14c47907b814cbb94aa
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spchangelogshippingsecondaryprimary-transact-sql"></a>sp_change_log_shipping_secondary_primary (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les paramètres de la base de données secondaire.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_change_log_shipping_secondary_primary  
[ @primary_server = ] 'primary_server',  
[ @primary_database = ] 'primary_database',  
[, [ @backup_source_directory = ] 'backup_source_directory']  
[, [ @backup_destination_directory = ] 'backup_destination_directory']  
[, [ @file_retention_period = ] file_retention_period]  
[, [ @monitor_server_security_mode = ] monitor_server_security_mode]  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
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
  
 [ **@file_retention_period** =] '*file_retention_period*'  
 Période de rétention, en minutes, de l'historique. *history_retention_period* est **int**, avec NULL comme valeur par défaut. Une valeur de 14420 sera utilisée en l'absence de toute autre spécification.  
  
 [ **@monitor_server_security_mode** =] '*monitor_server_security_mode*'  
 Mode de sécurité utilisé pour la connexion au serveur moniteur.  
  
 1 = Authentification Windows ;  
  
 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. *monitor_server_security_mode* est **bits** et ne peut pas être NULL.  
  
 [ **@monitor_server_login** =] '*monitor_server_login*'  
 Nom d'utilisateur du compte utilisé pour accéder au serveur moniteur.  
  
 [ **@monitor_server_password** =] '*monitor_server_password*'  
 Mot de passe du compte qui permet d'accéder au serveur moniteur.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_change_log_shipping_secondary_primary** doit être exécuté à partir de la **master** base de données sur le serveur secondaire. Elle effectue les actions suivantes :  
  
1.  Modifie les paramètres dans le **log_shipping_secondary** enregistre si nécessaire.  
  
2.  Si le serveur moniteur est différent du serveur secondaire, modifications enregistrement de moniteur dans **log_shipping_monitor_secondary** sur le moniteur de serveur à l’aide des arguments fournis, si nécessaire.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe peut exécuter cette procédure.  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de journaux de transaction & #40 ; SQL Server & #41 ;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
