---
title: "Création multidimensionnelles de modèles à l’aide de SQL Server Data Tools (SSDT) | Documents Microsoft"
ms.custom: 
ms.date: 03/17/2017
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.service: 
ms.component: data-mining
ms.reviewer: 
ms.suite: pro-bi
ms.technology: 
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- SSAS, environments
- Analysis Services, development
- SQL Server Analysis Services, environments
- projects [Analysis Services]
- solutions [Analysis Services]
ms.assetid: 132ed779-3ec8-4734-9698-802116d1b017
caps.latest.revision: 
author: Minewiskan
ms.author: owend
manager: kfile
ms.workload: On Demand
ms.openlocfilehash: 805970696879c47c203deb3dc423300f5274aa1d
ms.sourcegitcommit: d8ab09ad99e9ec30875076acee2ed303d61049b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2018
---
# <a name="creating-multidimensional-models-using-sql-server-data-tools-ssdt"></a>Création de modèles MDX à l'aide des Outils de données SQL Server (SSDT)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit deux environnements différents pour générer, déployer et gérer des solutions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] : [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Ces deux environnements implémentent un système de projet. Pour plus d’informations sur les projets Visual Studio, consultez [Projets en tant que conteneurs](http://go.microsoft.com/fwlink/?LinkId=63960) dans MSDN Library.  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] est un environnement de développement, basé sur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio 2010, qui permet de créer et de modifier des solutions Business Intelligence. Avec [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], vous créez des projets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] contenant des définitions d’objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] (cubes, dimensions, etc.), qui sont stockés dans des fichiers XML contenant des éléments ASSL ( [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Scripting Language). Ces projets se trouvent dans des solutions qui peuvent aussi contenir des projets provenant d’autres composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , notamment [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], vous pouvez développer des projets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans le cadre d’une solution qui n’est associée à aucune instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] spécifique. Vous pouvez déployer les objets vers une instance d’un serveur de test afin de tester le projet au cours du développement, puis utiliser le même projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour déployer vos objets vers des instances d’un ou de plusieurs serveurs intermédiaires ou de production. Les projets et éléments appartenant à une solution qui inclut [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peuvent être intégrés avec un système de contrôle de code source, tel que [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual SourceSafe. Pour plus d’informations sur la création d’un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] à l’aide de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consultez [Créer un projet Analysis Services &#40;SSDT&#41;](../../analysis-services/multidimensional-models/create-an-analysis-services-project-ssdt.md). [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] permet également de se connecter directement à une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour créer et modifier des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , sans utiliser un projet et sans stocker les définitions d'objets dans des fichiers XML. Pour plus d’informations, consultez [des bases de données Model multidimensionnelles ](../../analysis-services/multidimensional-models/multidimensional-model-databases-ssas.md), et [se connecter en Mode en ligne une base de données Analysis Services](../../analysis-services/multidimensional-models/connect-in-online-mode-to-an-analysis-services-database.md).  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un environnement de gestion et d’administration utilisé principalement pour administrer des instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Avec [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez gérer des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] (effectuer des sauvegardes, des traitements, etc.), mais également créer des objets directement sur une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante à l’aide de scripts XMLA. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] fournit un projet de scripts du serveur d’analyse dans lequel vous pouvez développer et enregistrer des scripts écrits dans une syntaxe MDX (Multidimensional Expressions), DMX (Data Mining Extensions) et XMLA (XML for Analysis). En général, les projets Scripts du serveur d’analyse servent à effectuer des tâches de gestion ou à recréer des objets, tels que des bases de données et des cubes, sur des instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Ces projets peuvent être enregistrés au sein d'une solution et intégrés avec un système de contrôle de code source. Pour plus d’informations sur la création d’un projet Scripts du serveur d’analyse dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] à l’aide de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consultez [Projet de script Analysis Services dans SQL Server Management Studio](../../analysis-services/instances/analysis-services-scripts-project-in-sql-server-management-studio.md).  
  
## <a name="introducing-solutions-projects-and-items"></a>Présentation des solutions, projets et éléments  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] fournissent tous deux des projets qui sont organisés en solutions. Une solution peut contenir plusieurs projets tandis qu'un projet contient généralement plusieurs éléments. Une nouvelle solution est automatiquement générée lorsque vous créez un projet et vous pouvez ajouter, si nécessaire, des projets supplémentaires à une solution existante. Les objets contenus dans un projet dépendent du type du projet. Les éléments figurant dans chaque conteneur de projet sont enregistrés en tant que fichiers dans les dossiers du projet dans le système de fichiers.  
  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] contient les projets suivants pour le type de projet Projets Business Intelligence.  
  
