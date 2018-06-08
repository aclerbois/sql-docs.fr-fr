---
title: sp_dbmmonitorupdate (Transact-SQL) | Documents Microsoft
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
- sp_dbmmonitorupdate
- sp_dbmmonitorupdate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorupdate
- database mirroring [SQL Server], monitoring
ms.assetid: 9ceb9611-4929-44ee-a406-c39ba2720fd5
caps.latest.revision: 21
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 0882644bb3cef95694cee44e308b21fc627cd489
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdbmmonitorupdate-transact-sql"></a>sp_dbmmonitorupdate (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Met à jour la table d'état de la surveillance de la mise en miroir de bases de données en insérant une nouvelle ligne de table pour chaque base de données en miroir et tronque les lignes antérieures à la période de rétention actuelle. La période de rétention par défaut est de 7 jours (168 heures). Lors de la mise à jour de la table, **sp_dbmmonitorupdate** évalue les mesures de performances.  
  
> [!NOTE]  
>  La première fois **sp_dbmmonitorupdate** s’exécute, il crée la table d’état de mise en miroir de base de données et la **dbm_monitor** rôle de base de données fixe dans le **msdb** base de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dbmmonitorupdate [ database_name ]  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Nom de la base de données dont vous souhaitez mettre à jour l'état de mise en miroir. Si *nom_base_de_données* n’est pas spécifié, la procédure met à jour la table d’état pour chaque base de données mise en miroir sur l’instance de serveur.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_dbmmonitorupdate** peuvent être exécutées uniquement dans le contexte de la **msdb** base de données.  
  
 Si aucune colonne de la table d'état ne s'applique au rôle d'un partenaire, la valeur est NULL sur ce partenaire. En outre, une colonne comporte la valeur NULL si les informations appropriées ne sont pas disponibles, comme lors d'un basculement ou d'un redémarrage de serveur.  
  
 Après avoir **sp_dbmmonitorupdate** crée le **dbm_monitor** rôle de base de données fixe dans le **msdb** de base de données, les membres du **sysadmin** rôle serveur fixe peut ajouter un utilisateur à la **dbm_monitor** rôle de base de données fixe. Le **dbm_monitor** rôle permet à ses membres afficher l’état de la mise en miroir de base de données mais pas le mettre à jour, mais pas afficher ou configurer les événements de mise en miroir de base de données.  
  
 Lors de la mise à jour de l’état de mise en miroir de base de données, **sp_dbmmonitorupdate** inspecte la valeur la plus récente de toutes les mesures de performance de mise en miroir pour lequel un seuil d’avertissement a été spécifié. Si la valeur dépasse le seuil, la procédure ajoute un événement d'informations au journal des événements. Tous les taux sont des moyennes établies depuis la dernière mise à jour. Pour plus d’informations, consultez [Utiliser des seuils d’avertissement et d’alertes sur des métriques de performances de mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant met à jour l'état de mise en miroir uniquement pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE msdb;  
EXEC sp_dbmmonitorupdate AdventureWorks2012 ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md)   
 [sp_dbmmonitorchangemonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)   
 [sp_dbmmonitorhelpalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
  
