---
title: Sys.Databases (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- databases
- databases_TSQL
- sys.databases
- sys.databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.databases catalog view
ms.assetid: 46c288c1-3410-4d68-a027-3bbf33239289
caps.latest.revision: 152
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 49f8ab952cad03717474d17a4697063050777e26
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysdatabases-transact-sql"></a>sys.databases (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Contient une ligne par base de données dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si une base de données n’est pas `ONLINE`, ou `AUTO_CLOSE` a la valeur `ON` et la base de données est fermée, les valeurs de certaines colonnes peuvent être `NULL`. Si une base de données est `OFFLINE`, la ligne correspondante n’est pas visible pour les utilisateurs à faibles privilèges. Pour afficher la ligne correspondante si la base de données est `OFFLINE`, un utilisateur doit avoir au moins les `ALTER ANY DATABASE` autorisation de niveau serveur, ou le `CREATE DATABASE` autorisation dans le `master` base de données.  
  

  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Nom de la base de données, unique dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans un serveur [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**database_id**|**int**|ID de la base de données, unique dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans un serveur [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**source_database_id**|**int**|Valeur autre que NULL = ID de la base de données source pour cet instantané.<br /> NULL = Pas un instantané de base de données.|  
|**owner_sid**|**varbinary(85)**|SID (identificateur de sécurité) du propriétaire externe de la base de données, tel qu'il est enregistré sur le serveur. Pour plus d’informations sur qui peut posséder une base de données, consultez la **autorisation de modifier des bases de données** section de [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).|  
|**create_date**|**datetime**|Date de création ou de nouvelle appellation de la base de données. Pour **tempdb**, cette valeur change chaque fois que le serveur redémarre.|  
|**compatibility_level**|**tinyint**|Entier correspondant à la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour laquelle le comportement est compatible :<br /> **Valeur** : **s’applique à**<br /> 70 : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]<br /> 80 : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]<br /> 90 : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]<br /> 100 : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /> 110 : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /> 120 : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /> 130 : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] |  
|**collation_name**|**sysname**|Classement pour la base de données. Joue le rôle du classement par défaut de la base de données.<br /> NULL = base de données n’est pas en ligne ou AUTO_CLOSE est activée et la base de données est fermé.|  
|**user_access**|**tinyint**|Paramètre d'accès utilisateur :<br /> 0 = MULTI_USER spécifié<br /> 1 = SINGLE_USER spécifié<br /> 2 = RESTRICTED_USER spécifié|  
|**user_access_desc**|**nvarchar(60)**|Description du paramètre d'accès utilisateur.|  
|**is_read_only**|**bit**|1 = La base de données est en lecture seule<br /> 0 = La base de données est en lecture/écriture|  
|**is_auto_close_on**|**bit**|1 = AUTO_CLOSE est activé<br /> 0 = AUTO_CLOSE est désactivé|  
|**is_auto_shrink_on**|**bit**|1 = AUTO_SHRINK est activé<br /> 0 = AUTO_SHRINK est désactivé|  
|**state**|**tinyint**|**Valeur &#124; s’applique à**<br /> 0 = ONLINE <br /> 1 = RESTORING <br /> 2 = restauration : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /> 3 = RECOVERY_PENDING : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /> 4 = SUSPECT <br /> 5 = d’urgence : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /> 6 = hors connexion : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /> 7 = COPIE : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)] <br /> 10 = OFFLINE_SECONDARY : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)] <br /><br /> **Remarque :** pour les bases de données Always On, interrogez la `database_state` ou `database_state_desc` colonnes de [sys.dm_hadr_database_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md).|  
|**state_desc**|**nvarchar(60)**|Description de l'état de la base de données. Consultez l’état.|  
|**is_in_standby**|**bit**|La base de données est en lecture seule pour le journal de restauration.|  
|**is_cleanly_shutdown**|**bit**|1 = La base de données s'est arrêtée correctement ; aucune récupération n'est requise au démarrage<br /> 0 = La base de données ne s'est pas arrêtée correctement ; une récupération est requise au démarrage|  
|**is_supplemental_logging_enabled**|**bit**|1 = SUPPLEMENTAL_LOGGING est activé<br /> 0 = SUPPLEMENTAL_LOGGING est désactivé|  
|**snapshot_isolation_state**|**tinyint**|État des transactions d'isolation d'instantané autorisées, telles qu'elles sont définies par l'option ALLOW_SNAPSHOT_ISOLATION :<br /> 0 = L'état d'isolation d'instantané est désactivé (valeur par défaut). L'isolation d'instantané n'est pas autorisée.<br /> 1 = L'état d'isolation d'instantané est activé. L'isolation d'instantané est autorisée.<br /> 2 = L'état d'isolation d'instantané est en cours de désactivation. Les modifications de toutes les transactions sont marquées d'une version. Il est impossible de démarrer de nouvelles transactions à l'aide de l'isolation d'instantané. La base de données demeure en cours de désactivation tant que toutes les transactions, qui étaient actives lors de l'exécution de ALTER DATABASE, ne sont pas terminées.<br /> 3 = L'état d'isolation d'instantané est en cours d'activation. Les modifications de toutes les nouvelles transactions sont marquées d'une version. Les transactions ne peuvent pas utiliser l'isolation d'instantané tant que son état n'a pas pour valeur 1 (activé). La base de données demeure en cours d'activation tant que toutes les transactions de mise à jour, qui étaient actives lors de l'exécution de ALTER DATABASE, ne sont pas terminées.|  
|**snapshot_isolation_state_desc**|**nvarchar(60)**|Description de l'état des transactions d'isolation de capture instantanée autorisées, telles qu'elles sont définies par l'option ALLOW_SNAPSHOT_ISOLATION.|  
|**is_read_committed_snapshot_on**|**bit**|1 = l'option READ_COMMITTED_SNAPSHOT est activée. Les opérations de lecture dans le niveau d'isolation validé en lecture reposent sur des analyses d'instantané ; elles ne nécessitent aucun verrou.<br /> 0 = l'option READ_COMMITTED_SNAPSHOT est désactivée (valeur par défaut). Les opérations de lecture dans le niveau d'isolation validé en lecture utilisent des verrous partagés.|  
|**recovery_model**|**tinyint**|Mode de récupération sélectionné :<br /> 1 = FULL<br /> 2 = BULK_LOGGED<br /> 3 = SIMPLE|  
|**recovery_model_desc**|**nvarchar(60)**|Description du mode de récupération sélectionné.|  
|**page_verify_option**|**tinyint**|Paramètre de l'option PAGE_VERIFY :<br /> 0 = AUCUN<br /> 1 = TORN_PAGE_DETECTION<br /> 2 = CHECKSUM|  
|**page_verify_option_desc**|**nvarchar(60)**|Description du paramètre de l'option PAGE_VERIFY.|  
|**is_auto_create_stats_on**|**bit**|1 = AUTO_CREATE_STATISTICS est activé<br /> 0 = AUTO_CREATE_STATISTICS est désactivé|  
|**is_auto_create_stats_incremental_on**|**bit**|Indique le paramètre par défaut de l'option incrémentielle des statistiques automatiques.<br /> 0 = Les statistiques créées automatiquement ne sont pas incrémentielles<br /> 1 = Les statistiques créées automatiquement sont incrémentielles, si possible<br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**is_auto_update_stats_on**|**bit**|1 = AUTO_UPDATE_STATISTICS est activé<br /> 0 = AUTO_UPDATE_STATISTICS est désactivé|  
|**is_auto_update_stats_async_on**|**bit**|1 = AUTO_UPDATE_STATISTICS_ASYNC est activé<br /> 0 = AUTO_UPDATE_STATISTICS_ASYNC est désactivé|  
|**is_ansi_null_default_on**|**bit**|1 = ANSI_NULL_DEFAULT est activé<br /> 0 = ANSI_NULL_DEFAULT est désactivé|  
|**is_ansi_nulls_on**|**bit**|1 = ANSI_NULLS est activé<br /> 0 = ANSI_NULLS est désactivé|  
|**is_ansi_padding_on**|**bit**|1 = ANSI_PADDING est activé<br /> 0 = ANSI_PADDING est désactivé|  
|**is_ansi_warnings_on**|**bit**|1 = ANSI_WARNINGS est activé<br /> 0 = ANSI_WARNINGS est désactivé|  
|**is_arithabort_on**|**bit**|1 = ARITHABORT est activé<br /> 0 = ARITHABORT est désactivé|  
|**is_concat_null_yields_null_on**|**bit**|1 = CONCAT_NULL_YIELDS_NULL est activé<br /> 0 = CONCAT_NULL_YIELDS_NULL est désactivé|  
|**is_numeric_roundabort_on**|**bit**|1 = NUMERIC_ROUNDABORT est activé<br /> 0 = NUMERIC_ROUNDABORT est désactivé|  
|**is_quoted_identifier_on**|**bit**|1 = QUOTED_IDENTIFIER est activé<br /> 0 = QUOTED_IDENTIFIER est désactivé|  
|**is_recursive_triggers_on**|**bit**|1 = RECURSIVE_TRIGGERS est activé<br /> 0 = RECURSIVE_TRIGGERS est désactivé|  
|**is_cursor_close_on_commit_on**|**bit**|1 = CURSOR_CLOSE_ON_COMMIT est activé<br /> 0 = CURSOR_CLOSE_ON_COMMIT est désactivé|  
|**is_local_cursor_default**|**bit**|1 = CURSOR_DEFAULT est local<br /> 0 = CURSOR_DEFAULT est global|  
|**is_fulltext_enabled**|**bit**|1 = Le texte intégral est activé pour la base de données<br /> 0 = Le texte intégral est désactivé pour la base de données|  
|**is_trustworthy_on**|**bit**|1 = La base de données est marquée comme digne de confiance<br /> 0 = La base de données n'est pas marquée comme digne de confiance|  
|**is_db_chaining_on**|**bit**|1 = Le chaînage des propriétés des bases de données croisées est activé<br /> 0 = Le chaînage des propriétés des bases de données croisées est désactivé|  
|**is_parameterization_forced**|**bit**|1 = Le paramétrage est forcé<br /> 0 = Le paramétrage est simple|  
|**is_master_key_encrypted_by_server**|**bit**|1 = La base de données a une clé principale chiffrée<br /> 0 = La base de données n'a aucune clé principale chiffrée|  
|**is_query_store_on**|**bit**|1 = la requête magasin est activée pour cette base de données. Vérifiez [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md) pour afficher l’état du magasin de requête.<br /> 0 = la requête magasin n’est pas activé.<br /> **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] via la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).|  
|**is_published**|**bit**|1 = La base de données est de type publication dans une topologie de réplication transactionnelle ou d'instantané<br /> 0 = N'est pas une base de données de publication|  
|**is_subscribed**|**bit**|Cette colonne n'est pas utilisée. Elle retourne toujours 0, indépendamment de l'état d'abonné de la base de données.|  
|**is_merge_published**|**bit**|1 = La base de données est de type publication dans une topologie de réplication de fusion<br /> 0 = N'est pas une base de données de publication dans une topologie de réplication de fusion|  
|**is_distributor**|**bit**|1 = La base de données est de type distribution dans une topologie de réplication<br /> 0 = N'est pas une base de données de distribution dans une topologie de réplication|  
|**is_sync_with_backup**|**bit**|1 = La base de données est marquée pour une synchronisation de réplication avec sauvegarde<br /> 0 = La base de données n'est pas marquée pour une synchronisation de réplication avec sauvegarde|  
|**service_broker_guid**|**uniqueidentifier**|Identificateur du Service Broker pour cette base de données. Utilisé en tant que le **broker_instance** de la cible dans la table de routage.|  
|**is_broker_enabled**|**bit**|1 = Le Service Broker dans cette base de données envoie et reçoit actuellement des messages.<br /> 0 = Tous les messages envoyés restent dans la file d'attente de transmission alors que les messages reçus ne sont pas mis en attente dans cette base de données.<br /> Le Service Broker des bases de données restaurées ou attachées est par défaut désactivé. L'exception à cette règle repose sur la mise en miroir de bases de données lorsque Service Broker est activé après un basculement.|  
|**log_reuse_wait**|**tinyint**|Réutilisation de l’espace du journal des transactions est en attente sur l’un des éléments suivants depuis le dernier point de contrôle. (Pour plus d’explications de ces valeurs, consultez [du journal des transactions](../../relational-databases/logs/the-transaction-log-sql-server.md).)<br /> 0 = Rien<br />   1 = Point de vérification (lorsqu'une base de données utilise un mode de récupération et a un groupe de fichiers de données optimisé en mémoire, la colonne log_reuse_wait doit indiquer checkpoint ou xtp_checkpoint). **S’applique aux** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br />  2 = sauvegarde de journal **s’applique aux** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br />  3 = sauvegarde active ou de restauration **s’applique aux** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br />  4 = transaction active **s’applique aux** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br />  5 = mise en miroir de base de données **s’applique aux** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br />  6 = réplication **s’applique aux** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br />  7 = Création de capture instantanée de base de données **s’applique aux** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br />  8 = analyse du journal **s’applique à**<br />  9 = un groupes de disponibilité AlwaysOn réplica secondaire applique les enregistrements de journal des transactions de cette base de données à une base de données secondaire correspondante. **S’applique aux** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Dans les versions précédentes de SQL Server, 9 = Autre (Temporaire).<br />  10 = pour usage interne uniquement **s’applique aux** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br />  11 = pour usage interne uniquement **s’applique aux** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /> 12 = pour usage interne uniquement **s’applique aux** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br />13 = page la plus ancienne **s’applique aux** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /> 14 = autre **s’applique aux** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br />  16 = XTP_CHECKPOINT (lorsqu'une base de données utilise un mode de récupération et a un groupe de fichiers de données optimisé en mémoire, la colonne log_reuse_wait doit indiquer checkpoint ou xtp_checkpoint). **S’applique aux** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]|  
|**log_reuse_wait_desc**|**nvarchar(60)**|La description de la réutilisation de l'espace du journal des transactions est en attente du dernier point de contrôle.|  
|**is_date_correlation_on**|**bit**|1 = DATE_CORRELATION_OPTIMIZATION est activé<br /> 0 = DATE_CORRELATION_OPTIMIZATION est désactivé|  
|**is_cdc_enabled**|**bit**|1 = La base de données est activée pour la capture des données modifiées. Pour plus d’informations, consultez [sys.sp_cdc_enable_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md).|  
|**is_encrypted**|**bit**|Indique si la base de données est chiffrée (reflète l'état dernièrement défini à l'aide de la clause ALTER DATABASE SET ENCRYPTION). Il peut s'agir de l'une des valeurs suivantes :<br /> 1 = Chiffrée<br /> 0 = Non chiffré<br /> Pour plus d’informations sur le chiffrement des bases de données, consultez [Chiffrement transparent des données &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).<br /> Si la base de données est en cours de déchiffrement, **is_encrypted** affiche la valeur 0. Vous pouvez consulter l’état du processus de chiffrement à l’aide de la [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) vue de gestion dynamique.|  
|**is_honor_broker_priority_on**|**bit**|Indique si la base de données honore les priorités de conversation (reflète l'état dernièrement défini à l'aide de la clause ALTER DATABASE SET HONOR_BROKER_PRIORITY). Il peut s'agir de l'une des valeurs suivantes :<br /> 1 = HONOR_BROKER_PRIORITY a la valeur ON<br /> 0 = HONOR_BROKER_PRIORITY a la valeur OFF|  
|**replica_id**|**uniqueidentifier**|Identificateur unique du réplica de disponibilité [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] local du groupe de disponibilité, le cas échéant, auquel la base de données participe.<br /> NULL = base de données ne fait pas partie d’un réplica de disponibilité du groupe de disponibilité.<br /> **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**group_database_id**|**uniqueidentifier**|Identificateur unique de la base de données dans un groupe Always On disponibilité, cas échéant, dans lequel la base de données participe. **group_database_id** est identique pour cette base de données sur le réplica principal et sur chaque réplica secondaire sur lequel la base de données a été jointe au groupe de disponibilité.<br /> NULL = La base de données ne fait pas partie d'un réplica de disponibilité dans un groupe de disponibilité.<br /> **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**resource_pool_id**|**int**|ID du pool de ressources qui est mappé à cette base de données. Ce pool de ressources contrôle la mémoire totale qui est disponible pour les tables optimisées en mémoire dans cette base de données.<br /> **S’applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]|  
|**default_language_lcid**|**smallint**|Indique l'ID local (lcid) de la langue par défaut d'une base de données à relation contenant-contenu.<br /> **Remarque** fonctionne comme le [configurer l’Option de Configuration de serveur de la langue par défaut](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) de **sp_configure**. Cette valeur est **null** pour une base de données sans relation contenant contenu.<br /> **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**default_language_name**|**nvarchar(128)**|Indique la langue par défaut d'une base de données à relation contenant-contenu.<br /> Cette valeur est **null** pour une base de données sans relation contenant contenu.<br /> **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**default_fulltext_language_lcid**|**int**|Indique l'ID local (lcid) de la langue de recherche en texte intégral par défaut de la base de données à relation contenant-contenu.<br /> **Remarque** fonctionne comme la valeur par défaut [configurer la langue de recherche en texte intégral par défaut Server Configuration Option](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md) de **sp_configure**. Cette valeur est **null** pour une base de données sans relation contenant contenu.<br /> **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**default_fulltext_language_name**|**nvarchar(128)**|Indique la langue par défaut de recherche en texte intégral de la base de données à relation contenant-contenu.<br /> Cette valeur est **null** pour une base de données sans relation contenant contenu.<br /> **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**is_nested_triggers_on**|**bit**|Indique si les déclencheurs imbriqués sont autorisés dans la base de données à relation contenant-contenu.<br /> 0 = Les déclencheurs imbriqués ne sont pas autorisés<br /> 1 = Les déclencheurs imbriqués sont autorisés<br /> **Remarque** fonctionne comme le [configurer l’Option de Configuration de serveur nested triggers](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md) de **sp_configure**. Cette valeur est **null** pour une base de données sans relation contenant contenu. Consultez [sys.configurations &#40;Transact-SQL&#41; ](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) pour plus d’informations.<br /> **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**is_transform_noise_words_on**|**bit**|Indique si les mots parasites doivent être transformés dans la base de données à relation contenant-contenu.<br /> 0 = Les mots parasites ne doivent pas être transformés.<br /> 1 = Les mots parasites doivent être transformés.<br /> **Remarque** fonctionne comme le [transformer les mots parasites Server Configuration Option](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md) de **sp_configure**. Cette valeur est **null** pour une base de données sans relation contenant contenu. Consultez [sys.configurations &#40;Transact-SQL&#41; ](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) pour plus d’informations.<br /> **S’applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]|  
|**two_digit_year_cutoff**|**smallint**|Indique la valeur d'un nombre entre 1 753 et 9 999 pour représenter l'année de coupure afin d'interpréter les années à deux chiffres comme des années à quatre chiffres.<br /> **Remarque** fonctionne comme le [configurer l’année à deux chiffres cutoff Server Configuration Option](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) de **sp_configure**. Cette valeur est **null** pour une base de données sans relation contenant contenu. Consultez [sys.configurations &#40;Transact-SQL&#41; ](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) pour plus d’informations.<br /> **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**containment**|**tinyint non null**|Indique l'état de la relation contenant-contenu de la base de données.<br />  0 = La relation contenant-contenu de base de données est désactivée. **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /> 1 = base de données est dans relation contenant-contenu partielle **s’applique aux**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]|  
|**containment_desc**|**nvarchar (60) non null**|Indique l'état de la relation contenant-contenu de la base de données.<br /> NONE = Base de données héritée (relation contenant-contenu nulle)<br /> PARTIAL = Base de données à relation contenant-contenu partielle<br /> **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**target_recovery_time_in_seconds**|**int**|Durée estimée pour récupérer la base de données, en secondes. Autorise la valeur Null.<br /> **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**delayed_durability**|**int**|Le paramètre de durabilité différée :<br /> 0 = DÉSACTIVÉ<br /> 1 = AUTORISÉ<br /> 2 = FORCÉ<br /> Pour plus d’informations, consultez [Contrôler la durabilité d’une transaction](../../relational-databases/logs/control-transaction-durability.md).<br /> **S’applique aux**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
|**delayed_durability_desc**|**nvarchar(60)**|Le paramètre de durabilité différée :<br /> DISABLED<br /> ALLOWED<br /> FORCED<br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /> **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
|**is_memory_optimized_elevate_to_snapshot_on**|**bit**|L'isolation SNAPSHOT permet d'accéder aux tables optimisées en mémoire lorsque le paramètre de session TRANSACTION ISOLATION LEVEL a une valeur correspondant à un niveau d'isolation inférieur, READ COMMITTED ou READ UNCOMMITTED.<br /> 1 = Le niveau d'isolation minimal est SNAPSHOT.<br /> 0 = Le niveau d'isolation n'est pas élevé.|  
|**is_federation_member**|**bit**|Indique si la base de données est membre d'une fédération.<br /> **S’applique à** : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|**is_remote_data_archive_enabled**|**bit**|Indique si la base de données est étirée.<br /> 0 = la base de données n’est pas compatible avec Stretch.<br /> 1 = la base de données est compatible avec Stretch.<br /> **S’applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /> Pour plus d’informations, consultez [Stretch Database](../../sql-server/stretch-database/stretch-database.md).|  
|**is_mixed_page_allocation_on**|**bit**|Indique si les tables et les index dans la base de données peuvent allouer premières pages issues d’extensions mixtes.<br /> 0 = les tables et index dans la base de données toujours allouent les pages initiales à partir des extensions uniformes.<br /> 1 = les tables et index dans la base de données peuvent allouer des premières pages issues d’extensions mixtes.<br /> **S’applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /> Pour plus d’informations, consultez l’option SET MIXED_PAGE_ALLOCATION de [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).|  
|**is_temporal_retention_enabled**|**bit**|Indique si la tâche de nettoyage de stratégie de rétention temporelle est activé.<br /> **S’applique à**: base de données SQL Azure|
|**catalog_collation_type**|**int**|Le paramètre de classement de catalogue :<br />0 = DATABASE_DEFAULT<br />2 = SQL_Latin_1_General_CP1_CI_AS<br /> **S’applique à**: base de données SQL Azure|
|**catalog_collation_type_desc**|**nvarchar(60)**|Le paramètre de classement de catalogue :<br />DATABASE_DEFAULT<br />SQL_Latin_1_General_CP1_CI_AS<br /> **S’applique à**: base de données SQL Azure|
  
