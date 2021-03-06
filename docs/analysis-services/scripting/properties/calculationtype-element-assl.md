---
title: "Élément CalculationType (ASSL) | Documents Microsoft"
ms.custom: 
ms.date: 03/06/2017
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.service: 
ms.component: 
ms.reviewer: 
ms.suite: pro-bi
ms.technology: 
ms.tgt_pltfrm: 
ms.topic: reference
apiname: CalculationType Element
apilocation: http://schemas.microsoft.com/analysisservices/2003/engine
apitype: Schema
applies_to: SQL Server 2016 Preview
f1_keywords: CalculationType
helpviewer_keywords: CalculationType element
ms.assetid: b974b3d3-fbf7-4d77-8f6e-4e05a258fe84
caps.latest.revision: "37"
author: Minewiskan
ms.author: owend
manager: kfile
ms.workload: Inactive
ms.openlocfilehash: 0c5aab004dba06237d695a6534504736c1e01970
ms.sourcegitcommit: f486d12078a45c87b0fcf52270b904ca7b0c7fc8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2018
---
# <a name="calculationtype-element-assl"></a>Élément CalculationType (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]Décrit le type de calcul défini dans le type [CalculationProperty](../../../analysis-services/scripting/objects/calculationproperty-element-assl.md) élément.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<CalculationProperty>  
   ...  
   <CalculationType>...</CalculationType>  
   ...  
</CalculationProperty>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|None|  
|Cardinalité|1-1 : élément obligatoire qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[CalculationProperty](../../../analysis-services/scripting/objects/calculationproperty-element-assl.md)|  
|Éléments enfants|None|  
  
## <a name="remarks"></a>Notes   
 La valeur de cet élément est limitée à l'une des chaînes du tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|*Membre*|La propriété de calcul s'applique à une définition de membre calculé.|  
|*Ensemble*|La propriété de calcul s'applique à une définition de jeu nommé.|  
|*Cellules*|La propriété de calcul s'applique à une définition de cellule calculée.|  
  
 L’énumération qui correspond aux valeurs autorisées pour **CalculationType** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.CalculationType>.  
  
## <a name="see-also"></a>Voir aussi  
 [Élément CalculationProperties &#40; ASSL &#41;](../../../analysis-services/scripting/collections/calculationproperties-element-assl.md)   
 [Élément MdxScript &#40; ASSL &#41;](../../../analysis-services/scripting/objects/mdxscript-element-assl.md)   
 [Élément MdxScripts &#40; ASSL &#41;](../../../analysis-services/scripting/collections/mdxscripts-element-assl.md)   
 [Propriétés &#40; ASSL &#41;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
