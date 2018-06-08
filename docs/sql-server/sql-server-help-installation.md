---
title: Visionneuse d’aide et contenu de l’aide SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 12/15/2017
ms.prod: sql
ms.prod_service: sql
ms.component: sql-non-specified
ms.technology: server-general
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2014
- SQL Server 2016
- SQL Server 2017
ms.assetid: 51f8a08c-51d0-41d8-8bc5-1cb4d42622fb
caps.latest.revision: 8
author: craigg-msft
ms.author: craigg
manager: craigg
ms.openlocfilehash: 3d00aa3a4085b17f8fcc40fc9317b8acd77e5998
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-offline-help-and-help-viewer"></a>Visionneuse d’aide et aide en mode hors connexion SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Vous pouvez utiliser la visionneuse d’aide dans SSMS (SQL Server Management Studio) ou VS (Visual Studio) pour télécharger et installer les packages d’aide SQL Server à partir de sources en ligne ou du disque et les afficher hors connexion. Cet article décrit les outils qui installent la visionneuse d’aide, comment installer le contenu de l’aide en mode hors connexion et comment afficher l’aide de [!INCLUDE[ssSQL14_md](../includes/sssql14-md.md)], SQL Server 2016 et SQL Server 2017.

> [!NOTE]
> L’aide de SQL Server 2016 et celle de SQL Server 2017 sont combinées, même si certaines rubriques s’appliquent aux versions individuelles là où cela est mentionné. La plupart des rubriques s’appliquent aux deux.

## <a name="install-the-help-viewer"></a>Installer la visionneuse d’aide

La visionneuse d’aide comporte deux versions : v2.x prend en charge l’aide de SQL Server 2016/SQL Server 2017, et v1.x prend en charge l’aide de [!INCLUDE[ssSQL14_md](../includes/sssql14-md.md)]. La visionneuse d’aide ne prend en charge ni les paramètres proxy ni le format ISO. 

Les outils suivants installent la visionneuse d’aide : 

|**Outil qui installe la visionneuse d’aide**|**Version de la visionneuse d’aide installée**|
|---------|---------|
|[SQL Server Management Studio 17.x](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)| v2.2|
|[SQL Server Data Tools pour Visual Studio 2015](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)| v2.2|
|Visual Studio 2017* | v2.3|
|Visual Studio 2015 | v2.2|
|SQL Server 2014 Management Studio | v1.x|
|Versions antérieures de Visual Studio | v1.x|
|SQL Server 2016 | v1.x|

\* Pour installer la visionneuse d’aide avec Visual Studio 2017, sur l’onglet Composants individuels dans le programme d’installation de Visual Studio, sélectionnez **Visionneuse d’aide** sous Outils de code, puis cliquez sur **Installer**. 

>[!NOTE]
> - SQL Server 2016 installe la visionneuse d’aide version 1.1, qui ne prend pas en charge l’aide de SQL Server 2016. 
> - L’installation de SQL Server 2017 n’installe aucune visionneuse d’aide.
> - La visionneuse d’aide version 2.x peut également prendre en charge l’aide de [!INCLUDE[ssSQL14_md](../includes/sssql14-md.md)], si vous installez le contenu à partir du disque.

## <a name="use-help-viewer-v2x"></a>Utiliser la visionneuse d’aide version 2.x

SSMS 17.x ainsi que VS 2015 et 2017 utilisent la visionneuse d’aide version 2.x, qui prend en charge l’aide de SQL Server 2016/2017. 

**Pour télécharger et installer le contenu de l’aide en mode hors connexion avec la visionneuse d’aide version 2.x**

1. Dans SSMS ou VS, cliquez sur **Ajouter et supprimer le contenu d’aide** dans le menu Aide. 

   ![HelpViewer2_Ajouter et supprimer le contenu](../sql-server/media/sql-server-help-installation/addremovecontent.png)  

   La visionneuse d’aide s’ouvre et affiche l’onglet Gérer le contenu.  
   