|Projet| Description|  
|-------------|-----------------|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Projet|Contient les définitions d’objets d’une seule base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Pour plus d’informations sur la création d’un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , consultez [Créer un projet Analysis Services &#40;SSDT&#41;](../../analysis-services/multidimensional-models/create-an-analysis-services-project-ssdt.md).|  
|Importer une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 2008|Fournit un Assistant qui vous permet de créer un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en important des définitions d’objets à partir d’une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante.|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Projet|Contient les définitions d’objets d’un ensemble de packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d’informations, consultez [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md).|  
|Assistant Projet Report Server|Fournit un Assistant qui vous guide dans le processus de création d'un projet de rapport à l'aide de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour plus d’informations, consultez [Reporting Services &#40;SSRS&#41;](../../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md).|  
|Projet de modèle de rapport|Contient les définitions d'objets d'un modèle de rapport [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour plus d’informations, consultez [Reporting Services &#40;SSRS&#41;](../../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md).|  
|Projet Report Server|Contient les définitions d'objets d'un ou plusieurs rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour plus d’informations, consultez [Reporting Services &#40;SSRS&#41;](../../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md).|  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] contient également plusieurs types de projets axés sur divers scripts ou requêtes, tel qu’indiqué dans le tableau suivant.  
  
|Projet| Description|  
|-------------|-----------------|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Scripts|Contient des scripts DMX, MDX et XMLA pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], ainsi que des connexions à des instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur lesquelles ces scripts peuvent être exécutés. Pour plus d’informations, consultez [Projet de script Analysis Services dans SQL Server Management Studio](../../analysis-services/instances/analysis-services-scripts-project-in-sql-server-management-studio.md).|  
|Scripts SQL Server Compact|Contient des scripts SQL pour SQL Server Compact, ainsi que des connexions aux instances SQL Server Compact sur lesquelles ces scripts peuvent être exécutés.|  
|Scripts SQL Server|Contient des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] et XQuery pour une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] , ainsi que des connexions à des instances du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] sur lesquelles ces scripts peuvent être exécutés. Pour plus d’informations, consultez [Moteur de base de données SQL Server](../../database-engine/configure-windows/sql-server-database-engine.md).|  
  
 Pour plus d’informations sur les solutions et les projets, consultez « Gestion des solutions, des projets et des fichiers » dans la documentation de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] .NET ou dans MSDN Library.  
  
## <a name="choosing-between-sql-server-management-studio-and-sql-server-data-tools"></a>Choix entre SQL Server Management Studio et les outils de données SQL Server  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est conçu pour l’administration et la configuration d’objets existants dans [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] est conçu pour le développement de solutions Business Intelligence qui incluent des fonctionnalités [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Voici quelques-unes des différences existant entre [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] fournit un environnement intégré pour se connecter à des instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] afin de configurer, de gérer et d’administrer des objets dans une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. À l’aide de scripts, vous pouvez également utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour créer ou modifier des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Cependant, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ne fournit aucune interface graphique pour créer et définir des objets.  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] fournit un environnement de développement intégré pour le développement de solutions Business Intelligence. Vous pouvez utiliser [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en mode projet, qui utilise les définitions XML des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] contenus dans les projets et les solutions. Quand [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] est utilisé dans ce mode, les modifications apportées aux objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] sont appliquées aux définitions de ces objets XML et ne sont pas appliquées aux objets d’une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tant que la solution n’est pas déployée. Vous pouvez également utiliser [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en mode en ligne, c'est-à-dire avec connexion directe à une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et utilisation des objets d'une base de données existante.  
  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] améliore le développement d’applications Business Intelligence, car vous pouvez travailler sur des projets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans un environnement multi-utilisateur sous contrôle de code source sans avoir besoin d’une connexion active à une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] fournit un accès direct aux objets existants pour effectuer des requêtes et des tests, et peut être utilisé pour implémenter plus rapidement des bases de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ayant déjà fait l’objet d’un script. Toutefois, après avoir déployé un projet dans l’environnement de production, vous devez observer la plus grande attention lors de l’utilisation d’une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et de ses objets avec [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Cela permet d'éviter d'écraser les modifications apportées directement aux objets dans une base de données existante, ainsi que celles apportées au projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui a initialement généré la solution déployée. Pour plus d’informations, consultez [Utilisation de projets et de bases de données Analysis Services en phase de développement](../../analysis-services/multidimensional-models/work-with-analysis-services-projects-and-databases-in-development.md)et [Utilisation de projets et de bases de données Analysis Services dans un environnement de production](../../analysis-services/multidimensional-models/work-with-analysis-services-projects-and-databases-in-production.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Créer un projet Analysis Services &#40;SSDT&#41;](../../analysis-services/multidimensional-models/create-an-analysis-services-project-ssdt.md)  
  
-   [Configurer les propriétés d’un projet Analysis Services &#40;SSDT&#41;](../../analysis-services/multidimensional-models/configure-analysis-services-project-properties-ssdt.md)  
  
-   [Générer des projets Analysis Services &#40; SSDT &#41;](../../analysis-services/multidimensional-models/build-analysis-services-projects-ssdt.md)  
  
-   [Déployer des projets Analysis Services &#40;SSDT&#41;](../../analysis-services/multidimensional-models/deploy-analysis-services-projects-ssdt.md)  
  
-   [Utilisation de projets et de bases de données Analysis Services en phase de développement](../../analysis-services/multidimensional-models/work-with-analysis-services-projects-and-databases-in-development.md)  
  
-   [Utilisation de projets et de bases de données Analysis Services dans un environnement de production](../../analysis-services/multidimensional-models/work-with-analysis-services-projects-and-databases-in-production.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Créer un projet Analysis Services &#40;SSDT&#41;](../../analysis-services/multidimensional-models/create-an-analysis-services-project-ssdt.md)   
 [Projet de Scripts Analysis Services dans SQL Server Management Studio](../../analysis-services/instances/analysis-services-scripts-project-in-sql-server-management-studio.md)   
 [Bases de données de modèle multidimensionnel ](../../analysis-services/multidimensional-models/multidimensional-model-databases-ssas.md)  
  
  
