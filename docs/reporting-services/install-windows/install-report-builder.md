---
title: Installer le Générateur de rapports | Microsoft Docs
ms.custom: ''
ms.date: 09/22/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: get-started-article
ms.assetid: 6b2291bb-1d20-4d08-81cb-a16dd8e01faf
caps.latest.revision: 20
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 3812692b30c5ff4436403d22de834e9fe8d6d73b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="install-report-builder"></a>Install Report Builder
  [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] est une application autonome installée sur votre ordinateur par vos propres soins ou par un administrateur. Vous pouvez l’installer à partir du Centre de téléchargement Microsoft, d’un serveur de rapport [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] le serveur de rapports ou d’un site SharePoint intégré avec [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Un administrateur installe et configure [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], accorde l’autorisation de télécharger l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] à partir du portail web, et gère les dossiers et les autorisations d’accès aux rapports, les parties des rapports et les datasets partagés enregistrés sur le serveur de rapports. Pour plus d’informations sur l’administration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Reporting Services Report Server &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md).  
  
## <a name="install-includessrbnoversionincludesssrbnoversion-mdmd-from--a--web-portal-or-sharepoint-library"></a>Installez l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] à partir d’un portail web ou d’une bibliothèque SharePoint 
  
 Vous pouvez démarrer l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] à partir d’un portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou d’un site SharePoint intégré avec [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour plus d’informations, consultez [Démarrer le Générateur de rapports](../../reporting-services/report-builder/start-report-builder.md).  
  
### <a name="sharepoint-site-integrated-with-includessrsnoversionincludesssrsnoversion-mdmd"></a>Site SharePoint intégré avec [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]
  
 Sur un site SharePoint intégré avec [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)],, si le menu **Nouveau document** ne répertorie pas **Rapport du Générateur de rapports**, **Modèle du générateur de rapports**et **Source de données du rapport**, leurs types de contenus doivent être ajoutés à la bibliothèque SharePoint. Pour plus d’informations, consultez [Ajouter des types de contenus Reporting Services à une bibliothèque SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  
 
## <a name="install-includessrbnoversionincludesssrbnoversion-mdmd-with-system-center-configuration-manager"></a>Installer [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] avec System Center Configuration Manager 
  
 Un administrateur peut également utiliser un logiciel tel que System Center Configuration Manager pour placer le programme sur votre ordinateur. Pour savoir comment utiliser des logiciels spécifiques pour installer l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)], consultez la documentation des logiciels en question. Pour plus d’informations, consultez le [site relatif à System Center Configuration Manager](https://www.microsoft.com/en-us/cloud-platform/system-center-configuration-manager).  
  
> [!IMPORTANT]  
>  Les fonctionnalités de sécurité de Windows Vista et Windows 7 requièrent des autorisations élevées pour exécuter des opérations en ligne de commande ; par conséquent, vous êtes invité à confirmer que vous avez l'autorisation d'exécuter la ligne de commande. Il ne s'agit pas d'une installation sans assistance. Pour effectuer une installation sans assistance, vous devez exécuter la ligne de commande en tant qu'administrateur.  
  
## <a name="system-requirements"></a>Configuration système requise
  
 Consultez la section **Configuration système requise** de la [page de téléchargement du Générateur de rapports](http://go.microsoft.com/fwlink/?LinkID=734968) du Centre de téléchargement Microsoft.
  
##  <a name="download"></a> Pour installer l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] à partir du site de téléchargement  
  
1.  Sur la [page Générateur de rapports du Centre de téléchargement Microsoft](http://go.microsoft.com/fwlink/?LinkID=734968) , cliquez sur **Télécharger**.  
  
2.  Une fois le téléchargement de l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] terminé, cliquez sur  **Exécuter**.  
  
     Cette opération lance l’Assistant SQL Server [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] .  
  
3.  Acceptez les conditions du contrat de licence, puis cliquez sur **Suivant**.  
  
4.  Dans la page **Serveur cible par défaut** , spécifiez éventuellement l'URL du serveur de rapports cible s'il est différent du serveur par défaut. Cliquez sur **Suivant**.  
  
    > [!NOTE]  
    >  Si vous prévoyez de travailler avec [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] lorsqu’il est connecté à un serveur de rapports, il est plus commode de spécifier l’URL du serveur à ce stade. Vous pouvez également le faire à partir de la boîte de dialogue **Options** dans [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)].  
  
5.  Cliquez sur **Installer** pour terminer l’installation de [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)].  
  
