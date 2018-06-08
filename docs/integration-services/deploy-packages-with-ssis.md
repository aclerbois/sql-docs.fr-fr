---
title: Déployer des packages avec SSIS | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: get-started-article
helpviewer_keywords:
- deployment tutorial [Integration Services]
- deploying packages [Integration Services]
- SSIS, tutorials
- Integration Services, tutorials
- deploying packages [Integration Services], installing
- SQL Server Integration Services, tutorials
- walkthroughs [Integration Services]
- deployment utility [Integration Services]
- deploying packages [Integration Services], configurations
ms.assetid: de18468c-cff3-48f4-99ec-6863610e5886
caps.latest.revision: 27
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e2fe1e201717808c8a6afef4e3ca756100b614a8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deploy-packages-with-ssis"></a>Déployer des packages avec SSIS
[!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] propose des outils qui simplifient le déploiement des packages vers un autre ordinateur. Ces outils de déploiement gèrent aussi les dépendances, telles que les configurations et les fichiers dont les packages ont besoin. Dans ce didacticiel, vous allez apprendre à utiliser ces outils pour installer des packages et leurs dépendances sur un ordinateur cible.    
    
Pour commencer, vous allez effectuer les tâches de préparation du déploiement. Vous allez créer un nouveau projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] et ajouter des packages et des fichiers de données existants au projet. Vous n'allez pas créer de nouveaux packages entièrement ; en revanche, vous allez travailler uniquement avec des packages finalisés et créés spécialement pour ce didacticiel. Vous n'allez pas modifier les fonctionnalités des packages de ce didacticiel ; cependant, une fois que les packages sont ajoutés au projet, il vous sera peut-être utile d'ouvrir les packages dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] et d'examiner le contenu de chaque package. Cette opération vous permet d'obtenir des informations sur les dépendances de package telles que les fichiers journaux et d'autres fonctionnalités intéressantes des packages.    
    
En préparation du déploiement, vous allez aussi mettre à jour les packages pour utiliser des configurations. Les configurations permettent de mettre à jour les propriétés des packages et les objets de package au moment de l'exécution. Dans ce didacticiel, vous allez utiliser des configurations pour mettre à jour les chaînes de connexion des fichiers texte et journaux et les emplacements des fichiers XML et XSD utilisés par le package. Pour plus d’informations, consultez [Configurations de package](../integration-services/packages/package-configurations.md) et [Créer des configurations de package](../integration-services/packages/create-package-configurations.md).    
    
Après avoir vérifié que les packages s'exécutent correctement dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], vous allez créer l'application de déploiement nécessaire pour installer les packages. Cette application de déploiement se compose des fichiers de package et des autres éléments que vous avez ajoutés au projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , des dépendances du package qu'intègre automatiquement [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] et de l'utilitaire de déploiement que vous avez créé. Pour plus d’informations, consultez [Créer un utilitaire de déploiement](../integration-services/packages/create-a-deployment-utility.md).    
    
Vous allez copier ensuite l'application de déploiement sur l'ordinateur cible et exécuter l'Assistant Installation de package pour installer les packages et les dépendances de package. Les packages seront installés dans la base de données msdb de SQL Server et les fichiers auxiliaires et de prise en charge seront installés dans le système de fichiers. Comme les packages déployés utilisent des configurations, vous allez mettre à jour la configuration pour utiliser des nouvelles valeurs qui permettent aux packages de s'exécuter correctement dans le nouvel environnement.    
    
Enfin, vous allez exécuter les packages dans [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] à l'aide de l'utilitaire d'exécution de package.    
    
L'objectif de ce didacticiel est de simuler la complexité de problèmes de déploiement réels que vous pouvez rencontrer. Cependant, si vous ne pouvez pas déployer les packages sur un autre ordinateur, vous pouvez toujours effectuer ce didacticiel en installant les packages dans la base de données msdb d'une instance locale de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], puis en exécutant les packages à partir de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] sur la même instance.    
    
## <a name="what-you-will-learn"></a>Contenu du didacticiel    
Le meilleur moyen de se familiariser avec les nouveaux outils et les nouvelles commandes et fonctionnalités de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] est de les utiliser. Ce didacticiel vous guide dans les étapes de création d'un projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] puis d'ajout des packages et autres fichiers nécessaires au projet. Une fois le projet terminé, vous allez créer une application de déploiement, copier cette application sur l'ordinateur de destination, puis installer les packages sur l'ordinateur de destination.    
    
## <a name="requirements"></a>Spécifications    
Ce didacticiel s’adresse aux utilisateurs qui ont une connaissance des notions fondamentales liées aux opérations effectuées sur les systèmes de fichiers, mais une maîtrise limitée des nouvelles fonctionnalités disponibles dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Pour mieux comprendre les concepts [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] de base que vous allez mettre en œuvre dans ce didacticiel, il est peut-être intéressant de terminer d’abord le didacticiel [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] suivant : [SSIS : comment créer un package ETL](../integration-services/ssis-how-to-create-an-etl-package.md).    
    
**Ordinateur source.** Les composants suivants **doivent être installés**sur l’ordinateur sur lequel vous allez créer l’application de déploiement :
- SQL Server  
- Exemples de données, packages finalisés, configurations et fichier LisezMoi. Ces fichiers sont installés ensemble si vous téléchargez les [exemples de bases de données Adventure Works 2014](https://msftdbprodsamples.codeplex.com/releases/view/125550).     
> **Remarque** Vérifiez que vous êtes autorisé à créer et à supprimer des tables dans AdventureWorks ou d’autres données que vous utilisez.         
    
-   [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md).    
    
**Ordinateur de destination.** Les composants suivants **doivent être installés**sur l’ordinateur vers lequel vous déployez des packages :    
    
- SQL Server
- Exemples de données, packages finalisés, configurations et fichier LisezMoi. Ces fichiers sont installés ensemble si vous téléchargez les [exemples de bases de données Adventure Works 2014](https://msftdbprodsamples.codeplex.com/releases/view/125550). 
    
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).    
    
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].    
    
-   Vous devez être autorisé à créer et à supprimer des tables dans AdventureWorks, et à exécuter des packages dans [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)].    
    
-   Vous devez disposer de l'autorisation d'accès en lecture et en écriture sur la table sysssispackages dans la base de données système msdb de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .    
    
Si vous envisagez de déployer les packages sur le même ordinateur que celui où vous créez l'application de déploiement, ce dernier doit avoir la configuration requise pour les ordinateurs source et de destination.    
    
**Durée estimée pour effectuer ce didacticiel :** 2 heures.    
    
## <a name="lessons-in-this-tutorial"></a>Leçons du didacticiel    
[Leçon 1 : Préparation à la création de l’application de déploiement](../integration-services/lesson-1-preparing-to-create-the-deployment-bundle.md)    
Au cours de cette leçon, vous allez vous préparer à déployer une solution ETL en créant un nouveau projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] et en ajoutant au projet les packages et les autres fichiers requis.    
    
[Leçon 2 : Créer l’application de déploiement dans SSIS](../integration-services/lesson-2-create-the-deployment-bundle-in-ssis.md)    
Au cours de cette leçon, vous allez élaborer un utilitaire de déploiement et vérifier que l'application de déploiement inclut les fichiers nécessaires.    
    
[Leçon 3 : Installer des packages SSIS](../integration-services/lesson-3-install-ssis-packages.md)    
Vous allez aussi copier l'application de déploiement sur l'ordinateur cible, installer les packages, puis les exécuter.    
    

