---
title: Installer ou désinstaller le Power Pivot pour complément SharePoint (SharePoint 2013) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c24287c48f450620dfbbebb016c78dd544005da4
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013"></a>Installer ou désinstaller le complément Power Pivot pour SharePoint (SharePoint 2013)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)] est un ensemble de composants de serveur d’applications et de services principaux qui fournissent l’accès aux données [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dans une batterie de serveurs [!INCLUDE[SPS2013](../../../includes/sps2013-md.md)] . Le complément [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint (**spPowerpivot.msi**) est un package d’installation utilisé pour installer les composants de serveur d’applications.  
  
-   Ce complément n'est pas requis pour les déploiements SharePoint 2010.  
  
-   Ce complément n'est pas requis dans un déploiement à un seul serveur qui inclut SharePoint 2013 et [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode SharePoint. Les composants installés par le complément sont inclus lorsque vous installez un serveur [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode SharePoint. Pour obtenir les diagrammes d’exemples de déploiement avec le complément, consultez [Topologies de déploiement pour les fonctionnalités SQL Server BI dans SharePoint](http://msdn.microsoft.com/library/39f76bc7-94e6-4dbc-bfa5-d56f4430bb26).  
  
 **Remarque :** cette rubrique décrit l'installation des fichiers solution [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] et de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour l'outil de Configuration de SharePoint 2013. Après l’installation, consultez la rubrique suivante pour plus d’informations sur l’outil de configuration et les fonctionnalités supplémentaires : [Configurer PowerPivot et déployer des solutions &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/configure-power-pivot-and-deploy-solutions-sharepoint-2013.md).  
  
 Pour plus d’informations sur le téléchargement de **spPowerPivot.msi**, voir [Microsoft® SQL Server® 2014 PowerPivot® pour Microsoft SharePoint®](http://go.microsoft.com/fwlink/?LinkID=324854).  
  
##  <a name="bkmk_background"></a> Arrière-plan  
  
-   **Serveur d’applications :** [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] disponibles dans SharePoint 2013 incluent l’utilisation de classeurs comme source de données, l’actualisation planifiée des données et le tableau de bord de gestion [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] .  
  
     [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)] est un package [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows Installer (**spPowerpivot.msi**) qui déploie les bibliothèques clientes Analysis Services et copie les fichiers d’installation de [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)] sur l’ordinateur. Le programme d'installation ne déploie pas ou ne configure pas de fonctionnalités [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dans SharePoint. Les composants suivants s'installent par défaut :  
  
    -   [!INCLUDE[ssGeminiShort](../../../includes/ssgeminishort-md.md)] 2013. Ce composant inclut des scripts PowerShell (fichiers .ps1), des packages de solution SharePoint (.wsp) et l'outil de configuration [!INCLUDE[ssGeminiShort](../../../includes/ssgeminishort-md.md)] 2013 pour déployer [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dans une batterie de serveurs SharePoint 2013.  
  
    -   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Fournisseur OLE DB pour Analysis Services (MSOLAP).  
  
    -   Fournisseur de données ADOMD.NET.  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   **Services principaux :** si vous utilisez [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour Excel pour créer des classeurs qui contiennent des données analytiques, Excel Services doit être configuré avec un serveur BI exécutant [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode SharePoint pour accéder à ces données dans un environnement serveur. Vous pouvez exécuter le programme d'installation de SQL Server sur un ordinateur qui possède un serveur SharePoint 2013 installé, ou sur un autre ordinateur sans logiciel SharePoint. Analysis Services n'a pas de dépendances de SharePoint.  
  
     Pour plus d'informations sur l'installation, la désinstallation et la configuration des services principaux, consultez :  
  
    -   [Installation d’Analysis Services en mode Power Pivot](../../../analysis-services/instances/install-windows/install-analysis-services-in-power-pivot-mode.md)  
  
    -   [Désinstaller Power Pivot pour SharePoint](../../../sql-server/install/uninstall-power-pivot-for-sharepoint.md)  
  
##  <a name="bkmk_where_to_install"></a> Où installer spPowerPivot.msi ?  
 La meilleure pratique recommandée consiste à installer **spPowerPivot.msi** sur tous les serveurs de la batterie de serveurs SharePoint pour la cohérence de configuration, y compris les serveurs d’applications et les serveurs Web frontaux. Le package d'installation inclut les fournisseurs de données Analysis Services, ainsi que l'outil de configuration de [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)] . Lorsque vous installez **spPowerPivot.msi** , vous pouvez personnaliser l'installation en excluant des composants.  
  
 **Fournisseurs de données :** plusieurs technologies SharePoint et SQL Server utilisent des fournisseurs de données Analysis Services, y compris Excel Services, PerformancePoint Services et Power View. L’installation de **spPowerPivot.msi** sur tous les serveurs SharePoint garantit que l’ensemble complet des fournisseurs de données Analysis Services et la connectivité [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] sont constamment disponibles dans la batterie de serveurs.  
  
> [!NOTE]  
>  Vous devez installer les fournisseurs de données Analysis Services sur un serveur SharePoint 2013 à l'aide de **spPowerPivot.msi**. D'autres packages d'installation disponibles dans le Feature Pack [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] ne sont pas pris en charge, car ils n'incluent pas les fichiers de prise en charge de SharePoint 2013 dont les fournisseurs de données ont besoin dans cet environnement.  
  
 **Outil de configuration :** l’outil de configuration [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013 n’est requis que sur un seul des serveurs SharePoint. Toutefois, la meilleure pratique recommandée dans les batteries de plusieurs serveurs consiste à installer l'outil de configuration sur au moins deux serveurs de façon à ce que vous ayez accès à l'outil de configuration si un des deux serveurs est hors connexion.  
  
##  <a name="bkmk_prereq"></a> Spécifications et conditions préalables requises  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SharePoint Server 2013.  
  
-   **spPowerPivot.msi** n’est disponible que sur les systèmes d’exploitation 64 bits, conformément aux spécifications des produits et technologies SharePoint.  
  
-   Un serveur dans [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] mode. Excel Services utilisera l’instance SQL Server Analysis Services en tant que serveur [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] . Analysis Services peut s'exécuter sur un ordinateur local ou distant.  
  
-   **Autorisations :** pour installer [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)], l'utilisateur actuel doit être administrateur sur l'ordinateur et membre du groupe Administrateur de batterie de serveurs SharePoint.  
  
-   Pour plus d’informations sur les spécifications et les conditions préalables requises pour [!INCLUDE[ssGeminiShort](../../../includes/ssgeminishort-md.md)] , accédez à [Configurations matérielle et logicielle requises pour le serveur Analysis Services en mode SharePoint](http://msdn.microsoft.com/library/fb86ca0a-518c-4c61-ae78-7680c57fae1f).  
  
##  <a name="bkmk_install"></a> Pour installer Power Pivot pour SharePoint  
 Le package d’installation **spPowerpivot.msi** prend en charge à la fois une interface utilisateur graphique et une installation en mode ligne de commande. Les deux méthodes d'installation requièrent que vous exécutiez le fichier .msi avec des privilèges d'administrateur. Après l’installation, consultez la rubrique suivante pour plus d’informations sur l’outil de configuration et les fonctionnalités supplémentaires : [Configurer PowerPivot et déployer des solutions &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/configure-power-pivot-and-deploy-solutions-sharepoint-2013.md).  
  
### <a name="user-interface-installation"></a>Installation de l'interface utilisateur  
 Pour installer [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)] avec l'interface utilisateur graphique, procédez comme suit :  
  
1.  Exécutez **SpPowerPivot.msi**.  
  
2.  Dans la page de bienvenue, cliquez sur **Suivant** .  
  
3.  Lisez et acceptez le contrat de licence, puis cliquez sur **Suivant**.  
  
4.  Dans la page **Sélection de fonctionnalités** , toutes les fonctionnalités sont sélectionnées par défaut.  
  
5.  Cliquez sur **Suivant**.  
  
6.  Cliquez sur **Installer** pour terminer l'installation.  
  
### <a name="command-line-installation"></a>Installation à partir de la ligne de commande  
 Pour une installation à partir de la ligne de commande, ouvrez une invite de commandes avec des autorisations administratives, puis exécutez **spPowerPivot.msi**. Par exemple :  
  
 `Msiexec.exe /i SpPowerPivot.msi`.  
  
 Pour créer un journal d'installation, utilisez les commutateurs d'enregistrement MsiExec standards. L'exemple suivant crée le fichier journal « Install_Log.txt » à l'aide du commutateur de journalisation commentée « v ».  
  
```  
Msiexec.exe /i SpPowerPivot.msi /L v c:\test\Install_Log.txt  
```  
  
### <a name="quiet-command-line-installation-for-scripting"></a>Installation silencieuse de script à partir de la ligne de commande  
 Vous pouvez utiliser le commutateur **/q** ou **/quiet** pour une installation « silencieuse » qui n’affiche pas de boîtes de dialogue ni d’avertissements. L'installation silencieuse est utile si vous souhaitez écrire un script d'installation du complément.  
  
> [!IMPORTANT]  
>  Si vous utilisez le commutateur **/q** pour une installation silencieuse à partir de la ligne de commande, le Contrat de Licence Utilisateur Final ne sera pas affiché. Quelle que soit la méthode d'installation, l'utilisation de ce logiciel est régie par un contrat de licence et vous devez respecter les termes contractuels de ce contrat.  
  
 **Pour effectuer une installation sans assistance :**  
  
1.  Ouvrez une invite de commandes **avec des autorisations d'administrateur**.  
  
2.  Exécutez la commande suivante :  
  
    ```  
    Msiexec.exe /i spPowerPivot.msi /q  
    ```  
  
### <a name="command-line-installation-to-include-specific-components"></a>Installation à partir de la ligne de commande pour inclure des composants spécifiques  
 L'outil de configuration [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)] n'est pas requis sur chaque serveur SharePoint, mais il est néanmoins recommandé de l'installer sur au moins deux serveurs de façon à ce qu'il soit disponible lorsque vous en avez besoin.  
  
 Lorsque vous installez le fichier spPowerPivot.msi, vous pouvez utiliser les options de ligne de commande pour installer des éléments spécifiques, comme les fournisseurs de données, et non l'outil de configuration [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)] . La ligne de commande suivante est un exemple d'installation de tous les composants, à l'exception de l'outil de configuration :  
  
```  
Msiexec /i spPowerPivot.msi AGREETOLICENSE="yes" ADDLOCAL=” SQL_OLAPDM,SQL_ADOMD,SQL_AMO,SQLAS_SP_Common”  
```  
  
|Option| Description|  
|------------|-----------------|  
|Analysis_Server_SP_addin|[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] Configuration|  
|SQL_OLAPDM|MSOLAP|  
|SQL_ADOMD|fournisseur ADOMD.net|  
|SQL_AMO|Fournisseur AMO|  
|SQLAS_SP_Common|Composants communs Analysis Services pour SharePoint 2013|  
  
##  <a name="bkmk_deploy_solution"></a> Déployer les fichiers solution SharePoint avec l’outil de configuration Power Pivot pour SharePoint 2013  
 Trois des fichiers copiés sur le disque dur par spPowerPivot.msi sont des fichiers solution de SharePoint. L'étendue d'un fichier solution correspond au niveau de l'application Web, alors que l'étendue des autres fichiers correspond au niveau de la batterie de serveurs. Les fichiers sont les suivants :  
  
-   `PowerPivotFarmSolution.wsp`  
  
-   `PowerPivotFarm14Solution.wsp`  
  
-   `PowerPivotWebApplicationSolution.wsp`  
  
 Les fichiers solution sont copiés dans le dossier suivant :  
  
 `ssInstallPathTools\PowerPivotTools\SPAddinConfiguration\Resources`  
  
 Après l'installation du fichier .msi, exécutez l'outil de configuration [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)] pour configurer et déployer des solutions dans la batterie de serveurs SharePoint.  
  
 **Pour démarrer l'outil de configuration de :**  
  
 Sur l’écran de démarrage de Windows, tapez « power », puis dans les résultats de la recherche d’applications, cliquez sur **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013**. Notez que les résultats de la recherche peuvent inclure deux liens car le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installe des outils de configuration de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] distincts pour SharePoint 2010 et SharePoint 2013. Lancez l'outil Configuration de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013.  
  
 ![Deux outils de configuration de PowerPivot](../../../analysis-services/instances/install-windows/media/as-powerpivot-configtools-bothicons.gif "Deux outils de configuration de PowerPivot")  
  
 **Or**  
  
