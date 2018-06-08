---
title: sp_dbmmonitorresults (Transact-SQL) | Documents Microsoft
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
- sp_dbmmonitorresults
- sp_dbmmonitorresults_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorresults
- database mirroring [SQL Server], monitoring
ms.assetid: d575e624-7d30-4eae-b94f-5a7b9fa5427e
caps.latest.revision: 46
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 16061dc41994cd032a9e6124d38abf3acb2e6be5
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdbmmonitorresults-transact-sql"></a>sp_dbmmonitorresults (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne les lignes d'état d'une base de données surveillée, à partir de la table d'état dans laquelle est stocké l'historique de la surveillance de la mise en miroir de bases de données, et vous permet de choisir si la procédure doit au préalable obtenir le dernier état.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dbmmonitorresults database_name   
   , rows_to_return  
    , update_status   
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Spécifie la base de données dont l'état de mise en miroir doit être retourné.  
  
 *rows_to_return*  
 Spécifie la quantité de lignes retournées :  
  
 0 = Dernière ligne  
  
 1 = Lignes des deux dernières heures  
  
 2 = Lignes des quatre dernières heures  
  
 3 = Lignes des huit dernières heures  
  
 4 = Lignes du dernier jour  
  
 5 = Lignes des deux derniers jours  
  
 6 = 100 dernières lignes  
  
 7 = 500 dernières lignes  
  
 8 = 1 000 dernières lignes  
  
 9 = 1 000 000 dernières lignes  
  
 *update_status*  
 Spécifie qu'avant de retourner les résultats, la procédure :  
  
 0 = ne met pas à jour l'état de la base de données. Les résultats sont calculés à l'aide des deux dernières lignes uniquement, dont l'âge dépend du moment auquel la table d'état a été actualisée ;  
  
 1 = met à jour l’état de la base de données en appelant **sp_dbmmonitorupdate** avant de calculer les résultats. Toutefois, si la table d’état a été mis à jour dans les 15 secondes précédentes, ou l’utilisateur n’est pas un membre de la **sysadmin** rôle de serveur fixe **sp_dbmmonitorresults** s’exécute sans mettre à jour l’état.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne le nombre demandé de lignes de l'état d'historique pour la base de données spécifiée. Chaque ligne contient les informations suivantes :  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Nom d'une base de données mise en miroir.|  
|**Rôle**|**int**|Rôle de mise en miroir actuel de l'instance du serveur :<br /><br /> 1 = Principal<br /><br /> 2 = Miroir|  
|**mirroring_state**|**int**|État de la base de données :<br /><br /> 0 = suspendu<br /><br /> 1 = déconnecté<br /><br /> 2 = Synchronisation<br /><br /> 3 = Basculement en attente<br /><br /> 4 = Synchronisé|  
|**witness_status**|**int**|L'état de connexion du témoin dans la session de mise en miroir de la base de données peut être :<br /><br /> 0 = Inconnu<br /><br /> 1 = Connected<br /><br /> 2 = Déconnecté|  
|**log_generation_rate**|**int**|Quantité de journal générée, en kilo-octets/s, depuis la précédente mise à jour de l'état de mise en miroir de cette base de données.|  
|**unsent_log**|**int**|Taille, en kilo-octets, du journal non envoyé dans la file d'attente d'envoi sur le principal.|  
|**send_rate**|**int**|Débit d'envoi du journal, en kilo-octets/s, depuis le principal vers le serveur miroir.|  
|**unrestored_log**|**int**|Taille, en kilo-octets, de la file d'attente de restauration par progression sur le serveur miroir.|  
|**recovery_rate**|**int**|Débit de la restauration par progression sur le serveur miroir, en kilo-octets/s.|  
|**transaction_delay**|**int**|Délai total, en millisecondes, de toutes les transactions.|  
|**transactions_per_sec**|**int**|Nombre de transactions par seconde sur l'instance du serveur principal.|  
|**average_delay**|**int**|Délai moyen de chaque transaction sur l'instance du serveur principal grâce à la mise en miroir de bases de données. En mode hautes performances (c'est-à-dire, lorsque la propriété SAFETY a pour valeur OFF), cette valeur est généralement 0.|  
|**time_recorded**|**datetime**|Heure à laquelle la ligne a été enregistrée lors de la surveillance de la mise en miroir de bases de données. Il s'agit de l'heure système du principal.|  
|**time_behind**|**datetime**|Heure système approximative du principal sur laquelle la base de données miroir est actuellement synchronisée. Cette valeur n'est significative que sur l'instance du serveur principal.|  
|**local_time**|**datetime**|Heure système sur l'instance du serveur local à laquelle cette ligne a été mise à jour.|  
  
## <a name="remarks"></a>Notes  
 **sp_dbmmonitorresults** peuvent être exécutées uniquement dans le contexte de la **msdb** base de données.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance au **sysadmin** rôle serveur fixe ou dans le **dbm_monitor** rôle de base de données fixe dans le **msdb** base de données. Le **dbm_monitor** rôle permet à ses membres afficher l’état de la mise en miroir de base de données mais pas le mettre à jour, mais pas afficher ou configurer les événements de mise en miroir de base de données.  
  
> [!NOTE]  
>  La première fois que **sp_dbmmonitorupdate** s’exécute, il crée le **dbm_monitor** rôle de base de données fixe dans le **msdb** base de données. Membres de la **sysadmin** rôle serveur fixe peut ajouter un utilisateur à la **dbm_monitor** rôle de base de données fixe.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne les lignes enregistrées au cours des deux heures précédentes sans mettre à jour l'état de la base de données.  
  
```  
USE msdb;  
EXEC sp_dbmmonitorresults AdventureWorks2012, 2, 0;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitoraddmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitoraddmonitoring-transact-sql.md)   
 [sp_dbmmonitordropmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorupdate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorupdate-transact-sql.md)  
  
  
