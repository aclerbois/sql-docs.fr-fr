---
title: Élément session (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 02b4c93919e6354a59aad9b42a4f6dc8373c880f
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34577501"
---
# <a name="session-element-xmla"></a>Élément Session (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Utilise l’en-tête SOAP dans un message de demande SOAP pour identifier une session explicite existante sur une instance d’Analysis Services.  
  
 **Espace de noms** urn:schemas-microsoft-com:xml-analysis  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
   <soap:Header>  
      ...  
      <Session  
         xmlns="urn:schemas-microsoft-com:xml-analysis"  
         SessionId="string" />  
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
  
## <a name="attributes"></a>Attributs  
  
|Attribute|Description|  
|---------------|-----------------|  
|SessionId|Attribut **String** obligatoire qui identifie la session à employer. [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] fait appel à un identificateur global unique (GUID) pour identifier une session.|  
  
## <a name="remarks"></a>Notes  
 Le **Session** élément d’en-tête identifie une session existante démarrée explicitement sur le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance. L'élément **Session** fait partie de l'en-tête SOAP dans les types de messages suivants :  
  
-   Une réponse SOAP contenant un [BeginSession](../../../analysis-services/xmla/xml-elements-headers/beginsession-element-xmla.md) élément d’en-tête SOAP.  
  
-   Une demande SOAP pour identifier la session sur laquelle exécuter le [Discover](../../../analysis-services/xmla/xml-elements-methods-discover.md) ou [Execute](../../../analysis-services/xmla/xml-elements-methods-execute.md) (méthode).  
  
 Un identificateur de session ne garantit pas qu'une session reste valide. La session spécifiée dans l'élément **Session** peut expirer. Par exemple, une session peut expirer si la session dépasse le délai d'attente imparti ou si la connexion associée à la session est fermée. Si la session arrive à expiration et n'est plus valide, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] y met un terme et restaure toutes les transactions en cours de traitement. Tous les messages SOAP transmis avec un identificateur de session qui n'est plus valide échouent avec une erreur SOAP indiquant que la session spécifiée est introuvable.  
  
 Si un **Session** élément n’est pas envoyé dans le cadre d’une demande SOAP, le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance commence implicitement une session pendant la durée de la **Discover** ou **Execute** appel de méthode, puis se termine cette session une fois l’appel de méthode terminé.  
  
## <a name="see-also"></a>Voir aussi
 [Élément EndSession &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-headers/endsession-element-xmla.md)   
 [Gestion des connexions et Sessions &#40;XMLA&#41;](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/managing-connections-and-sessions-xmla.md)   
 [En-têtes &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-headers/xml-elements-headers.md)  
  
  
