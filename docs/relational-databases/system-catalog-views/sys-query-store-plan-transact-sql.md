---
title: Sys.query_store_plan (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- QUERY_STORE_PLAN_TSQL
- SYS.QUERY_STORE_PLAN
- SYS.QUERY_STORE_PLAN_TSQL
- QUERY_STORE_PLAN
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_plan catalog view
- sys.query_store_plan catalog view
ms.assetid: b4d05439-6360-45db-b1cd-794f4a64935e
caps.latest.revision: 18
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 8883fb4fd7f70712f8e71ca9015380fd5c81ca93
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysquerystoreplan-transact-sql"></a>Sys.query_store_plan (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Contient des informations sur chaque plan d’exécution associé à une requête.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**plan_id**|**bigint**|Clé primaire.|  
|**query_id**|**bigint**|Clé étrangère. Joint à [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md).|  
|**plan_group_id**|**bigint**|ID du groupe de plan. Les requêtes de curseur nécessitent généralement plusieurs (remplir et fetch) plans. Remplir et les plans de récupération qui sont compilés ensemble sont dans le même groupe.<br /><br /> 0 signifie le plan n’est pas dans un groupe.|  
|**ENGINE_VERSION**|**nvarchar(32)**|Version du moteur permet de compiler le plan dans **'major.minor.build.revision'** format.|  
|**compatibility_level**|**smallint**|Niveau de compatibilité de base de données de la base de données référencée dans la requête.|  
|**query_plan_hash**|**binary(8)**|Hachage MD5 du plan individuel.|  
|**query_plan**|**nvarchar(max)**|Showplan XML pour le plan de requête.|  
|**is_online_index_plan**|**bit**|Plan a été utilisé pendant la création d’un index en ligne.|  
|**is_trivial_plan**|**bit**|Plan est un plan trivial (sortie dans l’étape 0 de l’optimiseur de requête).|  
|**is_parallel_plan**|**bit**|Plan est parallèle.|  
|**is_forced_plan**|**bit**|Plan est marqué comme étant forcé lorsque l’utilisateur exécute la procédure stockée **sys.sp_query_store_force_plan**. Le mécanisme de forçage *ne garantit pas* qu’exactement ce plan doit être utilisé pour la requête référencée par **query_id**. Le forçage de plan entraîne la requête doit être compilé à nouveau et produit en général exactement le plan identiques ou similaire au plan référencé par **plan_id**. Si le forçage de plan n’aboutit pas, **force_failure_count** est incrémenté et **last_force_failure_reason** est remplie avec la raison de l’échec.|  
|**is_natively_compiled**|**bit**|Plan comprend des procédures compilées en mode natif mémoire optimisée. (0 = FALSE, 1 = TRUE).|  
|**force_failure_count**|**bigint**|Nombre de fois forcer ce plan a échoué. Il peut être incrémenté uniquement lorsque la requête est recompilée (*pas à chaque exécution*). Il est réinitialisé à 0 à chaque fois **is_plan_forced** est remplacée par **FALSE** à **TRUE**.|  
|**last_force_failure_reason**|**int**|Motif d’échec de l’application forcée du plan.<br /><br /> 0 : aucune panne, le numéro d’erreur dans le cas contraire de l’erreur qui a provoqué l’échec forcé<br /><br /> 8637 : ONLINE_INDEX_BUILD<br /><br /> 8683 : INVALID_STARJOIN<br /><br /> 8684 : DÉLAI_ATTENTE<br /><br /> 8689 : NO_DB<br /><br /> 8690 : HINT_CONFLICT<br /><br /> 8691 : SETOPT_CONFLICT<br /><br /> 8694 : DQ_NO_FORCING_SUPPORTED<br /><br /> 8698 : NO_PLAN<br /><br /> 8712 : NO_INDEX<br /><br /> 8713 : VIEW_COMPILE_FAILED<br /><br /> \<autre valeur > : GENERAL_FAILURE|  
|**last_force_failure_reason_desc**|**nvarchar(128)**|Description textuelle du last_force_failure_reason_desc.<br /><br /> ONLINE_INDEX_BUILD : requête tente de modifier les données alors que la table cible a un index qui est en cours de création en ligne<br /><br /> INVALID_STARJOIN : plan contient la spécification de StarJoin non valide<br /><br /> Délai_attente : Nombre d’optimiseur a dépassé d’opérations autorisées lors de la recherche du plan spécifié par le plan forcé<br /><br /> NO_DB : Une base de données spécifiée dans le plan n’existe pas<br /><br /> HINT_CONFLICT : Impossible de compiler la requête, car le plan est en conflit avec un indicateur de requête<br /><br /> DQ_NO_FORCING_SUPPORTED : Ne peut pas exécuter la requête, car le plan est en conflit avec l’utilisation d’une requête distribuée ou d’opérations de recherche en texte intégral.<br /><br /> NO_PLAN : Processeur de requêtes pas pu créer le plan de requête car le plan forcé n’a pas pu être vérifié pour être valide pour la requête<br /><br /> No_index : Il existe de l’Index spécifié dans le plan n’est plus<br /><br /> VIEW_COMPILE_FAILED : Impossible de forcer le plan de requête en raison d’un problème dans une vue indexée référencée dans le plan<br /><br /> GENERAL_FAILURE : erreur générale forcé (non traitée avec des motifs ci-dessus)|  
|**count_compiles**|**bigint**|Planifiez les statistiques de compilation.|  
|**initial_compile_start_time**|**datetimeoffset**|Planifiez les statistiques de compilation.|  
|**last_compile_start_time**|**datetimeoffset**|Planifiez les statistiques de compilation.|  
|**last_execution_time**|**datetimeoffset**|Dernière heure d’exécution fait référence à la dernière heure de fin de la requête/plan.|  
|**avg_compile_duration**|**float**|Planifiez les statistiques de compilation.|  
|**last_compile_duration**|**bigint**|Planifiez les statistiques de compilation.|  
  
## <a name="plan-forcing-limitations"></a>Limitations de forçage de plan
Le Magasin des requêtes a un mécanisme qui permet de forcer l’optimiseur de requête à utiliser un certain plan d’exécution. Toutefois, il existe certaines limitations qui peuvent empêcher l’application d’un plan. 

Premièrement, si le plan contient les constructions suivantes :
* Instruction Insert bulk.
* Instruction Insert bulk.
* Référence à une table externe
* Requête distribuée ou opérations de recherche en texte intégral
* Utilisation de requêtes globales 
* Curseurs
* Spécification de jointure en étoile non valide 

Deuxièmement, quand les objets sur lesquels s’appuie le plan ne sont plus disponibles :
* Base de données (si la base de données d'où provient le plan n’existe plus)
* Index (absent ou désactivé)

Enfin, s’il y a des problèmes avec le plan lui-même :
* Non conforme pour la requête
* L’optimiseur de requête a dépassé le nombre d’opérations autorisées
* Code XML du plan incorrect

## <a name="permissions"></a>Autorisations  
 Requiert le **VIEW DATABASE STATE** autorisation.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [Sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Procédures stockées du magasin de requête &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
  
  
