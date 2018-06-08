---
title: Gestion des Solutions d’exploration de données et des objets | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ed8768dc456f1805b139138e8591f6f9749525eb
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="management-of-data-mining-solutions-and-objects"></a>Gestion des solutions et des objets d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] fournit des outils clients que vous pouvez utiliser pour gérer les structures et modèles d'exploration de données existants. Cette section décrit les opérations de gestion que vous pouvez effectuer avec chaque environnement.  
  
 Outre ces outils, vous pouvez gérer les objets d'exploration de données par programmation à l'aide d'AMO ou utiliser d'autres clients qui se connectent à une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , comme les Compléments d'exploration de données pour [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2007.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Déplacement d’objets d’exploration de données](../../analysis-services/data-mining/moving-data-mining-objects.md)  
  
 [Exigences et considérations concernant le traitement &#40;exploration de données&#41;](../../analysis-services/data-mining/processing-requirements-and-considerations-data-mining.md)  
  
 [Utilisation de SQL Server Profiler pour contrôler l’exploration de données &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/using-sql-server-profiler-to-monitor-data-mining-analysis-services-data-mining.md)  
  
## <a name="location-of-data-mining-objects"></a>Emplacement des objets d'exploration de données  
 Les structures et modèles d'exploration de données qui ont été traités sont stockés dans une instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 Si vous créez une connexion à une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en mode **immédiat** lors du développement des objets d'exploration de données, tous les objets que vous créez sont ajoutés immédiatement au serveur, au fur et à mesure que vous travaillez. Toutefois, si vous concevez des objets d'exploration de données en mode **hors connexion** , ce qui représente le paramètre par défaut lorsque vous travaillez dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], les objets d'exploration de données que vous créez ne sont que des conteneurs de métadonnées, tant que vous ne les avez pas déployés sur une instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Par conséquent, vous devez redéployer un objet sur le serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] chaque fois que vous y apportez une modification. Pour plus d’informations sur l’architecture d’exploration de données, consultez [Architecture physique &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/physical-architecture-analysis-services-data-mining.md).  
  
> [!NOTE]  
>  Certains clients, par exemple les compléments d’exploration de données pour [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)]2007, vous permettent également de créer des modèles et des structures d’exploration de données de la session qui utilisent une connexion à une instance, mais stockent la structure et les modèles d’exploration de données sur le serveur uniquement pendant la durée de la session. Vous pouvez toujours gérer ces modèles via le client, de la même façon que vous le feriez avec des structures et modèles stockés dans une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , mais les objets ne persistent pas une fois que vous êtes déconnecté de l'instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
## <a name="managing-data-mining-objects-in-sql-server-data-tools"></a>Gestion des objets d'exploration de données dans les outils de données SQL Server  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] propose de nombreuses fonctionnalités facilitant la création, l'exploration et la modification des objets d'exploration de données.  
  
 Les liens suivants permettent d'accéder à des informations sur la modification des objets d'exploration de données à l'aide de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]:  
  
-   [Modifier la vue de Source de données utilisée pour une Structure d’exploration de données](../../analysis-services/data-mining/edit-the-data-source-view-used-for-a-mining-structure.md)  
  
-   [Modifier les propriétés d’une Structure d’exploration de données](../../analysis-services/data-mining/change-the-properties-of-a-mining-structure.md)  
  
-   [Modifier les propriétés d’un modèle d’exploration de données](../../analysis-services/data-mining/change-the-properties-of-a-mining-model.md)  
  
-   [Afficher ou modifier la modélisation des indicateurs & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/view-or-change-modeling-flags-data-mining.md)  
  
