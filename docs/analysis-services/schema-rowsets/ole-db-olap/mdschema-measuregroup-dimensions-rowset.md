---
title: Ensemble de lignes MDSCHEMA_MEASUREGROUP_DIMENSIONS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 817507a53e327c6fdb16f73d3b0eb0fb5c264f24
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdschemameasuregroupdimensions-rowset"></a>Ensemble de lignes MDSCHEMA_MEASUREGROUP_DIMENSIONS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Énumère les dimensions des groupes de mesures.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 L'ensemble de lignes **MDSCHEMA_MEASUREGROUP_DIMENSIONS** contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**||Nom du catalogue auquel appartient ce groupe de mesures. Valeur**NULL** si le fournisseur ne prend pas en charge les catalogues.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**||Non pris en charge.|  
|**CUBE_NAME**|**DBTYPE_WSTR**||Nom du cube auquel appartient ce groupe de mesures.|  
|**MEASUREGROUP_NAME**|**DBTYPE_WSTR**||Nom du groupe de mesures.|  
|**MEASUREGROUP_CARDINALITY**|**DBTYPE_WSTR**||Nombre d'instances qu'une mesure dans le groupe de mesures peut avoir pour un membre de dimension unique. Les valeurs possibles sont :<br /><br /> **UNE**<br /><br /> **NOMBREUX**|  
|**DIMENSION_UNIQUE_NAME**|**DBTYPE_WSTR**||Nom unique de la dimension.|  
|**DIMENSION_CARDINALITY**|**DBTYPE_WSTR**||Nombre d'instances qu'un membre de dimension peut avoir pour une instance unique d'une mesure de groupe de mesures. Les valeurs possibles sont :<br /><br /> **UNE**<br /><br /> **NOMBREUX**|  
|**DIMENSION_IS_VISIBLE**|**DBTYPE_BOOL**||Valeur booléenne qui indique si les hiérarchies de la dimension sont visibles.<br /><br /> Retourne **TRUE** si une ou plusieurs hiérarchies de la dimension sont visibles ; sinon, **FALSE**.|  
|**DIMENSION_IS_FACT_DIMENSION**|**DBTYPE_BOOL**||Valeur booléenne indiquant si la dimension est une dimension de fait.<br /><br /> Retourne **TRUE** si la dimennsion est une dimension de fait ; sinon, **FALSE**.|  
|**DIMENSION_PATH**|**DBTYPE_HCHAPTER**||Liste de dimensions pour la dimension de référence.|  
|**DIMENSION_GRANULARITY**|**DBTYPE_WSTR**||Nom unique de la hiérarchie de granularité.|  
  
 L'ensemble de lignes prend en charge le tri sur **CATALOG_NAME**, **SCHEMA_NAME**, **CUBE_NAME**, **MEASUREGROUP_NAME**, **DIMENSION_UNIQUE_NAME**.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 L'ensemble de lignes **MDSCHEMA_MEASUREGROUP_DIMENSIONS** peut être restreint sur les colonnes répertoriées dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**CATALOG_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**SCHEMA_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**CUBE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**MEASUREGROUP_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DIMENSION_UNIQUE_NAME**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
|**DIMENSION_VISIBILITY**|**DBTYPE_UI2**|(Facultatif) Bitmap avec l'une des valeurs valides suivantes :<br /><br /> 1 Visible<br /><br /> 2 non visible<br /><br /> La restriction par défaut est la valeur 1.|  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB pour OLAP Schema Rowsets](../../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
  
