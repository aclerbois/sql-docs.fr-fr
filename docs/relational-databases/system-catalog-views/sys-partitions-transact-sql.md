---
title: Sys.partitions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- partitions
- partitions_TSQL
- sys.partitions_TSQL
- sys.partitions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.partitions catalog view
ms.assetid: 1c19e1b1-c925-4dad-a652-581692f4ab5e
caps.latest.revision: 60
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 4902bca55295c2e6e870e2f3488cc178c0f771fa
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syspartitions-transact-sql"></a>sys.partitions (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-asdw-pdw-md.md)]

  Contient une ligne pour chaque partition de toutes les tables et la plupart des types d'index de la base de données. Les types d'index spéciaux comme Texte intégral, Spatial et XML ne sont pas inclus dans cette vue. Tous les index et tables de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiennent au moins une partition, qu'ils soient ou non explicitement partitionnés.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|partition_id|**bigint**|Indique l'ID de partition. Unique dans une base de données.|  
|object_id|**int**|Indique l’ID de l’objet auquel appartient cette partition. Chaque table ou vue comporte au moins une partition.|  
|index_id|**int**|Indique l'ID de l'index de l'objet auquel cette partition appartient.<br /><br /> 0 = segment de mémoire<br />1 = index cluster<br />2 ou plus = index non-cluster|  
|partition_number|**int**|Numéro de partition en base 1 dans l'index ou le segment de mémoire propriétaire. Pour les tables et les index non partitionnés, la valeur de cette colonne est 1.|  
|hobt_id|**bigint**|Indique l'ID du segment de données ou de l'arbre B (B-tree) qui contient les lignes de cette partition.|  
|lignes|**bigint**|Indique le nombre approximatif de lignes dans cette partition.|  
|filestream_filegroup_id|**smallint**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Indique l'ID du groupe de fichiers FILESTREAM stocké sur cette partition.|  
|data_compression|**tinyint**|Indique l'état de compression pour chaque partition :<br /><br /> 0 = AUCUN <br />1 = LIGNE <br />2 = PAGE <br />3 = COLUMNSTORE : **s’applique aux**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br />4 = COLUMNSTORE_ARCHIVE : **s’applique aux**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]<br /><br /> **Remarque :** index de texte intégral sont compressés dans n’importe quelle édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|data_compression_desc|**nvarchar(60)**|Indique l'état de compression pour chaque partition. Les valeurs possibles pour les tables rowstore sont AUCUN, LIGNE et PAGE. Les valeurs possibles pour les tables columnstore sont COLUMNSTORE et COLUMNSTORE_ARCHIVE.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** . Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Questions fréquentes (FAQ) sur l’interrogation des catalogues système SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