2. Pour installer le dernier package de contenu de l’aide, choisissez **En ligne** sous Source d’installation.
   
   ![HelpViewer2_ManageContent_OnlineSource](../sql-server/media/sql-server-help-installation/helpviewer2-managecontent-onlinesource.png)  
   
   >[!NOTE]
   > Pour une installation à partir du disque (aide de SQL Server 2014), choisissez **Disque** sous Source d’installation et spécifiez l’emplacement du disque.
   
   L’option Chemin d’accès au stockage local sous l’onglet Gérer le contenu indique l’emplacement où le contenu est installé sur l’ordinateur local. Si vous souhaitez modifier l’emplacement, cliquez sur **Déplacer**, entrez un chemin de dossier différent dans le champ **Vers**, puis cliquez sur **OK**.
   Si l’installation de l’aide échoue après avoir modifié le chemin d’accès au stockage local, fermez et rouvrez la visionneuse d’aide, vérifiez que le nouvel emplacement apparaît dans le chemin d’accès au stockage local, puis recommencez l’installation.

3. Cliquez sur **Ajouter** en regard de chaque package de contenu (livre) que vous souhaitez installer. 
   Pour installer tout le contenu de l’aide de SQL Server, ajoutez l’ensemble des 13 livres sous SQL Server. 
   
4. Cliquez sur **Mettre à jour** en bas à droite. 
   La table des matières de l’aide sur la gauche est automatiquement mise à jour avec les livres ajoutés. 
   
   ![HelpViewer2_ManageContent_AddContent](../sql-server/media/sql-server-help-installation/helpviewer2-managecontent-addcontent.png)     
   
> [!NOTE]
> Les titres de nœud supérieur dans la table des matières SQL Server ne correspondent pas tous exactement aux noms des livres d’aide téléchargeables associés. Les titres de la table des matières sont mappés aux noms des livres comme suit :

| Volet de contenu | Livre SQL Server |
|-----|-----|
|Informations de référence sur le langage Analysis Services | Informations de référence sur le langage Analysis Services (MDX)|
|Informations de référence sur le langage DAX (Data Analysis Expressions) | Informations de référence sur le langage DAX (Data Analysis Expressions)|
|Informations de référence sur le langage DMX (Data Mining Extensions) | Informations de référence sur le langage DMX (Data Mining Extensions)|
|Guides du développeur pour SQL Server | Référence du développeur SQL Server|
|Télécharger SQL Server Management Studio | SQL Server Management Studio|
|Bien démarrer avec Machine Learning dans SQL Server | Microsoft Machine Learning Services|
|Informations de référence sur Power Query M | Informations de référence sur Power Query M|
|Pilotes SQL Server | Pilotes de connexion SQL Server|
|SQL Server sur Linux | SQL Server sur Linux|
|Documentation technique de SQL Server | Documentation technique de SQL Server (SSIS, SSRS, moteur de base de données, programme d’installation)|
|Outils et utilitaires pour Azure SQL Database | Outils SQL Server|
|Référence Transact-SQL (moteur de base de données) | Guide de référence Transact-SQL|
|Références relatives au langage Xquery (SQL Server) | Références relatives au langage Xquery (SQL Server)|

