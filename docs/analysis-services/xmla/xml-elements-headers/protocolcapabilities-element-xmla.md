---
title: Élément ProtocolCapabilities (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 85ddeb22fac03e5ae7f66521ac3ca8a46e210fe7
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574781"
---
# <a name="protocolcapabilities-element-xmla"></a>Élément ProtocolCapabilities (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Utilise l’en-tête SOAP dans un message de demande SOAP pour identifier les fonctionnalités de protocole entre une instance d’Analysis Services et une application cliente.  
  
 **espace de noms** `http://schemas.microsoft.com/analysisservices/2003/engine`  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
   <soap:Header>  
      ...  
      <ProtocolCapabilities xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
         <Capability>...</Capability>  
      </ProtocolCapabilities>  
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
|Éléments enfants|[Fonctionnalité](../../../analysis-services/xmla/xml-elements-properties/capability-element-xmla.md)|  
  
## <a name="remarks"></a>Notes  
 Le **ProtocolCapabilities** élément permet aux applications de client négocier les fonctionnalités de protocole, telles que XML binaire ou de la prise en charge de la compression, avec un [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance à tout moment. La négociation de protocole implique les étapes suivantes :  
  
1.  L'application cliente identifie sa fonctionnalité de protocole en envoyant une demande SOAP qui inclut l'élément **ProtocolCapabilities** dans le cadre de l'en-tête SOAP.  
  
2.  L'instance [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] reçoit et traite la demande SOAP.  
  
3.  Si le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance a la même fonctionnalité de protocole que celle demandée, elle transmet une réponse SOAP qui inclut le même **ProtocolCapabilities** élément envoyé dans la demande SOAP, et le protocole a été négocié correctement. Dans le cas inverse, les fonctionnalités de protocole ne sont pas négociées comme il se doit et l'instance retourne une erreur SOAP.  
  
 Après la négociation des fonctionnalités de protocole, la durée pendant laquelle l’application cliente et le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] utilisation de l’instance un protocole spécifique dépend de la session implicite ou explicite :  
  
-   Une session explicite est créé à l’aide de la [BeginSession](../../../analysis-services/xmla/xml-elements-headers/beginsession-element-xmla.md) élément d’en-tête. Pour ce type de session, le protocole négocié est utilisé jusqu'à l'envoi d'un nouvel élément **ProtocolCapabilities** par l'application cliente ou jusqu'à la fin de la session.  
  
-   Une session implicite est créée par une instance [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Elle n'est pas définie de manière explicite par l'application cliente lorsque vous soumettez une demande SOAP. Dans le cadre de ce type de session, le protocole négocié est employé uniquement jusqu'à ce que la demande SOAP soit terminée.  
  
 Les fonctionnalités de protocole n'ont pas à être négociées de manière explicite, ce qui signifie qu'une application cliente n'a pas pour obligation d'inclure un élément **ProtocolCapabilities** dans le cadre de la demande SOAP. Si une demande SOAP n’inclut pas un **ProtocolCapabilities** élément, le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance répond en adoptant le même format que la demande SOAP.  
  
## <a name="see-also"></a>Voir aussi
 [Gestion des connexions et Sessions &#40;XMLA&#41;](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/managing-connections-and-sessions-xmla.md)   
 [En-têtes &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-headers/xml-elements-headers.md)  
  
  
