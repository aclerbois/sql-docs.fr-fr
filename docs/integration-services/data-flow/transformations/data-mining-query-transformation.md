---
title: Requête d’exploration de données, transformation | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataminingquerytrans.f1
- sql13.dts.designer.dmquerytransformation.miningmodel.f1
- sql13.dts.designer.dmquerytransformation.query.f1
helpviewer_keywords:
- Data Mining Query transformation
- prediction queries [Integration Services]
ms.assetid: 7960133b-a3e1-48af-ba43-55ed78c38e71
caps.latest.revision: 43
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0a49d5b2f6217040dd513895a2eb3c0e633fcc33
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-mining-query-transformation"></a>transformation de requête d'exploration de données
  La transformation de requête d'exploration de données effectue des requêtes de prédiction par rapport aux modèles d'exploration de données. Cette transformation contient un générateur de requêtes qui permet de créer des requêtes DMX (Data Mining Extensions). Le générateur de requêtes vous permet de créer des instructions personnalisées afin d'évaluer les données d'entrée de la transformation par rapport à un modèle d'exploration de données existant à l'aide du langage DMX. Pour plus d’informations, consultez [Guide de référence du langage DMX & #40;Data Mining Extensions&#41;](../../../dmx/data-mining-extensions-dmx-reference.md).  
  
 Une transformation peut exécuter plusieurs requêtes de prédiction si les modèles sont basés sur la même structure d'exploration de données. Pour plus d’informations, consultez [Outils de requête d’exploration de données](../../../analysis-services/data-mining/data-mining-query-tools.md).  
  
## <a name="configuration-of-the-data-mining-query-transformation"></a>Configuration de la transformation de requête d’exploration de données  
 La transformation de requête d’exploration de données utilise un gestionnaire de connexions [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] pour se connecter au projet [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] ou à l’instance de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] qui contient la structure et les modèles d’exploration de données. Pour plus d'informations, consultez [Analysis Services Connection Manager](../../../integration-services/connection-manager/analysis-services-connection-manager.md).  
  
 Cette transformation a une entrée et une sortie. Elle ne prend pas en charge de sortie d'erreur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Pour plus d’informations sur la définition des propriétés, consultez [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="data-mining-query-transformation-editor-mining-model-tab"></a>Éditeur de transformation de requête d'exploration de données (onglet Modèle d'exploration de données)
  Utilisez l'onglet **Modèle d'exploration de données** de l' **Éditeur de transformation de requête d'exploration de données** pour sélectionner la structure d'exploration de données et ses modèles d'exploration de données.  
  
### <a name="options"></a>Options  
 **Connexion**  
 Sélectionnez une connexion Analysis Services en utilisant la zone de liste, ou créez une connexion en utilisant le bouton **Nouvelle** comme indiqué ci-dessous.  
  
 **Nouveau**  
 Créez une connexion à l’aide de la boîte de dialogue **Ajout d’un gestionnaire de connexions Analysis Services** .  
  
 **Structure d'exploration de données**  
 Sélectionnez dans la liste des structures de modèles d'exploration de données.  
  
 **Modèles d'exploration de données**  
 Affiche la liste des modèles d’exploration de données associés à la structure d’exploration de données sélectionnée.  
  
## <a name="data-mining-query-transformation-editor-query-tab"></a>Éditeur de transformation de requête d'exploration de données (onglet Requête)
  Utilisez l'onglet **Requête** de la boîte de dialogue **Éditeur de transformation de requête d'exploration de données** pour créer une requête de prédiction.  
  
### <a name="options"></a>Options  
 **Requête d'exploration de données**  
 Tapez une requête DMX (Data Mining Extensions) directement dans la zone de texte.  
  
 **Générer une nouvelle requête**  
 Cliquez sur **Générer une nouvelle requête** pour créer une requête DMX (Data Mining Extensions) à l’aide du générateur graphique de requêtes.  
  
