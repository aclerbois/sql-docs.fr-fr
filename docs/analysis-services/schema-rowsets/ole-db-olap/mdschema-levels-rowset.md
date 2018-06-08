---
title: Ensemble de lignes MDSCHEMA_LEVELS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: aa23399b731108dd6b395b5d1debeb182e9330f6
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdschemalevels-rowset"></a>Ensemble de lignes MDSCHEMA_LEVELS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit chaque niveau dans une hiérarchie particulière.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **MDSCHEMA_LEVELS** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type| Description|  
|-----------------|--------------------|-----------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Nom du catalogue auquel appartient ce niveau. Valeur**NULL** si le fournisseur ne prend pas en charge les catalogues.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Nom du schéma auquel appartient ce niveau. **NULL** si le fournisseur ne prend pas en charge les schémas.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Nom du cube auquel appartient ce niveau.|  
|**DIMENSION_UNIQUE_NAME**|**DBTYPE_WSTR**|Nom unique de la dimension à laquelle appartient ce niveau. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**HIERARCHY_UNIQUE_NAME**|**DBTYPE_WSTR**|Nom unique de la hiérarchie. Si le niveau appartient à plusieurs hiérarchies, il existe une ligne pour chaque hiérarchie dont il fait partie. Pour les fournisseurs qui produisent des noms uniques par qualification, chaque composant du nom est délimité.|  
|**NOM_NIVEAU**|**DBTYPE_WSTR**|Nom du niveau.|  
|**LEVEL_UNIQUE_NAME**|**DBTYPE_WSTR**|Nom unique du niveau avec caractères d'échappement appropriés.|  
|**LEVEL_GUID**|**DBTYPE_GUID**|Non pris en charge.|  
|**LEVEL_CAPTION**|**DBTYPE_WSTR**|Étiquette ou légende associée à la hiérarchie. Principalement utilisée à des fins d'affichage. Si une légende n’existe pas, **nom_niveau** est retourné.|  
|**LEVEL_NUMBER**|**DBTYPE_UI4**|Distance du niveau par rapport à la racine de la hiérarchie. Niveau de la racine est égal à zéro (**0)**.|  
|**LEVEL_CARDINALITY**|**DBTYPE_UI4**|Nombre de membres du niveau.|  
|**LEVEL_TYPE**|**DBTYPE_I4**|Type de niveau :<br /><br /> **MDLEVEL_TYPE_GEO_CONTINENT** (**0x2001**)<br /><br /> **MDLEVEL_TYPE_GEO_REGION** (**0 x 2002**)<br /><br /> **MDLEVEL_TYPE_GEO_COUNTRY** (**0x2003**)<br /><br /> **MDLEVEL_TYPE_GEO_STATE_OR_PROVINCE** (**0x2004**)<br /><br /> **MDLEVEL_TYPE_GEO_COUNTY** (**0x2005**)<br /><br /> **MDLEVEL_TYPE_GEO_CITY** (**0 x 2006**)<br /><br /> **MDLEVEL_TYPE_GEO_POSTALCODE** (**0 x 2007**)<br /><br /> **MDLEVEL_TYPE_GEO_POINT** (**0 x 2008**)<br /><br /> **MDLEVEL_TYPE_ORG_UNIT** (**0x1011**)<br /><br /> **MDLEVEL_TYPE_BOM_RESOURCE** (**0x1012**)<br /><br /> **MDLEVEL_TYPE_QUANTITATIVE** (**0x1013**)<br /><br /> **MDLEVEL_TYPE_ACCOUNT** (**0x1014**)<br /><br /> **MDLEVEL_TYPE_CUSTOMER** (**0x1021**)<br /><br /> **MDLEVEL_TYPE_CUSTOMER_GROUP** (**0x1022**)<br /><br /> **MDLEVEL_TYPE_CUSTOMER_HOUSEHOLD** (**0x1023**)<br /><br /> **MDLEVEL_TYPE_PRODUCT** (**0x1031**)<br /><br /> **MDLEVEL_TYPE_PRODUCT_GROUP** (**0x1032**)<br /><br /> **MDLEVEL_TYPE_SCENARIO** (**0x1015**)<br /><br /> **MDLEVEL_TYPE_UTILITY** (**0x1016**)<br /><br /> **MDLEVEL_TYPE_PERSON** (**0x1041**)<br /><br /> **MDLEVEL_TYPE_COMPANY** (**0x1042**)<br /><br /> **MDLEVEL_TYPE_CURRENCY_SOURCE** (**0x1051**)<br /><br /> **MDLEVEL_TYPE_CURRENCY_DESTINATION** (**0x1052**)<br /><br /> **MDLEVEL_TYPE_CHANNEL** (**0x1061**)<br /><br /> **MDLEVEL_TYPE_REPRESENTATIVE** (**0x1062**)<br /><br /> **MDLEVEL_TYPE_PROMOTION** (**0x1071**)|  
|**DESCRIPTION**|**DBTYPE_WSTR**|Description du niveau à l'intention des utilisateurs. NULL si aucune description n'existe.|  
|**CUSTOM_ROLLUP_SETTINGS**|**DBTYPE_I4**|Bitmap qui définit les options de cumul personnalisé :<br /><br /> **MDLEVELS_CUSTOM_ROLLUP_EXPRESSION** (**0 x 01**) indique une expression existe pour ce niveau. (Déconseillé)<br /><br /> **MDLEVELS_CUSTOM_ROLLUP_COLUMN** (**0 x 02**) indique qu’il existe une colonne de cumul personnalisé pour ce niveau.<br /><br /> **MDLEVELS_SKIPPED_LEVELS** (**0 x 04**) indique qu’il existe un niveau ignoré associé avec les membres de ce niveau.<br /><br /> **MDLEVELS_CUSTOM_MEMBER_PROPERTIES** (**0 x 08**) indique que les membres du niveau ont des propriétés de membre personnalisées.<br /><br /> **MDLEVELS_UNARY_OPERATOR** (**0 x 10**) indique que les membres du niveau ont des opérateurs unaires.|  
|**LEVEL_UNIQUE_SETTINGS**|**DBTYPE_I4**|Image bitmap qui spécifie quelles colonnes contiennent des valeurs uniques, si le niveau ne contient que des membres avec des noms ou des clés uniques. Le fichier Msmd.h définit les constantes de valeur binaire suivantes pour cette image bitmap :<br /><br /> **MDDIMENSIONS_MEMBER_KEY_UNIQUE** (**1**)<br /><br /> **MDDIMENSIONS_MEMBER_NAME_UNIQUE** (**2**)<br /><br /> <br /><br /> Notez que la clé est toujours unique dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Le nom doit être unique si le paramètre sur l’attribut est **UniqueInDimension** ou **UniqueInAttribute**|  
|**LEVEL_IS_VISIBLE**|**DBTYPE_BOOL**|Valeur booléenne qui indique si le niveau est visible.<br /><br /> Renvoie toujours True. Si le niveau n'est pas visible, il n'est pas inclus dans l'ensemble de lignes du schéma.|  
|**LEVEL_ORDERING_PROPERTY**|**DBTYPE_WSTR**|ID de l'attribut sur lequel le niveau est trié.|  
|**LEVEL_DBTYPE**|**DBTYPE_I4**|Le **DBTYPE** énumération de la colonne de clé de membre qui est utilisée pour l’attribut de niveau.<br /><br /> Null si des clés concaténées sont utilisées comme colonne des clés de membre.|  
|**LEVEL_MASTER_UNIQUE_NAME**|**DBTYPE_WSTR**|Retourne toujours la valeur Null.|  
|**LEVEL_NAME_SQL_COLUMN_NAME**|**DBTYPE_WSTR**|Représentation SQL des noms de membre du niveau.|  
|**LEVEL_KEY_SQL_COLUMN_NAME**|**DBTYPE_WSTR**|Représentation SQL des valeurs de clé des membres du niveau.|  
|**LEVEL_UNIQUE_NAME_SQL_COLUMN_NAME**|**DBTYPE_WSTR**|Représentation SQL des noms de membre uniques.|  
|**LEVEL_ATTRIBUTE_HIERARCHY_NAME**|**DBTYPE_WSTR**|Nom de la hiérarchie d'attribut fournissant la source du niveau.|  
|**LEVEL_KEY_CARDINALITY**|**DBTYPE_UI2**|Nombre de colonnes dans la clé de niveau.|  
|**LEVEL_ORIGIN**|**DBTYPE_UI2**|Bitmap qui définit comment le niveau a été alimenté :<br /><br /> **MD_ORIGIN_USER_DEFINED** identifie les niveaux dans une hiérarchie définie par l’utilisateur.<br /><br /> **MD_ORIGIN_ATTRIBUTE** identifie les niveaux d’une hiérarchie d’attribut.<br /><br /> **MD_ORIGIN_KEY_ATTRIBUTE** identifie les niveaux d’une hiérarchie d’attribut de clé.<br /><br /> **MD_ORIGIN_INTERNAL** identifie les niveaux des hiérarchies d’attributs qui ne sont pas activés.|  
  
 L’ensemble de lignes est trié sur **CATALOG_NAME**, **nom_schéma**, **CUBE_NAME**, **DIMENSION_UNIQUE_NAME**, **HIERARCHY_UNIQUE_NAME**, **LEVEL_NUMBER**.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **MDSCHEMA_LEVELS** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DIMENSION_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**HIERARCHY_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**NOM_NIVEAU**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**LEVEL_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**LEVEL_ORIGIN**|**DBTYPE_UI2**|(Facultatif) Une restriction par défaut est appliqué suite **MD_USER_DEFINED** et **MD_SYSTEM_ENABLED**|  
|**CUBE_SOURCE**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de 1. Une image bitmap avec l’une des valeurs valides suivantes :<br /><br /> 1 CUBE<br /><br /> 2 DIMENSION|  
|**LEVEL_VISIBILITY**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de 1. Une image bitmap avec l’une des valeurs suivantes :<br /><br /> 1 Visible<br /><br /> 2 non visible|  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB pour OLAP Schema Rowsets](../../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
  
