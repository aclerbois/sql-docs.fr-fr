---
title: Cible de l’élément (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: 98ec5c729f5735343f5eaa87e5232fcfb138cf38
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="target-element-assl"></a>Élément Target (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Identifie la cible de la [Action](../../../analysis-services/scripting/objects/action-element-assl.md) élément.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Action>  
   ...  
   <Target>...</Target>  
   ...  
</Action>  
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
|Élément parent|[Action](../../../analysis-services/scripting/objects/action-element-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 La valeur attendue de cet élément dépend de la valeur de la [TargetType](../../../analysis-services/scripting/properties/targettype-element-assl.md) élément parent **Action**. Le tableau suivant décrit les valeurs attendues de **Target** en fonction de la valeur de **TargetType**.  
  
|Valeur de TargetType|Valeur attendue|  
|----------------------|--------------------|  
|*Cube*|Nom d'un cube.|  
|*Cellules*|Expression de sous-cube.|  
|*Ensemble*|Expression d'ensemble ou nom d'un jeu nommé.<br /><br /> Remarque : Une instruction de sous-sélection ne peut pas être utilisée.|  
|*Hiérarchie, HierarchyMembers*|Nom d'une hiérarchie.|  
|*Dimension, DimensionMembers*|Nom d'une dimension.|  
|*Niveau de LevelMembers*|Nom d'un niveau.|  
|*Attribut, AttributeMembers*|Nom d'un attribut.|  
  
 L’élément qui correspond au parent de **cible** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.Action>.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
