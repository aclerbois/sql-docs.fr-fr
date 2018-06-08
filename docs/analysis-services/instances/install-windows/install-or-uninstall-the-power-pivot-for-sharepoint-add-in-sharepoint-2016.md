---
title: Installer ou désinstaller le Power Pivot pour complément SharePoint (SharePoint 2016) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0251696f4c3007f891a6a5c483db487c527a62ea
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2016"></a>Installer ou désinstaller le complément Power Pivot pour SharePoint (SharePoint 2016)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)] est un ensemble de composants de serveur d’applications et de services principaux qui fournissent l’accès aux données [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dans une batterie de serveurs [!INCLUDE[SPS2016](../../../includes/sps2016-md.md)] . Le complément [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint (**spPowerpivot16.msi**) est un package d’installation utilisé pour installer les composants de serveur d’applications.  
  
 **Remarque :** cette rubrique décrit l'installation des fichiers solution [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] et de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour l'outil de configuration de SharePoint 2016. Après l’installation, consultez la rubrique suivante pour plus d’informations sur l’outil de configuration et les fonctionnalités supplémentaires : [Configurer PowerPivot et déployer des solutions &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/configure-power-pivot-and-deploy-solutions-sharepoint-2013.md).  
  
 Pour plus d'informations sur le téléchargement de **spPowerPivot16.msi**, consultez [Microsoft® SQL Server® 2016 Power Pivot® pour Microsoft SharePoint®](https://www.microsoft.com/download/details.aspx?id=52675).  
  
##  <a name="bkmk_background"></a> Arrière-plan  
  
