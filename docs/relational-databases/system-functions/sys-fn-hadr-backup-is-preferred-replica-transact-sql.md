---
title: Sys.fn_hadr_backup_is_preferred_replica (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.fn_hadr_backup_is_preferred_replica_TSQL
- sys.fn_hadr_backup_is_preferred_replica
- fn_hadr_backup_is_preferred_replica_TSQL
- fn_hadr_backup_is_preferred_replica
dev_langs:
- TSQL
helpviewer_keywords:
- backup on secondary replicas
- active secondary replicas [SQL Server], backup on secondary replicas
- sys.fn_hadr_backup_is_preferred_replica function
ms.assetid: 61b9be77-e2f6-4da1-b2ae-a62cbe226145
caps.latest.revision: 19
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d89528595dc2cec10bd86f9605b8ef55331c8ab5
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfnhadrbackupispreferredreplica--transact-sql"></a>Sys.fn_hadr_backup_is_preferred_replica (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Utilisé pour déterminer si le réplica actuel est le réplica de sauvegarde par défaut.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_hadr_backup_is_preferred_replica ( 'dbname' )  
```  
  
## <a name="arguments"></a>Arguments  
 '*dbname*'  
 Nom de la base de données actuellement sauvegardée. *dbname* est de type sysname.  
  
## <a name="returns"></a>Valeur renvoyée  
 Retourne 1 si la base de données sur l'instance actuelle est sur le réplica par défaut. Dans le cas contraire, retourne la valeur 0.  
  
## <a name="remarks"></a>Notes  
 Utilisez cette fonction dans un script de sauvegarde pour déterminer si la base de données active est sur le réplica préféré pour les sauvegardes. Vous pouvez exécuter un script sur chaque réplica de disponibilité. Chacun de ces travaux recherche les mêmes données pour déterminer quel est le travail à exécuter, une seule des tâches planifiées se poursuit en fait à l’étape de sauvegarde. L'exemple de code devrait ressembler à ce qui suit :  
  
```  
If sys.fn_hadr_backup_is_preferred_replica( @dbname ) <> 1   
BEGIN  
-- If this is not the preferred replica, exit (probably without error).  
END  
-- If this is the preferred replica, continue to do the backup.  
  
```  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-sysfnhadrbackupispreferredreplica"></a>A. Utilisation de sys.fn_hadr_backup_is_preferred_replica  
 L'exemple suivant retourne 1 si la base de données active est le réplica de sauvegarde par défaut.  
  
```  
SELECT sys.fn_hadr_backup_is_preferred_replica ('TestDB');  
GO  
```  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Configurer la sauvegarde sur les réplicas de disponibilité & #40 ; SQL Server & #41 ;](../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de groupes de disponibilité AlwaysOn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/always-on-availability-groups-functions-transact-sql.md)   
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [Secondaires actifs : Sauvegarde sur les réplicas secondaires &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)[affichages catalogue de groupes de disponibilité Always On &#40;Transact-SQL    &#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)  
  
  
