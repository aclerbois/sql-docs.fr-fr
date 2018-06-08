---
title: Sys.internal_partitions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- Azure SQL Database
- SQL Server 2016 Preview
dev_langs:
- TSQL
ms.assetid: 0262df2b-5ba7-4715-b17b-3d9ce470a38e
caps.latest.revision: 13
author: ronortloff
ms.author: rortloff
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 9d0321089336774e4303418b776eec8a1608613b
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="sysinternalpartitions-transact-sql"></a>Sys.internal_partitions (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque ensemble de lignes qui effectue le suivi des données internes pour les index columnstore sur les tables sur disque. Ces ensembles de lignes sont internes à l’index columnstore et rowgroups stockent les lignes de suivi supprimé, les mappages de rowgroup et delta. Elles suivent les données pour chaque pour chaque partition de table ; chaque table possède au moins une partition. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée de nouveau les ensembles de lignes chaque fois qu’il reconstruit l’index columnstore.   
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|partition_id|**bigint**|ID de partition pour cette partition. Unique dans la base de données.|  
|object_id|**int**|ID d’objet pour la table qui contient la partition.|  
|index_id|**int**|ID d’index pour l’index columnstore défini sur la table.<br /><br /> 1 = index cluster columnstore<br /><br /> 2 = index non cluster|  
|partition_number|**int**|Le numéro de partition.<br /><br /> 1 = la première partition d’une table partitionnée, ou la partition unique d’une table non partitionnée.<br /><br /> 2 = deuxième partition et ainsi de suite.|  
|internal_object_type|**tinyint**|Objets d’ensemble de lignes qui effectuent le suivi de données interne pour l’index columnstore.<br /><br /> 2 = COLUMN_STORE_DELETE_BITMAP<br /><br /> 3 = COLUMN_STORE_DELTA_STORE<br /><br /> 4 = COLUMN_STORE_DELETE_BUFFER<br /><br /> 5 = COLUMN_STORE_MAPPING_INDEX|  
|internal_object_type_desc|**nvarchar(60)**|COLUMN_STORE_DELETE_BITMAP – cet index de bitmap effectue le suivi des lignes qui sont marquées comme supprimées du columnstore. La bitmap est pour chaque groupe de lignes, car les partitions peuvent avoir des lignes dans plusieurs groupes de lignes. Les lignes sont qui se trouvent toujours physiquement présent et permet de l’espace dans le columnstore.<br /><br /> COLUMN_STORE_DELTA_STORE : groupes de magasins de lignes, appelés rowgroups, qui n’ont pas été compressés dans le stockage en colonnes. Chaque partition de table peut avoir zéro ou plusieurs rowgroups de deltastore.<br /><br /> COLUMN_STORE_DELETE_BUFFER – de maintenance supprime les index columnstore non cluster actualisable. Lorsqu’une requête supprime une ligne à partir de la table rowstore sous-jacente, la mémoire tampon de suppression effectue le suivi de la suppression du ColumnStore. Lorsque le nombre de lignes supprimées dépasser 1048576, sont fusionnées dans la bitmap de suppression en arrière-plan des threads de moteur de Tuple ou par une commande Reorganize explicite.  À un moment donné dans le temps, l’union de la bitmap de suppression et de la mémoire tampon de suppression représente supprimés toutes les lignes.<br /><br /> COLUMN_STORE_MAPPING_INDEX – utilisé uniquement lorsque l’index cluster columnstore a un index non cluster secondaire. Clés d’index non cluster correspond au rowgroup correct et l’ID de ligne dans le columnstore. Elle stocke uniquement les clés pour les lignes qui se déplacent vers un autre groupe de lignes ; Cela se produit quand un rowgroup delta est compressé dans le columnstore et lorsqu’une opération de fusion fusionne les lignes à partir de deux groupes de lignes différents.|  
|Row_group_id|**int**|ID de rowgroup deltastore. Chaque partition de table peut avoir zéro ou plusieurs rowgroups de deltastore.|  
|hobt_id|**bigint**|ID de l’objet d’ensemble de lignes interne. Il s’agit d’une bonne clé de jointure avec d’autres vues de gestion dynamique pour obtenir plus d’informations sur les caractéristiques physiques de l’ensemble de lignes interne.|  
|lignes|**bigint**|Nombre approximatif de lignes dans cette partition.|  
|data_compression|**tinyint**|L’état de compression pour l’ensemble de lignes :<br /><br /> 0 = AUCUN<br /><br /> 1 = LIGNE<br /><br /> 2 = PAGE|  
|data_compression_desc|**nvarchar(60)**|L’état de compression pour chaque partition. Les valeurs possibles pour les tables rowstore sont AUCUN, LIGNE et PAGE. Les valeurs possibles pour les tables columnstore sont COLUMNSTORE et COLUMNSTORE_ARCHIVE.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** . Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée de nouveau columnstore index interne chaque fois qu’il crée ou reconstruit un index columnstore.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-view-all-of-the-internal-rowsets-for-a-table"></a>A. Afficher tous les ensembles de lignes interne pour une table  
 Cet exemple retourne tous les ensembles de lignes columnstore interne pour une table. Vous pouvez également utiliser hobt_id pour plus d’informations sur l’ensemble de lignes spécifique.  
  
```  
SELECT i.object_id, i.index_id, i.name, p.hobt_id, p.internal_object_type_id, p.internal_object_type_desc  
FROM sys.internal_partitions AS p  
JOIN sys.indexes AS i  
on i.object_id = p.object_id  
WHERE p.object_id = OBJECT_ID ( '<table name' ) ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Questions fréquentes (FAQ) sur l’interrogation des catalogues système SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
