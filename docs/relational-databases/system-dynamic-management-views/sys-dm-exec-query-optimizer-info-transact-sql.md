---
title: Sys.dm_exec_query_optimizer_info (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_exec_query_optimizer_info_TSQL
- dm_exec_query_optimizer_info
- sys.dm_exec_query_optimizer_info_TSQL
- sys.dm_exec_query_optimizer_info
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_optimizer_info dynamic management view
ms.assetid: 1d72cef1-22d8-4ae0-91db-6694fe918c9e
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 5779c87d467a52e28623419d6cf6bac392907fae
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecqueryoptimizerinfo-transact-sql"></a>sys.dm_exec_query_optimizer_info (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne des statistiques détaillées sur le fonctionnement de l'optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez utiliser cette vue lorsque vous paramétrez une charge de travail pour identifier des problèmes ou des améliorations d'optimisation des requêtes. Par exemple, vous pouvez utiliser le nombre total des optimisations, la valeur du temps écoulé et la valeur de coût final pour comparer les optimisations de requête de la charge en cours et les modifications observées au cours du processus de paramétrage. Certains compteurs fournissent des données qui s'appliquent uniquement à l'usage interne du diagnostic [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ces compteurs indiquent la mention « Interne uniquement ».  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_exec_query_optimizer_info**.  
  
|Nom|Type de données| Description|  
|----------|---------------|-----------------|  
|**counter**|**nvarchar(4000)**|Nom de l'événement statistique de l'optimiseur.|  
|**occurrence**|**bigint**|Nombre d'occurrences de l'événement d'optimisation pour ce compteur.|  
|**valeur**|**float**|Valeur moyenne de la propriété par occurrence de l'événement.|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
    
## <a name="remarks"></a>Notes  
 **Sys.dm_exec_query_optimizer_info** contient les propriétés suivantes (compteurs). Toutes les valeurs d'occurrence sont cumulatives et sont définies à 0 au redémarrage du système. Tous les champs de valeurs sont initialisés à NULL au redémarrage du système. Toutes les colonnes de valeurs qui indiquent une moyenne utilisent la valeur d'occurrence de la ligne comme dénominateur pour le calcul de la moyenne. Toutes les optimisations de requêtes sont mesurées lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] détermine les modifications de **dm_exec_query_optimizer_info**, y compris les deux requêtes générées par l’utilisateur et système. L’exécution d’un plan déjà mis en cache ne modifie pas les valeurs de **dm_exec_query_optimizer_info**, seules les optimisations sont significatifs.  
  
