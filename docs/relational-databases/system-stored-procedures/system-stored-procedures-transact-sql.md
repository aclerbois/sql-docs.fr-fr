---
title: Procédures système (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 02/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server (starting with 2016 CTP3)
f1_keywords:
- sql13.TSQLSysNoExpandPortal.f1
- sql13.TSQLSysNoExpandPortal.f1_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stored procedures [SQL Server]
- APIs [SQL Server]
- stored procedures [SQL Server], categories
- system stored procedures [SQL Server], categories
- system stored procedures [SQL Server]
ms.assetid: a5c4d5b8-5a24-4a2d-99b4-d003b546ee3a
caps.latest.revision: 49
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 481b0c451f5161231cf64402c5c758870a07be62
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34708637"
---
# <a name="system-stored-procedures-transact-sql"></a>Procédures stockées système (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], de nombreuses tâches d'administration et d'information peuvent être effectuées à l'aide de procédures stockées système. Les procédures stockées système sont regroupées par catégories dans le tableau suivant.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Catégorie|Description|  
|--------------|-----------------|  
|[Géo-réplication Active les procédures stockées](http://msdn.microsoft.com/library/81658ee4-4422-4d73-bf7a-86a07422cb0d)|Utilisé pour gérer les configurations de géo-réplication Active dans la base de données SQL Azure|  
|[Procédures stockées du catalogue](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)|Permettent d'implémenter les fonctions du dictionnaire de données ODBC et d'isoler les applications ODBC des modifications apportées aux tables système concernées.|  
|[Les procédures stockées de Capture de données modifiées](../../relational-databases/system-stored-procedures/change-data-capture-stored-procedures-transact-sql.md)|Permettent d'activer, de désactiver ou de créer des rapports sur des objets de capture des données modifiées.|  
|[Procédures stockées de curseur](../../relational-databases/system-stored-procedures/cursor-stored-procedures-transact-sql.md)|Permettent d'implémenter les fonctionnalités de variable de curseur.|  
|[Procédures stockées du collecteur de données](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)|Utilisé pour fonctionner avec le collecteur de données et les composants suivants : jeux d'éléments de collecte, éléments de collecte et types de collections.|  
|[Procédures stockées du moteur de base de données](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)|Utilisé pour la maintenance générale de la [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].|  
|[Procédures stockées de messagerie de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)|Permettent d'exécuter des opérations de messagerie électronique à partir d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Procédures stockées de Plan de Maintenance de base de données](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)|Permettent de définir les tâches de maintenance principales nécessaires pour gérer les performances des bases de données.|  
|[Procédures stockées de requêtes distribuées](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)|Servent à implémenter et gérer les requêtes distribuées.|  
|[FileStream et procédures stockées FileTable &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/54beca08-c012-4ebd-aa68-d8a10d221b64)|Utilisé pour configurer et gérer les fonctionnalités FILESTREAM et FileTable.|  
|[Procédures stockées de règles de pare-feu &#40;base de données SQL Azure&#41;](../../relational-databases/system-stored-procedures/firewall-rules-stored-procedures-azure-sql-database.md)|Utilisé pour configurer le pare-feu de base de données SQL Azure.|  
|[Procédures stockées de recherche en texte intégral](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)|Utilisées pour implémenter et effectuer les requêtes des index de texte intégral.|  
|[Procédures stockées étendues générales](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)|Permettent de fournir une interface depuis une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aux programmes externes pour plusieurs activités de maintenance.|  
|[Des journaux de procédures stockées](../../relational-databases/system-stored-procedures/log-shipping-stored-procedures-transact-sql.md)|Permettent de configurer, modifier et surveiller les configurations de la copie des journaux de transaction.|  
|[Les procédures stockées de l’entrepôt de données de gestion &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/management-data-warehouse-stored-procedures-transact-sql.md)|Utilisé pour configurer l’entrepôt de données de gestion.|  
|[Procédures stockées OLE Automation](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)|Permettent d'activer des objets Automation standard pour une utilisation dans un lot [!INCLUDE[tsql](../../includes/tsql-md.md)] standard.|  
|[Procédures stockées de gestion basée sur la stratégie](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)|Utilisées pour la Gestion basée sur des stratégies.|  
|[Procédures stockées PolyBase](http://msdn.microsoft.com/library/a522b303-bd1b-410b-92d1-29c950a15ede)|Ajouter ou supprimer un ordinateur d’un groupe de scale-out PolyBase.|  
|[Procédures stockées du magasin de requête &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)|Utilisé pour régler les performances.|  
|[Procédures stockées de réplication](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)|Servent à gérer la réplication.|  
|[Procédures stockées de sécurité](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)|Servent à gérer la sécurité.|  
|[Sauvegarde des procédures stockées de capture instantanée](http://msdn.microsoft.com/library/c278db87-5770-4037-a1e6-b9853a943339)|Utilisé pour supprimer la sauvegarde FILE_SNAPSHOT, ainsi que tous ses instantanés ou pour supprimer un instantané du fichier de sauvegarde individuels.|  
|[Les procédures stockées d’Index spatial](http://msdn.microsoft.com/library/1be0f34e-3d5a-4a1f-9299-bd482362ec7a)|Utilisé pour analyser et améliorer les performances d’indexation des index spatiaux.|  
|[Procédures stockées de l’Agent SQL Server](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)|Utilisées par [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour contrôler les performances et l'activité.|  
|[Procédures stockées SQL Server Profiler](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)|Utilisées par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin de gérer les activités planifiées et liées aux événements.|  
|[Étendre la base de données stockée procédures](../../relational-databases/system-stored-procedures/stretch-database-extended-stored-procedures-transact-sql.md)|Utilisé pour gérer les bases de données stretch.|  
|[Procédures stockées de Tables temporelles](http://msdn.microsoft.com/library/f28ca74e-7876-4592-b794-e78e3690fff6)|Utilisation des tables temporelles|  
|[Procédures stockées XML](../../relational-databases/system-stored-procedures/xml-stored-procedures-transact-sql.md)|Permettent de gérer le texte XML.|  
  
> [!NOTE]  
>  Sauf spécification contraire, toutes les procédures stockées système retournent la valeur 0 pour indiquer la réussite d'une procédure. Pour indiquer un échec, la procédure retourne une valeur différente de zéro.  
  
## <a name="api-system-stored-procedures"></a>Procédures système API  
 Les utilisateurs qui exécutent le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] sur des applications ADO, OLE DB et ODBC peuvent remarquer que celles-ci utilisent des procédures stockées système non abordées dans le manuel de référence [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ces procédures stockées sont utilisées par le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur Native Client OLE DB et le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client à implémenter les fonctionnalités d’une API de base de données. Ces procédures stockées sont simplement le mécanisme utilisé par le fournisseur ou le pilote afin de communiquer les requêtes des utilisateurs à une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elles sont uniquement destinées à l'utilisation interne du fournisseur ou du pilote. Les appeler explicitement à partir d’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-applications n’est pas pris en charge.  
  
 Le stockées sp_createorphan et sp_droporphans procédures sont utilisées pour ODBC **ntext**, **texte**, et **image** de traitement.  
  
 La procédure stockée sp_reset_connection est utilisée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin de prendre en charge les appels de procédure stockée distante dans une transaction. Cette procédure stockée peut aussi déclencher des événements Audit Login et Audit Logout si une connexion est réutilisée dans un regroupement de connexions.  
  
 Les procédures stockées système recensées dans les tableaux suivants sont uniquement utilisées dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou par le biais d'API clientes et ne sont pas destinées à une utilisation générale. Elles peuvent faire l'objet de modifications et la compatibilité n'est pas garantie.  
  
 Les procédures stockées suivantes sont documentées dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|||  
|-|-|  
|sp_catalogs|sp_column_privileges|  
|sp_column_privileges_ex|sp_columns|  
|sp_columns_ex|sp_databases|  
|sp_cursor|sp_cursorclose|  
|sp_cursorexecute|sp_cursorfetch|  
|sp_cursoroption|sp_cursoropen|  
|sp_cursorprepare|sp_cursorprepexec|  
|sp_cursorunprepare|sp_execute|  
|sp_datatype_info|sp_fkeys|  
|sp_foreignkeys|sp_indexes|  
|sp_pkeys|sp_primarykeys|  
|sp_prepare|sp_prepexec|  
|sp_prepexecrpc|sp_unprepare|  
|sp_server_info|sp_special_columns|  
|sp_sproc_columns|sp_statistics|  
|sp_table_privileges|sp_table_privileges_ex|  
|sp_tables|sp_tables_ex|  
  
 Les procédures stockées suivantes ne sont pas documentées :  
  
|||  
|-|-|  
|sp_assemblies_rowset|sp_assemblies_rowset_rmt|  
|sp_assemblies_rowset2|sp_assembly_dependencies_rowset|  
|sp_assembly_dependencies_rowset_rmt|sp_assembly_dependencies_rowset2|  
|sp_bcp_dbcmptlevel|sp_catalogs_rowset|  
|sp_catalogs_rowset;2|sp_catalogs_rowset;5|  
|sp_catalogs_rowset_rmt|sp_catalogs_rowset2|  
|sp_check_constbytable_rowset|sp_check_constbytable_rowset;2|  
|sp_check_constbytable_rowset2|sp_check_constraints_rowset|  
|sp_check_constraints_rowset;2|sp_check_constraints_rowset2|  
|sp_column_privileges_rowset|sp_column_privileges_rowset;2|  
|sp_column_privileges_rowset;5|sp_column_privileges_rowset_rmt|  
|sp_column_privileges_rowset2|sp_columns_90|  
|sp_columns_90_rowset|sp_columns_90_rowset_rmt|  
|sp_columns_90_rowset2|sp_columns_ex_90|  
|sp_columns_rowset|sp_columns_rowset;2|  
|sp_columns_rowset;5|sp_columns_rowset_rmt|  
|sp_columns_rowset2|sp_constr_col_usage_rowset|  
|sp_datatype_info_90|sp_ddopen;1|  
|sp_ddopen;10|sp_ddopen;11|  
|sp_ddopen;12|sp_ddopen;13|  
|sp_ddopen;2|sp_ddopen;3|  
|sp_ddopen;4|sp_ddopen;5|  
|sp_ddopen;6|sp_ddopen;7|  
|sp_ddopen;8|sp_ddopen;9|  
|sp_foreign_keys_rowset|sp_foreign_keys_rowset;2|  
|sp_foreign_keys_rowset;3|sp_foreign_keys_rowset;5|  
|sp_foreign_keys_rowset_rmt|sp_foreign_keys_rowset2|  
|sp_foreign_keys_rowset3|sp_indexes_90_rowset|  
|sp_indexes_90_rowset_rmt|sp_indexes_90_rowset2|  
|sp_indexes_rowset|sp_indexes_rowset;2|  
|sp_indexes_rowset;5|sp_indexes_rowset_rmt|  
|sp_indexes_rowset2|sp_linkedservers_rowset|  
|sp_linkedservers_rowset;2|sp_linkedservers_rowset2|  
|sp_oledb_database|sp_oledb_defdb|  
|sp_oledb_deflang|sp_oledb_language|  
|sp_oledb_ro_usrname|sp_primary_keys_rowset|  
|sp_primary_keys_rowset;2|sp_primary_keys_rowset;3|  
|sp_primary_keys_rowset;5|sp_primary_keys_rowset_rmt|  
|sp_primary_keys_rowset2|sp_procedure_params_90_rowset|  
|sp_procedure_params_90_rowset2|sp_procedure_params_rowset|  
|sp_procedure_params_rowset;2|sp_procedure_params_rowset2|  
|sp_procedures_rowset|sp_procedures_rowset;2|  
|sp_procedures_rowset2|sp_provider_types_90_rowset|  
|sp_provider_types_rowset|sp_schemata_rowset|  
|sp_schemata_rowset;3|sp_special_columns_90|  
|sp_sproc_columns_90|sp_statistics_rowset|  
|sp_statistics_rowset;2|sp_statistics_rowset2|  
|sp_stored_procedures|sp_table_constraints_rowset|  
|sp_table_constraints_rowset;2|sp_table_constraints_rowset2|  
|sp_table_privileges_rowset|sp_table_privileges_rowset;2|  
|sp_table_privileges_rowset;5|sp_table_privileges_rowset_rmt|  
|sp_table_privileges_rowset2|sp_table_statistics_rowset|  
|sp_table_statistics_rowset;2|sp_table_statistics2_rowset|  
|sp_tablecollations|sp_tablecollations_90|  
|sp_tables_info_90_rowset|sp_tables_info_90_rowset_64|  
|sp_tables_info_90_rowset2|sp_tables_info_90_rowset2_64|  
|sp_tables_info_rowset|sp_tables_info_rowset;2|  
|sp_tables_info_rowset_64|sp_tables_info_rowset_64;2|  
|sp_tables_info_rowset2|sp_tables_info_rowset2_64|  
|sp_tables_rowset;2|sp_tables_rowset;5|  
|sp_tables_rowset_rmt|sp_tables_rowset2|  
|sp_usertypes_rowset|sp_usertypes_rowset_rmt|  
|sp_usertypes_rowset2|sp_views_rowset|  
|sp_views_rowset2|sp_xml_schema_rowset|  
|sp_xml_schema_rowset2||  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [Procédures stockées &#40;moteur de base de données&#41;](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)   
 [Exécution de procédures stockées &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/stored-procedures-running.md)   
 [Exécution de procédures stockées](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Exécution de procédures stockées](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)  
  
  