-   **Serveur d’applications :** [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] disponibles dans SharePoint 2016 incluent l’utilisation de classeurs comme source de données, l’actualisation planifiée des données et le tableau de bord de gestion [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] .  
  
     [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] est un package [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows Installer (**spPowerpivot16.msi**) qui déploie les bibliothèques clientes Analysis Services et copie les fichiers d’installation de [!INCLUDE[ssGeminiShort2017](../../../includes/ssgeminishort2017-md.md)] sur l’ordinateur. Le programme d'installation ne déploie pas ou ne configure pas de fonctionnalités [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dans SharePoint. Les composants suivants s'installent par défaut :  
  
    -   [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)]. Ce composant inclut des scripts PowerShell (fichiers .ps1), des packages de solution SharePoint (.wsp) et l’outil de configuration [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] pour déployer [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dans une batterie de serveurs SharePoint 2016.  
  
    -   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Fournisseur OLE DB pour Analysis Services (MSOLAP).  
  
    -   Fournisseur de données ADOMD.NET.  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   **Services principaux :** si vous utilisez [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour Excel pour créer des classeurs qui contiennent des données analytiques, Office Online Server doit être configuré avec un serveur BI exécutant [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour accéder à ces données dans un environnement serveur. Vous pouvez exécuter le programme d'installation de SQL Server sur un ordinateur qui possède un serveur SharePoint 2016 installé, ou sur un autre ordinateur sans logiciel SharePoint. Analysis Services n'a pas de dépendances de SharePoint.  
  
     Pour plus d'informations sur l'installation, la désinstallation et la configuration des services principaux, consultez :  
  
    -   [Installation d’Analysis Services en mode Power Pivot](../../../analysis-services/instances/install-windows/install-analysis-services-in-power-pivot-mode.md)  
  
    -   [Désinstaller Power Pivot pour SharePoint](../../../sql-server/install/uninstall-power-pivot-for-sharepoint.md)  
  
##  <a name="bkmk_where_to_install"></a> Où installer spPowerPivot16.msi ?  
 Une bonne pratique pour garantir la cohérence de configuration consiste à installer **spPowerPivot16.msi** sur tous les serveurs de la batterie de serveurs SharePoint, notamment les serveurs d’applications et les serveurs web frontaux. Le package d'installation inclut les fournisseurs de données Analysis Services, ainsi que l'outil de configuration de [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] . Lorsque vous installez **spPowerPivot16.msi** , vous pouvez personnaliser l'installation en excluant des composants.  
  
 **Fournisseurs de données :** plusieurs technologies SharePoint et SQL Server utilisent des fournisseurs de données Analysis Services, y compris PerformancePoint Services et Power View. L'installation de **spPowerPivot16.msi** sur tous les serveurs SharePoint garantit que l'ensemble complet des fournisseurs de données Analysis Services et la connectivité [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] sont constamment disponibles dans la batterie de serveurs.  
  
> [!NOTE]  
>  Vous devez installer les fournisseurs de données Analysis Services sur un serveur SharePoint 2016 à l'aide de **spPowerPivot16.msi**. D'autres packages d'installation disponibles dans le Feature Pack [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] ne sont pas pris en charge, car ils n'incluent pas les fichiers de prise en charge de SharePoint 2016 dont les fournisseurs de données ont besoin dans cet environnement.  
  
 **Outil de configuration :** l'outil de configuration [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] est requis sur un seul des serveurs SharePoint. Toutefois, la meilleure pratique recommandée dans les batteries de plusieurs serveurs consiste à installer l'outil de configuration sur au moins deux serveurs de façon à ce que vous ayez accès à l'outil de configuration si un des deux serveurs est hors connexion.  
  
##  <a name="bkmk_prereq"></a> Spécifications et conditions préalables requises  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SharePoint Server 2016.  
  
-   **spPowerPivot16.msi** n’est disponible que sur les systèmes d’exploitation 64 bits, conformément aux spécifications des produits et technologies SharePoint.  
  
-   Un serveur dans [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] mode. Office Online Server utilisera l'instance SQL Server Analysis Services en tant que serveur [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] . Analysis Services peut s'exécuter sur le serveur SharePoint local ou sur un ordinateur distant. Il ne peut pas être installé sur le serveur Office Online Server.  
  
-   **Autorisations :** pour installer [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)], l'utilisateur actuel doit être administrateur sur l'ordinateur et dans le groupe Administrateur de batterie de serveurs SharePoint.  
  
-   Pour plus d’informations sur les spécifications et les conditions préalables requises pour [!INCLUDE[ssGeminiShort](../../../includes/ssgeminishort-md.md)] , accédez à [Configurations matérielle et logicielle requises pour le serveur Analysis Services en mode SharePoint](http://msdn.microsoft.com/library/fb86ca0a-518c-4c61-ae78-7680c57fae1f).  
  
##  <a name="bkmk_install"></a> Installer Power Pivot pour SharePoint  
 Le package d’installation **spPowerpivot16.msi** prend en charge à la fois une interface graphique utilisateur et un mode ligne de commande. Les deux méthodes d'installation requièrent que vous exécutiez le fichier .msi avec des privilèges d'administrateur. Après l’installation, consultez la rubrique suivante pour plus d’informations sur l’outil de configuration et les fonctionnalités supplémentaires : [Configurer PowerPivot et déployer des solutions &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/configure-power-pivot-and-deploy-solutions-sharepoint-2013.md).  
  
### <a name="user-interface-installation"></a>Installation de l'interface utilisateur  
 Pour installer [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] avec l'interface utilisateur graphique, procédez comme suit :  
  
1.  Exécutez **spPowerPivot16.msi**.  
  
2.  Sur la page de bienvenue, sélectionnez **Suivant** .  
  
3.  Lisez et acceptez le contrat de licence, puis sélectionnez **Suivant**.  
  
4.  Dans la page **Sélection de fonctionnalités** , toutes les fonctionnalités sont sélectionnées par défaut.  
  
5.  Sélectionnez **Suivant**.  
  
6.  Sélectionnez **Installer** pour terminer l'installation.  
  
### <a name="command-line-installation"></a>Installation à partir de la ligne de commande  
 Pour une installation à partir de la ligne de commande, ouvrez une invite de commandes avec des autorisations d’administrateur, puis exécutez **spPowerPivot16.msi**. Par exemple :  
  
 `Msiexec.exe /i spPowerPivot16.msi`.  
  
 Pour créer un journal d'installation, utilisez les commutateurs d'enregistrement MsiExec standards. L'exemple suivant crée le fichier journal « Install_Log.txt » à l'aide du commutateur de journalisation commentée « v ».  
  
```  
Msiexec.exe /i spPowerPivot16.msi /L v c:\test\Install_Log.txt  
```  
  
### <a name="quiet-command-line-installation-for-scripting"></a>Installation silencieuse de script à partir de la ligne de commande  
 Vous pouvez utiliser le commutateur **/q** ou **/quiet** pour une installation « silencieuse » qui n’affiche pas de boîtes de dialogue ni d’avertissements. L'installation silencieuse est utile si vous souhaitez écrire un script d'installation du complément.  
  
> [!IMPORTANT]  
>  Si vous utilisez le commutateur **/q** pour une installation silencieuse à partir de la ligne de commande, le Contrat de Licence Utilisateur Final ne sera pas affiché. Quelle que soit la méthode d'installation, l'utilisation de ce logiciel est régie par un contrat de licence et vous devez respecter les termes contractuels de ce contrat.  
  
 **Pour effectuer une installation sans assistance :**  
  
1.  Ouvrez une invite de commandes **avec des autorisations d'administrateur**.  
  
2.  Exécutez la commande suivante :  
  
    ```  
    Msiexec.exe /i spPowerPivot16.msi /q  
    ```  
  
### <a name="command-line-installation-to-include-specific-components"></a>Installation à partir de la ligne de commande pour inclure des composants spécifiques  
 L'outil de configuration [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] n'est pas requis sur chaque serveur SharePoint, mais il est néanmoins recommandé de l'installer sur au moins deux serveurs de façon à ce qu'il soit disponible lorsque vous en avez besoin.  
  
 Lorsque vous installez le fichier spPowerPivot16.msi, vous pouvez utiliser les options de ligne de commande pour installer des éléments spécifiques, comme les fournisseurs de données, et non l'outil de configuration [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] . La ligne de commande suivante est un exemple d'installation de tous les composants, à l'exception de l'outil de configuration :  
  
```  
Msiexec /i spPowerPivot16.msi AGREETOLICENSE="yes" ADDLOCAL=” SQL_OLAPDM,SQL_ADOMD,SQL_AMO,SQLAS_SP_Common”  
```  
  
|Option| Description|  
|------------|-----------------|  
|Analysis_Server_SP_addin16|[!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] Configuration|  
|SQL_OLAPDM|Fournisseur OLE DB Analysis Services pour SQL Server 2016|  
|SQL_ADOMD|Fournisseur ADOMD.NET|  
|SQL_AMO|Fournisseur SQL Server 2016 Analysis Management Objects (AMO)|  
|SQLAS_SP16_Common|Composants communs Analysis Services pour SharePoint 2016|  
  
##  <a name="bkmk_deploy_solution"></a> Déployer les fichiers solution SharePoint avec l'outil de configuration Power Pivot pour SharePoint 2016  
 Trois des fichiers copiés sur le disque dur par spPowerPivot16.msi sont des fichiers solution de SharePoint. L'étendue d'un fichier solution correspond au niveau de l'application Web, alors que l'étendue des autres fichiers correspond au niveau de la batterie de serveurs. Les fichiers sont les suivants :  
  
-   `PowerPivot16FarmSolution.wsp`  

-   `PowerPivot16WebApplicationSolution.wsp`  
  
 Les fichiers solution sont copiés dans le dossier suivant :  
  
 `ssInstallPathTools\PowerPivotTools\SPAddinConfiguration\Resources`  
  
 Après l'installation du fichier .msi, exécutez l'outil de configuration [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] pour configurer et déployer des solutions dans la batterie de serveurs SharePoint.  
  
 **Pour démarrer l'outil de configuration de :**  
  
 Dans l’écran de démarrage de Windows, tapez « power » et sélectionnez **Configuration [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)]** dans les résultats de recherche d’applications. Notez que les résultats de la recherche peuvent inclure deux liens car le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installe des outils de configuration de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] distincts pour SharePoint 2013 et SharePoint 2016. Assurez-vous de lancer l'outil de configuration [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] .  
  
 ![PowerPivot pour SharePoint 2016](../../../analysis-services/instances/install-windows/media/powerpivot-for-sharepoint-2016-configuration.png "Configuration PowerPivot pour SharePoint 2016")  
  
 **Or**  
  
1.  Accédez à **Démarrer**, **Tous les programmes**.  
  
2.  Sélectionnez [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)].  
  
