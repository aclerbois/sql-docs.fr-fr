---
title: Élément Return (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e8746fb9f8b397ef50b1a5c66a2132e5f0cf5c87
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34576191"
---
# <a name="return-element-xmla"></a>Élément return (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Contient des informations retournées par une [DiscoverResponse](../../../analysis-services/xmla/xml-elements-objects-discoverresponse.md) élément en réponse à une [Discover](../../../analysis-services/xmla/xml-elements-methods-discover.md) appel de méthode ou un [ExecuteResponse](../../../analysis-services/xmla/xml-elements-objects-executeresponse.md) élément en réponse à une [Execute](../../../analysis-services/xmla/xml-elements-methods-execute.md) appel de méthode.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<DiscoverResponse> <!-- or ExecuteResponse -->  
   <return>  
      <root>...</root>  
      <!-- or -->  
      <results>...</results> <!-- ExecuteResponse only -->  
   </return>  
</DiscoverResponse>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|None|  
|Valeur par défaut|None|  
|Cardinalité|1-1 : élément requis qui apparaît une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[DiscoverResponse](../../../analysis-services/xmla/xml-elements-objects-discoverresponse.md), [ExecuteResponse](../../../analysis-services/xmla/xml-elements-objects-executeresponse.md)|  
|Éléments enfants|Consultez le tableau ci-dessous.|  
  
|Ancêtre|Éléments enfants|  
|--------------|--------------------|  
|[DiscoverResponse](../../../analysis-services/xmla/xml-elements-objects-discoverresponse.md)|[racine](../../../analysis-services/xmla/xml-elements-properties/root-element-xmla.md)|  
|[ExecuteResponse](../../../analysis-services/xmla/xml-elements-objects-executeresponse.md)|[racine](../../../analysis-services/xmla/xml-elements-properties/root-element-xmla.md) ou [résultats](../../../analysis-services/xmla/xml-elements-properties/results-element-xmla.md)|  
  
## <a name="remarks"></a>Notes  
 Le **retourner** élément contient les données retournées par le **Discover** et **Execute** méthodes. En règle générale, les **retourner** élément contienne un seul **racine** élément qui contient les données retournées par une réussite **Discover** ou **Execute** appel de méthode ou un XML pour l’exception de Analysis (XMLA) retournée par un appel de méthode échoue. Si le **Execute** méthode contient un **lot** commande qui effectue plusieurs opérations, le **retourner** élément contient un **résultats** élément qui, à son tour, contient un **racine** élément pour chaque commande exécutée avec succès ou non par le **lot** commande.  
  
## <a name="see-also"></a>Voir aussi
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