## <a name="permissions"></a>Autorisations  
 Si l’appelant de `sys.databases` n’est pas le propriétaire de la base de données et la base de données n’est pas `master` ou `tempdb`, les autorisations minimales requises pour consulter la ligne correspondante sont `ALTER ANY DATABASE` ou `VIEW ANY DATABASE` autorisation de niveau serveur, ou `CREATE DATABASE` autorisation dans le `master` base de données. La base de données à laquelle l’appelant est connecté peut toujours être vue dans `sys.databases`.  
  
> [!IMPORTANT]  
>  Par défaut, le rôle public a le `VIEW ANY DATABASE` autorisation, ce qui permet des connexions d’accès aux informations de base de données. Pour bloquer une connexion à partir de la capacité de détecter une base de données, `REVOKE` le `VIEW ANY DATABASE` autorisation `public`, ou `DENY` le ' autorisation VIEW ANY DATABASE pour des connexions individuelles.  
  
## <a name="includesssdsincludessssds-mdmd-remarks"></a>Notes concernant [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
 Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], cette vue est disponible dans le `master` base de données et dans les bases de données utilisateur. Dans le `master` base de données, cette vue retourne des informations sur la `master` de base de données et toutes les bases de données utilisateur sur le serveur. Dans une base de données utilisateur, cette vue ne retourne des informations que sur la base de données active et la base de données master.  
  
 Utilisez la vue `sys.databases` dans la base de données `master` du serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] où la base de données est créée. Après le démarrage de la copie de base de données, vous pouvez interroger la `sys.databases` et `sys.dm_database_copies` vues depuis la `master` base de données du serveur de destination pour récupérer des informations sur la progression de la copie.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-query-the-sysdatabases-view"></a>A. Interroger la vue sys.databases  
 L’exemple suivant retourne quelques-unes des colonnes disponibles dans le `sys.databases` vue.  
  
