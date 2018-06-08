---
title: Sys.masked_columns (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 02/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.masked_columns
- masked_columns_tsql
- sys.masked_columns_tsql
- masked_columns
helpviewer_keywords:
- sys.masked_columns catalog view
ms.assetid: 671577e4-d757-4b8d-9aa9-0fc8d51ea9ca
caps.latest.revision: 9
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 432ea847081c8f0060954efdd6e7f2beea1a592d
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmaskedcolumns-transact-sql"></a>Sys.masked_columns (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Utilisez le **sys.masked_columns** vue pour interroger des colonnes de table qui ont une fonction est appliquée de masquage dynamique des données. Celle-ci hérite de la vue **sys.columns** . Elle retourne toutes les colonnes de la vue **sys.columns** , ainsi que les colonnes **is_masked** et **masking_function** , en indiquant si les colonnes sont masquées et, dans ce cas, la fonction de masquage est définie. Cette vue présente uniquement les colonnes auxquelles une fonction de masquage est appliquée.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Identificateur de l'objet auquel appartient cette colonne.|  
|name|**sysname**|Nom de la colonne. Unique dans l'objet.|  
|column_id|**int**|ID de la colonne. Unique dans l'objet.<br /><br /> Les ID de colonnes peuvent ne pas être séquentiels.|  
|**Sys.masked_columns** retourne plus de colonnes héritées de **sys.columns**.|divers|Consultez [sys.columns &#40;Transact-SQL&#41; ](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) pour plusieurs définitions de colonne.|  
|is_masked|**bit**|Indique si la colonne est masquée. 1 indique masquées.|  
|masking_function|**nvarchar(4000)**|La fonction de masquage de la colonne.|  
  
## <a name="remarks"></a>Notes  
  
## <a name="permissions"></a>Autorisations  
 Cette vue retourne des informations sur les tables où l’utilisateur a une sorte d’autorisation sur la table, ou si l’utilisateur a l’autorisation VIEW ANY DEFINITION.  
  
## <a name="example"></a>Exemple  
 La requête suivante jointures **sys.masked_columns** à **sys.tables** pour retourner des informations relatives à tous les masquage de colonnes.  
  
```  
SELECT tbl.name as table_name, c.name AS column_name, c.is_masked, c.masking_function  
FROM sys.masked_columns AS c  
JOIN sys.tables AS tbl   
    ON c.object_id = tbl.object_id  
WHERE is_masked = 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Masquage dynamique des données](../../relational-databases/security/dynamic-data-masking.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
  
  
