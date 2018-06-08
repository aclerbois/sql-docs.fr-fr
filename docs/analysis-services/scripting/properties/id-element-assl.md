---
title: ID d’élément (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: a9b0d85a5d5e5ae369048c64197122d45cb4c044
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="id-element-assl"></a>Élément ID (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Contient l'identificateur unique (ID) unique de l'élément parent.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Action> <!-- or one of the elements listed in the Element Relationships table -->  
   ...  
   <ID>...</ID>  
   ...  
</Action>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (jusqu'à 100 caractères)|  
|Valeur par défaut|Aucune|  
|Cardinalité|0-1: élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Action](../../../analysis-services/scripting/objects/action-element-assl.md), [agrégation](../../../analysis-services/scripting/objects/aggregation-element-assl.md), [AggregationDesign](../../../analysis-services/scripting/objects/aggregationdesign-element-assl.md), [Assembly](../../../analysis-services/scripting/objects/assembly-element-assl.md), [Cube](../../../analysis-services/scripting/objects/cube-element-assl.md), [CubeBinding](../../../analysis-services/scripting/data-type/cubebinding-data-type-out-of-line-assl.md), [CubeDimension](../../../analysis-services/scripting/data-type/cubedimension-data-type-assl.md), [base de données](../../../analysis-services/scripting/objects/database-element-assl.md), [DataSource](../../../analysis-services/scripting/objects/datasource-element-assl.md), [DataSourceView](../../../analysis-services/scripting/objects/datasourceview-element-assl.md), [Dimension ](../../../analysis-services/scripting/objects/dimension-element-assl.md), [DimensionAttribute](../../../analysis-services/scripting/data-type/dimensionattribute-data-type-assl.md), [hiérarchie](../../../analysis-services/scripting/objects/hierarchy-element-assl.md), [Kpi](../../../analysis-services/scripting/objects/kpi-element-assl.md), [niveau](../../../analysis-services/scripting/objects/level-element-assl.md), [MdxScript](../../../analysis-services/scripting/objects/mdxscript-element-assl.md), [Mesure](../../../analysis-services/scripting/objects/measure-element-assl.md), [MeasureGroup](../../../analysis-services/scripting/objects/measuregroup-element-assl.md), [MeasureGroupBinding](../../../analysis-services/scripting/data-type/measuregroupbinding-data-type-assl.md), [MiningModel](../../../analysis-services/scripting/objects/miningmodel-element-assl.md), [ MiningModelColumn](../../../analysis-services/scripting/data-type/miningmodelcolumn-data-type-assl.md), [MiningStructure](../../../analysis-services/scripting/objects/miningstructure-element-assl.md), [MiningStructureColumn](../../../analysis-services/scripting/data-type/miningstructurecolumn-data-type-assl.md), [Partition](../../../analysis-services/scripting/objects/partition-element-assl.md), [autorisation](../../../analysis-services/scripting/data-type/permission-data-type-assl.md), [Perspective](../../../analysis-services/scripting/objects/perspective-element-assl.md), [rôle](../../../analysis-services/scripting/objects/role-element-assl.md), [Server](../../../analysis-services/scripting/objects/server-element-assl.md), [Trace](../../../analysis-services/scripting/objects/trace-element-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 Chaque objet principal dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] a un **ID** élément en tant que propriété. La valeur d’un **ID** élément présente les restrictions suivantes :  
  
-   La valeur ne peut pas contenir des espaces de début ni de fin. [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] supprimera de manière implicite les espaces de début ou de fin de la valeur d’un **ID** élément.  
  
-   La valeur ne peut pas contenir des caractères de contrôle. [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] supprimera de manière implicite les caractères de contrôle de la valeur d’un **ID** élément.  
  
-   Les valeurs réservées suivantes ne peuvent pas être utilisées :  
  
    -   AUX  
  
    -   CLOCK$  
  
    -   COM1 à COM9 (COM1, COM2, COM3, et ainsi de suite)  
  
    -   CON  
  
    -   LPT1 à LPT9 (LPT1, LPT2, LPT3, et ainsi de suite)  
  
    -   NUL  
  
    -   PRN  
  
 Le tableau suivant répertorie les caractères supplémentaires qui ne peut pas être utilisés dans la valeur d’un **ID** élément, en fonction de l’élément parent.  
  
|Élément parent|Caractères|  
|--------------------|----------------|  
|[Server](../../../analysis-services/scripting/objects/server-element-assl.md)|La valeur doit se conformer aux règles énoncées pour les noms des ordinateurs équipés de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows. (Les adresses IP ne sont pas valides.)|  
|[DataSource](../../../analysis-services/scripting/objects/datasource-element-assl.md)|:/\\*&#124;?" [] de (){}<>|  
|[Niveau](../../../analysis-services/scripting/objects/level-element-assl.md), [élément d’attribut](../../../analysis-services/scripting/objects/attribute-element-assl.md)|.,;' `:/\\*&#124;?" & % $! [] de +={}<>|  
|Tous les autres éléments parents|.,;' `:/\\*&#124;?" & % $! %$!+=()[]5D;{}{}<>|  
  
## <a name="see-also"></a>Voir aussi  
 [Nom d’élément &#40;ASSL&#41;](../../../analysis-services/scripting/properties/name-element-assl.md)   
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