1.  Accédez à **Démarrer**, **Tous les programmes**.  
  
2.  Cliquez sur [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)].  
  
3.  Cliquez sur **Outils de configuration**.  
  
4.  Cliquez sur **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013**.  
  
 Pour plus d'informations sur l'outil de configuration, consultez [Power Pivot Configuration Tools](../../../analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools.md).  
  
##  <a name="bkmk_remove_addin"></a> Désinstaller ou réparer le complément  
  
> [!CAUTION]  
>  Si vous désinstallez **spPowerPivot.msi** , les fournisseurs de données et l'outil de configuration sont désinstallés. La désinstallation des fournisseurs de données empêchera le serveur de se connecter à [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)].  
  
 Pour désinstaller ou réparer [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)] , procédez selon l'une des méthodes suivantes :  
  
1.  **Panneau de configuration Windows :** sélectionnez [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)]**[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013**. Cliquez sur **Désinstaller** ou **Réparer**.  
  
2.  Exécutez le fichier spPowerPivot.msi et sélectionnez l'option **Supprimer** ou **Réparer** .  
  
 **Ligne de commande :** pour réparer ou désinstaller [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013 à l’aide de la ligne de commande, ouvrez une invite de commandes **avec des autorisations d’administrateur** et exécutez l’une des commandes suivantes :  
  
-   Pour réparer, exécutez la commande suivante :  
  
    ```  
    msiexec.exe /f spPowerPivot.msi  
    ```  
  
 - ou -  
  
-   Pour désinstaller, exécutez la commande suivante :  
  
    ```  
    msiexec.exe /uninstall spPowerPivot.msi  
    ```  
  
  
