---
title: Sys.xml_schema_components (Transact-SQL) | Documents Microsoft
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
- xml_schema_components
- sys.xml_schema_components_TSQL
- sys.xml_schema_components
- xml_schema_components_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_components catalog view
ms.assetid: 70142d3a-f8b5-4ee2-8287-3935f0f67aa2
caps.latest.revision: 35
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 90fbd2b92ccfcf01da2c2742a3b5a083f30f1e03
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "33221950"
---
# <a name="sysxmlschemacomponents-transact-sql"></a>sys.xml_schema_components (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne par composant d'un schéma XML. La paire (**collection_id**, **namespace_id**) est une clé étrangère composite pour l’espace de noms qui le contient. Pour les composants nommés, les valeurs de **symbol_space**, **nom**, **scoping_xml_component_id**, **is_qualified**, **xml_namespace_id**, **xml_collection_id** sont uniques.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**xml_component_id**|**Int**|ID unique du composant de schéma XML dans la base de données.|  
|**xml_collection_id**|**Int**|ID de la collection de schémas XML qui contient l'espace de noms de ce composant.|  
|**xml_namespace_id**|**Int**|ID de l'espace de noms XML à l'intérieur de la collection.|  
|**is_qualified**|**bit**|1 = ce composant possède un qualificateur d'espace de noms explicite.<br /><br /> 0 = il s'agit d'un composant d'étendue locale. Dans ce cas, la paire, **namespace_id**, **collection_id**, fait référence à « aucun espace de noms » **targetNamespace**.<br /><br /> Pour les composants de caractère générique, cette valeur sera égale à 1.|  
|**nom**|**nvarchar**<br /><br /> **(4000)**|Nom unique du composant de schéma XML. NULL si le composant est sans nom.|  
|**symbol_space**|**char(1)**|Espace dans lequel ce nom de symbole est unique, en fonction de **type**:<br /><br /> N = aucun<br /><br /> T = type<br /><br /> E = élément<br /><br /> M = groupe de modèles<br /><br /> A = attribut<br /><br /> G = groupe d'attributs|  
|**symbol_space_desc**|**nvarchar**<br /><br /> **(60)**|Description de l’espace dans lequel ce nom de symbole est unique, en fonction de **type**:<br /><br /> Aucune<br /><br /> TYPE<br /><br /> ELEMENT<br /><br /> MODEL_GROUP<br /><br /> ATTRIBUTE<br /><br /> ATTRIBUTE_GROUP|  
|**Type**|**char(1)**|Type du composant de schéma XML.<br /><br /> N = n'importe quel type (composant intrinsèque spécial)<br /><br /> Z = n'importe quel type simple (composant intrinsèque spécial)<br /><br /> P = type primitif (composant intrinsèque)<br /><br /> S = type simple<br /><br /> L = liste<br /><br /> U = Union<br /><br /> C = type simple complexe (dérivé de simple)<br /><br /> K = type complexe<br /><br /> E = élément<br /><br /> M = groupe de modèles<br /><br /> W = élément-caractère générique<br /><br /> A = attribut<br /><br /> G = groupe d'attributs<br /><br /> V = attribut-caractère générique|  
|**kind_desc**|**nvarchar**<br /><br /> **(60)**|Description du type de composant de schéma XML :<br /><br /> ANY_TYPE<br /><br /> ANY_SIMPLE_TYPE<br /><br /> PRIMITIVE_TYPE<br /><br /> SIMPLE_TYPE<br /><br /> LIST_TYPE<br /><br /> UNION_TYPE<br /><br /> COMPLEX_SIMPLE_TYPE<br /><br /> COMPLEX_TYPE<br /><br /> ELEMENT<br /><br /> MODEL_GROUP<br /><br /> ELEMENT_WILDCARD<br /><br /> ATTRIBUTE<br /><br /> ATTRIBUTE_GROUP<br /><br /> ATTRIBUTE_WILDCARD|  
|**dérivation**|**char(1)**|Mode de dérivation pour les types dérivés :<br /><br /> N = aucune (pas de dérivation)<br /><br /> X = extension<br /><br /> R = restriction<br /><br /> S = substitution|  
|**derivation_desc**|**nvarchar**<br /><br /> **(60)**|Description du mode de dérivation pour les types dérivés :<br /><br /> Aucune<br /><br /> EXTENSION<br /><br /> RESTRICTION<br /><br /> SUBSTITUTION|  
|**base_xml_component_id**|**Int**|ID du composant à partir duquel le composant est dérivé. NULL s'il n'y en a pas.|  
|**scoping_xml_component_id**|**Int**|ID unique du composant d'étendue. NULL s'il n'y en a pas (étendue globale).|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Schémas XML &#40;système de Type XML&#41; affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
