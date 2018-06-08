---
title: Programmation d’exploration de données | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1be416528bc923e757afb9a8f3e556790941bf11
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="data-mining-programming"></a>Programmation de l'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Si vous estimez que les outils et composants intégrés dans [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] ne répondent pas à vos besoins, vous pouvez étendre la puissance de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] en codant vos propres extensions. Dans cette optique, deux options sont à votre disposition :  
  
-   **XMLA**  
  
     [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] prend en charge XML for Analysis (XMLA) comme protocole de communication avec les applications clientes. Des commandes supplémentaires qui étendent la spécification XML for Analysis sont prises en charge par [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].  
  
     Dans la mesure où [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] utilise XMLA pour la définition, la manipulation et le contrôle de données, vous pouvez créer des structures et des modèles d'exploration de données à l'aide des outils visuels fournis par [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], puis étendre les objets d'exploration de données que vous avez créés en utilisant des scripts DMX (Data Mining Extensions) et ASSL (Analysis Services Scripting Language).  
  
     Vous pouvez créer et modifier des objets d'exploration de données entièrement dans des scripts XMLA et exécuter par programme des requêtes de prédiction sur les modèles à partir de vos propres applications.  
  
-   **Analysis Management Objects (AMO)**  
  
     [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] propose également une infrastructure complète permettant aux fournisseurs d'exploration de données tiers d'intégrer les objets d'exploration de données dans [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].  
  
     Créez des structures et des modèles d'exploration de données à l'aide d'AMO. Consultez les exemples suivants dans CodePlex :  
  
    -   Navigateur d'indicateur de performance clé (AMO)  
  
         Se connecte à l'instance SSAS spécifiée et répertorie tous les objets serveur et leurs propriétés, notamment la structure d'exploration de données et les modèles d'exploration de données.  
  
    -   Exemple AMO simple  
  
         L'exemple AS simple couvre l'accès par programme à la plupart des objets principaux et illustre la navigation dans les métadonnées et l'accès aux valeurs des objets.  
  
         L'exemple illustre également la création et le traitement d'une structure et d'un modèle d'exploration de données, et la navigation dans un modèle d'exploration de données existant.  
  
-   **DMX**  
  
     Vous pouvez utiliser DMX pour encapsuler des instructions de commande, des requêtes de prédiction et des requêtes de métadonnées, et retourner les résultats dans un format tabulaire, si vous avez créé une connexion à un serveur [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].  
  
## <a name="in-this-section"></a>Dans cette section  
 [OLE DB pour l’exploration de données](../analysis-services/data-mining-programming-ole-db.md)  
 Décrit les ajouts possibles à la spécification de manière à prendre en charge l'exploration de données et les données multidimensionnelles : nouveaux ensembles de lignes et nouvelles colonnes pour les schémas, langage DMX (Data Mining Extension) pour la création et la gestion des structures d'exploration de données.  
  
## <a name="related-reference"></a>Référence connexe  
 [Développement avec ADOMD.NET](../analysis-services/multidimensional-models/adomd-net/developing-with-adomd-net.md)  
 Présente les clients et serveur de programmation d’objets ADOMD.NET.  
  
 [Développement avec Analysis Management Objects & #40 ; AMO & #41 ;](../analysis-services/multidimensional-models/analysis-management-objects/developing-with-analysis-management-objects-amo.md)  
 Présente la bibliothèque de programmation AMO.  
  
 [Développement avec Analysis Services Scripting Language & #40 ; ASSL & #41 ;](../analysis-services/multidimensional-models/scripting-language-assl/developing-with-analysis-services-scripting-language-assl.md)  
 Présente XML for Analysis (XMLA) et ses extensions.  
  
## <a name="see-also"></a>Voir aussi  
 [Documentation du développeur Analysis Services](../analysis-services/analysis-services-developer-documentation.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence](../dmx/data-mining-extensions-dmx-reference.md)  
  
  
