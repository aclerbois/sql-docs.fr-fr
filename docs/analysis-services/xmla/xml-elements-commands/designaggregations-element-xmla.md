---
title: Élément DesignAggregations (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b32a7ecaf7c8268b88d3fc417cc1e41e024bfd2f
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574131"
---
# <a name="designaggregations-element-xmla"></a>Élément DesignAggregations (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Crée des agrégations pour une conception d’agrégation sur une instance Analysis Services.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Command>  
   <DesignAggregations>  
      <Object>...</Object>  
      <Time>...</Time>  
      <Steps>...</Steps>  
      <Optimization>...</Optimization>  
      <Storage>...</Storage>  
      <Materialize>...</Materialize>  
      <Queries>...</Queries>  
   </DesignAggregations>  
</Command>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|None|  
|Valeur par défaut|None|  
|Cardinalité|0-n : élément facultatif pouvant apparaître plusieurs fois.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Commandee](../../../analysis-services/xmla/xml-elements-properties/command-element-xmla.md)|  
|Éléments enfants|[Matérialiser](../../../analysis-services/xmla/xml-elements-properties/materialize-element-xmla.md), [objet](../../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md), [optimisation](../../../analysis-services/xmla/xml-elements-properties/optimization-element-xmla.md), [requêtes](../../../analysis-services/xmla/xml-elements-properties/queries-element-xmla.md), [étapes](../../../analysis-services/xmla/xml-elements-properties/steps-element-xmla.md), [stockage](../../../analysis-services/xmla/xml-elements-properties/storage-element-xmla.md) , [Heure](../../../analysis-services/xmla/xml-elements-properties/time-element-xmla.md)|  
  
## <a name="remarks"></a>Notes  
 La commande **DesignAggregations** est utilisée pour générer des définitions d'agrégation stockées par une conception d'agrégation. Une conception d'agrégation peut être utilisée ensuite pour matérialiser des agrégations pour une partition et peut être réutilisée entre des partitions.  
  
## <a name="see-also"></a>Voir aussi
 [Commandes &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-commands/xml-elements-commands.md)  
  
  
