---
title: Sys.xml_schema_collections (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.xml_schema_collections_TSQL
- sys.xml_schema_collections
- xml_schema_collections
- xml_schema_collections_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_collections catalog view
ms.assetid: f3f7f3dc-029f-4942-ab3c-75fa9814e40f
caps.latest.revision: 34
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 11e9a91fc27b704b54415acfe98a516a2176603a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysxmlschemacollections-transact-sql"></a>sys.xml_schema_collections (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Renvoie une ligne par collection de schémas XML. Une collection de schémas XML est un ensemble nommé de définitions XSD. La collection de schémas XML elle-même est contenue dans un schéma relationnel et identifiée par un nom [!INCLUDE[tsql](../../includes/tsql-md.md)] d'étendue sur le schéma. Les tuples suivants sont uniques : xml_collection_id et schema_id et  name.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|xml_collection_id|**int**|ID de la collection de schémas XML. Unique dans la base de données.|  
|schema_id|**int**|ID du schéma relationnel qui contient cette collection de schémas XML.|  
|principal_id|**int**|ID du propriétaire spécifique s’il est différent du propriétaire du schéma. Par défaut, le propriétaire du schéma détient les objets contenus dans le schéma. Toutefois, vous pouvez spécifier un autre propriétaire à l’aide de l’instruction ALTER AUTHORIZATION pour modifier la propriété.<br /><br /> NULL = Pas d'autre propriétaire individuel.|  
|name|**sysname**|Nom de la collection de schémas XML.|  
|create_date|**datetime**|Date de création de la collection de schémas XML.|  
|modify_date|**datetime**|Date de dernière modification de la collection de schémas XML.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Schémas XML &#40;système de Type XML&#41; affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)   
 [Questions fréquentes (FAQ) sur l’interrogation des catalogues système SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
