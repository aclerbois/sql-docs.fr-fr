---
title: Sys.assembly_types (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- assembly_types
- sys.assembly_types
- sys.assembly_types_TSQL
- assembly_types_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assembly_types catalog view
ms.assetid: 35f0384f-7a6d-41b1-9461-f1406d68f317
caps.latest.revision: 22
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d519c57b76e45a297598b6821658c640e8e97989
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysassemblytypes-transact-sql"></a>sys.assembly_types (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)]

  Contient une ligne pour chaque type défini par l'utilisateur et par un assembly CLR. Les éléments suivants **sys.assembly_types** apparaissent dans la liste des colonnes héritées (consultez [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)) après **rule_object_id**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|ID de l'assembly à partir duquel ce type est créé.|  
|**assembly_class**|**sysname**|Nom de la classe dans l'assembly qui définit ce type.|  
|**is_binary_ordered**|**bit**|Trier les octets de ce type revient à trier les opérateurs de comparaison sur ce même type.|  
|**is_fixed_length**|**bit**|La longueur du type est toujours identique à max_length.|  
|**prog_id**|**nvarchar(40)**|ProgID du type exposé à COM.|  
|**assembly_qualified_name**|**nvarchar(4000)**|Nom du type qualifié de l'assembly. Le formatage du nom est tel qu'il peut être transmis à Type.GetType().|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue de Types scalaires &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)  
  
  
