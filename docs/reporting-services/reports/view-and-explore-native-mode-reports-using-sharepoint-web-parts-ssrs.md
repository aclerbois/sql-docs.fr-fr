---
title: Afficher et explorer des rapports en mode natif à l’aide de composants WebPart SharePoint (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reports
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: dee8ee42-156b-43b6-b202-02dfb9404284
caps.latest.revision: 9
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 1072350a30a5f28ac16cda48f72720a383012d02
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-and-explore-native-mode-reports-using-sharepoint-web-parts-ssrs"></a>Afficher et explorer des rapports en mode natif à l’aide de composants WebPart SharePoint (SSRS)

> [!IMPORTANT]  
>  SQL Server Reporting Services ne prend plus en charge l’utilisation de composants WebPart (RSWebParts.cab) en mode natif pour accéder au contenu du serveur de rapports sur un site SharePoint à partir d’un serveur de rapports en mode natif. Utilisez à la place un [composant WebPart Visionneuse de rapports sur un site SharePoint](../../reporting-services/report-server-sharepoint/report-viewer-web-part-on-a-sharepoint-site.md) .  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit plusieurs composants WebPart qui fonctionnent avec des versions spécifiques d'un serveur de rapports et dans des modes de déploiement particulier.  
  
-   **Mode natif :** si vous souhaitez accéder au contenu d'un serveur de rapports sur un site SharePoint à partir d'un serveur de rapports en mode natif, utilisez les composants WebPart de SharePoint 2.0, Explorateur de rapports et Visionneuse de rapports, inclus dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Cette rubrique fournit des instructions pour l'installation et l'utilisation des composants WebPart 2.0.  
  
-   **Mode SharePoint :** si vous souhaitez accéder à un serveur de rapports qui s’exécute en mode SharePoint, utilisez les composants WebPart installés par le complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour les produits SharePoint. Pour plus d’informations sur le complément, consultez [Où trouver le complément Reporting Services pour les produits SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md).  
  
> [!NOTE]  
>  Le composant WebPart Visionneuse de rapports en mode natif (SPViewer.dwp) est différent du composant (ReportViewer.dwp) qui est installé par le complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour les produits SharePoint. Ces composants WebPart présentent des schémas et des implémentations différents, mais ils peuvent être installés tous les deux sur la même batterie de serveurs SharePoint. Vous pouvez distinguer visuellement les deux composants WebPart, car le composant WebPart Visionneuse de rapports installé via le complément **Actions** possède un menu dans la barre d’outils.  
  
 Pour plus d’informations sur les modes du serveur de rapports, consultez [Serveur de rapports Reporting Services](../../reporting-services/report-server-sharepoint/reporting-services-report-server.md).  
  
 Dans cette rubrique :  
  
-   [À propos de l'Explorateur de rapports et de la Visionneuse de rapports](#bkmk_aboutwebparts)  
  
-   [Conditions requises pour l'utilisation de composants WebPart](#bkmk_requirements)  
  
-   [Installation des composants WebPart](#bkmk_installingwebparts)  
  
-   [Ajouter et configurer des composants WebPart](#bkmk_configurewebparts)  
  
##  <a name="bkmk_aboutwebparts"></a> À propos de l'Explorateur de rapports et de la Visionneuse de rapports  
 L'Explorateur de rapports et la Visionneuse de rapports sont des composants WebPart de SharePoint 2.0 qui ont été introduits dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2000 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Service Pack 2 (SP2) et qui restent disponibles dans les versions actuelles.  
  
 Les composants WebPart fournissent un moyen d'afficher des rapports et d'explorer l'arborescence des dossiers du serveur de rapports à partir d'un site SharePoint.  
  
 Notez que la personnalisation des composants WebPart n'est pas prise en charge. Ces composants WebPart sont à utiliser tel quel et ne peuvent faire l'objet d'aucune modification ou extension.  
  
-   **L’Explorateur de rapports** (SPExplorer.dwp) se connecte au Gestionnaire de rapports sur le serveur de rapports. Vous pouvez parcourir les rapports disponibles sur un serveur de rapports et vous abonner à des rapports individuels. Si le Générateur de rapports est activé et que vous disposez des autorisations suffisantes, vous pouvez démarrer le Générateur à partir des composants WebPart de l'Explorateur de rapports.  
  
     L'Explorateur de rapports affiche le contenu d'un dossier à l'aide d'une page du Gestionnaire de rapports. L'accès à des éléments et des dossiers par l'intermédiaire de l'arborescence des dossiers du serveur de rapports est contrôlé par le biais des attributions de rôles sur le serveur de rapports. Lorsque vous sélectionnez un rapport, celui-ci s'ouvre dans une nouvelle fenêtre du navigateur. La Visionneuse HTML sur le serveur de rapports affiche le rapport et fournit la barre d'outils du rapport, au lieu du composant WebPart de visionneuse de rapports. Pour personnaliser les paramètres de la barre d'outils, veillez à spécifier les paramètres d'accès URL sur le serveur de rapports. Pour obtenir des instructions, consultez [Référence de paramètre d’accès URL](../../reporting-services/url-access-parameter-reference.md).  
  
-   La**Visionneuse de rapports** (SPViewer.dwp) affiche un rapport et fournit une barre d’outils qui vous permet de parcourir les pages, rechercher du contenu ou exporter le rapport. Vous pouvez ajouter le composant WebPart Visionneuse de rapports à une page de composant WebPart pour toujours afficher un rapport spécifique sur cette page ou **vous pouvez le connecter à l'Explorateur de rapports** pour afficher des rapports qui s'ouvrent via ce composant WebPart.  
  
##  <a name="bkmk_requirements"></a> Conditions requises pour l'utilisation de composants WebPart  
 La configuration requise pour utiliser les composants WebPart de l'Explorateur de rapports et de la Visionneuse de rapports est la suivante :  
  
-   Les versions prises en charge des produits et des technologies SharePoint sont les suivantes :  
  
    -   [!INCLUDE[winSPServ](../../includes/winspserv-md.md)] 3.0 et [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[offSPServ](../../includes/offspserv-md.md)] 2007.  
  
    -   [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] et [!INCLUDE[SPS2010](../../includes/sps2010-md.md)].  
  
-   La version du serveur de rapports en mode natif doit être [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou ultérieure.  
  
-   Le serveur de rapports doit s'exécuter en mode natif. Vous ne pouvez **pas** utiliser les composants WebPart Explorateur de rapports et Visionneuse de rapports pour vous connecter à des rapports ou afficher des rapports sur un serveur de rapports qui s'exécute en mode SharePoint.  
  
-   Le Gestionnaire de rapports doit être installé.  
  
 Les composants WebPart de l'Explorateur de rapports et de la Visionneuse de rapports sont distribués par un fichier .cab fourni avec [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Les rubriques suivantes fournissent des instructions pour l'installation, la configuration et l'utilisation des composants WebPart.  
  
##  <a name="bkmk_installingwebparts"></a> Installation des composants WebPart  
 Les composants WebPart sont fournis à un serveur SharePoint en tant que fichier .cab. Exécutez l'outil SharePoint Stsadm.exe sur le fichier .cab à partir de la ligne de commande pour installer les composants WebPart. Pour plus d'informations sur cet outil et le déploiement des composants WebPart, consultez la documentation de SharePoint.  
  
#### <a name="install-web-parts-using-powershell"></a>Installer les composants WebPart à l'aide de PowerShell  
  
1.  Copiez le fichier **RSWebParts.cab** dans un dossier sur le serveur SharePoint. Vous pouvez le copier dans n'importe quel dossier sur le serveur SharePoint, puis le supprimez ultérieurement au terme de l'installation des composants WebPart. Par défaut, SQL Server 2014 Reporting Services et antérieur installe le fichier RSWebParts.cab dans le dossier suivant :  
  
    ```  
    C:\Program Files (x86)\Microsoft SQL Server\110\Tools\Reporting Services\SharePoint  
    ```  
  
2.  Sur l'ordinateur sur lequel est installé le produit SharePoint, ouvrez **SharePoint 2010 Management Shell** avec des **privilèges d'administrateur**.  
  
3.  Exécutez la commande PowerShell suivante :  
  
    ```  
    Install-SPWebPartPack -LiteralPath "C:\Program Files (x86)\Microsoft SQL Server\110\Tools\Reporting Services\SharePoint\RSWebParts.cab" -GlobalInstall  
    ```  
  
4.  Vous devez voir un message similaire à ce qui suit, ce qui indique que le composant WebPart a été déployé.  
  
    > Name               SolutionId                                             Deployed  
  
    > ----                    ----------                                              -------\-  
  
    > rswebparts.cab    00000000-0000-0000-0000-000000000000     True  
  
     Pour plus d’informations sur l’utilisation de PowerShell, consultez [Install-SPWebPartPack (http://technet.microsoft.com/library/ff607840.aspx)](http://technet.microsoft.com/library/ff607840.aspx).  
  
#### <a name="install-web-parts-using-stsadmexe"></a>Installer des composants WebPart à l'aide de STSADM.exe  
  
1.  Copiez le fichier **RSWebParts.cab** au même emplacement sur le serveur SharePoint, comme décrit dans la section PowerShell de ce document.  
  
2.  Sur l'ordinateur où est installé le produit SharePoint, ouvrez une fenêtre d'invite de commandes avec des privilèges d'administrateur et accédez au dossier contenant l'outil **Stsadm.exe** . Le chemin d'accès par défaut pour [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] est le suivant :  
  
    > C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\14\BIN.  
  
3.  Exécutez Stsadm.exe sur le fichier.cab à l'aide de la syntaxe suivante :  
  
    ```  
    STSADM.EXE -o addwppack -filename "C:\Program Files (x86)\Microsoft SQL Server\110\Tools\Reporting Services\SharePoint\RSWebParts.cab" -globalinstall  
    ```  
  
4.  Vous devez voir le message « L'opération s'est déroulée avec succès ».  
  
     La définition de `-globalinstall` ajoute les composants WebPart au GAC (Global Assembly Cache). Cette étape est nécessaire si vous souhaitez vous connecter aux composants WebPart.  
  
##  <a name="bkmk_configurewebparts"></a> Ajouter et configurer des composants WebPart  
 Après avoir installé les composants WebPart, vous pouvez les ajouter à une ou plusieurs pages Web sur un site SharePoint. Vous devez être autorisé à créer des sites Web et à ajouter un contenu.  
  
 La procédure suivante permet d'ajouter les deux composants WebPart à une page, puis de connecter l'Explorateur de rapports et la Visionneuse de rapports ensemble de sorte que, lorsque vous cliquez sur un rapport dans l'Explorateur de rapports, il s'affiche également dans la Visionneuse de rapports.  
  
#### <a name="add-report-viewer"></a>Ajouter la Visionneuse de rapports  
  
1.  Dans Actions de site, cliquez sur **Modifier la page**.  
  
2.  Dans une zone sur la page, cliquez sur **Ajouter un composant WebPart**.  
  
3.  Dans la boîte de dialogue **Ajouter un composant WebPart** , faites défiler jusqu'à la catégorie **Divers** .  
  
4.  Sélectionnez **Visionneuse de rapports**.  
  
    > [!WARNING]  
    >  Ne sélectionnez pas **Visionneuse de rapports SQL Server Reporting Services** . Ce composant WebPart, qui est inscrit quand vous installez le complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour les produits SharePoint, est utilisé si vous exécutez un serveur de rapports en mode SharePoint. Il ne peut pas être utilisé pour afficher des rapports sur un serveur de rapports en mode natif.  
  
5.  Cliquez sur **Ajouter**.  
  
6.  Lorsque la page est en mode d'édition, cliquez sur **Modifier le composant WebPart** dans le composant WebPart Visionneuse de rapports.  
  
7.  Dans **URL du Gestionnaire de rapports**, tapez une URL vers une instance du Gestionnaire de rapports associée au serveur de rapports en mode natif auquel vous souhaitez accéder. Par défaut, une URL du Gestionnaire de rapports a la syntaxe suivante : **http://\<nom_serveur>/reports**.  
  
8.  Dans **Chemin d'accès au rapport**, spécifiez une barre oblique suivie du chemin d'accès du dossier et du nom du rapport. N'incluez **pas** le nom du serveur ou le répertoire virtuel du Gestionnaire de rapports. Par exemple, pour ouvrir le rapport « Company Sales » dans le dossier Adventure Works, spécifiez **/Adventure Works/Company Sales**. Voici un autre exemple, où le rapport « Products » se trouve dans le dossier racine du serveur de rapports **/Products**.  
  
9. Cliquez sur **OK**.  
  
#### <a name="add-report-explorer-and-connect-to-report-viewer"></a>Ajouter l'Explorateur de rapports et le connecter à la Visionneuse de rapports  
  
1.  Dans une autre zone de la page, cliquez sur **Ajouter un composant WebPart** et dans le dossier Divers, cliquez sur **Explorateur de rapports** , puis sur **Ajouter**.  
  
2.  Dans le menu contextuel du composant WebPart Explorateur de rapports, cliquez sur **Modifier le composant WebPart**.  
  
3.  Dans **URL du Gestionnaire de rapports**, tapez une URL vers une instance du Gestionnaire de rapports associée au serveur de rapports en mode natif auquel vous souhaitez accéder.  
  
4.  Vous pouvez, le cas échéant, définir le **chemin d'accès de démarrage**. Le chemin d'accès de démarrage est un dossier de l'arborescence des dossiers du serveur de rapports. Vous pouvez spécifier un chemin d'accès de démarrage si vous souhaitez que la page par défaut soit un dossier figurant plus bas dans l'arborescence des dossiers. Le chemin d'accès doit commencer par une barre oblique. Vous devez spécifier un chemin d'accès complet en commençant par le nœud racine de l'arborescence des dossiers du serveur de rapports mais qui n'inclut pas le nom du serveur ou du répertoire virtuel du Gestionnaire de rapports. Par exemple, pour ouvrir un dossier intitulé Adventure Works juste sous le nœud racine, spécifiez **/Adventure Works** dans le chemin d’accès de démarrage.  
  
5.  Cliquez sur **OK**.  
  
6.  L'Explorateur de rapports affiche une liste des éléments de rapport dans le serveur de rapports. Par défaut, si vous cliquez sur le nom d'un rapport, le rapport s'ouvre dans une nouvelle fenêtre. Réalisez la procédure suivante si vous souhaitez connecter l'Explorateur de rapports à la Visionneuse de rapports afin que, lorsque vous cliquez sur le nom d'un rapport dans l'Explorateur de rapports, il s'affiche dans la Visionneuse de rapports.  
  
    1.  Dans le menu contextuel de l'Explorateur de rapports, cliquez sur **Connexions**.  
  
    2.  Cliquez sur **Afficher le rapport dans**.  
  
    3.  Cliquez sur **Visionneuse de rapports**.  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