-   [Afficher ou modifier les paramètres d'algorithme](../../analysis-services/data-mining/view-or-change-algorithm-parameters.md)  
  
 En général, vous utiliserez [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] comme outil pour développer de nouveaux projets et les ajouter à des projets existants, puis gérerez les projets et les objets qui ont été déployés à l'aide d'outils tels qu' [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Toutefois, vous pouvez modifier directement les objets déjà déployés sur une instance de ssASnoversion en utilisant l'option **Immediate** et en vous connectant au serveur en mode en ligne. Pour plus d'informations, consultez [Connect in Online Mode to an Analysis Services Database](../../analysis-services/multidimensional-models/connect-in-online-mode-to-an-analysis-services-database.md).  
  
> [!WARNING]  
>  Toutes les modifications apportées à une structure ou un modèle d'exploration de données, y compris les modifications apportées aux métadonnées, telles qu'un nom ou une description, nécessitent un retraitement de la structure ou du modèle.  
  
 Si vous ne disposez pas du fichier solution utilisé pour créer le projet ou les objets d'exploration de données, vous pouvez importer le projet existant à partir du serveur via l'Assistant Importation d'Analysis Services, modifier les objets, puis redéployer à l'aide de l'option **Incremental** . Pour plus d’informations, consultez [Importer un projet d’exploration de données à l’aide de l’Assistant Importation d’Analysis Services](../../analysis-services/data-mining/import-a-data-mining-project-using-the-analysis-services-import-wizard.md).  
  
## <a name="managing-data-mining-objects-in-sql-server-management-studio"></a>Gestion des objets d'exploration de données dans SQL Server Management Studio  
 Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez écrire des scripts, traiter ou supprimer des structures et modèles d'exploration de données. Vous ne pouvez afficher qu'un ensemble restreint de propriétés en utilisant l'Explorateur d'objets. Vous pouvez toutefois afficher des métadonnées supplémentaires sur les modèles d'exploration de données en ouvrant une fenêtre **Requête DMX** et en sélectionnant une structure d'exploration de données.  
  
-   [Créer une requête DMX dans SQL Server Management Studio](../../analysis-services/data-mining/create-a-dmx-query-in-sql-server-management-studio.md)  
  
## <a name="managing-data-mining-objects-programmatically"></a>Gestion des objets d'exploration de données par programmation  
 Vous pouvez créer, modifier, traiter et supprimer des objets d'exploration de données en utilisant les langages de programmation suivants. Chaque langage est conçu pour des tâches différentes. Par conséquent, il peut y avoir des restrictions quant au type d'opérations que vous pouvez effectuer. Par exemple, certaines propriétés d'objets d'exploration de données ne peuvent pas être modifiées à l'aide de DMX (Data Mining Extensions) ; vous devez utiliser XMLA ou AMO.  
  
### <a name="analysis-management-objects-amo"></a>Objets AMO (Analysis Management Objects)  
 Les objets AMO (Analysis Management Objects) sont un modèle objet reposant sur XMLA qui vous donne un contrôle total sur les objets d'exploration de données. En utilisant des objets AMO, vous pouvez créer, déployer et surveiller les structures et modèles d'exploration de données.  
  
-   [Concepts et modèle objet AMO](../../analysis-services/multidimensional-models/analysis-management-objects/amo-concepts-and-object-model.md)  
  
-   <xref:Microsoft.AnalysisServices>  
  
 **Restrictions :** Aucun.  
  
### <a name="data-mining-extensions-dmx"></a>DMX (Data Mining Extensions)  
 Data Mining Extensions (DMX) peut être utilisé avec d'autres interfaces de commandes telles que [!INCLUDE[vstecado](../../includes/vstecado-md.md)] ou ADOMD.NET pour créer, supprimer et interroger les structures et modèles d'exploration de données.  
  
-   [Instructions de définition de données DMX &#40;Data Mining Extensions&#41;](../../dmx/dmx-statements-data-definition.md)  
  
 **Restrictions :** Certaines propriétés ne peuvent pas être modifiées avec DMX.  
  
### <a name="xml-for-analysis-xmla"></a>XML for Analysis (XMLA)  
 XML for Analysis (XMLA) est le langage de définition de données utilisé pour l'ensemble d'Analysis Services. XMLA vous permet de contrôler la plupart des objets d'exploration de données et opérations de serveur. Toutes les opérations de gestion entre le client et le serveur peuvent être effectuées avec XMLA. Pour plus de commodité, vous pouvez utiliser le langage de script [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] (ASSL) pour inclure le code XML dans un wrapper.  
  
 **Restrictions :** [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] génère des instructions XMLA qui ne sont prises en charge qu'en utilisation interne. Elles ne peuvent pas être utilisées dans des scripts DDL XML.  
  
## <a name="see-also"></a>Voir aussi  
 [Documentation du développeur Analysis Services](../../analysis-services/analysis-services-developer-documentation.md)  
  
  
