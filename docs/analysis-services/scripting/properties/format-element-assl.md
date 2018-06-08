---
title: Mettre en forme l’élément (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: 34220d27f4a0ca1f30b67b5066f5a06877816652
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="format-element-assl"></a>Élément Format (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Contient le format requis de la [DataItem](../../../analysis-services/scripting/data-type/dataitem-data-type-assl.md) élément.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<DataItem>  
   ...  
   <Format>...</Format>  
      ...  
</DataItem>  
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
|Élément parent|[DataItem](../../../analysis-services/scripting/data-type/dataitem-data-type-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 Les valeurs autorisées pour l'élément **Format** sont les formats Microsoft Office Excel et les chaînes *TrimRight*, *TrimLeft*, *TrimAll*et *TrimNone*. Pour la troncature, le paramètre par défaut est *TrimRight* .  
  
 La valeur de cet élément est limitée à l'une des chaînes du tableau suivant.  
  
|Valeur| Description|  
|-----------|-----------------|  
|Toute chaîne de format Excel|Les données sont mises en forme conformément à la chaîne de format nommée ou personnalisée spécifiée. Toutes les chaînes de format prises en charge par Excel peuvent être fournies.|  
|*TrimAll*|Les données sont tronquées à gauche et à droite.|  
|*TrimLeft*|Les données sont tronquées à gauche.|  
|*TrimNone*|Les données ne sont pas tronquées.|  
|*TrimRight*|Les données sont tronquées à droite.|  
  
 L’élément qui correspond au parent de **Format** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.DataItem>.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
