---
title: Élément d’expression (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: 75db3d3a652287f46d6a51dbb89e83dd028ba387
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="expression-element-assl"></a>Élément Expression (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Contient une expression MDX (Multidimensional Expressions) qui définit le contenu de l'élément parent.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<CellPermission> <!-- or StandardAction -->  
   ...  
   <Expression>...</Expression>  
   ...  
</CellPermission>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne|  
|Valeur par défaut|Aucune|  
|Cardinalité|1-1 : élément requis qui apparaît une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[CellPermission](../../../analysis-services/scripting/objects/cellpermission-element-assl.md), [StandardAction](../../../analysis-services/scripting/data-type/standardaction-data-type-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 Pour le **CellPermission** élément, le **Expression** élément contient une expression MDX logique qui identifie les cellules applicables aux droits indiqués par le [accès](../../../analysis-services/scripting/properties/access-element-assl.md) élément de la **CellPermission** élément. Si la valeur d’un **Expression** , élément pour un **CellPermission** élément est vide, le **CellPermission** élément est ignoré.  
  
 Pour le **StandardAction** élément, le **Expression** élément contient une expression MDX qui représente le contenu de l’action. Si la valeur d’un **Expression** , élément pour un **StandardAction** élément est vide, le **StandardAction** élément est ignoré.  
  
 Les éléments qui correspondent aux parents dans le modèle objet AMO (Analysis Management Objects) sont <xref:Microsoft.AnalysisServices.CellPermission> et <xref:Microsoft.AnalysisServices.StandardAction>.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