|Compteur|Occurrence|Valeur|  
|-------------|----------------|-----------|  
|optimizations|Nombre total d'optimisations.|Non applicable|  
|elapsed time|Nombre total d'optimisations.|Temps moyen écoulé par optimisation d'une instruction (requête) individuelle, en secondes.|  
|final cost|Nombre total d'optimisations.|Estimation du coût moyen d'un plan optimisé en unités de coût internes.|  
|trivial plan|Interne uniquement|Interne uniquement|  
|tâches|Interne uniquement|Interne uniquement|  
|no plan|Interne uniquement|Interne uniquement|  
|search 0|Interne uniquement|Interne uniquement|  
|search 0 time|Interne uniquement|Interne uniquement|  
|search 0 tasks|Interne uniquement|Interne uniquement|  
|search 1|Interne uniquement|Interne uniquement|  
|search 1 time|Interne uniquement|Interne uniquement|  
|search 1 tasks|Interne uniquement|Interne uniquement|  
|search 2|Interne uniquement|Interne uniquement|  
|search 2 time|Interne uniquement|Interne uniquement|  
|search 2 tasks|Interne uniquement|Interne uniquement|  
|gain stage 0 to stage 1|Interne uniquement|Interne uniquement|  
|gain stage 1 to stage 2|Interne uniquement|Interne uniquement|  
|délai d'expiration|Interne uniquement|Interne uniquement|  
|memory limit exceeded|Interne uniquement|Interne uniquement|  
|insert stmt|Nombre d'optimisations destinées à des instructions INSERT.|Non applicable|  
|delete stmt|Nombre d'optimisations destinées à des instructions DELETE.|Non applicable|  
|update stmt|Nombre d'optimisations destinées à des instructions UPDATE.|Non applicable|  
|contains subquery|Nombre d'optimisations associées à une requête qui contient au moins une sous-requête.|Non applicable|  
|unnest failed|Interne uniquement|Interne uniquement|  
|tables|Nombre total d'optimisations.|Nombre moyen de tables référencées par requête optimisée.|  
|indications|Nombre de définitions d'un certain indicateur. Les indicateurs pris en charge sont : les indicateurs de requête JOIN, GROUP, UNION et FORCE ORDER, l'option de configuration FORCE PLAN et les indicateurs de jointure.|Non applicable|  
|indicateur de commande|Nombre de définitions d'un indicateur de commande forcée.|Non applicable|  
|indicateur de jointure|Nombre de fois que l'algorithme de jointure a été forcé par un indicateur de jointure.|Non applicable|  
|view reference|Nombre de fois qu'une vue a été référencée dans une requête|Non applicable|  
|requête distante|Nombre d'optimisations dans lesquelles la requête faisait référence à au moins une source de données distante, par exemple une table dont le nom est en quatre parties ou un jeu de résultats OPENROWSET.|Non applicable|  
|maximum DOP|Nombre total d'optimisations.|Valeur moyenne réelle de MAXDOP pour un plan optimisé. Par défaut, réelle de MAXDOP est déterminée par le **degré maximal de parallélisme** configuration du serveur d’option et peuvent être remplacés pour une requête spécifique par la valeur de l’indicateur de requête MAXDOP.|  
|maximum recursion level|Nombre d'optimisations dans lesquelles un niveau MAXRECURSION supérieur à 0 a été spécifié à l'aide de l'indicateur de requête.|Niveau MAXRECURSION moyen dans les optimisations où un niveau de récursivité maximum est spécifié à l'aide de l'indicateur de requête.|  
|indexed views loaded|Interne uniquement|Interne uniquement|  
|indexed views matched|Nombre d'optimisations où une ou plusieurs vues indexées ont été trouvées.|Nombre moyen de vues mises en correspondance.|  
|indexed views used|Nombre d'optimisations où une ou plusieurs vues indexées sont utilisées dans le plan de sortie après avoir trouvé leur correspondance.|Nombre moyen de vues utilisées.|  
|indexed views updated|Nombre d'optimisations d'une instruction DML produisant un plan qui tient à jour une ou plusieurs vues indexées.|Nombre moyen de vues tenues à jour.|  
|dynamic cursor request|Nombre d'optimisations où une demande de curseur dynamique a été spécifiée.|Non applicable|  
|fast forward cursor request|Nombre d'optimisations où une demande de curseur vers l'avant a été spécifiée.|Non applicable|  
|merge stmt|Nombre d'optimisations destinées à des instructions MERGE.|Non applicable|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-viewing-statistics-on-optimizer-execution"></a>A. Affichage de statistiques sur l'exécution de l'optimiseur  
 Quelles sont les statistiques d'exécution de l'optimiseur actuelles pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ?  
  
```  
SELECT * FROM sys.dm_exec_query_optimizer_info;  
```  
  
### <a name="b-viewing-the-total-number-of-optimizations"></a>B. Affichage du nombre total d'optimisations  
 Quel est le nombre d'optimisations effectué ?  
  
```  
SELECT occurrence AS Optimizations FROM sys.dm_exec_query_optimizer_info  
WHERE counter = 'optimizations';  
```  
  
### <a name="c-average-elapsed-time-per-optimization"></a>C. Temps moyen écoulé par optimisation  
 Quel est le temps moyen consacré à chaque optimisation ?  
  
```  
SELECT ISNULL(value,0.0) AS ElapsedTimePerOptimization  
FROM sys.dm_exec_query_optimizer_info WHERE counter = 'elapsed time';  
```  
  
### <a name="d-fraction-of-optimizations-that-involve-subqueries"></a>D. Proportion des optimisations qui impliquent des sous-requêtes  
 Quelle est la proportion des requêtes optimisées qui contenaient une sous-requête ?  
  
```  
SELECT (SELECT CAST (occurrence AS float) FROM sys.dm_exec_query_optimizer_info WHERE counter = 'contains subquery') /  
       (SELECT CAST (occurrence AS float)   
        FROM sys.dm_exec_query_optimizer_info WHERE counter = 'optimizations')  
        AS ContainsSubqueryFraction;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


