---
title: Type d’élément (Dimension) (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: 3af3168c39f685702154bb7c76435bd1d241a642
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="type-element-dimension-assl"></a>Élément Type (Dimension) (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Fournit des informations sur le contenu de la dimension.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Dimension>  
      ...  
   <Type>...</Type>  
   ...  
</Dimension>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|*Regular*|  
|Cardinalité|0-1: élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Élément parent|[Dimension](../../../analysis-services/scripting/objects/dimension-element-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 Certaines valeurs de **Type**, par exemple *Accounts*, déterminent un comportement spécifique.  
  
 La valeur de cet élément est limitée à l'une des chaînes répertoriées dans le tableau suivant.  
  
|Valeur| Description|  
|-----------|-----------------|  
|*Regular*|La dimension est une dimension régulière.|  
|*Time*|La dimension est une dimension de temps.<br /><br /> Remarque : Cette valeur indique que la dimension prend en charge les fonctionnalités spécifiques aux dimensions de temps.|  
|*Geography*|La dimension contient des attributs géographiques.|  
|*Organisation*|La dimension contient des attributs d'organisation.|  
|*BillOfMaterials*|La dimension contient des attributs de nomenclature.|  
|*Comptes (Accounts)*|La dimension contient des attributs liés au compte.<br /><br /> Remarque : Cette valeur indique que la dimension prend en charge les fonctionnalités spécifiques aux dimensions de compte.|  
|*Customers*|La dimension contient des attributs liés au client.|  
|*Produits*|La dimension contient des attributs liés au produit.|  
|*Scénario*|La dimension contient des attributs liés au scénario.|  
|*Quantitative*|La dimension contient des attributs quantitatifs.|  
|*Utilitaire*|La dimension contient des attributs d'utilitaire.|  
|*Monétaire (Currency)*|La dimension contient des attributs de devise.|  
|*Taux de*|La dimension contient des attributs de taux de change.|  
|*Channel*|La dimension contient des attributs de canal.|  
|*Promotion*|La dimension contient des attributs liés à la promotion.|  
  
 L’énumération qui correspond aux valeurs autorisées pour **Type** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.DimensionType>.  
  
 L’élément qui correspond au parent de **Type** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.Dimension>.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
