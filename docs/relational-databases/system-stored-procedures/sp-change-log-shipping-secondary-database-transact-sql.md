---
title: sp_change_log_shipping_secondary_database (Transact-SQL) | Documents Microsoft
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
- sp_change_log_shipping_secondary_database
- sp_change_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_secondary_database
ms.assetid: 3ebcf2f1-980f-4543-a84b-fbaeea54eeac
caps.latest.revision: 23
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 5e4ee6324e92130f3f887fe3a36ecd5469cd9d99
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spchangelogshippingsecondarydatabase-transact-sql"></a>sp_change_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les paramètres de la base de données secondaire.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_change_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database',  
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
 [  **@restore_delay =** ] '*restore_delay*'  
 Durée, en minutes, de l'attente du serveur secondaire avant de restaurer un fichier de sauvegarde donné. *restore_delay* est **int** et ne peut pas être NULL. La valeur par défaut est 0 :  
  
 [  **@restore_all =** ] '*restore_all*'  
 Si la valeur est définie à 1, le serveur secondaire restaure toutes les sauvegardes du journal des transactions disponibles au moment de la restauration. Dans le cas contraire, le serveur s'arrête une fois qu'un fichier a été restauré. *restore_all* est **bits** et ne peut pas être NULL.  
  
 [  **@restore_mode =** ] '*restore_mode*'  
 Mode de restauration pour la base de données secondaire.  
  
 0 = restaurer le journal avec NORECOVERY.  
  
 1 = Restauration du journal avec l'option STANDBY.  
  
 *restaurer* est **bits** et ne peut pas être NULL.  
  
 [  **@disconnect_users =** ] '*disconnect_users*'  
 Avec la valeur 1, les utilisateurs sont déconnectés de la base de données secondaire lors de l'exécution d'une opération de restauration. Par défaut = 0. *disconnect_users* est **bits** et ne peut pas être NULL.  
  
 [  **@block_size =** ] '*block_size*'  
 Taille, en octets, qui définit la taille des blocs pour l'unité de sauvegarde. *block_size* est **int** avec la valeur par défaut -1.  
  
 [  **@buffer_count =** ] '*buffer_count*'  
 Nombre total de mémoires tampons utilisées par l'opération de sauvegarde ou de restauration. *buffer_count* est **int** avec la valeur par défaut -1.  
  
 [  **@max_transfer_size =** ] '*max_transfer_size*'  
 Taille, en octets, de la demande d'entrée ou de sortie maximale émise par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'unité de sauvegarde. *max_transfersize* est **int** et peut être NULL.  
  
 [  **@restore_threshold =** ] '*restore_threshold*'  
 Nombre de minutes pouvant s'écouler entre les opérations de restauration avant qu'une alerte ne soit générée. *restore_threshold* est **int** et ne peut pas être NULL.  
  
 [  **@threshold_alert =** ] '*threshold_alert ne*'  
 Alerte générée en cas de dépassement du seuil de restauration. *l’argument threshold_alert* est **int**, avec une valeur par défaut 14420.  
  
 [  **@threshold_alert_enabled =** ] '*threshold_alert_enabled*'  
 Spécifie si une alerte est déclenchée lorsque *restore_threshold*est dépassé. 1 = activées ; 0 = désactivées. *threshold_alert_enabled* est **bits** et ne peut pas être NULL.  
  
 [  **@history_retention_period =** ] '*history_retention_period*'  
 Période de rétention, en minutes, de l'historique. *history_retention_period* est **int**. Une valeur 1440 sera utilisée si aucun n’est spécifié.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_change_log_shipping_secondary_database** doit être exécuté à partir de la **master** base de données sur le serveur secondaire. Elle effectue les actions suivantes :  
  
1.  Modifie les paramètres de la **log_shipping_secondary_database** enregistre si nécessaire.  
  
2.  Modifie l’enregistrement du moniteur local dans **log_shipping_monitor_secondary** sur le serveur secondaire à l’aide des arguments fournis, si nécessaire.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe peut exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de **sp_change_log_shipping_secondary_database** pour mettre à jour les paramètres de base de données secondaire pour la base de données **LogShipAdventureWorks**.  
  
```  
EXEC master.dbo.sp_change_log_shipping_secondary_database   
 @secondary_database =  'LogShipAdventureWorks'  
,  @restore_delay = 0  
,  @restore_all = 1  
,  @restore_mode = 0  
,  @disconnect_users = 0  
,  @threshold_alert = 14420  
,  @threshold_alert_enabled = 1  
,  @history_retention_period = 14420;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de journaux de transaction & #40 ; SQL Server & #41 ;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