3.  Sélectionnez **Outils de configuration**.  
  
4.  Sur la page de bienvenue, sélectionnez **[!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] Configuration**.  
  
 Pour plus d'informations sur l'outil de configuration, consultez [Power Pivot Configuration Tools](../../../analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools.md).  
  
##  <a name="bkmk_remove_addin"></a> Désinstaller ou réparer le complément  
  
> [!CAUTION]  
>  Si vous désinstallez **spPowerPivot16.msi** , les fournisseurs de données et l'outil de configuration sont désinstallés. Désinstaller les fournisseurs de données empêchera le serveur de se connecter à [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)].  
  
 Pour désinstaller ou réparer [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] , procédez selon l'une des méthodes suivantes :  
  
1.  **Panneau de configuration Windows :** Sélectionnez [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)]**[!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)]**. Sélectionnez **Désinstaller** ou **Réparer**.  
  
2.  Exécutez le fichier spPowerPivot16.msi et sélectionnez l'option **Supprimer** ou **Réparer** .  
  
 **Ligne de commande :** pour réparer ou désinstaller [!INCLUDE[ssGeminiShort2016](../../../includes/ssgeminishort2016-md.md)] en ligne de commande, ouvrez une invite de commandes **avec des autorisations d'administrateur** et exécutez l'une des commandes suivantes :  
  
-   Pour réparer, exécutez la commande suivante :  
  
    ```  
    msiexec.exe /f spPowerPivot16.msi  
    ```  
  
 - ou -  
  
-   Pour désinstaller, exécutez la commande suivante :  
  
    ```  
    msiexec.exe /uninstall spPowerPivot16.msi  
    ```  
  
  