> [!NOTE]
> Si la visionneuse d’aide se fige (se bloque) pendant l’ajout du contenu, remplacez la ligne Dernière actualisation du cache = « \<jj/mm/aaaa> 00:00:00 » dans le fichier %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings ou HlpViewer_VisualStudiox_en-US.settings par une date dans le futur. Pour plus d’informations sur ce problème, consultez [La visionneuse d’aide Visual Studio se fige sur l’écran de démarrage](https://msdn.microsoft.com/en-us/library/dd831853.aspx).

**Pour afficher le contenu de l’aide en mode hors connexion dans SSMS avec la visionneuse d’aide version 2.x**

Pour afficher l’aide installée dans SSMS, appuyez sur Ctrl+Alt+F1 ou choisissez **Ajouter ou supprimer du contenu** dans le menu Aide pour lancer la visionneuse d’aide. 

   ![HelpViewer2_Ajouter et supprimer le contenu](../sql-server/media/sql-server-help-installation/addremovecontent.png)  

La visionneuse d’aide s’ouvre et affiche l’onglet Gérer le contenu, avec la table des matières de l’aide installée dans le volet gauche. Cliquez sur les rubriques dans la table des matières pour les afficher dans le volet droit. 
> [!TIP]
> Si le volet de contenu n’est pas visible, cliquez sur Contenu dans la marge gauche. Cliquez sur l’icône représentant une punaise pour garder le volet de contenu ouvert.  

   ![Visionneuse d’aide version 2.x avec du contenu](../sql-server/media/sql-server-help-installation/helpviewer2-withcontentinstalled.png)

**Pour afficher le contenu de l’aide en mode hors connexion dans VS avec la visionneuse d’aide version 2.x**

Pour afficher l’aide installée dans Visual Studio :
1. Pointez sur **Définir les préférences pour l’aide** dans le menu Aide, puis choisissez **Lancer dans la visionneuse d’aide**. 

   ![Visionneuse d’aide dans VS, Définir les préférences pour l’aide](../sql-server/media/sql-server-help-installation/launchviewer.png)

2. Cliquez sur **Afficher l’aide** dans le menu Aide pour afficher le contenu dans la visionneuse d’aide. 

   ![Afficher l’aide](../sql-server/media/sql-server-help-installation/viewhelp.png)

   La table des matières de l’aide s’affiche sur la gauche et la rubrique d’aide sélectionnée sur la droite. 
   
## <a name="use-help-viewer-v1x"></a>Utiliser la visionneuse d’aide version 1.x

Les versions antérieures de SSMS et VS utilisent la visionneuse d’aide version 1.x, qui prend en charge l’aide de SQL Server 2014. 

**Pour télécharger et installer le contenu de l’aide en mode hors connexion avec la visionneuse d’aide version 1.x**

Ce processus utilise la visionneuse d’aide version 1.x pour télécharger l’aide de SQL Server 2014 à partir du Centre de téléchargement Microsoft et l’installer sur votre ordinateur.

1. Accédez au site de téléchargement [Documentation du produit pour Microsoft SQL Server 2014](https://www.microsoft.com/en-us/download/details.aspx?id=42557) et cliquez sur **Télécharger**.  
2. Cliquez sur **Enregistrer** dans la zone de message pour enregistrer le fichier *SQLServer2014Documentation\_\*.exe* sur votre ordinateur.  
   
   >[!NOTE]
   >Pour les environnements protégés par un pare-feu et un proxy, enregistrez le fichier téléchargé sur un lecteur USB ou tout autre média amovible pouvant être amené dans l’environnement.   
   
3. Double-cliquez sur le fichier .exe pour décompresser le fichier de contenu d’aide, puis enregistrez le fichier dans un dossier local ou partagé.  
4. Ouvrez le **Gestionnaire de bibliothèque d’aide** en lançant SSMS ou VS et en cliquant sur **Gérer les paramètres de l’aide** dans le menu Aide.  
5. Cliquez sur **Installer du contenu à partir du disque** et accédez au dossier dans lequel vous avez décompressé le fichier de contenu d’aide.  
   
   ![HelpLibraryManager_MainPage_InstallFromDisk](../sql-server/media/sql-server-help-installation/helplibrarymanager-mainpage-installfromdisk.png)
   
   ![HelpLibraryManager_InstallContentFromDisk_dialog1](../sql-server/media/sql-server-help-installation/helplibrarymanager-installcontentfromdisk-dialog1.png)
   
   > [!IMPORTANT]
   > Pour éviter d’installer du contenu d’aide locale qui ne présente qu’une table des matières partielle, vous devez utiliser l’option **Installer du contenu à partir du disque** dans le **Gestionnaire de bibliothèque d’aide**.  Si vous avez utilisé l’option **Installer du contenu à partir d’une source en ligne** et que la visionneuse d’aide affiche une table des matières partielle, consultez ce [billet de blog](https://blogs.msdn.microsoft.com/womeninanalytics/2016/06/21/troubleshoot-local-help-for-sql-server-2014/) pour savoir comment résoudre ce problème. 
   
8. Cliquez successivement sur le fichier HelpContentSetup.msha, sur **Ouvrir**, puis sur **Suivant**.  
9. Cliquez sur **Ajouter** en regard de la documentation que vous voulez installer, puis cliquez sur **Mettre à jour**.  
   
   ![HelpLibraryManager_InstallContentFromDisk_dialog2](../sql-server/media/sql-server-help-installation/helplibrarymanager-installcontentfromdisk-dialog2.png)  
   
10. Cliquez sur **Terminer**, puis sur **Quitter**.

**Pour afficher le contenu de l’aide en mode hors connexion avec la visionneuse d’aide version 1.x**

11. Pour afficher l’aide installée, ouvrez le **Gestionnaire de bibliothèque d’aide**, cliquez sur **Choisir l’aide en ligne ou locale**, puis sur **Utiliser l’aide locale**.
12. Ouvrez la visionneuse d’aide pour afficher le contenu en cliquant sur **Afficher l’aide** dans le menu **Aide**. Le contenu que vous avez installé est répertorié dans le volet gauche.  
   
   ![HelpViewer1_withContentInstalled_ZoomedIn](../sql-server/media/sql-server-help-installation/helpviewer1-withcontentinstalled-zoomedin.png)  
   
## <a name="view-online-help"></a>Afficher l’aide en ligne

L’aide en ligne affiche toujours le contenu le plus récent. 

**Pour afficher l’aide en ligne de SQL Server dans SSMS 17.x**

- Cliquez sur **Afficher l’aide** dans le menu **Aide**. La documentation la plus récente de SQL Server 2016/2017 dans [ https://docs.microsoft.com/sql/https://docs.microsoft.com/en-us/sql/sql-server/sql-server-technical-documentation ](https://docs.microsoft.com/en-us/sql/sql-server/sql-server-technical-documentation) s’affiche dans un navigateur. 

   ![Afficher l’aide](../sql-server/media/sql-server-help-installation/viewhelp.png)

**Pour afficher l’aide en ligne de Visual Studio dans Visual Studio**

1. Pointez sur **Définir les préférences pour l’aide** dans le menu Aide, puis choisissez **Lancer dans le navigateur** ou **Lancer dans la visionneuse d’aide**. 
2. Cliquez sur **Afficher l’aide** dans le menu Aide. La toute dernière aide de Visual Studio s’affiche dans l’environnement sélectionné. 

**Pour afficher l’aide en ligne avec la visionneuse d’aide version 1.x**

1. Ouvrez le **Gestionnaire de bibliothèque d’aide** en cliquant sur **Gérer les paramètres de l’aide** dans le menu Aide.  
2. Dans la boîte de dialogue Gestionnaire de bibliothèque d’aide, cliquez sur **Choisir l’aide en ligne ou locale**.  
   
   ![HelpLibraryManager_MainPage_ChooseOnlineORLocal.png](../sql-server/media/sql-server-help-installation/helplibrarymanager-mainpage-chooseonlineorlocal.png.png)  
   
3. Cliquez sur **Utiliser l’aide en ligne**, sur **OK**, puis sur **Quitter**.  
   
   ![HelpLibraryManager_ChooseOnlineORLocalHelp_OnlineHelpSelected_dialog](../sql-server/media/sql-server-help-installation/helplibrarymanager-chooseonlineorlocalhelp-onlinehelpselected-dialog.png)
   
4. Ouvrez la visionneuse d’aide pour afficher le contenu en cliquant sur **Afficher l’aide** dans le menu **Aide**. 

## <a name="view-f1-help"></a>Afficher l’aide F1

Quand vous appuyez sur F1, ou cliquez sur **Aide** ou l’icône **?** dans une boîte de dialogue dans SSMS ou VS, une rubrique d’aide en ligne contextuelle s’affiche dans le navigateur ou la visionneuse d’aide. 

**Pour afficher l’aide F1**

1. Pointez sur **Définir les préférences pour l’aide** dans le menu Aide, puis choisissez **Lancer dans le navigateur** ou **Lancer dans la visionneuse d’aide**. 
2. Appuyez sur F1, ou cliquez sur **Aide** ou **?** dans les boîtes de dialogue là où ces options sont disponibles pour afficher les rubriques en ligne contextuelles dans l’environnement sélectionné.

>  [!NOTE]
>  L’aide F1 fonctionne uniquement quand vous êtes connecté. Il n’existe aucune source hors connexion pour l’aide F1. 
   

## <a name="next-steps"></a>Étapes suivantes
[Microsoft Help Viewer - Visual Studio](/visualstudio/ide/microsoft-help-viewer)  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
