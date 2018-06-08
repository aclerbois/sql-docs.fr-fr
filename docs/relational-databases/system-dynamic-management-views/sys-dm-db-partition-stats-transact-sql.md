---
title: Sys.dm_db_partition_stats (Transact-SQL) | Documents Microsoft
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
- dm_db_partition_stats
- dm_db_partition_stats_TSQL
- sys.dm_db_partition_stats_TSQL
- sys.dm_db_partition_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_partition_stats dynamic management view
ms.assetid: 9db9d184-b3a2-421e-a804-b18ebcb099b7
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 99e0d57f7d2bf82b0bffaf7ae97ba0181459290d
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbpartitionstats-transact-sql"></a>sys.dm_db_partition_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Renvoie le nombre de pages et de lignes de chaque partition de la base de données active.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_db_partition_stats**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**partition_id**|**bigint**|ID de la partition. Unique dans la base de données. Ceci est la même valeur que la **partition_id** dans les **sys.partitions** affichage catalogue|  
|**object_id**|**int**|ID d'objet de la table ou de la vue indexée à laquelle appartient la partition.|  
|**index_id**|**int**|ID du segment de mémoire ou de l'index auquel appartient la partition.<br /><br /> 0 = Segment de mémoire<br /><br /> 1 = Index cluster<br /><br /> > 1 = Index non cluster|  
|**partition_number**|**int**|Numéro de partition (basé sur la valeur 1) au sein de l'index ou du segment de mémoire.|  
|**in_row_data_page_count**|**bigint**|Nombre de pages utilisées pour stocker les données des lignes de cette partition. Si la partition fait partie d'un segment de mémoire, la valeur de cette colonne est le nombre de pages de données dans ce segment. Si la partition fait partie d'un index, la valeur de cette colonne est le nombre de pages au niveau feuille. (Les pages non-feuille dans l'arborescence binaire (B-tree) ne sont pas comprises dans ce nombre). Les pages IAM (Index Allocation Map) ne sont jamais incluses. Toujours 0 pour un index columnstore optimisé en mémoire xVelocity.|  
|**in_row_used_page_count**|**bigint**|Nombre total de pages utilisées pour stocker et gérer les données des lignes de cette partition. Ce nombre inclut les pages non-feuille B-tree, les pages IAM et toutes les pages incluses dans le **in_row_data_page_count** colonne. Toujours 0 pour un index columnstore.|  
|**in_row_reserved_page_count**|**bigint**|Nombre total de pages réservées pour stocker et gérer les données des lignes de cette partition, que les pages soient utilisées ou non. Toujours 0 pour un index columnstore.|  
|**lob_used_page_count**|**bigint**|Nombre de pages utilisées pour stocker et gérer la sortie de la ligne **texte**, **ntext**, **image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, et **xml** des colonnes dans la partition. Les pages IAM sont incluses.<br /><br /> Nombre total d'objets LOB utilisés pour stocker et gérer l'index columnstore dans la partition.|  
|**lob_reserved_page_count**|**bigint**|Nombre total de pages réservées pour stocker et gérer la sortie de la ligne **texte**, **ntext**, **image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, et **xml** colonnes de la partition, que les pages soient en cours d’utilisation ou non. Les pages IAM sont incluses.<br /><br /> Nombre total d'objets LOB réservés pour le stockage et la gestion d'un index columnstore dans la partition.|  
|**row_overflow_used_page_count**|**bigint**|Nombre de pages utilisées pour stocker et gérer le dépassement de ligne **varchar**, **nvarchar**, **varbinary**, et **sql_variant** colonnes au sein de la partition. Les pages IAM sont incluses.<br /><br /> Toujours 0 pour un index columnstore.|  
|**row_overflow_reserved_page_count**|**bigint**|Nombre total de pages réservées pour stocker et gérer le dépassement de ligne **varchar**, **nvarchar**, **varbinary**, et **sql_variant** colonnes de la partition, que les pages soient en cours d’utilisation ou non. Les pages IAM sont incluses.<br /><br /> Toujours 0 pour un index columnstore.|  
|**used_page_count**|**bigint**|Nombre total de pages utilisées pour la partition. Calculée comme **in_row_used_page_count** + **lob_used_page_count** + **row_overflow_used_page_count**.|  
|**reserved_page_count**|**bigint**|Nombre total de pages réservées pour la partition. Calculée comme **in_row_reserved_page_count** + **lob_reserved_page_count** + **row_overflow_reserved_page_count**.|  
|**row_count**|**bigint**|Nombre approximatif de lignes dans la partition.|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
|**distribution_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Id numérique unique associée à la distribution.|  
  
## <a name="remarks"></a>Notes  
 **Sys.dm_db_partition_stats** affiche des informations sur l’espace utilisé pour stocker et gérer dans la ligne des données métier et données de dépassement de ligne pour toutes les partitions dans une base de données. Une seule ligne est affichée par partition.  
  
 Les nombres sur lesquels le résultat est basé sont placés en mémoire cache ou stockés sur disque dans diverses tables système.  
  
 Les données dans la ligne, les données LOB et les données en dépassement de capacité des lignes représentent les trois unités d'allocation qui composent une partition. Le [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md) vue de catalogue peut être interrogée pour les métadonnées relatives à chaque unité d’allocation dans la base de données.  
  
 Si le segment de mémoire ou l'index n'est pas partitionné, il se compose d'une partition (dont le numéro est égal à 1) ; par conséquent, une seule ligne est renvoyée pour ce segment ou cet index. Le [sys.partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) vue de catalogue peut être interrogée pour les métadonnées relatives à chaque partition de toutes les tables et des index dans une base de données.  
  
 Le nombre total pour une table ou un index s'obtient en ajoutant les nombres obtenus pour l'ensemble des partitions concernées.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’autorisation VIEW DATABASE STATE pour interroger le **sys.dm_db_partition_stats** vue de gestion dynamique. Pour plus d’informations sur les autorisations sur les vues de gestion dynamique, consultez [fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-all-counts-for-all-partitions-of-all-indexes-and-heaps-in-a-database"></a>A. Renvoi de tous les nombres pour toutes les partitions de tous les index et segments de mémoire d'une base de données  
 Le code exemple suivant affiche tous les nombres pour toutes les partitions de tous les index et segments de mémoire de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_db_partition_stats;  
GO  
```  
  
### <a name="b-returning-all-counts-for-all-partitions-of-a-table-and-its-indexes"></a>B. Renvoi de tous les nombres pour toutes les partitions d'une table et de ses index  
 Le code exemple suivant affiche tous les nombres pour toutes les partitions de la table `HumanResources.Employee` et de ses index.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_db_partition_stats   
WHERE object_id = OBJECT_ID('HumanResources.Employee');  
GO  
```  
  
### <a name="c-returning-total-used-pages-and-total-number-of-rows-for-a-heap-or-clustered-index"></a>C. Renvoi du nombre total de pages utilisées et du nombre total de lignes d'un segment de mémoire ou d'un index cluster  
 Le code exemple suivant renvoie le nombre total de pages utilisées et le nombre total de lignes du segment de mémoire ou de l'index cluster de la table `HumanResources.Employee`. Du fait que la table `Employee` n'est pas partitionnée par défaut, la somme n'inclut qu'une seule partition.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT SUM(used_page_count) AS total_number_of_used_pages,   
    SUM (row_count) AS total_number_of_rows   
FROM sys.dm_db_partition_stats  
WHERE object_id=OBJECT_ID('HumanResources.Employee')    AND (index_id=0 or index_id=1);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
  


