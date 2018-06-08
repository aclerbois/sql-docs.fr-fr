---
title: Sys.sql_dependencies (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sql_dependencies
- sql_dependencies_TSQL
- sys.sql_dependencies_TSQL
- sys.sql_dependencies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_dependencies catalog view
ms.assetid: 1779aa87-a0b8-470a-a286-d7cc0b93ad2e
caps.latest.revision: 31
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: b458e22f8b0b803dcd359b870af6f14b85a95f48
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syssqldependencies-transact-sql"></a>sys.sql_dependencies (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque dépendance à une entité référencée telle qu'elle est référencée dans l'expression [!INCLUDE[tsql](../../includes/tsql-md.md)] ou les instructions qui définissent un autre objet de référence.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) à la place.  

  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**class**|**tinyint**|Identifie la classe de l'entité référencée :<br /><br /> 0 = objet ou colonne (références non liées au schéma uniquement)<br /><br /> 1 = Objet ou colonne (uniquement les références liées au schéma)<br /><br /> 2 = Types (uniquement les références liées au schéma)<br /><br /> 3 = Collections de schémas XML (uniquement les références liées au schéma)<br /><br /> 4 = Fonction de partition (uniquement les références liées au schéma)|  
|**class_desc**|**nvarchar(60)**|Description de la classe de l'entité référencée :<br /><br /> **OBJECT_OR_COLUMN_REFERENCE_NON_SCHEMA_BOUND**<br /><br /> **OBJECT_OR_COLUMN_REFERENCE_SCHEMA_BOUND**<br /><br /> **TYPE_REFERENCE**<br /><br /> **XML_SCHEMA_COLLECTION_REFERENCE**<br /><br /> **PARTITION_FUNCTION_REFERENCE**|  
|**object_id**|**int**|ID de l'objet de référence.|  
|**column_id**|**int**|Si l'ID de référence est une colonne, ID de la colonne de référence ; sinon, 0.|  
|**referenced_major_id**|**int**|ID de l'entité référencée, interprété par la valeur de la classe, en fonction de :<br /><br /> 0, 1 = ID d'objet de l'objet ou de la colonne.<br /><br /> 2 = ID de type.<br /><br /> 3 = ID de collection de schémas XML.|  
|**referenced_minor_id**|**int**|ID secondaire de l'entité référencée, interprété par la valeur de la classe, comme illustré ci-dessous :<br /><br /> Lorsque class =:<br /><br /> 0, **referenced_minor_id** est un ID de colonne ou si n’est pas une colonne, il est 0.<br /><br /> 1, **referenced_minor_id** est un ID de colonne ou si n’est pas une colonne, il est 0.<br /><br /> Dans le cas contraire, **referenced_minor_id** = 0.|  
|**is_selected**|**bit**|L'objet ou la colonne est sélectionné.|  
|**is_updated**|**bit**|L'objet ou la colonne est mis à jour.|  
|**is_select_all**|**bit**|L'objet est utilisé dans la clause SELECT *(au niveau de l'objet uniquement).|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** . Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Questions fréquentes (FAQ) sur l’interrogation des catalogues système SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
