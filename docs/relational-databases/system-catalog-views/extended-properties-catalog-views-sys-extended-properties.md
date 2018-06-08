---
title: Sys.extended_properties (Transact-SQL) | Documents Microsoft
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
- sys.extended_properties
- sys.extended_properties_TSQL
- extended_properties
- extended_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.extended_properties catalog view
ms.assetid: 439b7299-dce3-4d26-b1c7-61be5e0df82a
caps.latest.revision: 42
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: fb0a4c9692b88ccc895d9aa97f4520cc2d5aea4a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="extended-properties-catalog-views---sysextendedproperties"></a>Étendue des affichages catalogue de propriétés - sys.extended_properties
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)]

  Retourne une ligne pour chaque propriété étendue de la base de données actuelle.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|class|**tinyint**|Identifie la classe d'élément contenant la propriété. Les valeurs possibles sont les suivantes :<br /><br /> 0 = Base de données<br /><br /> 1 = Objet ou colonne<br /><br /> 2 = Paramètre<br /><br /> 3 = Schéma<br /><br /> 4 = Principal de base de données<br /><br /> 5 = Assembly<br /><br /> 6 = Type<br /><br /> 7 = Index<br /><br /> 10 = Collection du schéma XML<br /><br /> 15 = Type de message<br /><br /> 16 = Contrat de service<br /><br /> 17 = Service<br /><br /> 18 = Liaison au service distant<br /><br /> 19 = Itinéraire<br /><br /> 20 = Espace de données (groupe de fichiers ou schéma de partition)<br /><br /> 21 = Fonction de partition<br /><br /> 22 = Fichier de base de données<br /><br /> 27 = Repère de plan|  
|class_desc|**nvarchar(60)**|Description de la classe contenant la propriété étendue. Les valeurs possibles sont les suivantes :<br /><br /> DATABASE<br /><br /> OBJECT_OR_COLUMN<br /><br /> Paramètre<br /><br /> SCHEMA<br /><br /> DATABASE_PRINCIPAL<br /><br /> ASSEMBLY<br /><br /> TYPE<br /><br /> INDEX<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> MESSAGE_TYPE<br /><br /> SERVICE_CONTRACT<br /><br /> SERVICE<br /><br /> REMOTE_SERVICE_BINDING<br /><br /> ROUTE<br /><br /> DATASPACE<br /><br /> PARTITION_FUNCTION<br /><br /> DATABASE_FILE<br /><br /> PLAN_GUIDE|  
|major_id|**int**|ID de l'élément contenant la propriété étendue, interprété en fonction de sa classe. Pour la plupart des éléments, il s'agit de l'ID qui s'applique à ce que représente la classe. L'interprétation des principaux ID non standard est la suivante :<br /><br /> Si la valeur de class est 0, major_id est toujours 0.<br /><br /> Si la valeur de class est 1, 2 ou 7, major_id est object_id.|  
|minor_id|**int**|ID secondaire de l'élément contenant la propriété étendue, interprété en fonction de sa classe. Pour la plupart des éléments, la valeur est 0, sinon l'ID est le suivant :<br /><br /> Si class = 1, minor_id est column_id avec la colonne, autrement 0 avec l'objet.<br /><br /> Si class = 2, minor_id est parameter_id.<br /><br /> Si class = 7, minor_id est index_id.|  
|name|**sysname**|Nom de propriété, unique avec class, major_id et minor_id.|  
|valeur|**sql_variant**|Valeur de la propriété étendue.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Étendue des affichages catalogue de propriétés &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/f39fd324-efd4-4468-884c-bf77ed1a026f)   
 [Sys.fn_listextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_addextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)  
  
  
