---
title: Sys.pdw_nodes_column_store_segments (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/28/2018
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.component: design
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: e2fdf8e9-1b74-4682-b2d4-c62aca053d7f
caps.latest.revision: 9
author: hirokib
ms.author: elbutter
manager: jrj
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 92eee101d27c4483207ba1be9933b750aabdea3d
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="syspdwnodescolumnstoresegments-transact-sql"></a>Sys.pdw_nodes_column_store_segments (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

Contient une ligne pour chaque colonne dans un index columnstore.  

| Nom de colonne                 | Type de données  |  Description                                                  |
| --------------------------- | ---------- | ------------------------------------------------------------ |
| **partition_id**            | **bigint** | Indique l'ID de partition. Unique dans une base de données.     |
| **hobt_id**                 | **bigint** | ID du segment ou de l'index d'arbre B (B-tree) pour la table ayant cet index columnstore. |
| **column_id**               | **int**    | ID de la colonne columnstore.                                |
| **segment_id**              | **int**    | Retourne le segment de la colonne. Pour la compatibilité descendante, le nom de colonne continue à être appelée segment_id même si cela est l’ID de groupe de lignes. Vous pouvez identifier un segment à l’aide de < hobt_id, partition_id, column_id >, < segment_id >. |
| **version**                 | **int**    | Version du format de segment de colonne.                        |
| **encoding_type**           | **int**    | Type de codage utilisé pour ce segment :<br /><br /> 1 = VALUE_BASED - non chaîne/binaire avec aucun dictionnaire (semblable à 4 avec des variantes internes)<br /><br /> 2 = VALUE_HASH_BASED - colonne de type chaîne ou binaire avec des valeurs communes dans le dictionnaire<br /><br /> 3 = STRING_HASH_BASED - colonne de type chaîne ou binaire avec des valeurs communes dans le dictionnaire<br /><br /> 4 = STORE_BY_VALUE_BASED - non chaîne/binaire avec aucun dictionnaire<br /><br /> 5 = STRING_STORE_BY_VALUE_BASED - type chaîne ou binaire avec aucun dictionnaire<br /><br /> Tous les encodages de tirer parti de codage de bit-compression et la longueur de l’exécution lorsque cela est possible. |
| **row_count**               | **int**    | Nombre de lignes dans le groupe de lignes.                             |
| **has_nulls**               | **int**    | 1 si le segment de colonne a des valeurs NULL.                     |
| **base_id**                 | **bigint** | ID de la valeur de base si le type d’encodage 1 est utilisé.  Si le type d’encodage 1 n'est pas utilisé, ID a la valeur 1. |
| **magnitude**               | **float**  | Grandeur si le type d’encodage 1 est utilisé.  Si le type d’encodage 1 n'est pas utilisé, ordre de grandeur a la valeur 1. |
| **primary__dictionary_id**  | **int**    | ID du dictionnaire principal. Une valeur non nulle pointe vers le dictionnaire local pour cette colonne dans le segment actuel (par exemple, le groupe de lignes). La valeur -1 indique qu’il n’existe aucun dictionnaire local pour ce segment. |
| **secondary_dictionary_id** | **int**    | ID du dictionnaire secondaire. Une valeur non nulle pointe vers le dictionnaire local pour cette colonne dans le segment actuel (par exemple, le groupe de lignes). La valeur -1 indique qu’il n’existe aucun dictionnaire local pour ce segment. |
| **min_data_id**             | **bigint** | ID de données minimal dans le segment de colonne.                       |
| **max_data_id**             | **bigint** | ID de données maximale dans le segment de colonne.                       |
| **null_value**              | **bigint** | Valeur utilisée pour représenter les valeurs NULL.                               |
| **on_disk_size**            | **bigint** | Taille de segment en octets.                                    |
| **pdw_node_id**             | **int**    | Identificateur unique d’un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nœud. |

## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Joindre sys.pdw_nodes_column_store_segments avec d’autres tables système pour déterminer le nombre de segments de columnstore par table logique. 

```sql
SELECT  sm.name           as schema_nm
,       tb.name           as table_nm
,       nc.name           as col_nm
,       nc.column_id
,       COUNT(*)          as segment_count
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb                   ON  sm.[schema_id]          = tb.[schema_id]
JOIN    sys.[pdw_table_mappings] mp       ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt         ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[pdw_nodes_partitions] np     ON  np.[object_id]          = nt.[object_id]
                                          AND np.[pdw_node_id]        = nt.[pdw_node_id]
                                          AND np.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_nodes_columns] nc        ON  np.[object_id]          = nc.[object_id]
                                          AND np.[pdw_node_id]        = nc.[pdw_node_id]
                                          AND np.[distribution_id]    = nc.[distribution_id]
JOIN    sys.[pdw_nodes_column_store_segments] rg  ON  rg.[partition_id]         = np.[partition_id]
                                                      AND rg.[pdw_node_id]      = np.[pdw_node_id]
                                                      AND rg.[distribution_id]  = np.[distribution_id]
                                                      AND rg.[column_id]        = nc.[column_id]
GROUP BY    sm.name
,           tb.name
,           nc.name
,           nc.column_id  
ORDER BY    table_nm
,           nc.column_id
,           sm.name
```

## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **VIEW SERVER STATE**.  

## <a name="see-also"></a>Voir aussi  
 [Entrepôt de données SQL et les vues de catalogue de l’entrepôt de données en parallèle](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [CRÉER des INDEX COLUMNSTORE &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)   
 [sys.pdw_nodes_column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-row-groups-transact-sql.md)   
 [sys.pdw_nodes_column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-dictionaries-transact-sql.md)  

  

