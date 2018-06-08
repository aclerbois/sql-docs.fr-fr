---
title: Sys.dm_exec_cached_plans (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_cached_plans
- dm_exec_cached_plans
- dm_exec_cached_plans_TSQL
- sys.dm_exec_cached_plans_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_cached_plans dynamic management view
ms.assetid: 95b707d3-3a93-407f-8e88-4515d4f2039d
caps.latest.revision: 44
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f181f3d3aaea01de64dd053c82eee4006d371139
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexeccachedplans-transact-sql"></a>sys.dm_exec_cached_plans (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque plan de requête qui est mis en cache par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour une exécution plus rapide. Vous pouvez faire appel à cette vue de gestion dynamique pour rechercher des plans de requête en cache, du texte de requête en cache, la quantité de mémoire occupée par les plans en cache et le nombre de réutilisations de ces plans.  
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d'exposer ces informations, chaque ligne contenant des données qui n'appartient pas au locataire connecté est filtrée. En outre, les valeurs dans les colonnes **memory_object_address** et **pool_id** sont filtrées ; la valeur de colonne est définie avec la valeur NULL.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_exec_cached_plans**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|bucketid|**int**|ID du compartiment de hachage dans lequel l'entrée est en cache. La valeur indique une plage comprise entre 0 et la taille de la table de hachage pour le type du cache.<br /><br /> Pour les caches Plans d'objets et Plans SQL, la taille de la table de hachage peut atteindre 10 007 sur des systèmes 32 bits et 40 009 sur des systèmes 64 bits. Pour le cache Arborescences liées, la taille de la table de hachage peut faire jusqu'à 1 009 sur des systèmes 32 bits et 4 001 sur des systèmes 64 bits. Pour le cache Procédures stockées étendues, la taille de la table de hachage peut faire jusqu'à 127 sur des systèmes 32 bits et 64 bits.|  
|refcounts|**int**|Nombre d'objets du cache faisant référence à cet objet. **RefCounts** doit être au moins 1 pour une entrée dans le cache.|  
|usecounts|**int**|Nombre de fois que l'objet du cache a été trouvé. Non incrémenté lorsque les requêtes paramétrables recherchent un plan dans le cache. Peut être incrémenté plusieurs fois lors de l'utilisation du plan d'exécution de requêtes.|  
|size_in_bytes|**int**|Nombre d'octets mobilisés par l'objet dans le cache.|  
|memory_object_address|**varbinary(8)**|Adresse mémoire de l'entrée en cache. Cette valeur peut être utilisée avec [sys.dm_os_memory_objects](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md) pour obtenir la répartition mémoire du plan mis en cache et avec [sys.dm_os_memory_cache_entries](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-entries-transact-sql.md)entrées pour obtenir le coût de mise en cache de l’entrée.|  
|cacheobjtype|**nvarchar(34)**|Type d'objet dans le cache. Il peut s'agir de l'une des valeurs suivantes :<br /><br /> Compiled Plan (plan compilé)<br /><br /> Compiled Plan Stub (stub du plan compilé)<br /><br /> Parse Tree (arborescence d'analyse)<br /><br /> Extended Proc (procédure étendue)<br /><br /> CLR Compiled Func (fonction compilée CLR)<br /><br /> CLR Compiled Proc (procédure compilée CLR)|  
|objtype|**nvarchar(16)**|Type d'objet. Vous trouverez ci-dessous les valeurs possibles et leurs descriptions correspondantes.<br /><br /> Traitement : Procédure stockée<br />Préparée : L’instruction préparée<br />Adhoc : de requête Ad hoc. Fait référence à [!INCLUDE[tsql](../../includes/tsql-md.md)] soumis en tant qu’événements de langage à l’aide de **osql** ou **sqlcmd** au lieu d’appels de procédure distante.<br />ReplProc : Filtre de procédure de réplication<br />Déclencheur : déclencheur<br />Vue : vue<br />Par défaut : par défaut<br />UsrTab : Table d’utilisateur<br />SysTab : (Table système)<br />Validation : Contrainte de vérification<br />Règle : règle|  
|plan_handle|**varbinary(64)**|Identificateur du plan en mémoire. Cet identificateur est temporaire et il reste constant uniquement tant que le plan est dans le cache. Cette valeur peut être utilisée avec les fonctions de gestion dynamique suivantes :<br /><br /> [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)<br /><br /> [sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)<br /><br /> [sys.dm_exec_plan_attributes](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md)|  
|pool_id|**int**|ID du pool de ressources par rapport auquel cette utilisation de la mémoire de plan est prise en compte.|  
|pdw_node_id|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
 <sup>1</sup>  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-batch-text-of-cached-entries-that-are-reused"></a>A. Retour du texte du traitement des entrées mises en cache qui sont réutilisées  
 L'exemple suivant retourne le texte SQL de toutes les entrées en cache ayant été utilisées plusieurs fois.  
  
```  
SELECT usecounts, cacheobjtype, objtype, text   
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_sql_text(plan_handle)   
WHERE usecounts > 1   
ORDER BY usecounts DESC;  
GO  
```  
  
### <a name="b-returning-query-plans-for-all-cached-triggers"></a>B. Retour des plans de requête pour tous les déclencheurs en cache  
 L'exemple suivant retourne les plans de requête de tous les déclencheurs en cache.  
  
```  
SELECT plan_handle, query_plan, objtype   
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_query_plan(plan_handle)   
WHERE objtype ='Trigger';  
GO  
```  
  
### <a name="c-returning-the-set-options-with-which-the-plan-was-compiled"></a>C. Retour des options SET ayant servi à compiler le plan  
 L'exemple suivant retourne les options SET ayant servi à compiler le plan. Le `sql_handle` pour le plan est également retourné. L’opérateur PIVOT est utilisé pour la sortie du `set_options` et `sql_handle` attributs sous forme de colonnes plutôt que sous forme de lignes. Pour plus d’informations sur la valeur retournée dans `set_options`, consultez [sys.dm_exec_plan_attributes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md).  
  
```  
SELECT plan_handle, pvt.set_options, pvt.sql_handle  
FROM (  
      SELECT plan_handle, epa.attribute, epa.value   
      FROM sys.dm_exec_cached_plans   
      OUTER APPLY sys.dm_exec_plan_attributes(plan_handle) AS epa  
      WHERE cacheobjtype = 'Compiled Plan'  
      ) AS ecpa   
PIVOT (MAX(ecpa.value) FOR ecpa.attribute IN ("set_options", "sql_handle")) AS pvt;  
GO  
```  
  
### <a name="d-returning-the-memory-breakdown-of-all-cached-compiled-plans"></a>D. Retour de la répartition mémoire de tous les plans compilés en cache  
 L'exemple suivant retourne une répartition de la mémoire utilisée par tous les plans compilés du cache.  
  
```  
SELECT plan_handle, ecp.memory_object_address AS CompiledPlan_MemoryObject,   
    omo.memory_object_address, type, page_size_in_bytes   
FROM sys.dm_exec_cached_plans AS ecp   
JOIN sys.dm_os_memory_objects AS omo   
    ON ecp.memory_object_address = omo.memory_object_address   
    OR ecp.memory_object_address = omo.parent_address  
WHERE cacheobjtype = 'Compiled Plan';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [Sys.dm_exec_plan_attributes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_os_memory_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md)   
 [sys.dm_os_memory_cache_entries &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-entries-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)  
  
  


