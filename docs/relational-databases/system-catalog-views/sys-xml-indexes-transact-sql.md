---
title: Sys.xml_indexes (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.xml_indexes_TSQL
- xml_indexes_TSQL
- sys.xml_indexes
- xml_indexes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_indexes catalog view
ms.assetid: 3408de72-b067-4fda-b5d5-8e856dfd9db3
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: bbc15a06d6179165bd2f6f1d3c19eb24d42dfcc8
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysxmlindexes-transact-sql"></a>sys.xml_indexes (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne par index XML.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**\<héritée de colonnes >**||Hérite des colonnes de [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).|  
|**using_xml_index_id**|**int**|NULL = Index XML principal.<br /><br /> Nonnull = Index XML secondaire.<br /><br /> Nonnull est une référence d'autojointure à l'index XML principal.|  
|**secondary_type**|**char(1)**|Description du type d'index secondaire :<br /><br /> P = PATH index XML secondaire<br /><br /> V = VALUE index XML secondaire<br /><br /> R = PROPERTY index XML secondaire<br /><br /> NULL = Index XML principal|  
|**secondary_type_desc**|**nvarchar(60)**|Description du type d'index secondaire :<br /><br /> PATH = PATH index XML secondaire<br /><br /> VALUE = VALUE index XML secondaire<br /><br /> PROPERTY = PROPERTY index XML secondaires<br /><br /> NULL = Index XML principal|  
|**xml_index_type**|**tinyint**|Type d'index :<br /><br /> 0 = Index XML principal<br /><br /> 1 = Index XML secondaire<br /><br /> 2 = Index XML sélectif<br /><br /> 3 = Index XML secondaire sélectif|  
|**xml_index_type_description**|**nvarchar(60)**|Description du type d'index :<br /><br /> PRIMARY_XML<br /><br /> Index XML secondaire<br /><br /> Index XML sélectif<br /><br /> Index XML secondaire sélectif|  
|**path_id**|**int**|NULL pour tous les index XML, à l'exception de l'index XML secondaire sélectif.<br /><br /> Sinon, ID du chemin d'accès promu sur lequel l'index XML secondaire sélectif est créé. Cette valeur est identique à path_id de la vue système sys.selective_xml_index_paths.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue de l’objet &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
