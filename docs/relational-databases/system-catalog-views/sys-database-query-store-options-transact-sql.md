---
title: Sys.database_query_store_options (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DATABASE_QUERY_STORE_OPTIONS_TSQL
- DATABASE_QUERY_STORE_OPTIONS
- SYS.DATABASE_QUERY_STORE_OPTIONS_TSQL
- SYS.DATABASE_QUERY_STORE_OPTIONS
dev_langs:
- TSQL
helpviewer_keywords:
- database_query_store_options catalog view
- sys.database_query_store_options catalog view
ms.assetid: 16b47d55-8019-41ff-ad34-1e0112178067
caps.latest.revision: 24
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a9ce2b5f63405a0754782e0dddae5584c1b47ee2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysdatabasequerystoreoptions-transact-sql"></a>Sys.database_query_store_options (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Renvoie les options de magasin de requêtes pour cette base de données.  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**desired_state**|**smallint**|Indique le mode de fonctionnement souhaité du magasin de requêtes, définie par l’utilisateur.<br /> 0 = désactivé <br /> 1 = READ_ONLY<br /> 2 = EN LECTURE/ÉCRITURE|  
|**desired_state_desc**|**nvarchar(64)**|Description textuelle du mode de fonctionnement souhaité du magasin de requêtes :<br />OFF<br />READ_ONLY<br />READ_WRITE|  
|**actual_state**|**smallint**|Indique le mode d’opération du magasin de requêtes. En plus de la liste des États souhaités requis par l’utilisateur, l’état réel peut être un état d’erreur.<br /> 0 = désactivé <br /> 1 = READ_ONLY<br /> 2 = EN LECTURE/ÉCRITURE<br /> 3 = ERREUR|  
|**actual_state_desc**|**nvarchar(64)**|Description textuelle du mode de fonctionnement réel du magasin de requêtes.<br />OFF<br />READ_ONLY<br />READ_WRITE<br />ERROR<br /><br /> Il existe des situations lorsque l’état effectif est différent de l’état souhaité :<br /><br /> Magasin de requêtes peuvent fonctionner en mode lecture seule, même si en lecture-écriture a été spécifié par l’utilisateur. Par exemple, qui peut se produire si la base de données est en mode lecture seule ou si la taille du magasin de requête a dépassé le quota.<br /><br /> Très rarement, le magasin de requêtes peut finir dans un état d’erreur en raison d’erreurs internes. Si cela se produit, magasin de requêtes peut être récupéré en exécutant **sp_query_store_consistency_check** une procédure stockée dans la base de données concernée.|  
|**readonly_reason**|**int**|Lorsque le **desired_state_desc** est en lecture/écriture et la **actual_state_desc** est en lecture seule, **readonly_reason** retourne mapper un bit pour indiquer la raison pour laquelle le magasin de requêtes est en mode lecture seule.<br /><br /> 1 – base de données est en mode lecture seule<br /><br /> 2 – base de données est en mode mono-utilisateur<br /><br /> 4 – base de données est en mode d’urgence<br /><br /> 8 – base de données est le réplica secondaire (s’applique à Always On et Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)] géo-réplication). Cette valeur peut être observée efficacement uniquement sur **lisible** réplicas secondaires<br /><br /> 65536 : le magasin de requêtes a atteint la limite de taille définie par l’option MAX_STORAGE_SIZE_MB.<br /><br /> 131072 - le nombre d’instructions différents dans le magasin de requêtes a atteint la limite de mémoire interne. Envisagez de requêtes que vous n’avez pas besoin de suppression ou mise à niveau vers un niveau de service plus élevé pour activer le transfert de magasin de requêtes en mode lecture-écriture.<br />S'applique uniquement à [!INCLUDE[ssSDS](../../includes/sssds-md.md)].<br /><br /> 262144 – la taille des éléments en mémoire en attente d’être rendues persistantes sur disque a atteint la limite de mémoire interne. Magasin de requêtes sera en mode lecture seule jusqu'à ce que les éléments en mémoire sont conservées sur le disque. <br />S'applique uniquement à [!INCLUDE[ssSDS](../../includes/sssds-md.md)].<br /><br />524288 – base de données a atteint la limite de taille de disque. Magasin de requêtes fait partie de base de données utilisateur, et si l’espace n’est disponible pour une base de données, ce qui signifie que le magasin de requêtes ne peut pas croître plus.<br />S'applique uniquement à [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. <br /> <br /> Pour basculer les opérations de magasin de requêtes arrière en mode lecture-écriture, consultez **est de vérifier le magasin de requêtes collecte les données de requête en continu** section de [meilleure pratique au magasin de requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md).|  
|**current_storage_size_mb**|**bigint**|Taille du magasin de requêtes sur le disque en mégaoctets.|  
|**flush_interval_seconds**|**bigint**|Définit la période pour le vidage régulière des données de magasin de requêtes sur le disque. Valeur par défaut est 900 (15 min).<br /><br /> Modification à l’aide de la `ALTER DATABASE <database> SET QUERY_STORE (DATA_FLUSH_INTERVAL_SECONDS  = <interval>)` instruction.|  
|**interval_length_minutes**|**bigint**|L’intervalle d’agrégation de statistiques. Les valeurs arbitraires ne sont pas autorisés. Utilisez une des opérations suivantes : 1, 5, 10, 15, 30, 60 et 1440 minutes. La valeur par défaut est 60 minutes.|  
|**max_storage_size_mb**|**bigint**|Taille maximale de disque pour le magasin de requêtes. Valeur par défaut est de 100 Mo.<br />Pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] édition Premium, par la valeur par défaut est de 1 Go et pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] édition De base, elle est de 10 Mo.<br /><br /> Modification à l’aide de la `ALTER DATABASE <database> SET QUERY_STORE (MAX_STORAGE_SIZE_MB = <size>)` instruction.|  
|**stale_query_threshold_days**|**bigint**|Nombre de jours pendant lesquels les requêtes avec aucun paramètre de stratégie sont conservés dans le magasin de requêtes. Valeur par défaut est 30. La valeur 0 pour désactiver la stratégie de rétention.<br />Pour l’édition [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] de base, la valeur par défaut est 7 jours.<br /><br /> Modification à l’aide de la `ALTER DATABASE <database> SET QUERY_STORE ( CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = <value> ) )` instruction.|  
|**max_plans_per_query**|**bigint**|Limite le nombre maximal de plans stockées. Valeur par défaut est 200. Si la valeur maximale est atteinte, le magasin de requêtes cesse de capturer les nouveaux plans de cette requête. Valeur 0 au paramètre de supprimer la limitation en ce qui concerne le nombre de plans capturées.<br /><br /> Modification à l’aide de la `ALTER DATABASE<database> SET QUERY_STORE (MAX_PLANS_PER_QUERY = <n>)` instruction.|  
|**query_capture_mode**|**smallint**|Le mode de capture de requête actuellement active :<br /><br /> 1 = ALL - toutes les requêtes sont capturées. C’est la valeur de la configuration par défaut pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]).<br /><br /> 2 = AUTO - capture les requêtes appropriées en fonction de l’exécution du nombre et consommation de ressources. C’est la valeur de la configuration par défaut pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].<br /><br /> 3 = aucune - cesser de capturer les nouvelles requêtes. Le magasin de requêtes continuera à recueillir des statistiques de compilation et d’exécution pour les requêtes qui ont déjà été capturées. Utilisez cette configuration avec précaution, car vous risquez de manquer pour capturer les requêtes importantes.|  
|**query_capture_mode_desc**|**nvarchar(60)**|Description textuelle du mode de capture du magasin de requêtes :<br /><br /> TOUS (valeur par défaut pour [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)])<br /><br /> AUTOMATIQUE (valeur par défaut pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)])<br /><br /> Aucune|  
|**size_based_cleanup_mode**|**smallint**|Contrôle si le nettoyage est activé automatiquement quand la quantité totale de données approche de la taille maximale :<br /><br /> 1 = OFF : taille en fonction de nettoyage n’est pas activé automatiquement.<br /><br /> 2 = AUTO - taille en fonction de nettoyage est automatiquement activé lors de la taille sur disque atteint 90 % de **max_storage_size_mb**. Il s’agit de la valeur de configuration par défaut.<br /><br />Le nettoyage basée sur la taille supprime les requêtes les moins coûteuses et les plus anciennes en premier. Il s’arrête à 80 % environ de max_storage_size_mb.|  
|**size_based_cleanup_mode_desc**|**smallint**|Description textuelle du mode de nettoyage basée sur la taille du magasin de requêtes :<br /><br /> OFF <br /><br /> AUTOMATIQUE (par défaut)|  
|**wait_stats_capture_mode**|**smallint**|Contrôle si le magasin de requêtes exécute la capture de statistiques d’attente : <br /><br /> 0 = désactivé <br /><br /> 1 = activé<br /> **S'applique à**: [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|
|**wait_stats_mode_capture_desc**|**nvarchar(60)**|Description textuelle du mode de capture de statistiques attente réel : <br /><br /> OFF <br /><br /> (Par défaut)<br /> **S'applique à**: [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].| 
  
## <a name="permissions"></a>Autorisations  
 Requiert le **VIEW DATABASE STATE** autorisation.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [Sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)   
 [Procédures stockées du magasin de requête &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
  
  
