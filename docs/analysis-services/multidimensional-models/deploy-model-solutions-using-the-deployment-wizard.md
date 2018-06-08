---
title: Déployer des Solutions de modèle à l’aide de l’Assistant Déploiement | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cde312f2bfad67c6cfe61a9b8379973c1f59a56c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="deploy-model-solutions-using-the-deployment-wizard"></a>Deploy Model Solutions Using the Deployment Wizard
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  Le [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Assistant de déploiement utilise les fichiers de sortie JSON générés à partir d’un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] projet sous forme de fichiers d’entrée. Il est facile de modifier ces fichiers d'entrée pour personnaliser le déploiement d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Le script de déploiement généré peut alors être immédiatement exécuté ou enregistré en vue d'un déploiement ultérieur.  

> [!NOTE]
> Le [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Assistant/utilitaire de déploiement est installé avec [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) (SSMS). Veillez à l’aide de la version la plus récente. Si l’exécution à partir de l’invite de commandes, par défaut, la dernière version de l’Assistant de déploiement est installée vers C:\Program Files (x86) \Microsoft SQL Server\140\Tools\Binn\ManagementStudio. 
  
 Vous pouvez déployer un projet à l'aide de l'Assistant comme l'explique la présente rubrique. Vous pouvez aussi automatiser le déploiement ou utiliser la fonctionnalité de Synchronisation. Si la base de données déployée est de grande taille, envisagez d'utiliser des partitions sur les systèmes cibles. Vous pouvez automatiser la création de partition et de remplissage à l’aide d’objets AMO (Analysis Management), Scriting langage TMSL (Tabular Model) et le modèle d’objet tabulaire (TOM).  
  
> [!IMPORTANT]  
>  Les fichiers de sortie, ni le script de déploiement contient l’id utilisateur ou le mot de passe si elles sont spécifiées dans la chaîne de connexion pour une source de données ou à des fins d’emprunt d’identité. Dans la mesure où ces informations sont nécessaires pour le traitement dans ce scénario, vous devez les ajouter manuellement. Si le déploiement n'inclut pas de traitement, vous pouvez ajouter ces informations de connexion et d'emprunt d'identité en fonction de vos besoins après le déploiement. Si le déploiement inclut un traitement, vous pouvez ajouter ces informations dans l'Assistant ou dans le script de déploiement une fois qu'il a été enregistré.  
  
## <a name="in-this-section"></a>Dans cette section  
 Les rubriques suivantes expliquent comment utiliser l'Assistant Déploiement de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , les fichiers d'entrée et le script de déploiement :  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[L’Assistant Déploiement de Analysis Services en cours d’exécution](../../analysis-services/multidimensional-models/running-the-analysis-services-deployment-wizard.md)|Décrit les différentes façons d'exécuter l'Assistant Déploiement de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
|[Précisions sur les fichiers d’entrée utilisés pour créer le Script de déploiement](../../analysis-services/multidimensional-models/deployment-script-files-input-used-to-create-deployment-script.md)|Décrit les fichiers que l'Assistant Déploiement de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise comme valeurs d'entrée et leur contenu, en fournissant des liens à des rubriques qui expliquent comment modifier les valeurs stockées dans chacun des fichiers d'entrée.|  
|[Comprendre le Script de déploiement d’Analysis Services](../../analysis-services/multidimensional-models/understanding-the-analysis-services-deployment-script.md)|Décrit le contenu du script de déploiement et explique comment il s'exécute.|  
  
## <a name="see-also"></a>Voir aussi  
 [Déployer des Solutions de modèle à l’aide de XMLA](../../analysis-services/multidimensional-models/deploy-model-solutions-using-xmla.md)   
 [Synchroniser les bases de données Analysis Services](../../analysis-services/multidimensional-models/synchronize-analysis-services-databases.md)   
 [Précisions sur les fichiers d’entrée utilisés pour créer le Script de déploiement](../../analysis-services/multidimensional-models/deployment-script-files-input-used-to-create-deployment-script.md)   
 [Déployer des Solutions de modèle avec l’utilitaire de déploiement](../../analysis-services/multidimensional-models/deploy-model-solutions-with-the-deployment-utility.md)  
  
  
