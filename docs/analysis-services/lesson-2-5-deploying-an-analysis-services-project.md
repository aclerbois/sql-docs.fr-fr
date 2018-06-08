---
title: Déploiement d’une analyse des Services de projet | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 16952381ad550cac079a8919b395186ec6b5f337
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-2-5---deploying-an-analysis-services-project"></a>Leçon 2-5-déploiement d’un projet Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

Pour afficher les données du cube et de dimension pour les objets du cube Didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] dans le projet Didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], vous devez déployer le projet sur une instance spécifiée d'[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], puis traiter le cube et ses dimensions. Le *déploiement* d'un projet [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] entraîne la création des objets définis dans une instance d'[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]. Le*traitement* des objets dans une instance de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] copie les données à partir des sources de données sous-jacentes dans les objets du cube. Pour plus d’informations, consultez [Déployer des projets Analysis Services &#40;SSDT&#41;](../analysis-services/multidimensional-models/deploy-analysis-services-projects-ssdt.md) et [Configurer les propriétés d’un projet Analysis Services &#40;SSDT&#41;](../analysis-services/multidimensional-models/configure-analysis-services-project-properties-ssdt.md).  
  
À ce stade du processus de développement, vous déployez généralement le cube sur une instance d' [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] sur un serveur de développement. À la fin du développement de votre projet Business Intelligence, vous utiliserez généralement l'Assistant Déploiement d' [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] pour le déployer à partir du serveur de développement sur un serveur de production. Pour plus d’informations, consultez [Déploiement d’une solution de modèle multidimensionnel](../analysis-services/multidimensional-models/multidimensional-model-solution-deployment.md) et [Déployer des solutions de modèles à l’aide de l’assistant Déploiement](../analysis-services/multidimensional-models/deploy-model-solutions-using-the-deployment-wizard.md).  
  
Au cours de la tâche suivante, vous allez vérifier les propriétés de déploiement du projet Didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , puis déployer le projet sur votre instance locale d' [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].  
  
### <a name="to-deploy-the-analysis-services-project"></a>Pour déployer le projet Analysis Services  
  
1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **Didacticiel Analysis Services** , puis cliquez sur **Propriétés**.  
  
    La boîte de dialogue **Pages de propriétés de Didacticiel Analysis Services** s’affiche et présente les propriétés de la configuration (développement) active. Vous pouvez définir plusieurs configurations, chacune avec des propriétés différentes. Par exemple, un développeur peut souhaiter configurer un même projet pour qu'il soit déployé sur des ordinateurs de développement différents et avec des propriétés de déploiement différentes, telles que des noms de bases de données différents ou des propriétés de traitement différentes. Notez la valeur de la propriété **Output Path** . Cette propriété spécifie l'emplacement dans lequel sont enregistrés les scripts de déploiement XMLA du projet lorsqu'un projet est créé. Ces scripts sont ceux qui sont utilisés pour déployer les objets dans le projet sur une instance d' [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].  
  
2.  Dans le nœud **Propriétés de configuration** dans le volet gauche, cliquez sur **Déploiement**.  
  
    Vérifiez les propriétés de déploiement du projet. Par défaut, le modèle de projet [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] configure un projet [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] pour déployer de façon incrémentielle tous les projets sur l'instance par défaut d' [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] sur l'ordinateur local, pour créer une base de données [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] avec le même nom que le projet, et pour traiter les objets après le déploiement en utilisant l'option de traitement par défaut. Pour plus d’informations, consultez [Configurer les propriétés d’un projet Analysis Services &#40;SSDT&#41;](../analysis-services/multidimensional-models/configure-analysis-services-project-properties-ssdt.md).  
  
    > [!NOTE]  
    > Si vous souhaitez déployer le projet dans une instance nommée de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] sur l’ordinateur local, ou à une instance sur un serveur distant, modifiez le **Server** nom de la propriété à l’instance appropriée, tel que \<  *Nom_serveur**>\\<** InstanceName ** >*.  
  
3.  Cliquez sur **OK**.  
  
4.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **Analysis Services Tutorial** , puis cliquez sur **Déployer**. Cela peut prendre un certain temps.  
  
    > [!NOTE]  
    > Si vous obtenez des erreurs pendant le déploiement, utilisez SQL Server Management Studio pour vérifier les autorisations relatives à la base de données. Le compte que vous avez spécifié pour la connexion à la source de données doit avoir une connexion sur l'instance SQL Server. Double-cliquez sur la connexion pour consulter les propriétés de mappage des utilisateurs. Le compte doit avoir les autorisations db_datareader sur la base de données **AdventureWorksDW2012** .  
  
    [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] génère et déploie le projet Didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] sur l'instance spécifiée de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] à l'aide d'un script de déploiement. La progression du déploiement apparaît dans deux fenêtres : la fenêtre **Sortie** et la fenêtre **État d’avancement du déploiement – Didacticiel Analysis Services** .  
  
    Ouvrez la fenêtre de sortie, si nécessaire, en cliquant sur **Sortie** dans le menu **Affichage** . La fenêtre **Sortie** affiche la progression globale du déploiement. La fenêtre **État d’avancement du déploiement – Analysis Services Tutorial** affiche le détail de chaque étape du déploiement. Pour plus d’informations, consultez [Générer des projets Analysis Services &#40;SSDT&#41;](../analysis-services/multidimensional-models/build-analysis-services-projects-ssdt.md) et [Déployer des projets Analysis Services &#40;SSDT&#41;](../analysis-services/multidimensional-models/deploy-analysis-services-projects-ssdt.md).  
  
5.  Examinez le contenu des fenêtres **Sortie** et **État d’avancement du déploiement – Analysis Services Tutorial** pour vérifier que le cube a été généré, déployé et traité sans erreurs.  
  
6.  Pour masquer la fenêtre **État d’avancement du déploiement - Analysis Services Tutorial** , cliquez sur l’icône **Masquer automatiquement** (en forme de punaise) dans la barre d’outils de la fenêtre.  
  
7.  Pour masquer la fenêtre **Sortie** , cliquez sur l’icône **Masquer automatiquement** dans la barre d’outils de la fenêtre.  
  
Vous avez correctement déployé le cube Didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] sur votre instance locale d' [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], puis traité le cube déployé.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Exploration du Cube](../analysis-services/lesson-2-6-browsing-the-cube.md)  
  
## <a name="see-also"></a>Voir aussi  
[Déployer des projets Analysis Services & #40 ; SSDT & #41 ;](../analysis-services/multidimensional-models/deploy-analysis-services-projects-ssdt.md)  
[Configurer les propriétés d’un projet Analysis Services &#40;SSDT&#41;](../analysis-services/multidimensional-models/configure-analysis-services-project-properties-ssdt.md)  
  
  
  
