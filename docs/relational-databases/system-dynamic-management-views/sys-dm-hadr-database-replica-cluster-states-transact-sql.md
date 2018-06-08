---
title: Sys.dm_hadr_database_replica_cluster_states (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_database_replica_cluster_states
- dm_hadr_database_replica_cluster_states_TSQL
- sys.dm_hadr_database_replica_cluster_states_TSQL
- dm_hadr_database_replica_cluster_states
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_database_replica_cluster_states dynamic management view
ms.assetid: 6f719071-ebce-470d-aebd-1f55ee8cd70a
caps.latest.revision: 18
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: c47ab1243a4493fb49b47247233ed478e3cd70ca
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmhadrdatabasereplicaclusterstates-transact-sql"></a>sys.dm_hadr_database_replica_cluster_states (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne contenant des informations destinées à vous fournir un aperçu de l’intégrité des bases de données de disponibilité dans les groupes de disponibilité Always On dans chaque groupe de disponibilité Always On sur le cluster de Clustering de basculement Windows Server (WSFC). Requête **sys.dm_hadr_database_replica_states** pour répondre aux questions suivantes :  
  
-   Toutes les bases de données d'un groupe de service sont-elles prêtes pour un basculement ?  
  
-   Est-ce-qu'après un basculement forcé, une base de données secondaire s'est interrompue localement et a signalé son état suspendu au nouveau réplica principal ?  
  
-   Si le réplica principal n'est pas disponible actuellement, quel réplica secondaire permettrait la perte minimale de données s'il devenait le réplica principal ?  
  
-   Lorsque la valeur de la [sys.databases](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)**log_reuse_wait_desc** colonne est « AVAILABILITY_REPLICA », quel réplica secondaire dans un groupe de disponibilité retarde la troncation du journal sur une base de données primaire donnée ?  
   
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|Identificateur du réplica de disponibilité dans le groupe de disponibilité.|  
|**group_database_id**|**uniqueidentifier**|Identificateur de la base de données dans le groupe de disponibilité. Cet identificateur est identique sur chaque réplica auquel cette base de données est attachée.|  
|**database_name**|**sysname**|Nom d'une base de données qui appartient au groupe de disponibilité.|  
|**is_failover_ready**|**bit**|Indique si la base de données secondaire est synchronisée avec la base de données primaire correspondante. une des :<br /><br /> 0 = la base de données n'est pas marquée comme étant synchronisée dans le cluster. La base de données n'est pas prête pour un basculement.<br /><br /> 1 = la base de données est marquée comme étant synchronisée dans le cluster. La base de données est prête pour un basculement.|  
|**is_pending_secondary_suspend**|**bit**|Indique si, après un basculement forcé, la base de données est en attente de suspension. Peut prendre une des valeurs suivantes :<br /><br /> 0 = n'importe quel état, sauf HADR_SYNCHRONIZED_ SUSPENDED.<br /><br /> 1 = HADR_SYNCHRONIZED_ SUSPENDED. Lorsqu'un basculement forcé se termine, chacune des bases de données secondaires est définie à HADR_SYNCHONIZED_SUSPENDED et reste dans cet état jusqu'à ce que le nouveau réplica principal reçoive un accusé de réception de cette base de données secondaire au message SUSPEND.<br /><br /> NULL = inconnu (aucun quorum)|  
|**is_database_joined**|**bit**|Indique si la base de données sur ce réplica de disponibilité a été attachée au groupe de disponibilité. Peut prendre une des valeurs suivantes :<br /><br /> 0 = la base de données n'est pas attachée au groupe de disponibilité sur ce réplica de disponibilité.<br /><br /> 1 = la base de données est attachée au groupe de disponibilité sur ce réplica de disponibilité.<br /><br /> NULL = inconnu (le réplica de disponibilité ne possède pas de quorum).|  
|**recovery_lsn**|**numeric(25,0)**|Sur le réplica principal, la fin du journal des transactions avant que le réplica n'écrive de nouveaux enregistrements de journal après la récupération ou le basculement. Sur le réplica principal, la ligne d'une base de données secondaire donnée aura la valeur avec laquelle le réplica principal nécessite que le réplica secondaire se synchronise (autrement dit, restaurer et réinitialiser).<br /><br /> Sur les réplicas secondaires cette valeur est NULL. Notez que chaque réplica secondaire aura la valeur maximale ou une valeur inférieure à laquelle le réplica principal a indiqué au réplica secondaire de revenir.|  
|**truncation_lsn**|**numeric(25,0)**|Valeur de troncation du journal [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], qui peut être supérieure au LSN de troncation en local si la troncation du journal en local est bloquée (notamment par une opération de sauvegarde).|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues et fonctions de gestion dynamiques de groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Affichages catalogue des groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Surveiller les groupes de disponibilité & #40 ; Transact-SQL & #41 ;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Sys.dm_hadr_database_replica_states & #40 ; Transact-SQL & #41 ;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
  
