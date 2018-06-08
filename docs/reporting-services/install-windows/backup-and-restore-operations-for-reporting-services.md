---
title: Opérations de sauvegarde et de restauration pour Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- databases [Reporting Services], backing up
- databases [Reporting Services], restoring
- databases [Reporting Services], moving
- backing up databases [Reporting Services]
- moving databases
- restoring databases [Reporting Services]
- files [Reporting Services], restoring
- files [Reporting Services], backing up
ms.assetid: 157bc376-ab72-4c99-8bde-7b12db70843a
caps.latest.revision: 43
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 1882b8d07d7288082ad4963cd0a735d072f19404
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="backup-and-restore-operations-for-reporting-services"></a>Opérations de sauvegarde et de restauration pour Reporting Services

  Cette rubrique offre une vue d'ensemble de tous les fichiers de données utilisés dans une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , et décrit quand et comment les fichiers doivent être sauvegardés. L'élaboration d'un plan de sauvegarde et de restauration des fichiers de base de données du serveur de rapports constitue l'étape la plus importante d'une stratégie de récupération. Cependant, une stratégie de récupération plus complète inclut des sauvegardes des clés de chiffrement, des extensions ou assemblys personnalisés, des fichiers de configuration et des fichiers sources pour les rapports et les modèles.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Mode natif | [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Mode SharePoint  
  
 Les opérations de sauvegarde et de restauration sont souvent utilisées pour déplacer partiellement ou intégralement une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   Si vous ne déplacez que les bases de données du serveur de rapports, vous pouvez utiliser la sauvegarde et la restauration ou l'attachement et le détachement pour déplacer les bases de données sur une autre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Déplacement des bases de données du serveur de rapports vers un autre ordinateur &#40;en mode natif SSRS&#41;](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md).  
  
-   Le déplacement d'une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur un nouvel ordinateur s'appelle une migration. Lorsque vous faites migrer une installation, vous exécutez le programme d'installation pour installer une nouvelle instance de serveur de rapports, puis vous copiez les données de l'ancienne instance sur le nouvel ordinateur. Pour plus d'informations sur la migration d'une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , consultez les rubriques suivantes :  
  
    -   [Mettre à niveau et migrer Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)  
  
    -   [Migrer une installation Reporting Services &#40;mode SharePoint&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md)  
  
    -   [Faire migrer une installation Reporting Services &#40;mode natif&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  
  
## <a name="backing-up-the-report-server-databases"></a>Sauvegarde des bases de données du serveur de rapports  
 Dans la mesure où un serveur de rapports est un serveur sans état, toutes les données d’application sont stockées dans les bases de données **reportserver** et **reportservertempdb** qui s’exécutent sur une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Vous pouvez sauvegarder les bases de données **reportserver** et **reportservertempdb** à l’aide de l’une des méthodes de sauvegarde de bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prises en charge. Les recommandations spécifiques aux bases de données du serveur de rapports sont les suivantes :  
  
-   Utilisez le mode de récupération complet pour sauvegarder la base de données **reportserver** .  
  
-   Utilisez le mode de récupération simple pour sauvegarder la base de données **reportservertempdb** .  
  
-   Vous pouvez utiliser des planifications de sauvegarde différentes pour chaque base de données. La sauvegarde de **reportservertempdb** présente un seul intérêt, celui de ne pas avoir à recréer la base de données en cas de défaillance matérielle. En effet, il est inutile de récupérer les données dans **reportservertempdb**, mais la structure de la table est indispensable. Si votre base de données **reportservertempdb**est inutilisable, le seul moyen de la récupérer consiste à recréer la base de données du serveur de rapports. Si vous recréez la base de données **reportservertempdb**, il est important qu’elle porte le même nom que la base de données du serveur de rapports primaire.  
  
 Pour plus d’informations sur la sauvegarde et la récupération des bases de données relationnelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).  
  
> [!IMPORTANT]  
>  Si votre serveur de rapports est en mode SharePoint, vous devez également vous occuper de bases de données supplémentaires, notamment les bases de données de configuration SharePoint et la base de données des alertes [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. En mode SharePoint, trois bases de données sont créées pour chaque application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Les bases de données **reportserver**, **reportservertempdb**et **dataalerting** . Pour plus d’informations, consultez [Applications de service SharePoint de sauvegarde et de restauration Reporting Services](../../reporting-services/report-server-sharepoint/backup-and-restore-reporting-services-sharepoint-service-applications.md).  
  
## <a name="backing-up-the-encryption-keys"></a>Sauvegarde des clés de chiffrement  
 Vous devez sauvegarder les clés de chiffrement lorsque vous configurez pour la première fois une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Vous devez également sauvegarder les clés chaque fois que vous modifiez l'identité des comptes de services ou renommez l'ordinateur. Pour plus d’informations, consultez [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md). Pour les serveurs de rapports en mode SharePoint, consultez la section « Gestion des clés » de la rubrique [Gérer une application de service SharePoint Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).  
  
## <a name="backing-up-the-configuration-files"></a>Sauvegarde des fichiers de configuration  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise des fichiers de configuration pour stocker les paramètres des applications. Vous devez sauvegarder les fichiers lorsque vous configurez pour la première fois le serveur, puis lorsque vous avez déployé des extensions personnalisées. Les fichiers à sauvegarder sont les suivants :  
  
-   Rsreportserver.config  
  
-   Rssvrpolicy.config  
  
-   Rsmgrpolicy.config  
  
-   Reportingservicesservice.exe.config  
  
-   Web.config pour les applications [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] Report Server et Gestionnaire de rapports.  
  
-   Machine.config pour [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]  
  
## <a name="backing-up-data-files"></a>Sauvegarde des fichiers de données  
 Sauvegardez les fichiers que vous créez et gérez dans le Concepteur de rapports et le Générateur de modèles. entre autres les fichiers de définition de rapport (.rdl), de modèle de rapport (.smdl), de source de données partagée (.rds), de vue de données (.dv), de source de données (.ds), de projet de serveur de rapports (.rptproj) et de solution de rapport (.sln).  
  
 N'oubliez pas de sauvegarder les fichiers de script (.rss) que vous avez créés pour les tâches d'administration ou de déploiement.  
  
 Vérifiez que vous êtes en possession d'une copie de sauvegarde de tous les assemblys et extensions personnalisés que vous utilisez.  

## <a name="next-steps"></a>Étapes suivantes

[Base de données du serveur de rapports](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
[Fichiers de configuration de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
[Utilitaire rskeymgmt](../../reporting-services/tools/rskeymgmt-utility-ssrs.md)   
[Copier des bases de données avec la sauvegarde et la restauration](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)   
[Administrer une base de données du serveur de rapports](../../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md)   
[Configurer et gérer des clés de chiffrement](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
