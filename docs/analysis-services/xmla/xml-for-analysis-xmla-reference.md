---
title: Référence XML for Analysis (XMLA) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4e8c3645175d8d1f3251fd621825f5a542669cbb
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34576761"
---
# <a name="xml-for-analysis--xmla-reference"></a>Référence XML for Analysis (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]   

Azure Analysis Services et SQL Server Analysis Services utilisent XML pour le protocole Analysis (XMLA) pour les communications entre les applications clientes et une instance Analysis Services. Au niveau de base, d'autres bibliothèques clientes telles que ADOMD.NET et AMO génèrent des requêtes et décodent les réponses en XMLA, servant d'intermédiaires à une instance d'Analysis Services qui utilise exclusivement XMLA.  
  
 Pour prendre en charge la détection et la manipulation des données en mode tabulaire et multidimensionnel, la spécification XMLA définit deux méthodes généralement accessibles, [Discover](../../analysis-services/xmla/xml-elements-methods-discover.md) et [Execute](../../analysis-services/xmla/xml-elements-methods-execute.md)et un collection de types d’éléments et les données XML. Du fait que XML autorise l'exploitation d'une architecture client et serveur faiblement couplée, ces deux méthodes gèrent les informations entrantes et sortantes au format XML. 

Analysis Services est compatible avec la spécification XMLA 1.1, la spécification, mais l’étend pour inclure la fonctionnalité définition et manipulation de données, implémentée sous forme d’annotations sur le **Discover** et **Execute** méthodes. La syntaxe XML étendue est TMSL Tabular Model Scripting Language () et Analysis Services Scripting Language (ASSL). 

TMSL est la syntaxe de définition de modèle de commande et l’objet pour les bases de données de modèle tabulaire au niveau de compatibilité 1200 et supérieur. TMSL communique avec Analysis Services via le protocole XMLA, où le `XMLA.Execute` méthode accepte les deux scripts TMSL instruction basée sur JSON, ainsi que les scripts basés sur XML traditionnels dans Analysis Services Scripting Language (ASSL de XMLA). Pour plus d’informations, consultez [TMSL Tabular Model Scripting Language () référence](../tabular-model-scripting-language-tmsl-reference.md).

ASSL est la syntaxe de définition de modèle de commande et d’objet pour les bases de données de modèle multidimensionnel et en mode tabulaire au niveau de compatibilité 1103 ou inférieur. Cette définition s’appuie sur la spécification XMLA sans la décomposer. L'interopérabilité basée sur XMLA est garantie si vous utilisez uniquement XMLA ou XMLA et ASSL. Pour plus d’informations, consultez [Analysis Services Scripting Language (ASSL de XMLA)](../scripting/analysis-services-scripting-language-assl-for-xmla.md).
  
 En tant que développeur, vous pouvez utiliser XMLA en tant qu’interface si les spécifications de solution spécifient des protocoles standard tels que XML, SOAP et HTTP. Les développeurs et administrateurs peuvent également utiliser XMLA sur une base ad hoc pour récupérer des informations à partir du serveur ou exécuter des commandes. 
  
## <a name="in-this-section"></a>Contenu de cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Types de données XML (XMLA)](../../analysis-services/xmla/xml-data-types/xml-data-types-xmla.md)|Décrit des types de données dans la spécification XMLA.|  
|[Éléments XML - commandes (XMLA)](../../analysis-services/xmla/xml-elements-commands/xml-elements-commands.md)|Éléments qui peuvent être utilisés dans l’élément de commande lors d’un appel de la méthode Execute.|  
|[Éléments XML - commandes (XMLA)](../../analysis-services/xmla/xml-elements-commands/xml-elements-commands.md)|Éléments qui peuvent être utilisés dans l’élément de commande lors d’un appel de la méthode Execute.|  
|[Éléments XML - en-têtes (XMLA)](../../analysis-services/xmla/xml-elements-headers/xml-elements-headers.md)| Éléments d’en-tête implémentés par Microsoft Analysis Services.|  
|[Éléments XML - propriétés (XMLA)](../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)| Éléments pour représenter des informations sur les propriétés et valeurs pour les types d’en-têtes, les méthodes, les objets, les commandes et les données XMLA.|  
|[Les éléments XML - méthodes - découvrir (XMLA)](../../analysis-services/xmla/xml-elements-methods-discover.md)| Récupère les informations, telles que la liste des bases de données disponibles ou des détails concernant un objet spécifique, à partir d’une instance d’Analysis Services.|  
|[Les éléments XML - méthodes - exécutent (XMLA)](../../analysis-services/xmla/xml-elements-methods-execute.md)| Envoie le code XML pour les commandes Analysis (XMLA) à une instance d’Analysis Services.|  
|[XML éléments - objets - DiscoverResponse (XMLA)](../../analysis-services/xmla/xml-elements-objects-discoverresponse.md)| Contient les informations retournées par une instance d’Analysis Services en réponse à un appel de méthode de découverte.|  
|[XML éléments - objets - ExecuteResponse (XMLA)](../../analysis-services/xmla/xml-elements-objects-executeresponse.md)| Contient les informations retournées par une instance d’Analysis Services en réponse à un appel de méthode d’exécution.|  
|[Éléments XML - objets (XMLA)](../../analysis-services/xmla/xml-elements-objects.md)| Objets implémentés par Analysis Services.|  
|[Conformité XML for Analysis (XMLA)](../../analysis-services/xmla/xml-for-analysis-compliance-xmla.md)|Décrit le niveau de compatibilité avec la spécification XMLA 1.1.|  
  
## <a name="see-also"></a>Voir aussi
 [Ensembles de lignes de schéma XML for Analysis](../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)  
 [Développement avec ADOMD.NET](../../analysis-services/multidimensional-models/adomd-net/developing-with-adomd-net.md)  
 [Développement avec AMO &#40;Analysis Management Objects&#41;](../../analysis-services/multidimensional-models/analysis-management-objects/developing-with-analysis-management-objects-amo.md)  
