---
title: Configuration et administration d’un serveur de rapports SQL Server Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server-sharepoint
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: bee75418d6c2ede62169ec1ef3b42770881499e0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configuration-and-administration-of-a-sql-server-reporting-services-report-server"></a>Configuration et administration d’un serveur de rapports SQL Server Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

SQL Server Reporting Services est une plateforme de création de rapports basée sur serveur qui fournit une plage complète d’outils et de services prêts à l’emploi pour vous aider à créer, déployer et gérer des rapports pour votre organisation, ainsi que des fonctions de programmation pour vous permettre d’étendre et de personnaliser vos fonctionnalités de création de rapports. Vous pouvez intégrer votre environnement de création de rapports à un produit SharePoint afin de tirer parti des avantages liés à l'utilisation de l'environnement de collaboration fourni par les sites SharePoint.

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

Utilisez les sections suivantes pour mieux comprendre les concepts, procédures et scénarios de déploiement relatifs à l’intégration de votre environnement Reporting Services à un produit ou une technologie SharePoint :  
  
-   Options de menu dans une bibliothèque SharePoint  
  
    -   [Gestionnaire des alertes de données pour les utilisateurs SharePoint](../../reporting-services/data-alert-manager-for-sharepoint-users.md)  
  
    -   [Create and Manage Subscriptions for SharePoint Mode Report Servers](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-sharepoint-mode-report-servers.md)  
  
    -   [Mettre à jour les informations d'identification dans les sources de données de rapport à partir d'un site SharePoint](../../reporting-services/report-data/update-credentials-in-report-data-sources-from-a-sharepoint-site.md)  
  
    -   [Gérer des datasets partagés](../../reporting-services/report-data/manage-shared-datasets.md)  
  
    -   [Définir les paramètres sur un rapport publié &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md)  
  
    -   [Définir les options de traitement &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md)  
  
    -   [Options d’actualisation du cache &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/227da40c-6bd2-48ec-aa9c-50ce6c1ca3a6)  
  
-   [Fonctionnalités de collection de sites de Reporting Services](../../reporting-services/report-server-sharepoint/site-collection-features-reporting-services.md)  
  
-   [Activer les fonctionnalités d'intégration Report Server et Power View dans SharePoint](../../reporting-services/report-server-sharepoint/site-collection-features-report-server-and-power-view.md)  
  
-   [Paramètres et fonctions du site Reporting Services &#40;mode SharePoint&#41;](../../reporting-services/report-server-sharepoint/site-settings-and-features-reporting-services.md)  
  
-   [Activer la fonctionnalité Synchronisation de fichiers de serveur de rapports dans l'Administration centrale de SharePoint](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md)  
  
-   [Ajouter des types de contenu Reporting Services à une bibliothèque SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md)  
  
-   [Rapports en mode local contre rapports en mode connecté dans la Visionneuse de rapports &#40;Reporting Services en mode SharePoint&#41;](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md)  
  
-   [Télécharger des documents vers une bibliothèque SharePoint &#40;Reporting Services en mode SharePoint&#41;](../../reporting-services/report-server-sharepoint/upload-documents-to-a-sharepoint-library-reporting-services-in-sharepoint-mode.md)  
  
-   [Définir les options de traitement &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md)  
  
 Pour plus d’informations générales sur Reporting Services, consultez [Reporting Services](../../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md) dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations sur les autres composants, outils et ressources de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez la [Documentation en ligne de SQL Server](../../sql-server/sql-server-technical-documentation.md).  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
