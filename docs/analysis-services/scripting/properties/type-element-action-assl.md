---
title: Type d’élément (Action) (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: 8b87ab109aaad0c40e1debb3a8ea84047b6e8f6b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="type-element-action-assl"></a>Élément Type (Action) (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Contient le type de la [Action](../../../analysis-services/scripting/objects/action-element-assl.md) élément.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Action>  
   ...  
   <Type>...</Type>  
   ...  
</Action>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|Aucune|  
|Cardinalité|1-1 : élément requis qui apparaît une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Élément parent|[Action](../../../analysis-services/scripting/objects/action-element-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 La valeur de cet élément est limitée à l'une des chaînes répertoriées dans le tableau suivant.  
  
|Valeur| Description|  
|-----------|-----------------|  
|*URL*|Affiche une page qui varie selon le cas dans un navigateur Internet.|  
|*HTML*|Exécute un script HTML dans un navigateur Internet.|  
|*Instruction*|Exécute une commande OLE DB.|  
|*Extraction*|Récupère un ensemble de lignes pour l'extraction.<br /><br /> Cette valeur est identique à *ensemble de lignes* et identifie les actions d’extraction. Il peut uniquement être utilisée sur les actions dont [TargetType](../../../analysis-services/scripting/properties/targettype-element-assl.md) a la valeur *cellules*.|  
|*jeu de données*|Récupère un dataset.|  
|*Rowset*|Récupère un ensemble de lignes.|  
|*Ligne de commande*|Exécute une commande à une invite de commandes.|  
|*Propriétaire*|Effectue une opération en utilisant une interface différente de celles répertoriées plus haut dans ce tableau.|  
|*Rapport*|Affiche une page qui varie selon le cas dans un navigateur Internet.<br /><br /> Cette valeur est identique à *Url* et identifie les actions de rapport.|  
  
 L’élément qui correspond au parent de **Type** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.Action>.  
  
## <a name="see-also"></a>Voir aussi  
 [Type de données DrillThroughAction &#40;ASSL&#41;](../../../analysis-services/scripting/data-type/drillthroughaction-data-type-assl.md)   
 [Type de données ReportAction &#40;ASSL&#41;](../../../analysis-services/scripting/data-type/reportaction-data-type-assl.md)   
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
