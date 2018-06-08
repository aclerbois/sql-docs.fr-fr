---
title: Sys.tables (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- tables_TSQL
- sys.tables_TSQL
- sys.tables
- tables
dev_langs:
- TSQL
helpviewer_keywords:
- sys.tables catalog view
ms.assetid: 8c42eba1-c19f-4045-ac82-b97a5e994090
caps.latest.revision: 70
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6b44d85fd3d1c252a91c719bc0df21fa7e84b996
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="systables-transact-sql"></a>sys.tables (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une ligne pour chaque table utilisateur dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|\<héritée de colonnes >||Pour obtenir la liste des colonnes qui hérite de cette vue, consultez [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|lob_data_space_id|**int**|Une valeur différente de zéro représente l'ID d'espace de données (groupe de fichiers ou schéma de partition) qui contient les données d'objet binaire volumineux (LOB) de cette table. Exemples de types de données LOB **varbinary (max)**, **varchar (max)**, **geography**, ou **xml**.<br /><br /> 0 = La table ne contient pas de données LOB.|  
|filestream_data_space_id|**int**|ID d'espace de données pour un groupe de fichiers FILESTREAM ou un schéma de partition composé de groupes de fichiers FILESTREAM.<br /><br /> Pour signaler le nom d’un groupe de fichiers FILESTREAM, exécutez la requête `SELECT FILEGROUP_NAME (filestream_data_space_id) FROM sys.tables`.<br /><br /> sys.tables peut être joint aux vues suivantes sur filestream_data_space_id = data_space_id.<br /><br /> -sys.filegroups<br /><br /> -sys.partition_schemes<br /><br /> -sys.indexes<br /><br /> -est introuvable<br /><br /> -sys.fulltext_catalogs<br /><br /> -sys.data_spaces<br /><br /> -sys.destination_data_spaces<br /><br /> -sys.master_files<br /><br /> -sys.database_files<br /><br /> -backupfilegroup (jointure sur filegroup_id)|  
|max_column_id_used|**int**|ID de colonne maximum utilisé à ce jour par cette table.|  
|lock_on_bulk_load|**bit**|La table est verrouillée pour le chargement en masse. Pour plus d’informations, consultez [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|uses_ansi_nulls|**bit**|Lorsque la table a été créée, l'option de base de données SET ANSI_NULLS avait pour valeur ON.|  
|is_replicated|**bit**|1 = la table est publiée à l'aide de la réplication d'instantané ou de la réplication transactionnelle.|  
|has_replication_filter|**bit**|1 = la table possède un filtre de réplication.|  
|is_merge_published|**bit**|1 = la table est publiée à l'aide de la réplication de fusion.|  
|is_sync_tran_subscribed|**bit**|1 = la table est abonnée à l'aide d'un abonnement avec mise à jour immédiate.|  
|has_unchecked_assembly_data|**bit**|1 = La table contient des données persistantes qui dépendent d'un assembly dont la définition a été modifiée lors de la dernière exécution de ALTER ASSEMBLY. Cette valeur est réinitialisée à 0 après exécution correcte de l'opération DBCC CHECKDB ou DBCC CHECKTABLE suivante.|  
|text_in_row_limit|**int**|Taille maximale en octets du texte de la ligne.<br /><br /> 0 = l'option « text in row » n'est pas définie. Pour plus d’informations, consultez [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|large_value_types_out_of_row|**bit**|1 = les types de valeur élevée sont stockés en dehors de la ligne. Pour plus d’informations, consultez [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|is_tracked_by_cdc|**bit**|1 = la table est activée pour la capture des données modifiées. Pour plus d’informations, consultez [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).|  
|lock_escalation|**tinyint**|Valeur de l'option LOCK_ESCALATION pour la table :<br /><br /> 0 = TABLE<br /><br /> 1 = DISABLE<br /><br /> 2 = AUTO|  
|lock_escalation_desc|**nvarchar(60)**|Description textuelle de l'option lock_escalation pour la table. Les valeurs possibles sont : TABLE, AUTO et DISABLE.|  
|is_filetable|**bit**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> 1 = La table est un FileTable.<br /><br /> Pour plus d’informations sur les FileTables, consultez [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).|  
|durabilité|**tinyint**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Les valeurs possibles sont les suivantes :<br /><br /> 0 = SCHEMA_AND_DATA<br /><br /> 1 = SCHEMA_ONLY<br /><br /> La valeur 0 est la valeur par défaut.|  
|durability_desc|**nvarchar(60)**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Les valeurs possibles sont les suivantes :<br /><br /> SCHEMA_ONLY<br /><br /> SCHEMA_AND_DATA<br /><br /> La valeur de SCHEMA_AND_DATA indique que la table est une table en mémoire durable. SCHEMA_AND_DATA est la valeur par défaut des tables optimisées en mémoire. La valeur de SCHEMA_ONLY indique que les données de la table ne seront pas conservées lors du redémarrage de la base de données contenant des objets optimisés en mémoire.|  
|is_memory_optimized|**bit**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Les valeurs possibles sont les suivantes :<br /><br /> 0 = Non optimisé en mémoire.<br /><br /> 1 = optimisé en mémoire.<br /><br /> La valeur 0 est la valeur par défaut.<br /><br /> Tables optimisées en mémoire sont des tables en mémoire utilisateur dont le schéma est rendu persistant sur disque similairement à d’autres tables utilisateur. Les tables optimisées en mémoire sont accessibles à partir des procédures stockées compilées en mode natif.|  
|temporal_type|**tinyint**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> La valeur numérique qui représente le type de table :<br /><br /> 0 = NON_TEMPORAL_TABLE<br /><br /> 1 = HISTORY_TABLE<br /><br /> 2 = SYSTEM_VERSIONED_TEMPORAL_TABLE|  
|temporal_type_desc|**nvarchar(60)**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Description de type de table :<br /><br /> NON_TEMPORAL_TABLE<br /><br /> HISTORY_TABLE<br /><br /> SYSTEM_VERSIONED_TEMPORAL_TABLE|  
|history_table_id|**int**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Lorsque temporal_type IN (2, 4) renvoie les object_id de la table qui conserve des données historiques, sinon retourne la valeur NULL.|  
|is_remote_data_archive_enabled|**bit**|**S’applique aux**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> Indique si la table est compatible avec Stretch.<br /><br /> 0 = la table n’est pas compatible avec Stretch.<br /><br /> 1 = la table est compatible avec Stretch.<br /><br /> Pour plus d'informations, consultez [Stretch Database](../../sql-server/stretch-database/stretch-database.md).|  
|is_external|**bit**|**S’applique aux**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)], et [!INCLUDE[sssdwfull](../../includes/sssdwfull-md.md)].<br /><br /> Indique la table est une table externe.<br /><br /> 0 = la table n’est pas une table externe.<br /><br /> 1 = la table est une table externe.| 
|history_retention_period|**int**|**S'applique à**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>La valeur numérique qui représente la durée de la période de rétention d’historique temporelle selon les unités spécifiées avec history_retention_period_unit. |  
|history_retention_period_unit|**int**|**S'applique à**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>La valeur numérique qui représente le type d’unité de période de rétention de l’historique temporelles. <br /><br />-1 : INFINIE <br /><br />3 : JOUR <br /><br />4 : SEMAINE <br /><br />5 : MOIS <br /><br />6 : ANNÉE |  
|history_retention_period_unit_desc|**nvarchar(10)**|**S'applique à**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>Description de type d’unité de période de rétention de l’historique temporelles. <br /><br />INFINITE <br /><br />DAY <br /><br />WEEK <br /><br />MONTH <br /><br />YEAR |  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne toutes les tables utilisateur qui ne possèdent pas de clé primaire.  
  
```  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name   
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasPrimaryKey') = 0  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
L’exemple suivant montre comment connexes des données temporelles peuvent être exposées.  
   
**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].
  
```  
SELECT T1.object_id, T1.name as TemporalTableName, SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,  
T2.name as HistoryTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,  
T1.temporal_type_desc  
FROM sys.tables T1  
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id  
ORDER BY T1.temporal_type desc  
```  

L’exemple suivant montre comment les informations sur la rétention d’historique temporelle peuvent être exposées.  

**S'applique à**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].  
  
```  
SELECT DB.is_temporal_history_retention_enabled, SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema, 
T1.name as TemporalTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema, T2.name as HistoryTableName,
T1.history_retention_period, T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases where name = DB_NAME()) DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2 
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [DBCC CHECKDB & #40 ; Transact-SQL & #41 ;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)   
 [Interrogation des catalogues système SQL Server FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [OLTP en mémoire &#40;Optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
