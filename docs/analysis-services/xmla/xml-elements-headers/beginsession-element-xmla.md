---
title: Élément BeginSession (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 34bdcfb37eaf5e2f960b7ea282c2628eca91916f
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574841"
---
# <a name="beginsession-element-xmla"></a>Élément BeginSession (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Utilise un en-tête SOAP dans un message de demande SOAP pour démarrer une nouvelle session sur une instance d’Analysis Services.  
  
 **Espace de noms** urn:schemas-microsoft-com:xml-analysis  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
   <soap:Header>  
      ...  
      <BeginSession  
         xmlns="urn:schemas-microsoft-com:xml-analysis" />  
      ...  
   </soap:Header>  
   <soap:Body>  
      ...  
   </soap:Body>  
</soap:Envelope>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|None|  
|Valeur par défaut|None|  
|Cardinalité|0-1 : élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|None|  
|Éléments enfants|None|  
  
## <a name="remarks"></a>Notes  
 Le **BeginSession** élément d’en-tête fait partie d’une demande SOAP envoyée à un [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance et démarre explicitement une nouvelle session sur l’instance. L’en-tête SOAP retourné par la réponse SOAP contient un [Session](../../../analysis-services/xmla/xml-elements-headers/session-element-xmla.md) élément qui identifie la nouvelle session. Ce nouvel identificateur de session est stocké et envoyé dans les demandes SOAP suivantes à l’aide de la **Session** élément d’en-tête.  
  
 Si le **BeginSession** élément d’en-tête n’est pas envoyée, une session n’est pas démarrée explicitement. Si une session n'est pas démarrée explicitement, les transactions sur cette session ne peuvent pas être gérées. En d’autres termes, vous ne pouvez pas utiliser le code XML suivant pour les commandes Analysis (XMLA) : [BeginTransaction](../../../analysis-services/xmla/xml-elements-commands/begintransaction-element-xmla.md), [CommitTransaction](../../../analysis-services/xmla/xml-elements-commands/committransaction-element-xmla.md), et [RollbackTransaction](../../../analysis-services/xmla/xml-elements-commands/rollbacktransaction-element-xmla.md). Toutes les méthodes et les commandes XMLA exécutées sur une session démarrée implicitement sont considérées comme des transactions atomiques.  
  
## <a name="see-also"></a>Voir aussi
 [Élément EndSession &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-headers/endsession-element-xmla.md)   
 [Élément session &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-headers/session-element-xmla.md)   
 [Gestion des connexions et Sessions &#40;XMLA&#41;](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/managing-connections-and-sessions-xmla.md)   
 [En-têtes &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-headers/xml-elements-headers.md)  
  
  