## <a name="to-install-includessrbnoversionincludesssrbnoversion-mdmd-from-a-share"></a>Pour installer [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] à partir d’un partage  
  
1.  Contactez votre administrateur afin de connaître l’emplacement du fichier ReportBuilder3.msi que vous exécutez pour installer [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] sur votre ordinateur local.  
  
2.  Recherchez le fichier ReportBuilder3.msi, le package MSI Windows Installer pour [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)]et cliquez dessus.  
  
     Cette opération lance l’Assistant SQL Server [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] .  
  
3.  Suivez les étapes suivantes décrites dans [Pour installer le Générateur de rapports à partir du site de téléchargement](#download).  
  
## <a name="to-install-includessrbnoversionincludesssrbnoversion-mdmd-from-the-command-line"></a>Pour installer l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] à partir de la ligne de commande 

 Vous pouvez également effectuer une installation de l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] à partir de la ligne de commande et spécifier des arguments afin de personnaliser l’installation. Outre les paramètres MSI standard intrinsèques, vous pouvez utiliser les paramètres personnalisés fournis par l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] : RBINSTALLDIR et REPORTSERVERURL. RBINSTALLDIR spécifie le dossier d’installation racine pour l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)]. REPORTSERVERURL spécifie le serveur de rapports par défaut utilisé par l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] pour enregistrer des rapports.  
  
 Si vous souhaitez effectuer une installation totalement sans assistance, sans aucune interaction avec l’interface utilisateur, spécifiez l’option **/quiet** . Par défaut, l'indicateur d'option quiet supprime les erreurs d'installation. Il est par conséquent recommandé d’inclure l’option **/l** , qui spécifie l’enregistrement dans le journal, lorsque vous utilisez l’option quiet.   
  
1.  Sur la [page Générateur de rapports du Centre de téléchargement Microsoft](http://go.microsoft.com/fwlink/?LinkID=734968), cliquez sur **Télécharger**.  
  
2.  Une fois le téléchargement de l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] terminé, cliquez sur  **Enregistrer**.  
  
3.  Dans le menu **Démarrer** , cliquez sur **Exécuter**.  
  
4.  Dans la zone **Ouvrir** , tapez **cmd.**  
  
5.  Dans la fenêtre d’invite de commandes, naviguez jusqu’au dossier où vous avez enregistré ReportBuilder3.msi.  
  
6.  Tapez une commande au format suivant :  
  
     `msiexec/i ReportBuilder3.msi /option [value] [/option [value]]`  
  
     Les deux options spécifiques à l’installation de l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] sont : RBINSTALLDIR et REPORTSERVERURL. Il est inutile d’inclure ces arguments dans la ligne de commande. Voici la ligne de commande de base :  
  
     `msiexec /i ReportBuilder3_x86.msi /quiet`  
  
7.  Pour exécuter la commande, appuyez sur ENTRÉE.  
  
## <a name="set-includessrbnoversionincludesssrbnoversion-mdmd-defaults"></a>Définir les paramètres par défaut de l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)]  
  
-   Après avoir installé l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)], vous pouvez définir des options par défaut. Cliquez sur **Fichier** > **Options**.  
  
     Le plus utile est de définir le site SharePoint ou le portail web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] par défaut. Pour plus d’informations, consultez [Set default options for Report Builder](../../reporting-services/report-builder/set-default-options-for-report-builder.md).  
  
-   Cliquez sur **Générateur de rapports** .  
  
     Si le serveur de rapports ne figure pas dans la liste des serveurs existants, fermez la boîte de dialogue **Ouvrir un rapport** , puis cliquez sur **Se connecter** en bas de l’ [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion-md.md)] pour vous connecter au serveur.  
  
## <a name="see-also"></a> Voir aussi  
 [Démarrer le Générateur de rapports](../../reporting-services/report-builder/start-report-builder.md)   
 [Désinstaller le générateur de rapports](../../reporting-services/install-windows/uninstall-report-builder.md)  
  
  