```  
SELECT name, user_access_desc, is_read_only, state_desc, recovery_model_desc  
FROM sys.databases;  
```  
  
### <a name="b-check-the-copying-status-in-includesssdsincludessssds-mdmd"></a>B. Vérifier l'état de copie dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
 L’exemple suivant interroge la `sys.databases` et `sys.dm_database_copies` l’opération de copie de vues pour retourner des informations sur une base de données.  
  
**S’applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
  
```  
-- Execute from the master database.  
SELECT a.name, a.state_desc, b.start_date, b.modify_date, b.percentage_complete  
FROM sys.databases AS a  
INNER JOIN sys.dm_database_copies AS b ON a.database_id = b.database_id  
WHERE a.state = 7;  
```  
### <a name="c-check-the-temporal-retention-policy-status-in-includesssdsincludessssds-mdmd"></a>C. Vérifiez l’état de la stratégie de rétention temporelles [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
 L’exemple suivant interroge la `sys.databases` pour retourner des informations si la tâche de nettoyage temporelle de rétention est activée. N’oubliez pas qu’après l’opération de restauration rétention temporelle est désactivée par défaut. Utilisez `ALTER DATABASE` pour l’activer explicitement.
  
**S’applique à** : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```  
-- Execute from the master database.  
SELECT a.name, a.is_temporal_history_retention_enabled 
FROM sys.databases AS a;
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.database_mirroring_witnesses &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
 [Sys.database_recovery_status &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-recovery-status-transact-sql.md)   
 [Affichages catalogue de bases de données et de fichiers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [sys.dm_database_copies &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)  
  
  
