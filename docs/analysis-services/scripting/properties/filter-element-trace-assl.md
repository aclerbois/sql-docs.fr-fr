---
title: Élément Filter (Trace) (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: 9efa3d3993c9902590a694dd6e543a83e6ae8da6
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="filter-element-trace-assl"></a>Élément Filter (Trace) (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Contient un fragment de document XML qui décrit le [Trace](../../../analysis-services/scripting/objects/trace-element-assl.md) filtre.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Trace>  
   ...  
   <Filter>...</Filter>  
   ...  
</Trace>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne|  
|Valeur par défaut|Aucune|  
|Cardinalité|0-1: élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Élément parent|[Trace](../../../analysis-services/scripting/objects/trace-element-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 L’élément qui correspond au parent de **filtre** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.Trace>.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés &#40;ASSL&#41;](../../../analysis-services/scripting/properties/properties-assl.md)   
 [Effectue le suivi élément &#40;ASSL&#41;](../../../analysis-services/scripting/collections/traces-element-assl.md)  
  
  
