---
title: Ensemble de lignes MDSCHEMA_DIMENSIONS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e1e509ad466a0d173fe67a8fd1f3ac5190647eb7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdschemadimensions-rowset"></a>Ensemble de lignes MDSCHEMA_DIMENSIONS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Décrit les dimensions partagées et privées dans une base de données.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **MDSCHEMA_DIMENSIONS** ensemble de lignes contient les colonnes suivantes :  
  
|Nom de colonne|Indicateur de type| Description|  
|-----------------|--------------------|-----------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Nom de la base de données.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Non pris en charge.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Nom du cube.|  
|**DIMENSION_NAME**|**DBTYPE_WSTR**|Nom de la dimension. Si une dimension appartient à plusieurs cubes ou groupes de mesures, il existe une ligne pour chaque combinaison unique de dimension, groupe de mesures et cube.|  
|**DIMENSION_UNIQUE_NAME**|**DBTYPE_WSTR**|Le nom unique de la dimension.|  
|**DIMENSION_GUID**|**DBTYPE_GUID**|Non pris en charge.|  
|**DIMENSION_CAPTION**|**DBTYPE_WSTR**|Légende de la dimension. Celle-ci doit être utilisée lors de l'affichage du nom de la dimension à l'utilisateur, par exemple dans l'interface utilisateur ou les rapports.|  
|**DIMENSION_ORDINAL**|**DBTYPE_UI4**|Position de la dimension dans le cube.|  
|**DIMENSION_TYPE**|**DBTYPE_I2**|Type de la dimension. Les valeurs valides sont les suivantes :<br /><br /> **MD_DIMTYPE_UNKNOWN** (**0**)<br /><br /> **MD_DIMTYPE_TIME** (**1**)<br /><br /> **MD_DIMTYPE_MEASURE** (**2**)<br /><br /> **MD_DIMTYPE_OTHER** (**3**)<br /><br /> **MD_DIMTYPE_QUANTITATIVE** (**5**)<br /><br /> **MD_DIMTYPE_ACCOUNTS** (**6**)<br /><br /> **MD_DIMTYPE_CUSTOMERS** (**7**)<br /><br /> **MD_DIMTYPE_PRODUCTS** (**8**)<br /><br /> **MD_DIMTYPE_SCENARIO** (**9**)<br /><br /> **MD_DIMTYPE_UTILIY** (**10**)<br /><br /> **MD_DIMTYPE_CURRENCY** (**11**)<br /><br /> **MD_DIMTYPE_RATES** (**12**)<br /><br /> **MD_DIMTYPE_CHANNEL** (**13**)<br /><br /> **MD_DIMTYPE_PROMOTION** (**14**)<br /><br /> **MD_DIMTYPE_ORGANIZATION** (**15**)<br /><br /> **MD_DIMTYPE_BILL_OF_MATERIALS** (**16**)<br /><br /> **MD_DIMTYPE_GEOGRAPHY** (**17**)|  
|**DIMENSION_CARDINALITY**|**DBTYPE_UI4**|Spécifie le nombre de membres dans l'attribut de clé.|  
|**DEFAULT_HIERARCHY**|**DBTYPE_WSTR**|Hiérarchie à partir de la dimension. Conservée pour la compatibilité ascendante.|  
|**DESCRIPTION**|**DBTYPE_WSTR**|Description conviviale de la dimension.|  
|**IS_VIRTUAL**|**DBTYPE_BOOL**|Toujours **FALSE**.|  
|**IS_READWRITE**|**DBTYPE_BOOL**|Valeur booléenne indiquant si la dimension est activée en écriture.<br /><br /> **TRUE** si la dimension est activée en écriture.|  
|**DIMENSION_UNIQUE_SETTINGS**|**DBTYPE_I4**|Image bitmap qui spécifie quelles colonnes contiennent des valeurs uniques si la dimension ne contient que des membres possédant un nom unique. Les constantes de valeur binaire suivantes sont définies dans Msmd.pour cette image bitmap :<br /><br /> **MDDIMENSIONS_MEMBER_KEY_UNIQUE**|  
|**DIMENSION_MASTER_UNIQUE_NAME**|**DBTYPE_WSTR**|Toujours **NULL**.|  
|**DIMENSION_IS_VISIBLE**|**DBTYPE_BOOL**|Toujours **TRUE**.<br /><br /> Remarque : Une dimension n’est pas visible, sauf si un ou plusieurs hiérarchies de la dimension sont visibles.|  
  
 L’ensemble de lignes est trié sur **CATALOG_NAME**, **nom_schéma**, **CUBE_NAME**, **DIMENSION_NAME**.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **MDSCHEMA_DIMENSIONS** ensemble de lignes peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DIMENSION_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DIMENSION_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_SOURCE**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de 1. Une image bitmap avec l’une des valeurs valides suivantes :<br /><br /> 1 CUBE<br /><br /> 2 DIMENSION|  
|**DIMENSION_VISIBILITY**|**DBTYPE_UI2**|(Facultatif) Restriction par défaut est une valeur de 1. Une image bitmap avec l’une des valeurs valides suivantes :<br /><br /> 1 Visible<br /><br /> 2 non visible|  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB pour OLAP Schema Rowsets](../../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
  
