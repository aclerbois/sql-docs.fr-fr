---
title: sp_help_log_shipping_secondary_database (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_help_log_shipping_secondary_database
- sp_help_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_secondary_database
ms.assetid: 11ce42ca-d3f1-44c8-9cac-214ca8896b9a
caps.latest.revision: 28
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 0e2a943234d835d1f78cf57c096fd8492849bfa0
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelplogshippingsecondarydatabase-transact-sql"></a>sp_help_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Cette procédure stockée extrait les paramètres d'une ou plusieurs bases de données secondaires.  
  

  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database' OR  
[ @secondary_id = ] 'secondary_id'  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@secondary_database =** ] '*secondary_database*'  
 Nom de la base de données secondaire. *secondary_database* est **sysname**, sans valeur par défaut.  
  
 [  **@secondary_id =** ] '*secondary_id*'  
 ID du serveur secondaire dans la configuration d'envoi de journaux. *secondary_id* est **uniqueidentifier** et ne peut pas être NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne| Description|  
|-----------------|-----------------|  
|**secondary_id**|ID du serveur secondaire dans la configuration d'envoi de journaux.|  
|**primary_server**|Le nom de l’instance principale de la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans la configuration d’envoi de journaux.|  
|**primary_database**|Nom de la base de données primaire dans la configuration d'envoi de journaux.|  
|**backup_source_directory**|Répertoire où les fichiers de sauvegarde des journaux de transactions du serveur principal sont stockés.|  
|**backup_destination_directory**|Répertoire sur le serveur secondaire où sont copiés les fichiers de sauvegarde.|  
|**file_retention_period**|Durée, en minutes, de conservation d'un fichier de sauvegarde sur le serveur secondaire avant sa suppression.|  
|**copy_job_id**|ID associé au travail de copie sur le serveur secondaire.|  
|**restore_job_id**|ID associé au travail de restauration sur le serveur secondaire.|  
|**monitor_server**|Le nom de l’instance de la [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilisé en tant que serveur moniteur dans la configuration d’envoi de journaux.|  
|**monitor_server_security_mode**|Mode de sécurité utilisé pour la connexion au serveur moniteur.<br /><br /> 1 = Authentification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.|  
|**secondary_database**|Nom de la base de données secondaire dans la configuration de la copie des journaux de transactions.|  
|**restore_delay**|Durée, en minutes, de l'attente du serveur secondaire avant de restaurer un fichier de sauvegarde donné. La valeur par défaut est 0 minute.|  
|**restore_all**|Si la valeur est définie à 1, le serveur secondaire restaure toutes les sauvegardes du journal des transactions disponibles au moment de la restauration. Dans le cas contraire, le serveur s'arrête une fois qu'un fichier a été restauré.|  
|**restore_mode**|Mode de restauration pour la base de données secondaire.<br /><br /> 0 = Restauration du journal avec l'option NORECOVERY.<br /><br /> 1 = Restauration du journal en mode STANDBY.|  
|**disconnect_users**|Si la valeur est définie à 1, les utilisateurs sont déconnectés de la base de données secondaire au moment de la restauration. Par défaut = 0.|  
|**block_size**|Taille, en octets, qui définit la taille des blocs pour l'unité de sauvegarde.|  
|**buffer_count**|Nombre total de mémoires tampons utilisées par l'opération de sauvegarde ou de restauration.|  
|**max_transfer_size**|Taille, en octets, de la demande d'entrée ou de sortie maximale émise par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'unité de sauvegarde.|  
|**restore_threshold**|Nombre de minutes pouvant s'écouler entre les opérations de restauration avant qu'une alerte ne soit générée.|  
|**threshold_alert**|L’alerte à déclencher lorsque le seuil de restauration est dépassé.|  
|**threshold_alert_enabled**|Détermine si les alertes de seuil de restauration sont activées.<br /><br /> 1 = Activé.<br /><br /> 0 = Désactivées.|  
|**last_copied_file**|Nom de fichier du dernier fichier de sauvegarde copié sur le serveur secondaire.|  
|**last_copied_date**|Heure et date de la dernière copie sur le serveur secondaire.|  
|**last_copied_date_utc**|Date et heure de la dernière opération de copie sur le serveur secondaire, au format UTC (Coordinated Universal Time).|  
|**last_restored_file**|Nom du dernier fichier de sauvegarde restauré dans la base de données secondaire.|  
|**last_restored_date**|Heure et date auxquelles a eu lieu la dernière opération de restauration de la base de données secondaire.|  
|**last_restored_date_utc**|Date et heure de la dernière opération de restauration sur la base de données secondaire, au format UTC (Coordinated Universal Time).|  
|**history_retention_period**|Durée de conservation (en minutes) des enregistrements historiques d'envoi des journaux pour une base de données secondaire donnée avant leur suppression.|  
|**last_restored_latency**|Durée écoulée (en minutes) entre la création de la sauvegarde du journal sur le serveur principal et sa restauration sur le serveur secondaire.<br /><br /> La valeur initiale est NULL.|  
  
## <a name="remarks"></a>Notes  
 Si vous incluez le *secondary_database* paramètre, le jeu de résultats contient des informations sur cette base de données secondaire ; si vous incluez le *secondary_id* paramètre, le jeu de résultats contient des informations sur toutes les bases de données secondaires associées à cet identificateur secondaire.  
  
 **sp_help_log_shipping_secondary_database** doit être exécuté à partir de la **master** base de données sur le serveur secondaire.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe peut exécuter cette procédure.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_help_log_shipping_secondary_primary &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)   
 [À propos de journaux de transaction & #40 ; SQL Server & #41 ;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
