---
title: sp_add_log_shipping_secondary_database (Transact-SQL) | Documents Microsoft
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
- sp_add_log_shipping_secondary_database
- sp_add_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_secondary_database
ms.assetid: d29e1c24-3a3c-47a4-a726-4584afa6038a
caps.latest.revision: 22
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 70abdc5cb67beeb779af3722d3b5e7f91b923e21
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddlogshippingsecondarydatabase-transact-sql"></a>sp_add_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Définit une base de données secondaire pour la copie des journaux de transaction.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database',  
[ @primary_server = ] 'primary_server',   
[ @primary_database = ] 'primary_database',  
[, [ @restore_delay = ] 'restore_delay']  
[, [ @restore_all = ] 'restore_all']  
[, [ @restore_mode = ] 'restore_mode']  
[, [ @disconnect_users = ] 'disconnect_users']  
[, [ @block_size = ] 'block_size']  
[, [ @buffer_count = ] 'buffer_count']  
[, [ @max_transfer_size = ] 'max_transfer_size']  
[, [ @restore_threshold = ] 'restore_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@secondary_database** =] '*secondary_database*'  
 Nom de la base de données secondaire. *secondary_database* est **sysname**, sans valeur par défaut.  
  
 [ **@primary_server** =] '*primary_server*'  
 Le nom de l’instance principale de la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration d’envoi de journaux. *primary_server* est **sysname** et ne peut pas être NULL.  
  
 [ **@primary_database** =] '*primary_database*'  
 Nom de la base de données sur le serveur principal. *primary_database* est **sysname**, sans valeur par défaut.  
  
 [ **@restore_delay** =] '*restore_delay*'  
 Durée, en minutes, de l'attente du serveur secondaire avant de restaurer un fichier de sauvegarde donné. *restore_delay* est **int** et ne peut pas être NULL. La valeur par défaut est 0 :  
  
 [ **@restore_all** =] '*restore_all*'  
 Si la valeur est définie à 1, le serveur secondaire restaure toutes les sauvegardes du journal des transactions disponibles au moment de la restauration. Dans le cas contraire, le serveur s'arrête une fois qu'un fichier est restauré. *restore_all* est **bits** et ne peut pas être NULL.  
  
 [ **@restore_mode** =] '*restore_mode*'  
 Mode de restauration pour la base de données secondaire.  
  
 0 = Restauration du journal avec l'option NORECOVERY.  
  
 1 = Restauration du journal avec l'option STANDBY.  
  
 *restaurer* est **bits** et ne peut pas être NULL.  
  
 [ **@disconnect_users** =] '*disconnect_users*'  
 Si la valeur est définie à 1, les utilisateurs sont déconnectés de la base de données secondaire au moment de la restauration. Par défaut = 0. *Déconnecter* utilisateurs est **bits** et ne peut pas être NULL.  
  
 [ **@block_size** =] '*block_size*'  
 Taille, en octets, qui définit la taille des blocs pour l'unité de sauvegarde. *block_size* est **int** avec la valeur par défaut -1.  
  
 [ **@buffer_count** =] '*buffer_count*'  
 Nombre total de mémoires tampons utilisées par l'opération de sauvegarde ou de restauration. *buffer_count* est **int** avec la valeur par défaut -1.  
  
 [ **@max_transfer_size** =] '*max_transfer_size*'  
 Taille, en octets, de la demande d'entrée ou de sortie maximale émise par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'unité de sauvegarde. *max_transfersize* est **int** et peut être NULL.  
  
 [ **@restore_threshold** =] '*restore_threshold*'  
 Nombre de minutes pouvant s'écouler entre les opérations de restauration avant qu'une alerte ne soit générée. *restore_threshold* est **int** et ne peut pas être NULL.  
  
 [ **@threshold_alert** =] '*threshold_alert ne*'  
 Est de l’alerte à déclencher lorsque le seuil de sauvegarde est dépassé. *l’argument threshold_alert* est **int**, avec 14 420 comme valeur par défaut.  
  
 [ **@threshold_alert_enabled** =] '*threshold_alert_enabled*'  
 Indique si une alerte est générée lorsque *backup_threshold* est dépassé. La valeur 1 par défaut indique que l'alerte sera générée. *threshold_alert_enabled* est **bits**.  
  
 [ **@history_retention_period** =] '*history_retention_period*'  
 Est la durée en minutes pendant laquelle l’historique est conservé. *history_retention_period* est **int**, avec NULL comme valeur par défaut. La valeur 14 420 est utilisée si aucune autre valeur n'est spécifiée.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_add_log_shipping_secondary_database** doit être exécuté à partir de la **master** base de données sur le serveur secondaire. Elle effectue les actions suivantes :  
  
1.  **sp_add_log_shipping_secondary_primary** doit être appelée avant cette procédure stockée pour initialiser l’informations de base de données sur le serveur secondaire de copie des journaux de principal.  
  
2.  Ajoute une entrée pour la base de données secondaire **log_shipping_secondary_databases** à l’aide des arguments fournis.  
  
3.  Ajoute un enregistrement moniteur local dans **log_shipping_monitor_secondary** sur le serveur secondaire à l’aide des arguments fournis.  
  
4.  Si le serveur moniteur est différent du serveur secondaire, ajoute un enregistrement moniteur dans **log_shipping_monitor_secondary** sur le moniteur de serveur à l’aide des arguments fournis.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe peut exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de la **sp_add_log_shipping_secondary_database** procédure stockée pour ajouter la base de données **LogShipAdventureWorks** en tant que base de données secondaire dans une configuration d’envoi de journaux avec la base de données primaire [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] résidant sur le serveur principal TRIBECA.  
  
```  
EXEC master.dbo.sp_add_log_shipping_secondary_database   
@secondary_database = N'LogShipAdventureWorks'   
,@primary_server = N'TRIBECA'   
,@primary_database = N'AdventureWorks2012'   
,@restore_delay = 0   
,@restore_mode = 1   
,@disconnect_users = 0   
,@restore_threshold = 45     
,@threshold_alert_enabled = 0   
,@history_retention_period = 1440 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de journaux de transaction & #40 ; SQL Server & #41 ;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
