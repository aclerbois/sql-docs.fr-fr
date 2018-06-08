---
title: Élément DiscretizationMethod (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: 81064689788574061f103b973a5435beb3e83893
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discretizationmethod-element-assl"></a>Élément DiscretizationMethod (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Définit la méthode à utiliser pour la discrétisation.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<DimensionAttribute> <!-- or ScalarMiningStructureColumn -->  
   ...  
   <DiscretizationMethod>...</DiscretizationMethod>  
   ...  
</DimensionAttribute>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|*Aucun*|  
|Cardinalité|0-1: élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[DimensionAttribute](../../../analysis-services/scripting/data-type/dimensionattribute-data-type-assl.md), [ScalarMiningStructureColumn](../../../analysis-services/scripting/data-type/scalarminingstructurecolumn-data-type-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 La valeur de l'élément **DiscretizationMethod** détermine comment les valeurs de l'élément **DimensionAttribute** ou **ScalarMiningStructureColumn** sont discrétisées ou organisées en un jeu de groupes spécifique. Pour plus d’informations sur les méthodes de discrétisation, consultez [méthodes de discrétisation &#40;d’exploration de données&#41;](../../../analysis-services/data-mining/discretization-methods-data-mining.md).  
  
 La valeur de cet élément est limitée à l'une des chaînes du tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|*Automatique*|Équivaut à la méthode de discretisation AUTOMATIC pour les colonnes de structure d'exploration de données.|  
|*EqualAreas*|Équivaut à la méthode de discretisation EQUAL_AREAS pour les colonnes de structure d'exploration de données.|  
|*Clusters*|Équivaut à la méthode de discretisation CLUSTERS pour les colonnes de structure d'exploration de données.|  
|*Seuils*|Équivaut à la méthode de discretisation THRESHOLDS pour les colonnes de structure d'exploration de données.|  
|*EqualRanges*|Équivaut à la méthode de discretisation EQUAL_RANGES pour les colonnes de structure d'exploration de données.|  
  
 L’énumération qui correspond aux valeurs autorisées pour **DiscretizationMethod** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.DiscretizationMethod>.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
