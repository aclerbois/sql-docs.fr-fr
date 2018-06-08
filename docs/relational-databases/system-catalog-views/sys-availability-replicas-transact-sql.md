---
title: Sys.availability_replicas (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- availability_replicas_TSQL
- availability_replicas
- sys.availability_replicas
- sys.availability_replicas_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_replicas catalog view
ms.assetid: 0a06e9b6-a1e4-4293-867b-5c3f5a8ff62c
caps.latest.revision: 62
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 215db72fe7432ab1d35a1bb4ca0ae63f28768f7c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysavailabilityreplicas-transact-sql"></a>sys.availability_replicas (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

Retourne une ligne pour chacun des réplicas de disponibilité qui appartiennent à aucun groupe de disponibilité Always On dans le cluster de basculement WSFC.  
  
Si l'instance de serveur locale ne peut pas communiquer avec le cluster de basculement WSFC, par exemple parce que le cluster est fermé ou le quorum a été perdu, seules les lignes des réplicas de disponibilité locaux sont retournées. Ces lignes contiendront uniquement les colonnes de données qui sont mises en cache localement dans les métadonnées.  
  
 
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|ID unique du réplica.|  
|**group_id**|**uniqueidentifier**|ID unique du groupe de disponibilité auquel le réplica appartient.|  
|**replica_metadata_id**|**int**|ID de l'objet des métadonnées locales pour les réplicas de disponibilité du moteur de base de données.|  
|**replica_server_name**|**nvarchar (256)**|Nom de serveur de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge ce réplica et, pour une instance non définie par défaut, son nom d'instance.|  
|**owner_sid**|**varbinary(85)**|Identificateur de sécurité (SID) enregistré sur cette instance de serveur pour le propriétaire externe de ce réplica de disponibilité.<br /><br /> NULL pour les réplicas de disponibilité non locaux.|  
|**endpoint_url**|**nvarchar(128)**|Représentation de chaîne du point de terminaison de mise en miroir de bases de données spécifié par l'utilisateur, qui est utilisé par les connexions entre les réplicas principal et secondaire pour la synchronisation des données. Pour plus d’informations sur la syntaxe des URL de point de terminaison, consultez [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).<br /><br /> NULL = Impossible de communiquer avec le cluster de basculement WSFC.<br /><br /> Pour modifier ce point de terminaison, utilisez l’option ENDPOINT_URL de [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction.|  
|**availability_mode**|**tinyint**|Mode de disponibilité du réplica, parmi :<br /><br /> 0 &#124; validation asynchrone. Le réplica principal peut valider des transactions sans attendre que le réplica secondaire écrive le journal sur le disque.<br /><br /> 1 &#124; validation synchrone. Le réplica principal attend pour valider une transaction donnée que le réplica secondaire ait écrit la transaction sur le disque.<br /><br />4 &#124; configuration uniquement. Le réplica principal envoie des métadonnées de configuration de groupe de disponibilité pour le réplica de façon synchrone. Données de l’utilisateur ne sont pas transmises au réplica. Disponible dans SQL Server 2017 CU1 et versions ultérieures.<br /><br /> Pour plus d’informations, consultez [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).|  
|**availability_mode_desc**|**nvarchar(60)**|Description de **disponibilité\_mode**, un des :<br /><br /> ASYNCHRONE\_VALIDER<br /><br /> SYNCHRONE\_VALIDER<br /><br /> CONFIGURATION\_UNIQUEMENT<br /><br /> Pour cela modifier le mode de disponibilité d’un réplica de disponibilité, utilisez l’option AVAILABILITY_MODE de [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction.<br/><br>Vous ne pouvez pas modifier le mode de disponibilité d’un réplica à CONFIGURATION\_uniquement. Vous ne pouvez pas modifier une CONFIGURATION\_réplica des données uniquement vers un réplica secondaire ou principal. |  
|**basculement\_mode**|**tinyint**|Le [le mode de basculement](../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) du réplica de disponibilité, parmi :<br /><br /> 0 &#124; basculement manuel. Un basculement vers un réplica secondaire qui est manuel doit être initialisé manuellement par l'administrateur de la base de données. Le type de basculement exécuté dépendra de la synchronisation du réplica secondaire, à savoir :<br /><br /> Si le réplica de disponibilité n'est pas synchronisé ou si sa synchronisation est en cours, seul le basculement forcé (avec perte de données) est possible.<br /><br /> Si le mode de disponibilité est défini sur la validation synchrone (**disponibilité\_mode** = 1) et le réplica de disponibilité est actuellement synchronisé, le basculement manuel sans perte de données peut se produire.<br /><br /> 1 &#124; le basculement automatique. Le réplica est une cible potentielle des basculements automatiques.  Le basculement automatique est pris en charge uniquement si le mode de disponibilité est défini sur la validation synchrone (**disponibilité\_mode** = 1) et le réplica de disponibilité est actuellement synchronisé.<br /><br /> Pour afficher une synthèse de l’état de synchronisation de base de données de chaque base de données de disponibilité d’un réplica de disponibilité, utilisez le **synchronisation\_intégrité** et **synchronisation\_intégrité\_desc** les colonnes de la [sys.dm_hadr_availability_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md) vue de gestion dynamique. La récupération prend en compte l'état de synchronisation de chaque base de données de disponibilité et le mode de disponibilité de son réplica de disponibilité.<br /><br /> **Remarque :** pour afficher l’état de synchronisation d’une base de données de disponibilité donné, interrogez la **synchronisation\_état** et **synchronisation\_l’intégrité** colonnes de la [sys.dm_hadr_database_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) vue de gestion dynamique.|  
|**failover\_mode\_desc**|**nvarchar(60)**|Description de **basculement\_mode**, un des :<br /><br /> MANUAL<br /><br /> AUTOMATIC<br /><br /> Pour modifier le mode de basculement, utilisez le basculement\_option MODE de [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction.|  
|**session\_timeout**|**int**|Délai d'attente en secondes. Le délai d'attente est le temps maximum pendant lequel le réplica attend de recevoir un message d'un autre réplica avant qu'on ne considère que la connexion entre le réplica principal et secondaire a échoué. Le délai d'expiration de session détecte si les réplicas secondaries sont connectés au réplica primaire.<br /><br /> En cas de détection d’échec de la connexion avec un réplica secondaire, le réplica principal considère que le réplica secondaire ne sont ne pas\_SYNCHRONIZED. En cas de détection d'un échec de connexion avec le réplica principal, un réplica secondaire tente simplement de se reconnecter.<br /><br /> **Remarque :** des délais d’expiration de Session ne provoquent pas de basculements automatiques.<br /><br /> Pour modifier cette valeur, utilisez l’option SESSION_TIMEOUT de [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction.|  
|**principal\_rôle\_autoriser\_connexions**|**tinyint**|Indique si la disponibilité permet toutes les connexions ou uniquement les connexions en lecture-écriture, parmi :<br /><br /> 2 = Toutes (valeur par défaut)<br /><br /> 3 = Lecture-écriture|  
|**principal\_rôle\_autoriser\_connexions\_desc**|**nvarchar(60)**|Description de **principal\_rôle\_autoriser\_connexions**, un des :<br /><br /> ALL<br /><br /> LIRE\_ÉCRIRE|  
|**secondaire\_rôle\_autoriser\_connexions**|**tinyint**|Si un réplica de disponibilité qui revêt le rôle secondaire (autrement dit, un réplica secondaire) peut accepter les connexions des clients, les options suivantes sont disponibles :<br /><br /> 0 = Non. Aucune connexion n'est autorisée aux bases de données dans le réplica secondaire et les bases de données ne sont pas disponibles pour un accès en lecture. Il s'agit du paramètre par défaut.<br /><br /> 1 = Lecture seule. Seules les connexions aux bases de données en lecture seule sont autorisées dans le réplica secondaire. Toutes les bases de données du réplica sont disponibles pour l'accès en lecture.<br /><br /> 2 = Toutes. Toutes les connexions sont autorisées aux bases de données dans le réplica secondaire pour un accès en lecture seule.<br /><br /> Pour plus d’informations, consultez [Secondaires actifs : réplicas secondaires lisibles &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).|  
|**secondary_role_allow_connections_desc**|**nvarchar(60)**|Description de **secondary_role_allow_connections**, un des :<br /><br /> Non<br /><br /> READ_ONLY<br /><br /> ALL|  
|**create_date**|**datetime**|Date de création du réplica.<br /><br /> NULL = Le réplica ne se trouve pas sur cette instance de serveur.|  
|**modify_date**|**datetime**|Date de la dernière modification du réplica.<br /><br /> NULL = Le réplica ne se trouve pas sur cette instance de serveur.|  
|**backup_priority**|**int**|Représente la priorité spécifiée par l'utilisateur pour l'exécution des sauvegardes sur ce réplica par rapport aux autres réplicas dans le même groupe de disponibilité. La valeur est un entier compris entre 0 et 100.<br /><br /> Pour plus d’informations, consultez [Secondaires actifs : sauvegarde sur les réplicas secondaires &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**read_only_routing_url**|**nvarchar (256)**|Point de terminaison de connectivité (URL) du réplica de disponibilité en lecture seule. Pour plus d’informations, consultez [Configurer le routage en lecture seule pour un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW ANY DEFINITION sur l'instance de serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Surveiller les groupes de disponibilité & #40 ; Transact-SQL & #41 ;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Surveiller des groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
