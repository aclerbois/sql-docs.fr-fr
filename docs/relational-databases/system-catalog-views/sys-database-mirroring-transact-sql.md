---
title: Sys.database_mirroring (Transact-SQL) | Documents Microsoft
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
- sys.database_mirroring
- database_mirroring
- sys.database_mirroring_TSQL
- database_mirroring_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_mirroring catalog view
ms.assetid: 480de2b0-2c16-497d-a6a3-bf7f52a7c9a0
caps.latest.revision: 53
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 50b7b4df6a6583831c81b021db98df2a42fc3620
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysdatabasemirroring-transact-sql"></a>sys.database_mirroring (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque base de données dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si la base de données n’est pas en ligne ou la mise en miroir de base de données n’est pas activée, les valeurs de toutes les colonnes sauf database_id sera NULL.  
  
 Pour afficher la ligne d’une base de données autre que master ou tempdb, vous devez être le propriétaire de la base de données ou avoir au moins les autorisations de niveau serveur ALTER ANY DATABASE ou VIEW ANY DATABASE ou CREATE DATABASE dans la base de données master. Pour afficher des valeurs non NULL dans une base de données miroir, vous devez être un membre de la **sysadmin** rôle serveur fixe.  
  
> [!NOTE]  
>  Si une base de données ne participe pas à la mise en miroir, toutes les colonnes avec le préfixe « mirroring_ » possèdent la valeur NULL.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de la base de données. Unique dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**mirroring_guid**|**uniqueidentifier**|ID du partenariat de mise en miroir.<br /><br /> NULL = la base de données n’est pas accessible ou n’est pas mise en miroir.<br /><br /> Remarque : Si la base de données ne participe pas à la mise en miroir, le préfixe « Mirroring » de toutes les colonnes sont NULL.|  
|**mirroring_state**|**tinyint**|État de la base de données miroir et de la session de mise en miroir de base de données.<br /><br /> 0 = suspendu<br /><br /> 1 = Déconnecté de l'autre partenaire<br /><br /> 2 = Synchronisation<br /><br /> 3 = Basculement en attente<br /><br /> 4 = Synchronisé<br /><br /> 5 = les serveurs partenaires ne sont pas synchronisés. Le basculement n'est maintenant pas possible.<br /><br /> 6 = les serveurs partenaires sont synchronisés. Le basculement est éventuellement possible. Pour plus d’informations sur la configuration requise pour le basculement, consultez [Modes de fonctionnement de la mise en miroir de base de données](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).<br /><br /> NULL = La base de données n'est pas accessible ou n'a pas été mise en miroir.|  
|**mirroring_state_desc**|**nvarchar(60)**|La description de l'état de la base de données miroir et de la session de mise en miroir de base de données peut être :<br /><br /> DISCONNECTED<br /><br /> SYNCHRONIZED<br /><br /> SYNCHRONIZING<br /><br /> PENDING_FAILOVER<br /><br /> SUSPENDED<br /><br /> UNSYNCHRONIZED<br /><br /> SYNCHRONIZED<br /><br /> NULL<br /><br /> Pour plus d’informations, consultez [États de la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/mirroring-states-sql-server.md).|  
|**mirroring_role**|**tinyint**|Rôle actuel de la base de données locale dans la session de mise en miroir de base de données.<br /><br /> 1 = Principal<br /><br /> 2 = Miroir<br /><br /> NULL = La base de données n'est pas accessible ou n'a pas été mise en miroir.|  
|**mirroring_role_desc**|**nvarchar(60)**|Description du rôle de base de données locale dans la mise en miroir, pouvant posséder l'une des valeurs suivantes :<br /><br /> PRINCIPAL<br /><br /> MIRROR|  
|**mirroring_role_sequence**|**int**|Nombre de fois où des partenaires de mise en miroir ont fait basculer les rôles principaux et en miroir en raison d'un basculement ou d'un service forcé.<br /><br /> NULL = La base de données n'est pas accessible ou n'a pas été mise en miroir.|  
|**mirroring_safety_level**|**tinyint**|Paramètre de sécurité pour les mises à jour sur la base de données miroir :<br /><br /> 0 = État inconnu<br /><br /> 1 = Off [asynchrone]<br /><br /> 2 = Complet [synchrone]<br /><br /> NULL = La base de données n'est pas accessible ou n'a pas été mise en miroir.|  
|**mirroring_safety_level_desc**|**nvarchar(60)**|Paramètre de sécurité de transaction pour les mises à jour sur la base de données miroir, pouvant posséder l'une des valeurs suivantes :<br /><br /> UNKNOWN<br /><br /> OFF<br /><br /> FULL<br /><br /> NULL|  
|**mirroring_safety_sequence**|**int**|Mettre à jour le numéro de séquence pour les modifications au niveau de la sécurité de transaction.<br /><br /> NULL = La base de données n'est pas accessible ou n'a pas été mise en miroir.|  
|**mirroring_partner_name**|**nvarchar(128)**|Nom du serveur partenaire de mise en miroir de bases de données.<br /><br /> NULL = La base de données n'est pas accessible ou n'a pas été mise en miroir.|  
|**mirroring_partner_instance**|**nvarchar(128)**|Nom d'instance et nom d'ordinateur de l'autre partenaire. Les clients nécessitent ces informations pour se connecter au partenaire s'il devient le serveur principal.<br /><br /> NULL = La base de données n'est pas accessible ou n'a pas été mise en miroir.|  
|**mirroring_witness_name**|**nvarchar(128)**|Nom du serveur du témoin de mise en miroir de bases de données.<br /><br /> NULL= Il n'existe aucun témoin.|  
|mirroring_witness_state|**tinyint**|L'état du témoin dans la session de mise en miroir de la base de données peut être :<br /><br /> 0 = Inconnu<br /><br /> 1 = Connected<br /><br /> 2 = Déconnecté<br /><br /> NULL = Aucun témoin existant, la base de données n'est pas en ligne, ou la base de données n'a pas été mise en miroir.|  
|**mirroring_witness_state_desc**|**nvarchar(60)**|La description de l'état peut être :<br /><br /> UNKNOWN<br /><br /> CONNECTED<br /><br /> DISCONNECTED<br /><br /> NULL|  
|**mirroring_failover_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal (LSN) du dernier enregistrement du journal des transactions dont le renforcement sur disque est garanti sur les deux partenaires. Après un basculement, le **mirroring_failover_lsn** est utilisé par les partenaires comme point de réconciliation à partir duquel le nouveau serveur miroir commence la synchronisation de la nouvelle base de données miroir avec la nouvelle base de données principale.|  
|**mirroring_connection_timeout**|**int**|Délai d'attente de connexion de mise en miroir en secondes. Nombre de secondes à patienter avant la réponse d'un serveur partenaire ou témoin avant de les considérer comme indisponibles. La valeur de délai d'attente par défaut est de 10 secondes.<br /><br /> NULL = La base de données n'est pas accessible ou n'a pas été mise en miroir.|  
|**mirroring_redo_queue**|**int**|Quantité maximale de journaux à restaurer par progression sur le miroir. Si mirroring_redo_queue_type a la valeur UNLIMITED, qui est le paramètre par défaut, cette colonne est NULL. Si la base de données n'est pas en ligne, cette colonne possède aussi la valeur NULL.<br /><br /> Sinon, cette colonne contient la quantité maximale de journaux en mégaoctets. Lorsque la valeur maximale est atteinte, le journal est temporairement bloqué sur le principal pendant que le serveur en miroir mirror se met à niveau. Cette fonction limite le temps de basculement.<br /><br /> Pour plus d’informations, consultez [Estimer l’interruption de service au cours d’un basculement de rôle &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md).|  
|**mirroring_redo_queue_type**|**nvarchar(60)**|UNLIMITED indique que la mise en miroir ne limite la file d'attente de restauration par progression. Il s'agit du paramètre par défaut.<br /><br /> Mo indique la taille maximale de la file d'attente de restauration par progression en mégaoctets. Sachez que si la taille de la file d'attente a été spécifiée en kilo-octets ou mégaoctets, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] convertit cette valeur en mégaoctets.<br /><br /> Si la base de données n'est pas en ligne, cette colonne possède la valeur NULL.|  
|**mirroring_end_of_log_lsn**|**numeric(25,0)**|end-of-log local qui a été vidé sur le disque. Cela est comparable à la valeur LSN renforcé à partir du serveur miroir (voir la **mirroring_failover_lsn** colonne).|  
|**mirroring_replication_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal maximum que la réplication peut envoyer.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.database_mirroring_witnesses &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
 [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [Affichages catalogue de bases de données et de fichiers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [Questions fréquentes (FAQ) sur l’interrogation des catalogues système SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
