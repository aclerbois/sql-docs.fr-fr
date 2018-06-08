---
title: Vérifier une installation de Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- checking report server installations
- verifying report server installations
- Report Designer [Reporting Services], verifying installations
- installing Reporting Services, verifying installations
- report servers [Reporting Services], verifying installations
- Setup [Reporting Services], verifying installations
ms.assetid: 82a51a99-66f0-4b0c-b05b-07d22387adb0
caps.latest.revision: 45
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 8f53a3644eecad3486d74cf185d14a2bb019dfd0
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34550540"
---
# <a name="verify-a-reporting-services-installation"></a>Verify a Reporting Services Installation
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peuvent être installés au choix dans l'un des deux modes, natif ou SharePoint. Les étapes que vous devez suivre pour vérifier l'installation dépendent du mode de serveur de rapports.  
  
##  <a name="bkmk_sharepointmode"></a> Vérifiez l'installation en mode SharePoint  
  
### <a name="to-verify-the-reporting-services-service"></a>Pour vérifier le service Reporting Services  
  
1.  Dans l'Administration centrale de SharePoint, cliquez sur **Gérer les services sur le serveur** dans le groupe **Paramètres système** .  
  
2.  Vérifiez que le service **SQL Server Reporting Services** est installé et présente l'état **Exécution** .  
  
     Si vous ne voyez pas le service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans la liste, vérifiez qu'il est installé. Pour plus d’informations, consultez [Installer le premier serveur de rapports en mode SharePoint](http://msdn.microsoft.com/en-us/b29d0f45-0068-4c84-bd7e-5b8a9cd1b538).  
  
### <a name="to-verify-the-service-application"></a>Pour vérifier l'application de service  
  
1.  Pour vérifier que vous disposez d'au moins une application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] depuis l'Administration centrale, cliquez sur **Gérer les applications de service** dans le groupe **Gestion des applications** .  
  
2.  Vérifiez qu'il existe une application de service de type **Application de service SQL Server Reporting Services** , ainsi qu'un proxy d'application correspondant.  
  
3.  Cliquez **à côté** du nom de l'application de service, puis sur **Propriétés** dans la barre d'outils SharePoint.  Si vous cliquez sur le nom de l'application de service, les pages de gestion de l'application de service sont affichées, et non la page de propriétés.  
  
4.  Vérifiez que l' **Association d'application Web** est configurée de manière à désigner l'application Web souhaitée.  
  
### <a name="to-verify-the-site-collection-feature"></a>Pour vérifier la fonctionnalité de collection de sites  
  
1.  Dans les paramètres du site, cliquez sur **Fonctionnalités de la collection de sites** dans le groupe **Administration de la collection de sites** .  
  
2.  Vérifiez que la **Fonctionnalité d'intégration du serveur de rapports** est active.  
  
### <a name="to-verify-reporting-server-content-types"></a>Pour vérifier les types de contenu de serveur de rapports  
  
