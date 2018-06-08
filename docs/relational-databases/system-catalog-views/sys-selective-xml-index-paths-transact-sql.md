---
title: Sys.selective_xml_index_paths (Transact-SQL) | Documents Microsoft
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
- xml_schema_attributes_TSQL
- xml_schema_attributes
- sys.xml_schema_attributes_TSQL
- sys.xml_schema_attributes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_attributes catalog view
ms.assetid: 07a73d71-ec3e-4894-947a-5859ca62c606
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0078ffca12182f0478f67b05d03dd14eb64b3ef8
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysselectivexmlindexpaths-transact-sql"></a>sys.selective_xml_index_paths (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Disponible à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Service Pack 1, chaque ligne dans sys.selective_xml_index_paths représente un chemin d'accès promu pour un index XML sélectif spécifique.  
  
 Si vous créez un index XML sélectif sur xmlcol de la table T à l'aide de l'instruction suivante,  
  
```  
CREATE SELECTIVE XML INDEX sxi1 ON T(xmlcol)   
FOR ( path1 = '/a/b/c' AS XQUERY 'xs:string',  
      path2 = '/a/b/d' AS XQUERY 'xs:double'  
    )  
```  
  
 Il y aura deux nouvelles lignes dans sys.selective_xml_index_paths correspondant à l'index sxi1.  

  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de table avec une colonne XML.|  
|**index_id**|**int**|ID unique de l'index XML sélectif.|  
|**path_id**|**int**|ID du chemin d'accès XML promu.|  
|**path**|**nvarchar(4000)**|Chemin d'accès promu. Par exemple, '/a/b/c/d/e'.|  
|**nom**|**sysname**|Chemin d'accès.|  
|**path_type**|**tinyint**|0 = XQUERY<br /><br /> 1 = SQL|  
|**path_type_desc**|**sysname**|En fonction de **path_type** 'XQUERY' ou 'SQL' de valeur.|  
|**xml_component_id**|**int**|ID unique du composant de schéma XML dans la base de données.|  
|**xquery_type_description**|**nvarchar(4000)**|Nom du type xsd spécifié.|  
|**is_xquery_type_inferred**|**bit**|1 = le type est déduit.|  
|**xquery_max_length**|**smallint**|Longueur maximale (en caractères du type xsd).|  
|**is_xquery_max_length_inferred**|**bit**|1 = la longueur maximale est déduite.|  
|**is_node**|**bit**|0 = l'indicateur node() n'est pas présent.<br /><br /> 1 = l'indicateur d'optimisation node() est appliqué.|  
|**system_type_id**|**tinyint**|ID du type de système de la colonne.|  
|**user_type_id**|**tinyint**|ID du type d'utilisateur de la colonne.|  
|**max_length**|**smallint**|Length maximale (en octets) du type.<br /><br /> -1 = le type de données de colonne est varchar(max), nvarchar(max), varbinary(max) ou xml.|  
|**precision**|**tinyint**|Précision maximale du type s'il est basé sur un nombre. Sinon, 0.|  
|**scale**|**tinyint**|Échelle maximale du type s'il est basé sur un nombre. Sinon, il prend la valeur 0.|  
|**collation_name**|**sysname**|Nom du classement du type s'il est de type caractère. Sinon, NULL.|  
|**is_singleton**|**bit**|0 = l'indicateur SINGLETON n'est pas présent.<br /><br /> 1 = l'indicateur d'optimisation SINGLETON est appliqué.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Schémas XML &#40;système de Type XML&#41; affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
