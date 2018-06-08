---
title: Sys.query_context_settings (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/22/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS
- QUERY_CONTEXT_SETTINGS
dev_langs:
- TSQL
helpviewer_keywords:
- sys.query_context_settings catalog view
ms.assetid: 3c1887df-6bd8-491e-82fc-d25ad9589faf
caps.latest.revision: 16
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 448727aa29d55e0cd41b0859f620ad393b738e09
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysquerycontextsettings-transact-sql"></a>Sys.query_context_settings (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Contient des informations sur la sémantique qui affecte les paramètres de contexte associées à une requête. Il existe un nombre de paramètres de contexte disponibles dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui influencent la sémantique de requête (en définissant le résultat correct de la requête). Le même texte de requête compilé avec différents paramètres peut produire des résultats différents (selon les données sous-jacentes).  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**context_settings_id**|**bigint**|Clé primaire. Cette valeur est exposée dans Showplan XML pour les requêtes.|  
|**set_options**|**varbinary(8)**|Masque de bits refléter l’état de plusieurs options SET. Pour plus d’informations, consultez [sys.dm_exec_plan_attributes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md).|  
|**ID_langue**|**smallint**|Id de la langue. Pour plus d’informations, consultez [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).|  
|**date_format**|**smallint**|Le format de date. Pour plus d’informations, consultez [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md).|  
|**date_first**|**tinyint**|La première valeur de date. Pour plus d’informations, consultez [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md).|  
|**status**|**varbinary(2)**|Champ de masque de bits qui indique le type de requête ou le contexte dans lequel la requête a été exécutée. <br />Valeur de la colonne peut être de combinaison de plusieurs indicateurs (exprimée au format hexadécimal) :<br /><br /> 0 x 0 – requête standard (sans indicateurs spécifiques)<br /><br /> 0 x 1 - requête a été exécutée par l’intermédiaire d’une des procédures stockées des API de curseur<br /><br /> 0 x 2 : requête de notification<br /><br /> 0 x 4 – requête interne<br /><br /> 0 x 8 – automatique de la requête paramétrable sans paramétrage universel<br /><br /> extraction de curseur 0 x 10 – actualiser la requête<br /><br /> 0 x 20 - requête qui est utilisé dans les demandes de mise à jour de curseur<br /><br /> 0 x 40 - jeu de résultats initial est retournée lorsqu’un curseur est ouvert (Fetch de curseur automatique)<br /><br /> 0 x 80 – requête chiffrée<br /><br /> 0 x 100 – requête dans le contexte du prédicat de sécurité de niveau ligne|  
|**required_cursor_options**|**int**|Options de curseur spécifiées par l'utilisateur (type de curseur par exemple).|  
|**acceptable_cursor_options**|**int**|Options de curseur dans lesquelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut convertir implicitement afin de prendre en charge l'exécution de l'instruction.|  
|**merge_action_type**|**smallint**|Le type de plan d’exécution de déclencheur utilisé comme le résultat d’une **fusion** instruction.<br /><br /> 0 indique un plan de non-déclencheur, un plan de déclencheur qui ne s’exécute en tant que le résultat de l’un **fusion** instruction ou un plan de déclencheur qui s’exécute en tant que le résultat d’un **fusion** instruction qui spécifie uniquement un **supprimer** action.<br /><br /> 1 indique une **insérer** plan de déclencheur qui s’exécute en tant que le résultat d’une **fusion** instruction.<br /><br /> 2 indique un **mise à jour** plan de déclencheur qui s’exécute en tant que le résultat d’une **fusion** instruction.<br /><br /> 3 indique un **supprimer** plan de déclencheur qui s’exécute en tant que le résultat d’une **fusion** une instruction contenant un correspondant **insérer** ou **mise à jour** action.<br /><br /> <br /><br /> Pour les déclencheurs imbriqués exécutés par des actions en cascade, cette valeur est l’action de la **fusion** instruction qui a provoqué la cascade.|  
|**default_schema_id**|**int**|ID du schéma par défaut, qui est utilisé pour résoudre les noms qui ne sont pas qualifiées.|  
|**is_replication_specific**|**bit**|Utilisé pour la réplication.|  
|**is_contained**|**varbinary(1)**|1 indique une relation contenant-contenu de la base de données.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert le **VIEW DATABASE STATE** autorisation.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [Sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [Sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)   
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Procédures stockées du magasin de requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  
