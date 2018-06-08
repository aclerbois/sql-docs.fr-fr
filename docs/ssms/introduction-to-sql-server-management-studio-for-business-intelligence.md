---
title: Introduction à SQL Server Management Studio pour BI | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio for Integration Services
- SQL Server Management Studio for Reporting Services
- SQL Server Management Studio for Analysis Services
ms.assetid: ffaa77b7-03d0-4d7a-aa42-c5091a4f2ceb
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 823a2ed8a1a4c81587d71d1646e19312c0afea8f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="introduction-to-sql-server-management-studio-for-business-intelligence"></a>Introduction à SQL Server Management Studio pour Business Intelligence
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Pour accéder, configurer, gérer et administrer [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)], [!INCLUDE[ssISnoversion](../includes/ssisnoversion_md.md)] et [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion_md.md)], utilisez [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)]. Même si les trois technologies de décisionnel reposent sur [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)], les tâches d'administration associées à chacune d'entre elle sont légèrement différentes.  
  
> [!NOTE]  
> Pour créer et modifier des solutions [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion_md.md)]et [!INCLUDE[ssISnoversion](../includes/ssisnoversion_md.md)] , utilisez [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull_md.md)], et non [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)]. [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull_md.md)] est un environnement de développement basé sur [!INCLUDE[msCoName](../includes/msconame_md.md)][!INCLUDE[vsprvs](../includes/vsprvs_md.md)].  
  
## <a name="managing-analysis-services-solutions-using-sql-server-management-studio"></a>Gestion des solutions Analysis Services à l'aide de SQL Server Management Studio  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)] vous permet de gérer des objets [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] , par exemple effectuer des sauvegardes et le traitement d’objets.  
  
[!INCLUDE[ssManStudio](../includes/ssmanstudio_md.md)] fournit un projet Script [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] dans lequel vous pouvez développer et enregistrer des scripts écrits dans une syntaxe MDX (Multidimensional Expressions) et XMLA (XML for Analysis). Ces projets Script [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] servent à effectuer les tâches de gestion ou recréer des objets, tels que les bases de données et les cubes, sur des instances [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] . Par exemple, vous pouvez développer un script XMLA dans un projet de Script [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] qui crée directement des objets sur une instance [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] existante. Les projets de Script [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] peuvent être enregistrés au sein d'une solution et intégrés avec un système de contrôle de code source.  
  
Pour plus d’informations sur la façon d’utiliser [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)], consultez [Développement et implémentation à l’aide de SQL Server Management Studio](http://msdn.microsoft.com/en-us/c4f5a06b-e2e4-4660-a3a8-6fd356742c02).  
  
## <a name="managing-integration-services-solutions-using-sql-server-management-studio"></a>Gestion des solutions Integration Services à l'aide de SQL Server Management Studio  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull_md.md)] vous permet d’utiliser le service [!INCLUDE[ssISnoversion](../includes/ssisnoversion_md.md)] pour gérer les packages et surveiller les packages en cours d’exécution. Vous pouvez également utiliser [!INCLUDE[ssManStudio](../includes/ssmanstudio_md.md)] pour organiser des packages en dossiers, exécuter des packages, importer et exporter des packages, migrer des packages DTS (Data Transformation Services) et mettre à jour des packages [!INCLUDE[ssISnoversion](../includes/ssisnoversion_md.md)] .  
  
## <a name="managing-reporting-services-projects-using-sql-server-management-studio"></a>Gestion des projets Reporting Services à l'aide de SQL Server Management Studio  
Utiliser SQL Server Management Studio pour activer les fonctionnalités de Reporting Services, administrer le serveur et les bases de données, et gérer des rôles et des travaux.  
  
Vous gérez des planifications partagées à l'aide du dossier Planifications partagées et gérer des bases de données du serveur de rapports (ReportServer, ReportServerTempdb). Vous créez aussi un RSExecRole dans la base de données système Master quand vous déplacez une base de données du serveur de rapports vers un moteur de base de données SQL Server ([!INCLUDE[ssNoVersion](../includes/ssnoversion_md.md)] [!INCLUDE[ssDE](../includes/ssde_md.md)]). Pour plus d'informations sur ces tâches, consultez les rubriques suivantes :  
  
-   [Management Studio - Rubriques de procédures](http://msdn.microsoft.com/en-us/60685458-9108-47bf-820a-5e7db454d408)  
  
-   [Administration d'une base de données du serveur de rapports](http://msdn.microsoft.com/en-us/97b2e1b5-3869-4766-97b9-9bf206b52262)  
  
-   [Procédure : créer le rôle RSExecRole](http://msdn.microsoft.com/en-us/7ac17341-df7e-4401-870e-652caa2859c0)  
  
Vous gérez également le serveur en activant et configurant différentes fonctionnalités, en définissant les valeurs par défaut du serveur, et en gérant des rôles et des travaux. Pour plus d'informations sur ces tâches, consultez les rubriques suivantes :  
  
-   [Procédure : définir les propriétés du serveur de rapports (Management Studio)](http://msdn.microsoft.com/en-us/1ed0f84b-b12a-4e49-b65c-a11a99f9093f)  
  
-   [Procédure : créer, supprimer ou modifier un rôle (Management Studio)](http://msdn.microsoft.com/en-us/3d1d56e6-a283-44a7-8417-36cb4d2c74d1)  
  
-   [Activation et désactivation de l'impression côté client pour Reporting Services](http://msdn.microsoft.com/en-us/0e709c96-7517-4547-8ef6-5632f8118524)  
  
## <a name="see-also"></a> Voir aussi  
[Développement et implémentation à l’aide de SQL Server Data Tools](http://msdn.microsoft.com/en-us/132ed779-3ec8-4734-9698-802116d1b017)  
[Reporting Services dans les outils de données SQL Server](http://msdn.microsoft.com/en-us/0903c7b2-ac59-45f1-b7d0-922ecd9d76f8)  
  
