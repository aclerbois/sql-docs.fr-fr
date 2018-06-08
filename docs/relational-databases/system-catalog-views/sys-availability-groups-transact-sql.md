---
title: Sys.availability_groups (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.availability_groups_TSQL
- availability_groups_TSQL
- sys.availability_groups
- availability_groups
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups catalog view
ms.assetid: da7fa55f-c008-45d9-bcfc-3513b02d9e71
caps.latest.revision: 42
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d92fb6ad5444abd447352d691887ce066c9fd7d8
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysavailabilitygroups-transact-sql"></a>sys.availability_groups (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque groupe de disponibilité pour lequel l’instance locale de SQL Server héberge un réplica de disponibilité. Chaque ligne contient une copie mise en cache des métadonnées du groupe de disponibilité.  
   
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificateur unique (GUID) du groupe de disponibilité.|  
|**nom**|**sysname**|Nom du groupe de disponibilité. Il s'agit d'un nom spécifié par l'utilisateur qui doit être unique dans le cluster de basculement Windows Server (WSFC).|  
|**resource_id**|**nvarchar(40)**|ID de ressource pour la ressource de cluster WSFC.|  
|**resource_group_id**|**nvarchar(40)**|ID du groupe de ressources pour le groupe de ressources du cluster WSFC du groupe de disponibilité.|  
|**failure_condition_level**|**int**|Défini par l’utilisateur niveau de condition échec sous lequel un basculement automatique doit être déclenché, une des valeurs entières indiqués dans le tableau situé immédiatement sous ce tableau.<br /><br /> Les niveaux de condition d'échec (1-5) s'étendent du moins restrictif, niveau 1, au plus restrictif, le niveau 5. Un niveau de condition donné comprend tous les niveaux moins restrictifs. Par conséquent, le niveau de condition le plus strict, le niveau 5, inclut les quatre niveaux de condition moins restrictifs (1 à 4), le niveau 4 inclut les niveaux 1 à 3, et ainsi de suite.<br /><br /> Pour modifier cette valeur, utilisez l’option FAILURE_CONDITION_LEVEL de le [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction.|  
|**health_check_timeout**|**int**|Temps d’attente (en millisecondes) pour le [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) procédures stockées système pour retourner des informations d’intégrité du serveur, avant que l’instance de serveur est censée pour être lente ou suspendue. La valeur par défaut est 30 000 millisecondes (ou 30 secondes).<br /><br /> Pour modifier cette valeur, utilisez l’option HEALTH_CHECK_TIMEOUT de le [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction.|  
|**automated_backup_preference**|**tinyint**|Emplacement par défaut des sauvegardes effectuées sur des bases de données de disponibilité dans ce groupe de disponibilité. Voici les valeurs possibles et leurs descriptions.<br /><br /> <br /><br /> 0 : principal. Les sauvegardes doivent toujours avoir lieu sur le réplica principal.<br /><br /> 1 : secondaire uniquement. Les sauvegardes sur un réplica secondaire sont préférables.<br /><br /> 2 : préférer secondaire. Les sauvegardes sur un réplica secondaire sont préférables, mais celles sur le réplica principal sont acceptables si aucun réplica secondaire n'est disponible pour les opérations de sauvegarde. Il s'agit du comportement par défaut.<br /><br /> 3 : n’importe quel réplica. Aucune préférence : les sauvegardes sont effectuées sur le réplica principal ou sur un réplica secondaire.<br /><br /> <br /><br /> Pour plus d’informations, consultez [Secondaires actifs : sauvegarde sur les réplicas secondaires &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**automated_backup_preference_desc**|**nvarchar(60)**|Description de **automated_backup_preference**, un des :<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> Aucune|  
|**version**|**smallint**|La version des métadonnées de groupe de disponibilité stockées dans le Cluster de basculement Windows. Ce numéro de version est incrémenté lorsque de nouvelles fonctionnalités sont ajoutées.|  
|**basic_features**|**bit**|Spécifie s’il s’agit d’un groupe de disponibilité de base. Pour plus d’informations, consultez [Groupes de disponibilité de base &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).|  
|**dtc_support**|**bit**|Spécifie si la prise en charge DTC a été activée pour ce groupe de disponibilité. Le **DTC_SUPPORT** option de **créer un groupe de disponibilité** contrôle ce paramètre.|  
|**db_failover**|**bit**|Spécifie si le groupe de disponibilité prend en charge le basculement pour les conditions de contrôle d’intégrité de base de données. Le **DB_FAILOVER** option de **créer un groupe de disponibilité** contrôle ce paramètre.|  
|**is_distributed**|**bit**|Spécifie s’il s’agit d’un groupe de disponibilité distribué. Pour plus d’informations, consultez [Groupes de disponibilité distribués &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/distributed-availability-groups-always-on-availability-groups.md).|  
  
## <a name="failure-condition-level--values"></a>Valeurs de niveau de condition de défaillance  
 Le tableau suivant décrit les niveaux de condition d’échec possibles pour le **failure_condition_level** colonne.  
  
|Valeur|Condition d’échec|  
|-----------|-----------------------|  
|1|Spécifie qu'un basculement automatique doit être initialisé lorsque l'une des conditions suivantes se produit :<br /><br /> <br /><br /> -La [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] service est arrêté.<br /><br /> -Le bail du groupe de disponibilité pour la connexion au cluster de basculement WSFC expire car aucun accusé de réception n’est reçu à partir de l’instance de serveur. Pour plus d’informations, consultez [Fonctionnement : délai d’expiration de bail Always On SQL Server](http://blogs.msdn.com/b/psssql/archive/2012/09/07/how-it-works-sql-server-Always%20On-lease-timeout.aspx).|  
|2|Spécifie qu'un basculement automatique doit être initialisé lorsque l'une des conditions suivantes se produit :<br /><br /> <br /><br /> -L’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne se connecte pas au cluster et spécifié par l’utilisateur **health_check_timeout** dépassement du seuil du groupe de disponibilité.<br /><br /> -Le réplica de disponibilité est en état d’échec.|  
|3|Spécifie qu'un basculement automatique doit être initialisé sur les erreurs internes critiques [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telles que les verrouillages spinlock orphelins, les violations graves d'accès en écriture, ou en cas de vidages trop importants.<br /><br /> Ceci est la valeur par défaut.|  
|4|Spécifie qu'un basculement automatique doit être initialisé sur les erreurs internes modérées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telles qu'une condition persistante de mémoire insuffisante dans le pool de ressources interne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|5|Spécifie qu'un basculement automatique doit être initialisé sur tous les états d'échec qualifiés, notamment :<br /><br /> <br /><br /> -Insuffisance des threads de travail du moteur SQL.<br /><br /> -Détection d’un blocage insoluble.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW ANY DEFINITION sur l'instance de serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Surveiller les groupes de disponibilité & #40 ; Transact-SQL & #41 ;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Surveiller des groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
