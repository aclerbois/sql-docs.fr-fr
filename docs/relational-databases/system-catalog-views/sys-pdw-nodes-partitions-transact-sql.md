---
title: Sys.pdw_nodes_partitions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.component: system-catalog-views
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: b4216752-4813-4b2c-b259-7d8ffc6cc190
caps.latest.revision: 11
author: ronortloff
ms.author: rortloff
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: f05d5ce80bf7ca286050a160d5cfbc41a689e67e
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syspdwnodespartitions-transact-sql"></a>Sys.pdw_nodes_partitions (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Contient une ligne pour chaque partition de toutes les tables et la plupart des types d’index dans un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] base de données. Toutes les tables et index contient au moins une partition, ils sont partitionnés explicitement ou non.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|partition_id|`bigint`|ID de la partition. Unique dans une base de données.|  
|object_id|`int`|ID de l’objet auquel appartient cette partition. Chaque table ou vue comporte au moins une partition.|  
|index_id|`int`|ID de l’index de l’objet auquel appartient cette partition.|  
|partition_number|`int`|numéro de partition de base 1 dans l’index ou le segment de mémoire propriétaire. Pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], la valeur de cette colonne est 1.|  
|hobt_id|`bigint`|ID du segment de données ou de l'arbre B qui contient les lignes de cette partition.|  
|lignes|`bigint`|Nombre approximatif de lignes dans cette partition. |  
|data_compression|`int`|Indique l'état de compression pour chaque partition :<br /><br /> 0 = AUCUN<br /><br /> 1 = LIGNE<br /><br /> 2 = PAGE<br /><br /> 3 = COLUMNSTORE|  
|data_compression_desc|`nvarchar(60)`|Indique l'état de compression pour chaque partition. Les valeurs possibles sont NONE, ROW et PAGE.|  
|pdw_node_id|`int`|Identificateur unique d’un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nœud.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER.  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

### <a name="example-a-display-rows-in-each-partition-within-each-distribution"></a>Exemple a : lignes d’affichage dans chaque partition au sein de chaque point de distribution 

S’applique à : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
 
Pour afficher le nombre de lignes dans chaque partition au sein de chaque point de distribution, utilisez [DBCC PDW_SHOWPARTITIONSTATS (SQL Server PDW)](../../t-sql/database-console-commands/dbcc-pdw-showpartitionstats-transact-sql.md) .

### <a name="example-b-uses-system-views-to-view-rows-in-each-partition-of-each-distribution-of-a-table"></a>L’exemple b : vues du système utilise pour afficher les lignes dans chaque partition de chaque point de distribution d’une table

S’applique à : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]
 
Cette requête renvoie le nombre de lignes dans chaque partition de chaque point de distribution de la table `myTable`.  
 
```  
SELECT o.name, pnp.index_id, pnp.partition_id, pnp.rows,   
    pnp.data_compression_desc, pnp.pdw_node_id  
FROM sys.pdw_nodes_partitions AS pnp  
JOIN sys.pdw_nodes_tables AS NTables  
    ON pnp.object_id = NTables.object_id  
AND pnp.pdw_node_id = NTables.pdw_node_id  
JOIN sys.pdw_table_mappings AS TMap  
    ON NTables.name = TMap.physical_name 
    AND substring(TMap.physical_name,40, 10) = pnp.distribution_id 
JOIN sys.objects AS o  
    ON TMap.object_id = o.object_id  
WHERE o.name = 'myTable'  
ORDER BY o.name, pnp.index_id, pnp.partition_id;  
```    
  
## <a name="see-also"></a>Voir aussi  
 [Entrepôt de données SQL et les vues de catalogue de l’entrepôt de données en parallèle](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  

