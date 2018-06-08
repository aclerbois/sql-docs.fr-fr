---
title: Sys.Foreign_Keys (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- foreign_keys
- sys.foreign_keys
- sys.foreign_keys_TSQL
- foreign_keys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.foreign_keys catalog view
ms.assetid: e960df1a-13fc-43ee-ba91-34c1b719ac2c
caps.latest.revision: 37
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8a7f33be3e5bc933abe96aa6700a8ef94c7a2e83
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysforeignkeys-transact-sql"></a>sys.foreign_keys (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Contient une ligne pour chaque objet qui est une contrainte FOREIGN KEY, avec **sys.object.type** = F.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**\<Colonnes héritent de sys.objects >**||Pour obtenir la liste des colonnes qui hérite de cette vue, consultez [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**referenced_object_id**|**int**|ID de l'objet référencé.|  
|**key_index_id**|**int**|ID de l'index de clé dans l'objet référencé.|  
|**is_disabled**|**bit**|La contrainte FOREIGN KEY est désactivée.|  
|**is_not_for_replication**|**bit**|La contrainte FOREIGN KEY a été créée à l'aide de l'option NOT FOR REPLICATION.|  
|**is_not_trusted**|**bit**|Le système n'a pas vérifié la contrainte FOREIGN KEY.|  
|**delete_referential_action**|**tinyint**|Action référentielle déclarée pour cette clé FOREIGN KEY lorsqu'une suppression a lieu.<br /><br /> 0 = Pas d'action<br /><br /> 1 = Cascade<br /><br /> 2 = Définir avec une valeur NULL<br /><br /> 3 = Définir avec une valeur par défaut|  
|**delete_referential_action_desc**|**nvarchar(60)**|Description de l'action référentielle déclarée pour cette clé FOREIGN KEY lorsqu'une suppression a lieu :<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**update_referential_action**|**tinyint**|Action référentielle déclarée pour cette clé FOREIGN KEY lorsqu'une mise à jour a lieu.<br /><br /> 0 = Pas d'action<br /><br /> 1 = Cascade<br /><br /> 2 = Définir avec une valeur NULL<br /><br /> 3 = Définir avec une valeur par défaut|  
|**update_referential_action_desc**|**nvarchar(60)**|Description de l'action référentielle déclarée pour cette clé FOREIGN KEY lorsqu'une mise à jour a lieu.<br /><br /> NO_ACTION<br /><br /> CASCADE<br /><br /> SET_NULL<br /><br /> SET_DEFAULT|  
|**is_system_named**|**bit**|1 = Le nom a été créé par le système.<br /><br /> 0 = Le nom a été fourni par l'utilisateur.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Questions fréquentes (FAQ) sur l’interrogation des catalogues système SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
