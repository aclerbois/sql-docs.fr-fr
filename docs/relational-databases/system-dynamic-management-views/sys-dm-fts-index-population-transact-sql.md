---
title: Sys.dm_fts_index_population (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_index_population
- dm_fts_index_population
- sys.dm_fts_index_population_TSQL
- dm_fts_index_population_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_population dynamic management view
ms.assetid: 82d1c102-efcc-4b60-9a5e-3eee299bcb2b
caps.latest.revision: 38
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d82b044186f61ff09abdf3b0a31766e03f36dbcf
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmftsindexpopulation-transact-sql"></a>sys.dm_fts_index_population (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne des informations sur les remplissages d'index de recherche en texte intégral et d'expression de clé sémantique actuellement en cours dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de la base de données qui contient l'index de texte intégral en cours de remplissage.|  
|**catalog_id**|**int**|ID du catalogue de texte intégral qui contient cet index de texte intégral.|  
|**table_id**|**int**|ID de la table pour laquelle l'index de recherche en texte intégral est rempli.|  
|**memory_address**|**varbinary(8)**|Adresse mémoire de la structure des données internes utilisées pour représenter un remplissage actif.|  
|**population_type**|**int**|Type de remplissage. Il peut s'agir :<br /><br /> 1 = Remplissage complet<br /><br /> 2 = Remplissage incrémentiel avec cachet temporel<br /><br /> 3 = Mise à jour manuelle des modifications suivies<br /><br /> 4 = Mise à jour en arrière-plan des modifications suivies|  
|**population_type_description**|**nvarchar(120)**|Description du type de remplissage.|  
|**is_clustered_index_scan**|**bit**|Indique si le remplissage implique une analyse sur l'index cluster.|  
|**range_count**|**int**|Nombre de sous-plages dans lesquelles ce remplissage a été mis en parallèle.|  
|**completed_range_count**|**int**|Nombre de plages pour lesquelles le traitement est terminé.|  
|**outstanding_batch_count**|**int**|Nombre actuel de lots en attente pour ce remplissage. Pour plus d’informations, consultez [sys.dm_fts_outstanding_batches &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md).|  
|**status**|**int**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> État de ce remplissage. Remarque : certains états sont transitoires. Il peut s'agir :<br /><br /> 3 = Démarrage<br /><br /> 5 = Traitement normal<br /><br /> 7 = A arrêté le traitement<br /><br /> Par exemple, cet état se produit lorsqu'une fusion automatique est en cours.<br /><br /> 11 = Remplissage abandonné<br /><br /> 12 = Traiter une extraction de ressemblance sémantique|  
|**status_description**|**nvarchar(120)**|Description de l'état du remplissage.|  
|**completion_type**|**int**|État de la manière dont ce remplissage s'est terminé.|  
|**completion_type_description**|**nvarchar(120)**|Description du type d'achèvement.|  
|**worker_count**|**int**|Cette valeur est toujours 0.|  
|**queued_population_type**|**int**|Type du remplissage, d'après les modifications suivies, que suivra l'éventuel remplissage en cours.|  
|**queued_population_type_description**|**nvarchar(120)**|Description du remplissage à suivre, le cas échéant. Par exemple, lorsque CHANGE TRACKING = AUTO et que le remplissage complet initial est en cours, cette colonne affiche « Remplissage automatique ».|  
|**start_time**|**datetime**|Heure de début du remplissage.|  
|**incremental_timestamp**|**timestamp**|Représente le cachet temporel de départ d'un remplissage complet. Pour tous les autres types de remplissage, cette valeur est le dernier point de contrôle validé représentant la progression des remplissages.|  
  
## <a name="remarks"></a>Notes  
 Lorsque l'indexation sémantique statistique est activée en plus de l'indexation de texte intégral, l'extraction et le remplissage sémantique d'expressions clés et l'extraction de données de ressemblance du document se produisent simultanément avec l'indexation de texte intégral. Le remplissage de l'index de ressemblance du document se produit ultérieurement dans une deuxième phase. Pour plus d’informations, consultez [gérer et surveiller la recherche sémantique](../../relational-databases/search/manage-and-monitor-semantic-search.md).  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
  
## <a name="physical-joins"></a>Jointures physiques  
 ![Jointures significatives de cette vue de gestion dynamique](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-index-population-1.gif "jointures significatives de cette vue de gestion dynamique")  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|From|Pour|Relation|  
|----------|--------|------------------|  
|dm_fts_active_catalogs.database_id|dm_fts_index_population.database_id|Un à un|  
|dm_fts_active_catalogs.catalog_id|dm_fts_index_population.catalog_id|Un à un|  
|dm_fts_population_ranges.parent_memory_address|dm_fts_index_population.memory_address|Plusieurs-à-un|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Recherche en texte intégral et les fonctions et vues de gestion dynamique de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  

