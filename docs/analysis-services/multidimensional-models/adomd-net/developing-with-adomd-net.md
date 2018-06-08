---
title: Développement avec ADOMD.NET | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 794692334fdee4d3cbc87fc9ccf1c2313dcc1262
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="developing-with-adomdnet"></a>Développement avec ADOMD.NET
  ADOMD.NET est un [!INCLUDE[msCoName](../../../includes/msconame-md.md)] fournisseur de données .NET Framework est conçu pour communiquer avec [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. ADOMD.NET utilise le protocole XMLA (XML for Analysis) pour communiquer avec les sources de données analytiques en utilisant des connexions TCP/IP ou HTTP pour transmettre et recevoir des demandes et des réponses SOAP conformes à la spécification XML for Analysis. Les commandes peuvent être envoyées dans une syntaxe MDX (Multidimensional Expressions), DMX (Data Mining Extensions), ASSL (Analysis Services Scripting Language), voire une syntaxe limitée de SQL ; il se peut qu'elles ne retournent pas de résultats. Les données analytiques, les indicateurs de performance clés et les modèles d'exploration de données peuvent être interrogés et manipulés par le biais du modèle objet ADOMD.NET. ADOMD.NET vous permet également de consulter et d'utiliser les métadonnées en récupérant des ensembles de lignes de schéma compatibles OLE DB ou en utilisant le modèle objet ADOMD.NET.  
  
 Le fournisseur de données ADOMD.NET est représenté par le **Microsoft.AnalysisServices.AdomdClient** espace de noms.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Programmation du client ADOMD.NET](../../../analysis-services/multidimensional-models-adomd-net-client/adomd-net-client-programming.md)|Explique comment utiliser les objets clients ADOMD.NET pour récupérer des données et des métadonnées à partir de sources de données analytiques.|  
|[Programmation du serveur ADOMD.NET](../../../analysis-services/multidimensional-models-adomd-net-server/adomd-net-server-programming.md)|Explique comment utiliser les objets serveur ADOMD.NET pour créer des procédures stockées et des fonctions définies par l'utilisateur.|  
|[Redistribution d’ADOMD.NET](../../../analysis-services/multidimensional-models/adomd-net/redistributing-adomd-net.md)|Décrit le processus de redistribution d'ADOMD.NET.|  
|<xref:Microsoft.AnalysisServices.AdomdClient>|Décrit les objets qui sont contenus dans le **Microsoft.AnalysisServices.AdomdClient** espace de noms.|  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions multidimensionnelles & #40 ; MDX & #41 ; Référence](../../../mdx/multidimensional-expressions-mdx-reference.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence](../../../dmx/data-mining-extensions-dmx-reference.md)   
 [Ensembles de lignes de schéma de Analysis Services](../../../analysis-services/schema-rowsets/analysis-services-schema-rowsets.md)   
 [Développement avec Analysis Services Scripting Language &#40;ASSL&#41;](../../../analysis-services/multidimensional-models/scripting-language-assl/developing-with-analysis-services-scripting-language-assl.md)   
 [Accès aux données de modèle multidimensionnel &#40;Analysis Services - données multidimensionnelles&#41;](../../../analysis-services/multidimensional-models/mdx/multidimensional-model-data-access-analysis-services-multidimensional-data.md)  
  
  
