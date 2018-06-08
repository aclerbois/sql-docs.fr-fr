---
title: Outils d’exploration de données | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 70d08ebe145e64f21299f5e46d33bbc7ae41536f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="data-mining-tools"></a>Outils d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit les outils suivants que vous pouvez utiliser pour créer des solutions d'exploration de données :  
  
-   L' **Assistant Exploration de données** dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] vous permet de créer facilement des structures et des modèles d'exploration de données basés sur des sources de données relationnelles ou des données mutidimensionnelles dans des cubes.  
  
     Dans l'Assistant, choisissez les données à utiliser, puis appliquez des techniques spécifiques d'exploration de données, telles que le clustering, les réseaux neuronaux ou la modélisation de série chronologique.  
  
-   Les**visionneuses de modèle** sont fournies à la fois dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], pour la consultation de vos modèles d'exploration de données après leur création.  Vous pouvez parcourir des modèles à l'aide de visionneuses adaptées à chaque algorithme, ou approfondir l'analyse à l'aide de la visionneuse de contenu du modèle.  
  
-   Le **Générateur de requêtes de prédiction** est fourni à la fois dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour vous aider à créer des requêtes de prédiction. Vous pouvez également tester la précision des modèles sur un jeu de données d'exclusion ou des données externes, ou utiliser la validation croisée pour évaluer la qualité de votre ensemble de données.  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est l'interface où vous gérez des solutions d'exploration de données existantes déployées dans une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Vous pouvez retraiter des structures et des modèles pour mettre à jour les données qu'ils contiennent.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contient des outils que vous pouvez utiliser pour nettoyer les données, pour automatiser des tâches telles que la création de prédictions et la mise à jour de modèles, et créer des solutions d'exploration de texte.  
  
 Les sections suivantes fournissent des informations complémentaires sur les outils d'exploration de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="data-mining-wizard"></a>Assistant Exploration de données  
 Utilisez l'Assistant Exploration de données pour commencer la création de solutions d'exploration de données. Cet Assistant est facile et rapide à utiliser, et vous guide tout au long du processus de création d'une structure d'exploration de données et d'un modèle d'exploration de données initial qui lui est associé ; il inclut les tâches de sélection d'un type d'algorithme et d'une source de données, ainsi que la définition des données de cas utilisées pour l'analyse.  
  
 **Pour plus d’informations** : [Assistant Exploration de données &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/data-mining-wizard-analysis-services-data-mining.md).  
  
## <a name="data-mining-designer"></a>Concepteur d'exploration de données  
 Après avoir créé une structure et un modèle d'exploration de données à l'aide de l'Assistant Exploration de données, vous pouvez utiliser le Concepteur d'exploration de données depuis [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour utiliser les structures et les modèles existants.  
  
 Le concepteur contient des outils permettant de réaliser ces tâches :  
  
-   Modifier les propriétés des structures d'exploration de données, ajouter des colonnes et créer des alias de colonne, modifier la méthode de placement dans un conteneur ou la distribution attendue des valeurs.  
  
-   Ajouter de nouveaux modèles à une structure existante ; copier les modèles, modifier les propriétés de modèle ou les métadonnées, ou définir des filtres sur un modèle d'exploration de données.  
  
-   Parcourir des schémas et des règles dans le modèle ; explorer les associations ou les arbres de décision. Obtenir des statistiques détaillées  
  
     Les visionneuses personnalisées sont fournies pour chaque heure différente du modèle, afin de vous aider à analyser vos données et à explorer les schémas indiqués par l'exploration de données.  
  
-   Valider les modèles en créant des graphiques de courbes d'élévation ou en analysant la courbe de bénéfices concernant les modèles. Comparer des modèles à l'aide de matrices de classification, ou valider un ensemble de données et ses modèles à l'aide de la validation croisée.  
  
-   Créer des prédictions et des requêtes de contenu sur des modèles d'exploration de données existants. Générer des requêtes uniques ou configurer des requêtes afin de générer des prédictions pour des tables entières de données externes.  
  
## <a name="sql-server-management-studio"></a>SQL Server Management Studio  
 Après avoir créé et déployé des modèles d'exploration de données sur un serveur, vous pouvez utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour gérer la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui héberge les objets d'exploration de données. Vous pouvez également continuer à exécuter des tâches qui utilisent le modèle, telles que l'exploration des modèles, le traitement de nouvelles données et la création de prédictions.  
  
 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] contient également les éditeurs de requêtes que vous pouvez utiliser pour concevoir et exécuter des requêtes DMX (data mining extensions), ou pour travailler avec des objets d’exploration de données à l’aide de XMLA.  
  
## <a name="integration-services-data-mining-tasks-and-transformations"></a>Tâches et transformations d'exploration de données d'Integration Services  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fournit de nombreux composants qui prennent en charge l'exploration de données.  
  
 Certains outils dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sont conçus pour vous aider à automatiser des tâches courantes d'exploration de données, y compris la prédiction, la création de modèles et le traitement. Par exemple :  
  
-   Créez un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui met automatiquement à jour le modèle chaque fois que le dataset est actualisé avec de nouveaux clients  
  
-   Exécutez la segmentation ou l'échantillonnage personnalisé des enregistrements de cas.  
  
-   Générez automatiquement des modèles transmis aux paramètres.  
  
 Toutefois, vous pouvez également utiliser l'exploration de données dans un flux de travail de package, en tant qu'entrée à d'autres processus. Par exemple :  
  
-   Utilisez les valeurs de probabilité générées par le modèle pour pondérer les scores pour l'exploration de texte ou d'autres tâches de classification.  
  
-   Générez automatiquement des prédictions basées sur des données antérieures et utilisez ces valeurs pour évaluer la validité de nouvelles données.  
  
-   Utilisation de la régression logistique pour segmenter les clients entrants par risque.  
  
 **Pour plus d’informations** [Projets connexes pour des solutions d’exploration de données](../../analysis-services/data-mining/related-projects-for-data-mining-solutions.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Référence DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-reference.md)   
 [Tâches liées aux modèles d’exploration de données et procédures](../../analysis-services/data-mining/mining-model-tasks-and-how-tos.md)   
 [Tâches de la visionneuse modèle d’exploration de données et de procédures](../../analysis-services/data-mining/mining-model-viewer-tasks-and-how-tos.md)   
 [Solutions d’exploration de données](../../analysis-services/data-mining/data-mining-solutions.md)  
  
  
