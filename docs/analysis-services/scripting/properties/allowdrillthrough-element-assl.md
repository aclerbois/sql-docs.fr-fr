---
title: Élément AllowDrillThrough (ASSL) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: assl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 25bad363975494fc8bd6c8cb343a165d8bb85f85
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="allowdrillthrough-element-assl"></a>Élément AllowDrillThrough (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Détermine si l'extraction est autorisée sur l'élément parent.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<MiningModel> <!-- or MiningModelPermission -->  
<!-- or MiningStructurePermission -->   ...  
   <AllowDrillThrough>...</AllowDrillThrough>  
   ...  
</MiningModel>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Booléen|  
|Valeur par défaut|**False**|  
|Cardinalité|0-1 : Élément facultatif qui peut se produire une seule fois seulement.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Élément MiningModel](../../../analysis-services/scripting/objects/miningmodel-element-assl.md), [MiningModelPermission](../../../analysis-services/scripting/objects/miningmodelpermission-element-assl.md), [MiningStructurePermission](../../../analysis-services/scripting/objects/miningstructurepermission-element-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 Les éléments qui correspondent aux parents de **AllowDrillThrough** dans le modèle d’objet objets AMO (Analysis Management) sont <xref:Microsoft.AnalysisServices.MiningModel>, <xref:Microsoft.AnalysisServices.MiningModelPermission>, et <xref:Microsoft.AnalysisServices.MiningStructurePermission>.  
  
## <a name="drillthrough-on-mining-structures"></a>Extraction sur des structures d'exploration de données  
 Dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], vous pouvez définir **AllowDrillthrough** autorisations pour les structures d’exploration de données, ainsi que les modèles d’exploration de données. Lorsque vous assignez cette autorisation à un rôle, tout membre de ce rôle peut interroger le modèle d'exploration de données et retourner les colonnes de la structure qui n'étaient pas incluses dans le modèle. Par exemple, vous créez un modèle qui utilise uniquement les colonnes : clé du client, revenus du client et achats du client. Si vous activez l'extraction sur le modèle, les utilisateurs peuvent retourner des informations dans d'autres colonnes de la structure d'exploration de données, telles que les adresses de messagerie ou les noms des clients.  
  
 Par conséquent, pour protéger les données sensibles, soyez prudent lorsque vous ajoutez des colonnes à la structure d'exploration de données. Également, accordez l'autorisation **AllowDrillthrough** sur une structure seulement lorsqu'elle est requise.  
  
 Pour procéder à l'extraction des colonnes de la structure, utilisez une requête avec l'une des syntaxes suivantes :  
  
 `SELECT * FROM <structure>.CASES`  
  
 ou  
  
 `SELECT StructureColumn('<structure-column-name>') FROM <model>.CASES`  
  
## <a name="see-also"></a>Voir aussi  
 [Élément role & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/objects/role-element-assl.md)   
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
