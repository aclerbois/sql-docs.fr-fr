---
title: Sys.pdw_nodes_column_store_row_groups (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 17a4c925-d4b5-46ee-9cd6-044f714e6f0e
caps.latest.revision: 10
author: ronortloff
ms.author: rortloff
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 99577a71d2819ca0d2ce86d901eecf5ce87ae7ab
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syspdwnodescolumnstorerowgroups-transact-sql"></a>sys.pdw_nodes_column_store_row_groups (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Fournit des informations sur les index cluster columnstore sur une base par segment pour aider l’administrateur de décisions de système de gestion dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. **Sys.pdw_nodes_column_store_row_groups** possède une colonne pour le nombre total de lignes stockées physiquement (y compris celles qui sont marquées comme étant supprimées) et une colonne pour le nombre de lignes marquées comme supprimées. Utilisez **sys.pdw_nodes_column_store_row_groups** pour déterminer quelle ligne groupes ont un pourcentage élevé de lignes supprimées et doivent être reconstruits.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de la table sous-jacente. Il s’agit de la table physique sur le nœud de calcul, pas l’object_id de la table logique sur le nœud de contrôle. Par exemple, object_id ne correspond pas avec l’object_id dans sys.tables.<br /><br /> Pour joindre avec sys.tables, utilisez sys.pdw_index_mappings.|  
|**index_id**|**int**|ID de l’index cluster columnstore sur *object_id* table.|  
|**partition_number**|**int**|ID de la partition de table qui contient le groupe de lignes *row_group_id*. Vous pouvez utiliser *partition_number* pour joindre cette vue DMV à sys.partitions.|  
|**row_group_id**|**int**|ID de ce groupe de lignes. Cet ID est unique dans la partition.|  
|**dellta_store_hobt_id**|**bigint**|hobt_id des groupes de lignes delta, ou NULL si le type de groupe de lignes n'est pas delta. Un groupe de lignes delta est un groupe de lignes en lecture/écriture qui reçoit des enregistrements. Un groupe de lignes delta a la **ouvrir** état. Un groupe de lignes delta est toujours au format rowstore et n'a pas été compressé au format columnstore.|  
|**state**|**tinyint**|Numéro d'ID associé à state_description.<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED|  
|**state_desccription**|**nvarchar(60)**|Description de l'état permanent du groupe de lignes :<br /><br /> OPEN - Un groupe de lignes en lecture/écriture qui reçoit des enregistrements. Un groupe de lignes ouvert est toujours au format rowstore et n'a pas été compressé au format columnstore.<br /><br /> CLOSED – Un groupe de lignes qui a été renseigné, mais pas encore compressé par le processus du moteur de tuple.<br /><br /> COMPRESSED – Un groupe de lignes rempli et compressé.|  
|**total_rows**|**bigint**|Nombre total de lignes stockées physiquement dans le groupe de lignes. Certaines peuvent avoir été supprimées, mais elles sont toujours stockées. Le nombre maximal de lignes d'un groupe de lignes est 1 048 576 (hexadécimal FFFFF).|  
|**deleted_rows**|**bigint**|Nombre de lignes stockées physiquement dans le groupe de lignes qui sont marquées pour suppression.<br /><br /> Toujours 0 pour le DELTA des groupes de lignes.|  
|**size_in_bytes**|**int**|Taille combinée, en octets, de toutes les pages de ce groupe de lignes. Cette taille n’inclut pas la taille requise pour stocker les métadonnées ou les dictionnaires partagés.|  
|**pdw_node_id**|**int**|Id unique d’un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nœud.|  
|**distribution_id**|**int**|Id unique de la distribution.|
  
## <a name="remarks"></a>Notes  
 Retourne une ligne pour chaque groupe de lignes columnstore pour chaque table ayant un index columnstore cluster ou non cluster.  
  
 Utilisez **sys.pdw_nodes_column_store_row_groups** pour déterminer le nombre de lignes incluses dans le groupe de lignes et de la taille du groupe de lignes.  
  
 Lorsque le nombre de lignes supprimées dans un groupe de lignes atteint un fort pourcentage du nombre total de lignes, la table est moins efficace. Reconstruisez l'index columnstore pour réduire la taille de la table, ce qui réduit les E/S disque nécessaires pour lire la table. Pour reconstruire l’index columnstore utilisez le **reconstruire** option de le **ALTER INDEX** instruction.  
  
 Le columnstore modifiable insère au préalable de nouvelles données dans un **ouvrir** rowgroup, qui est au format rowstore et est parfois également appelé table delta.  Une fois qu’un rowgroup ouvert est saturé, son état passe à **fermé**. Un rowgroup fermé est compressé au format columnstore par le moteur de tuple et l’état passe à **compressé**.  Le moteur de tuple est un processus en arrière-plan qui se réveille régulièrement et vérifie s'il existe des rowgroups fermés prêts à être compressés dans un rowgroup columnstore.  Le moteur de tuple libère également les rowgroups dans lesquels chaque ligne a été supprimée. Rowgroups libérés sont marqués en tant que **RETIRÉE**. Pour exécuter le moteur de tuple immédiatement, utilisez le **RÉORGANISER** option de le **ALTER INDEX** instruction.  
  
 Lorsqu'un groupe de lignes columnstore est rempli, il est compressé, et cesse de recevoir de nouvelles lignes. Lorsque vous supprimez des lignes d'un groupe compressé, elles sont conservées mais sont marquées comme étant supprimées. Les mises à jour dans un groupe compressé sont implémentées comme une suppression du groupe compressé, et une insertion dans un groupe ouvert.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **VIEW SERVER STATE**.  
  
## <a name="examples-includesssdwincludessssdw-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant joint le **sys.pdw_nodes_column_store_row_groups** table à d’autres tables système pour retourner des informations sur des tables spécifiques. La colonne calculée `PercentFull` est une estimation de l'efficacité du groupe de lignes. Pour trouver des informations sur une seule table, supprimez les traits d’union de commentaire devant la clause WHERE et fournir un nom de table.  
  
```  
SELECT IndexMap.object_id,   
  object_name(IndexMap.object_id) AS LogicalTableName,   
  i.name AS LogicalIndexName, IndexMap.index_id, NI.type_desc,   
  IndexMap.physical_name AS PhyIndexNameFromIMap,   
  CSRowGroups.*,  
  100*(ISNULL(deleted_rows,0))/total_rows AS PercentDeletedRows   
FROM sys.tables AS t  
JOIN sys.indexes AS i  
    ON t.object_id = i.object_id  
JOIN sys.pdw_index_mappings AS IndexMap  
    ON i.object_id = IndexMap.object_id  
    AND i.index_id = IndexMap.index_id  
JOIN sys.pdw_nodes_indexes AS NI  
    ON IndexMap.physical_name = NI.name  
    AND IndexMap.index_id = NI.index_id  
JOIN sys.pdw_nodes_column_store_row_groups AS CSRowGroups  
    ON CSRowGroups.object_id = NI.object_id   
    AND CSRowGroups.pdw_node_id = NI.pdw_node_id  
AND CSRowGroups.index_id = NI.index_id      
--WHERE t.name = '<table_name>'   
ORDER BY object_name(i.object_id), i.name, IndexMap.physical_name, pdw_node_id;  
```  

Les éléments suivants [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] exemple compte les lignes par partition pour une colonne en cluster stocke ainsi que la manière dont plusieurs lignes sont dans des groupes ouvert, fermé ou compressés ligne :  

```
SELECT
    s.name AS [Schema Name]
    ,t.name AS [Table Name]
    ,rg.partition_number AS [Partition Number]
    ,SUM(rg.total_rows) AS [Total Rows]
    ,SUM(CASE WHEN rg.State = 1 THEN rg.Total_rows Else 0 END) AS [Rows in OPEN Row Groups]
    ,SUM(CASE WHEN rg.State = 2 THEN rg.Total_Rows ELSE 0 END) AS [Rows in Closed Row Groups]
    ,SUM(CASE WHEN rg.State = 3 THEN rg.Total_Rows ELSE 0 END) AS [Rows in COMPRESSED Row Groups]
FROM sys.pdw_nodes_column_store_row_groups rg
JOIN sys.pdw_nodes_tables pt
ON rg.object_id = pt.object_id AND rg.pdw_node_id = pt.pdw_node_id AND pt.distribution_id = rg.distribution_id
JOIN sys.pdw_table_mappings tm
ON pt.name = tm.physical_name
INNER JOIN sys.tables t
ON tm.object_id = t.object_id
INNER JOIN sys.schemas s
ON t.schema_id = s.schema_id
GROUP BY s.name, t.name, rg.partition_number
ORDER BY 1, 2
```
  
## <a name="see-also"></a>Voir aussi  
 [Entrepôt de données SQL et les vues de catalogue de l’entrepôt de données en parallèle](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [CRÉER des INDEX COLUMNSTORE &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)   
 [sys.pdw_nodes_column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-segments-transact-sql.md)   
 [sys.pdw_nodes_column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-dictionaries-transact-sql.md)  
  
  