1.  Pour vérifier ou ajouter des types de contenus de serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , consultez [Ajouter des types de contenus Reporting Services à une bibliothèque SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  
  
### <a name="to-verify-you-can-launch-report-builder"></a>Pour vérifier que vous pouvez lancer le Générateur de rapports  
  
1.  Depuis une bibliothèque de documents, cliquez sur **Documents** dans le ruban SharePoint.  
  
2.  Cliquez sur **Nouveau document** et cliquez sur **Rapport du Générateur de rapports**. Si vous ne voyez pas cette option, reportez-vous à la procédure précédente sur l'ajout de types de contenu de serveur de rapports à une bibliothèque.  
  
### <a name="create-a-basic-report"></a>Créer un rapport de base  
  
1.  Dans une bibliothèque de documents SharePoint, créez un rapport de base [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui ne contient qu'une seule zone de texte, un titre par exemple. Le rapport ne contient aucune source de données ni aucun dataset. Le but est de vérifier que vous pouvez ouvrir le Générateur de rapports et afficher un aperçu d'un rapport de base.  
  
2.  Enregistrez le rapport dans la bibliothèque de documents, puis exécutez le rapport depuis la bibliothèque. Pour plus d’informations sur la création de rapports avec le Générateur de rapports, consultez [Démarrer le Générateur de rapports](http://msdn.microsoft.com/en-us/8c8c7d2e-b315-418d-bf65-90e7685e4259).  
  
### <a name="reporting-services-samples"></a>Exemples Reporting Services  
  
1.  Suivez l'un des didacticiels de Reporting Services. Pour plus d’informations, consultez [Didacticiels de Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md).  
  
2.  Téléchargez l’exemple de base de données AdventureWorks et les exemples de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] depuis GitHub. Pour plus d’informations, consultez [Exemples de bases de données AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases).  
  
##  <a name="bkmk_nativemode"></a> Vérifier une installation en mode natif  
 Lorsque vous installez un serveur de rapports en mode natif en utilisant la configuration par défaut, le programme d'installation installe et déploie le serveur. Vous pouvez vérifier que le programme d'installation a déployé le serveur de rapports en réalisant quelques tests simples. Pour effectuer ces interventions, vous devez être un administrateur local. Pour permettre à d'autres utilisateurs de réaliser des tests, vous devez configurer leur accès au serveur de rapports.  
  
### <a name="to-verify-that-the-report-server-is-installed-and-running"></a>Pour vérifier que le serveur de rapports est installé et s'exécute correctement  
  
1.  Exécutez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis connectez-vous à l'instance du serveur de rapports que vous venez d'installer. La page URL du service Web inclut un lien vers le service Web Report Server. Cliquez sur le lien pour vérifier que vous pouvez accéder au serveur. Si la base de données du serveur de rapports n'est pas configurée, configurez-la avant de cliquer sur le lien.  
  
2.  Ouvrez la fenêtre Services et vérifiez que le service Windows Report Server est en cours d'exécution. Pour afficher l’état du service Report Server, cliquez sur **Démarrer**, pointez sur **Panneau de configuration**, double-cliquez sur **Outils d’administration**, puis double-cliquez sur **Services**. Lorsque la liste des services s’affiche, recherchez **Report Server (MSSQLSERVER)**. L'état doit être **Démarré**.  
  
3.  Ouvrez un navigateur et entrez l'URL du serveur de rapports dans la barre d'adresses. L'adresse se compose du nom du serveur et de celui du répertoire virtuel spécifiés pour le serveur de rapports lors de l'installation. Par défaut, le répertoire virtuel du serveur de rapports est appelé **ReportServer**. Vous pouvez utiliser l’URL suivante pour vérifier l’installation du serveur de rapports : http://*\<nom ordinateur>*/ReportServer*\<_nom instance>*. L'URL sera différente si vous avez installé le serveur de rapports en tant qu'instance nommée. Pour plus d’informations sur le format des URL, consultez [Configurer des URL de serveurs de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md). Si vous êtes administrateur local sur Windows Vista ou Windows Server 2008, consultez [Configurer un serveur de rapports en mode natif pour l’administration locale &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  
  
4.  Exécutez les rapports pour tester le fonctionnement du serveur de rapports. Pour cette étape, vous pouvez créer un rapport d'exemple à partir d'un didacticiel. Pour plus d’informations, consultez [Créer un rapport de tableau de base &#40;didacticiel SSRS&#41;](../../reporting-services/create-a-basic-table-report-ssrs-tutorial.md).  
  
### <a name="to-verify-that-the-includessrswebportalincludesssrswebportalmd-is-installed-and-running"></a>Pour vérifier que le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] est installé et s’exécute correctement  
  
1.  Ouvrez un navigateur et tapez l’URL du portail web dans la barre d’adresses. L’adresse se compose du nom du serveur et de celui du répertoire virtuel spécifiés pour le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] lors de l’installation ou dans la page URL du portail web de l’outil de configuration de Reporting Services. Par défaut, le répertoire virtuel du [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] est **Reports**. Vous pouvez utiliser l’URL suivante pour vérifier l’installation du [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] :  
  
     http://*\<nom ordinateur>*/Reports*\<_nom instance>*.  
  
2.  Utilisez le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] pour créer un dossier ou chargez un fichier pour vérifier si les définitions sont transmises à la base de données du serveur de rapports. Si ces opérations réussissent, cela signifie que la connexion fonctionne.  
  
     Pour plus d’informations, consultez [Portail web &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/en-us/7349e626-6ed5-4d21-b05f-cf042ad9ad70).  
  
### <a name="to-verify-that-report-designer-is-installed-and-running"></a>Pour vérifier que le Générateur de rapports est installé et s'exécute correctement  
  
1.  Ouvrez [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]et créez un projet basé sur un type de projet Report Server. Pour plus d’informations sur l’utilisation de l’Assistant Projet Report Server, consultez [Reporting Services dans les outils de données SQL Server &#40;SSDT&#41;](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md) dans la documentation en ligne de SQL Server.  
  
2.  Si vous avez installé des exemples de rapports, ouvrez les exemples de fichiers de projet de rapport et publiez les rapports sur un serveur de rapport.  
  
## <a name="see-also"></a> Voir aussi  
 [Dépanner une installation de Reporting Services](../../reporting-services/install-windows/troubleshoot-a-reporting-services-installation.md)   
 [Cause et résolution des erreurs Reporting Services](../../reporting-services/troubleshooting/cause-and-resolution-of-reporting-services-errors.md)  
  
  
