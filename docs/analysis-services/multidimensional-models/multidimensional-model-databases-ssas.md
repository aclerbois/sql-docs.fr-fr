---
title: Bases de données de modèle multidimensionnel (SSAS) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1ec839c9638b3baf79cba0148cc932ead7820f7b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="multidimensional-model-databases-ssas"></a>Bases de données de modèle multidimensionnel (SSAS)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est une collection de sources de données, de vues de source de données, de cubes, de dimensions et de rôles. Éventuellement, une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peut inclure des structures pour l'exploration de données et des assemblys personnalisés qui permettent d'ajouter des fonctions définies par l'utilisateur à la base de données.  
  
 Les cubes sont les objets de requête fondamentaux dans Analysis Services. Lorsque vous vous connectez à une base de données Analysis Services via une application cliente, vous vous connectez à un cube dans cette base de données. Une base de données peut contenir plusieurs cubes si vous réutilisez les dimensions, les assemblys, les rôles ou les structures d'exploration de données dans plusieurs contextes.  
  
 Vous pouvez créer et modifier une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] par programmation ou à l'aide de l'une des méthodes interactives suivantes :  
  
-   Déployer un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à partir de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] dans une instance désignée d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Ce processus crée une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , s'il n'existe aucune base de données de ce nom dans cette instance, et instancie les objets désignés dans la base de données nouvellement créée. Si vous utilisez une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], les modifications apportées aux objets dans le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prennent effet uniquement lorsque le projet est déployé dans une instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
-   Créez une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] vide dans une instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], puis connectez-vous directement à cette base de données à l’aide de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et créez des objets dans cette dernière (plutôt que dans un projet). Si vous utilisez une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de cette façon, les modifications apportées aux objets prennent effet dans la base de données à laquelle vous vous connectez lorsque vous enregistrez l'objet modifié.  
  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] utilise l'intégration avec le logiciel de contrôle de code source pour prendre en charge plusieurs développeurs qui utilisent différents objets simultanément dans un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Un développeur peut également interagir directement avec une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , plutôt que dans un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Cependant, les objets d'une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peuvent devenir désynchronisés avec le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui a été utilisé pour son déploiement. Une fois le déploiement effectué, vous pouvez administrer une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez également apporter certaines modifications à une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], notamment aux partitions et aux rôles. Cela peut également entraîner la désynchronisation des objets d'une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] avec le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui a été utilisé pour son déploiement.  
  
## <a name="related-tasks"></a>Tâches associées  
 [Attacher et détacher des bases de données Analysis Services](../../analysis-services/multidimensional-models/attach-and-detach-analysis-services-databases.md)  
  
 [Sauvegarde et restauration de bases de données Analysis Services](../../analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases.md)  
  
 [Document and Script d’une base de données Analysis Services](../../analysis-services/multidimensional-models/document-and-script-an-analysis-services-database.md)  
  
 [Modifier ou supprimer une base de données Analysis Services](../../analysis-services/multidimensional-models/modify-or-delete-an-analysis-services-database.md)  
  
 [Déplacer une base de données Analysis Services](../../analysis-services/multidimensional-models/move-an-analysis-services-database.md)  
  
 [Renommer une base de données multidimensionnelle &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/rename-a-multidimensional-database-analysis-services.md)  
  
 [Niveau de compatibilité d’une base de données multidimensionnelle &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/compatibility-level-of-a-multidimensional-database-analysis-services.md)  
  
 [Définir les propriétés de base de données multidimensionnelle &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/set-multidimensional-database-properties-analysis-services.md)  
  
 [Synchroniser les bases de données Analysis Services](../../analysis-services/multidimensional-models/synchronize-analysis-services-databases.md)  
  
 [Basculer une base de données Analysis Services entre les modes ReadOnly et ReadWrite](../../analysis-services/multidimensional-models/switch-an-analysis-services-database-between-readonly-and-readwrite-modes.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Connexion en mode en ligne à une base de données Analysis Services](../../analysis-services/multidimensional-models/connect-in-online-mode-to-an-analysis-services-database.md)   
 [Créer un projet Analysis Services &#40;SSDT&#41;](../../analysis-services/multidimensional-models/create-an-analysis-services-project-ssdt.md)   
 [Interrogation de données multidimensionnelles avec MDX](../../analysis-services/multidimensional-models/mdx/querying-multidimensional-data-with-mdx.md)  
  
  
