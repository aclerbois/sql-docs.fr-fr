---
title: Installer Analysis Services en Mode Power Pivot | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 59d3f4dadc2de71f8fa4438ec48a2783164a485a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="install-analysis-services-in-power-pivot-mode"></a>Installation d’Analysis Services en mode Power Pivot
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Les procédures de cette rubrique guident à travers l'installation d'un serveur [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour un déploiement SharePoint. Ces étapes comprennent l'exécution de l'Assistant Installation de SQL Server ainsi que des tâches de configuration qui utilisent l'Administration centrale de SharePoint.  
  
##  <a name="bkmk_background"></a> Arrière-plan  
 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint est une collection de services intermédiaires et de services principaux qui fournissent un accès aux données [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dans une batterie de serveurs SharePoint 2016 ou SharePoint 2013.  
  
-   **Services principaux :** si vous utilisez [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour Excel pour créer des classeurs qui contiennent des données analytiques, vous devez disposer de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint pour accéder à ces données dans un environnement serveur. Vous pouvez exécuter le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur un ordinateur sur lequel SharePoint Server est installé ou sur un autre ordinateur sans logiciel SharePoint. [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] ne dépend pas de SharePoint.  
  
     **Remarque :** cette rubrique décrit l'installation du serveur [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] et les services principaux.  
  
-   **Niveau intermédiaire :** améliorations apportées aux expériences [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dans SharePoint, notamment la Galerie [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] , l’actualisation planifiée des données, le tableau de bord de gestion et les fournisseurs de données. Pour plus d'informations sur l'installation et la configuration du niveau intermédiaire, consultez :  
  
    -   [Installer ou désinstaller le complément Power Pivot pour SharePoint (SharePoint 2016)](../../../analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2016.md)  
  
    -   [Installer ou désinstaller le complément Power Pivot pour SharePoint &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013.md)  
  
    -   [Configurer PowerPivot et déployer des solutions &#40;SharePoint 2016&#41;](../../../analysis-services/instances/install-windows/configure-power-pivot-and-deploy-solutions-sharepoint-2016.md)  
  
    -   [Configurer PowerPivot et déployer des solutions &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/configure-power-pivot-and-deploy-solutions-sharepoint-2013.md)  
  
##  <a name="bkmk_prereq"></a> Configuration requise  
  
1.  Vous devez être administrateur local pour exécuter le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
2.  SharePoint Enterprise Edition est requis pour [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint. Vous pouvez également utiliser l'édition Enterprise Evaluation.  
  
3.  L'ordinateur doit être associé à un domaine dans la même forêt Active Directory qu'Office Online Server (SharePoint 2016) or  Excel Services (SharePoint 2013).  
  
4.  Le nom de l'instance [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] doit être disponible. Vous ne pouvez pas avoir une instance existante nommée [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]sur l’ordinateur sur lequel vous installez Analysis Services en mode [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] .  
  
     **Remarque :** le nom de l'instance doit être POWERPIVOT.  
  
5.  Consultez [Configurations matérielle et logicielle requises pour le serveur Analysis Services en mode SharePoint](http://msdn.microsoft.com/library/fb86ca0a-518c-4c61-ae78-7680c57fae1f).  
  
6.  Consultez les notes de publication sous [SQL Server 2016 Release Notes](../../../sql-server/sql-server-2016-release-notes.md).  
  
###  <a name="bkmk_sqleditions"></a> Configuration requise de SQL Server  
 Les fonctionnalités de Business Intelligence ne sont pas toutes disponibles dans toutes les éditions de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Pour plus d’informations, consultez [Analysis Services fonctionnalités prises en charge par les éditions de SQL Server 2016](../../../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md) et [éditions et composants de SQL Server 2016](../../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
##  <a name="InstallSQL"></a> Étape 1 : installer Power Pivot pour SharePoint  
 Au cours de cette étape, vous exécutez le programme d'installation de SQL Server pour installer un serveur [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] . Dans une étape suivante, vous allez configurer Excel Services pour utiliser ce serveur pour les modèles de données du classeur.  
  
1.  Exécutez l'Assistant Installation de SQL Server (Setup.exe).  
  
2.  Sélectionnez **Installation** dans le volet de navigation gauche.  
  
3.  Sélectionnez **Nouvelle installation autonome SQL Server ou ajout de fonctionnalités à une installation existante**.  
  
4.  Si la page **Clé de produit** s'affiche, indiquez l'édition d'évaluation ou entrez une clé de produit pour une copie sous licence de l'édition entreprise. Sélectionnez **Suivant**. Pour plus d'informations sur les éditions, consultez [Editions and Components of SQL Server 2016](../../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
5.  Lisez et acceptez les termes du contrat de licence logiciel Microsoft, puis sélectionnez **Suivant**.  
  
6.  Si la page **Règles globales** s'affiche, passez en revue les informations sur les règles de l'Assistant Installation.  
  
7.  Dans la page **Microsoft Update** , il est recommandé d'utiliser Microsoft Update pour rechercher les mises à jour. Sélectionnez ensuite **Suivant**.  
  
8.  La page **Installer les fichiers d'installation** s'exécute pendant plusieurs minutes. Vérifiez les avertissements de règle ou les échecs de règle, puis sélectionnez **Suivant**.  
  
9. Si une autre page **Règles de support du programme d'installation**s'affiche, examinez les avertissements, puis cliquez sur **Suivant**.  
  
     **Remarque :** le Pare-feu Windows étant activé, un avertissement s'affiche indiquant qu'il faut ouvrir les ports pour activer l'accès à distance.  
  
10. Dans la page **Rôle d'installation** , sélectionnez **Installation de fonctionnalités SQL Server**  
  
     Sélectionnez **Suivant**.  
  
11. Dans la page Sélection de fonctionnalités, sélectionnez **Analysis Services**. Cette option vous permet d’installer l’un des trois modes [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] . Vous sélectionnerez le mode au cours d’une étape ultérieure. Sélectionnez **Suivant**.  
  
12. Sur la page **Configuration de l’instance** , sélectionnez **Instance nommée** et entrez **POWERPIVOT** comme nom de l’instance, puis cliquez sur **Suivant**.  
  
     ![Installation de SQL - Configuration de l’Instance Page d’accueil](../../../analysis-services/instances/install-windows/media/sql2016-pp-instance-config-landing-page.png "l’installation de SQL - Configuration de l’Instance Page d’accueil")  
  
13. Dans la page **Configuration du serveur** , configurez tous les services pour le **Type de démarrage**automatique. Spécifiez le compte de domaine et le mot de passe de votre choix pour **SQL Server Analysis Services**, **(1)** dans le diagramme suivant.  
  
    -   Pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)], vous pouvez utiliser un compte d' **utilisateur de domaine** ou le compte **NetworkService** . N'utilisez pas de compte LocalSystem ou LocalService.  
  
    -   Si vous avez ajouté le Moteur de base de données SQL Server et l'Agent SQL Server, vous pouvez configurer les services à exécuter sous des comptes d'utilisateur de domaine ou sous un compte virtuel par défaut.  
  
    -   Ne configurez jamais de comptes de service avec votre propre compte d'utilisateur de domaine. Cela aurait pour conséquence d'accorder aux ressources du réseau les mêmes autorisations que celles dont vous bénéficiez. Si un utilisateur malveillant compromet le serveur, cet utilisateur est connecté avec vos informations d'identification de domaine. L'utilisateur dispose des autorisations pour télécharger ou utiliser les mêmes données et applications que vous.  
  
     Sélectionnez **Suivant**.  
  
     ![Installation de SQL - page d’accueil de Configuration du serveur](../../../analysis-services/instances/install-windows/media/sql2016-pp-server-config-landing-page.png "d’installation de SQL - page d’accueil de Configuration du serveur")  
  
14. Si vous installez le [!INCLUDE[ssDE](../../../includes/ssde-md.md)], la page **Configuration du moteur de base de données** s'affiche. Dans la page Configuration du [!INCLUDE[ssDE](../../../includes/ssde-md.md)] , sélectionnez **Ajouter l'utilisateur actuel** pour accorder vos autorisations d'administrateur de compte d'utilisateur sur l'instance du moteur de base de données.  
  
     Sélectionnez **Suivant**.  
  
15. Dans la page **Configuration d’Analysis Services** , sélectionnez le **Mode PowerPivot** sous **Mode serveur**  
  
     ![Installation de SQL - Page d’accueil de la Configuration de Analysis Services](../../../analysis-services/instances/install-windows/media/sql2016-pp-as-config-landing-page.png "l’installation de SQL - Page d’accueil de la Configuration de Analysis Services")  
  
16. Dans la page **Configuration d'Analysis Services** , sélectionnez **Ajouter l'utilisateur actuel** pour accorder vos autorisations administratives au compte d'utilisateur. Vous devrez avoir des autorisations d'administrateur pour configurer le serveur une fois l'installation terminée.  
  
    -   Dans la même page, ajoutez le compte d'utilisateur Windows de toute personne qui a aussi besoin d'autorisations d'administration. Par exemple, tout utilisateur qui souhaite se connecter à l'instance du [!INCLUDE[ssGeminiSrv](../../../includes/ssgeminisrv-md.md)] dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] , pour résoudre des problèmes de connexion de base de données ou obtenir des informations sur la version, doit avoir des autorisations d'administrateur système sur le serveur. Ajoutez le compte d'utilisateur de toute personne susceptible de devoir dépanner ou administrer le serveur.  
  
    -   > [!NOTE]  
        >  Les applications de service qui nécessitent l'accès à l'instance de serveur Analysis Services doivent avoir des autorisations d'administration Analysis Services. Par exemple, ajoutez les comptes de service pour Excel Services, Power View et les services Performance Point. En outre, ajoutez le compte de batterie de serveurs SharePoint, utilisé comme identité de l'application Web qui héberge l'Administration centrale.  
  
     Sélectionnez **Suivant**.  
  
17. Dans la page **Rapport d'erreurs** , sélectionnez **Suivant**.  
  
18. Dans la page **Prêt pour l'installation** , sélectionnez **Installer**.  
  
19. Si vous voyez la boîte de dialogue **Redémarrage requis de l'ordinateur**, sélectionnez **OK**.  
  
20. Une fois l'installation terminée, cliquez sur **Fermer**.  
  
21. Redémarrez l'ordinateur.  
  
22. Si vous avez un pare-feu dans votre environnement, consultez la rubrique de la Documentation en ligne de SQL Server [Configure the Windows Firewall to Allow Analysis Services Access](../../../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md).  
  
### <a name="verify-the-sql-server-installation"></a>Vérifier l'installation de SQL Server  
 Vérifiez que le service Analysis Services est en cours d'exécution.  
  
1.  Dans Microsoft Windows,sélectionnez **Démarrer**, **Tous les programmes**, puis le groupe **Microsoft SQL Server 2016** .  
  
2.  Sélectionnez **SQL Server Management Studio**.  
  
3.  Connectez-vous à l’instance Analysis Services, par exemple **[nom serveur]\POWERPIVOT**. Si vous vous connectez à l'instance, vous avez vérifié que le service s'exécute.  
  
##  <a name="bkmk_config"></a> Étape 2 : configurer l'intégration SharePoint de base pour Analysis Services  
 Les étapes suivantes décrivent les modifications de configuration nécessaires pour que vous puissiez interagir avec des modèles de données avancés Excel dans une bibliothèque de documents SharePoint. Effectuez ces étapes après avoir installé SharePoint et SQL Server Analysis Services.  
  
### <a name="sharepoint-2016"></a>SharePoint 2016  
 Excel Services a été supprimé de SharePoint 2016. Office Online Server est maintenant utilisé pour l’hébergement d’Excel.  
  
#### <a name="grant-office-online-server-machine-account-administration-rights-on-analysis-services"></a>Accordez au compte d’ordinateur Office Online Server des droits d’administration pour Analysis Services  
 Vous n'avez pas besoin de terminer cette section si vous avez ajouté le compte d’ordinateur Office Online Server en tant qu'administrateur Analysis Services pendant l’installation d’Analysis Services.  
  
1.  Sur le serveur Analysis Services, démarrez SQL Server Management Studio et connectez-vous à l'instance Analysis Services, par exemple `[MyServer]\POWERPIVOT`.  
  
2.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur le nom de l’instance, puis sélectionnez **Propriétés**.  
  
     ![Afficher les propriétés d’un serveur SSAS](../../../analysis-services/instances/install-windows/media/as-ssms-proeprties.gif "afficher les propriétés d’un serveur SSAS")  
  
3.  Dans le volet gauche, sélectionnez **Sécurité**. Ajoutez le compte d’ordinateur sur lequel Office Online Server est installé.  
  
     ![Paramètres de sécurité d’un serveur SSAS](../../../analysis-services/instances/install-windows/media/as-ssms-security.gif "des paramètres de sécurité d’un serveur SSAS")  
  
#### <a name="register-analysis-services-server-with-office-online-server"></a>Inscription d’un serveur Analysis Services avec Office Online Server  
 Effectuez les étapes suivantes sur l’Office Online Server.  
  
-   Ouvrez une fenêtre de commande PowerShell en tant qu’administrateur.  
  
-   Chargez le module PowerShell `OfficeWebApps` .  
  
     `Import-Module OfficeWebApps`  
  
-   Ajoutez le serveur Analysis Services, par exemple `[MyServer]\POWERPIVOT`.  
  
     `New-OfficeWebAppsExcelBIServer -ServerId [MyServer]\POWERPIVOT]`  
  
### <a name="sharepoint-2013"></a>SharePoint 2013  
  
#### <a name="grant-excel-services-server-administration-rights-on-analysis-services"></a>Accorder des droits d'administration de serveur Excel Services dans Analysis Services  
 Vous n'avez pas besoin de terminer cette section si lors de l'installation d'Analysis Services, vous avez ajouté le compte de service de l'application Excel Services en tant qu'administrateur Analysis Services.  
  
1.  Sur le serveur Analysis Services, démarrez SQL Server Management Studio et connectez-vous à l'instance Analysis Services, par exemple `[MyServer]\POWERPIVOT`.  
  
2.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur le nom de l’instance, puis sélectionnez **Propriétés**.  
  
     ![Afficher les propriétés d’un serveur SSAS](../../../analysis-services/instances/install-windows/media/as-ssms-proeprties.gif "afficher les propriétés d’un serveur SSAS")  
  
3.  Dans le volet gauche, sélectionnez **Sécurité**. Ajoutez la connexion de domaine que vous avez configurée pour l'application Excel Services à l'étape 1.  
  
     ![Paramètres de sécurité d’un serveur SSAS](../../../analysis-services/instances/install-windows/media/as-ssms-security.gif "des paramètres de sécurité d’un serveur SSAS")  
  
#### <a name="configure-excel-services-for-analysis-services-integration"></a>Configurer Excel Services pour l'intégration d'Analysis Services  
  
1.  Dans l'Administration centrale de SharePoint, sous le groupe Gestion des applications, cliquez sur **Gérer les applications de service**.  
  
2.  Cliquez sur le nom de votre application de service, par défaut **Application Excel Services**.  
  
3.  Dans la page **Gérer l'application Excel Services**, cliquez sur **Paramètres du modèle de données**.  
  
4.  Cliquez sur **Ajouter un serveur**.  
  
5.  Sous **Nom du serveur**, entrez le nom du serveur Analysis Services et le nom de l'instance [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] . Par exemple, `MyServer\POWERPIVOT`. Le nom de l'instance [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] est obligatoire.  
  
     Tapez une description.  
  
6.  Cliquez sur **OK**.  
  
7.  Les modifications prendront effet après quelques minutes ou vous pouvez **Arrêter** et **Démarrer** le service **Services de calcul Excel**. Pour  
  
     Une autre option consiste à ouvrir une invite de commandes avec des privilèges d'administrateur, puis taper `iisreset /noforce`.  
  
     Vérifiez que le serveur est reconnu par Excel Services en examinant les entrées du journal ULS. Les entrées ressemblent à l'exemple suivant :  
  
    ```  
    Excel Services Application            Data Model        27           Medium                Check Administrator Access ([ServerName]\POWERPIVOT): Pass.        f127bd9b-bae3-e0e0-9b48-3f7b5ad1eae6  
    Excel Services Application            Data Model        27           Medium                Check Server Version ([ServerName]\POWERPIVOT): Pass (11.0.2809.24 >= 11.0.2800.0).         f127bd9b-bae3-e0e0-9b48-3f7b5ad1eae6  
    Excel Services Application            Data Model        27           Medium                Check Deployment Mode ([ServerName]\POWERPIVOT): Pass.            f127bd9b-bae3-e0e0-9b48-3f7b5ad1eae6  
  
    ```  
  
##  <a name="bkmk_verify"></a> Étape 3 : vérifier l'intégration  
 Les étapes suivantes vous guident tout au long des processus de création et de téléchargement d'un classeur pour vérifier l'intégration d'Analysis Services. Vous aurez besoin d'une base de données SQL Server pour terminer les étapes.  
  
1.  **Remarque :** si vous disposez déjà d'un classeur avancé avec des segments ou des filtres, téléchargez-le dans votre bibliothèque de documents SharePoint et vérifiez que vous pouvez interagir avec les segments et les filtres à partir de la vue de la bibliothèque de documents.  
  
2.  Démarrez un nouveau classeur dans Excel.  
  
3.  Dans l'onglet Données, sélectionnez **À partir d'autres sources** sur le ruban du groupe **Obtenir des données externes**.  
  
4.  Sélectionnez **À partir de SQL Server**.  
  
5.  Dans l' **Assistant Connexion de données**, entrez le nom de l'instance SQL Server qui contient la base de données à utiliser.  
  
6.  Sous Références de connexion, vérifiez que l'option **Utiliser l'authentification Windows** est sélectionnée, puis cliquez sur **Suivant**.  
  
7.  Sélectionnez la base de données à utiliser.  
  
8.  Assurez-vous que la case à cocher **Connexion à une table spécifique** est activée.  
  
9. Activez la case à cocher **Activer la sélection de plusieurs tables et ajouter des tables au modèle de données Excel** .  
  
10. Sélectionnez les tables à importer.  
  
11. Activez la case à cocher **Importer les relations entre les tables sélectionnées**, puis cliquez sur **Suivant**. Importer plusieurs tables d'une base de données relationnelle vous permet de travailler avec les tables qui sont déjà associées. Vous réduisez le nombre d'étapes nécessaires, car vous n'avez pas à créer les relations manuellement.  
  
12. Dans la page **Enregistrement du fichier de connexion de données et fin** de l'Assistant, entrez un nom pour votre connexion, puis sélectionnez **Terminer**.  
  
13. La boîte de dialogue **Importer les données** s'affiche. Choisissez **Rapport de tableau croisé dynamique**, puis sélectionnez **OK**.  
  
14. Une liste de champs de tableau croisé dynamique s'affiche dans le classeur.   
    Dans la liste de champs, sélectionnez l'onglet **Tous** .  
  
15. Ajoutez des champs aux zones Ligne, Colonnes et Valeur de la liste de champs.  
  
16. Ajoutez un segment ou un filtre au tableau croisé dynamique. **N'ignorez pas cette étape**. Un segment ou un filtre est l'élément qui vous permet de vérifier votre installation d'Analysis Services.  
  
17. Enregistrez le classeur dans une bibliothèque de documents dans votre batterie de serveurs SharePoint. Vous pouvez également enregistrer le classeur sur un partage de fichiers et le charger dans la bibliothèque de documents SharePoint.  
  
18. Sélectionnez le nom de votre classeur pour l'afficher dans Excel Online, puis cliquez sur le segment ou modifier le filtre que vous avez ajouté précédemment. Si une mise à jour des données est exécutée, vous savez qu'Analysis Services est installé et disponible dans Excel. Si vous ouvrez le classeur dans Excel vous utiliserez une copie mise en cache et non pas le serveur Analysis Services.  
  
##  <a name="bkmk_firewall"></a> Configurer le pare-feu Windows pour autoriser l'accès à Analysis Services  
 Utilisez les informations de la rubrique [Configurer le pare-feu Windows pour autoriser l'accès à Analysis Services](../../../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md) pour déterminer si vous devez débloquer des ports dans un pare-feu pour permettre l'accès à Analysis Services ou [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint. Vous pouvez suivre les étapes fournies dans la rubrique pour configurer les paramètres des ports et du pare-feu. Dans la pratique, il est conseillé d'effectuer ces étapes en même temps pour permettre l'accès à votre serveur Analysis Services.  
  
##  <a name="bkmk_upgrade_workbook"></a> Mettre à niveau les classeurs et l'actualisation planifiée des données  
 Les étapes nécessaires à la mise à niveau des classeurs créés dans les versions antérieures de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dépendent de la version de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] dans laquelle le classeur a été créé. Pour plus d’informations, consultez [Mettre à niveau les classeurs et l’actualisation planifiée des données &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/upgrade-workbooks-and-scheduled-data-refresh-sharepoint-2013.md).  
  
##  <a name="bkmk_multiple_servers"></a> Au-delà de l'installation de serveur unique - Power Pivot pour Microsoft SharePoint  
 **Serveur Web frontal (WFE)** ou **niveau intermédiaire**: pour utiliser un serveur [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode SharePoint dans une batterie de serveurs SharePoint plus large et installer des fonctionnalités [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] supplémentaires dans la batterie de serveurs, exécutez le package d’installation **spPowerPivot16.msi (SharePoint 2016) ou spPowerPivot.msi (SharePoint 2013),** sur chaque serveur SharePoint. Le fichier spPowerPivot16.msi ou spPowerPivot.msi installe les fournisseurs de données nécessaires et l'outil de configuration [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2016 ou 2013.  
  
 Pour plus d'informations sur l'installation et la configuration du niveau intermédiaire, consultez :  
  
-   [Installer ou désinstaller le complément Power Pivot pour SharePoint &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013.md)  
  
-   [Installer ou désinstaller le complément Power Pivot pour SharePoint &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013.md)  
  
-   Pour télécharger le fichier .msi, consultez [Microsoft SQL Server 2016 Power Pivot pour Microsoft SharePoint 2016](http://go.microsoft.com/fwlink/?LinkID=324854).  
  
-   [Configurer PowerPivot et déployer des solutions &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/configure-power-pivot-and-deploy-solutions-sharepoint-2013.md)  
  
 **Redondance et charge du serveur :** l'installation d'un deuxième (ou plus) serveur [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] permet une redondance des fonctionnalités du serveur [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] . Les serveurs supplémentaires distribuent également la charge entre plusieurs serveurs. Pour plus d'informations, consultez les documents suivants :  
  
-   [Configurer Analysis Services pour le traitement des modèles de données dans Excel Services (SharePoint 2013)](http://technet.microsoft.com/library/jj614437(v=office.15)).  
  
-   [Gérer les paramètres de modèle de données Excel Services (SharePoint 2013)](http://technet.microsoft.com/library/jj219780(v=office.15)).  
  
 ![Les paramètres SharePoint](../../../analysis-services/media/as-sharepoint2013-settings-gear.gif "paramètres SharePoint") [envoyer les informations de commentaires et de contact par le biais des commentaires de SQL Server](https://feedback.azure.com/forums/908035-sql-server).  
  
## <a name="see-also"></a>Voir aussi  
 [Migrer PowerPivot vers SharePoint 2013](../../../analysis-services/instances/install-windows/migrate-power-pivot-to-sharepoint-2013.md)   
 [Installer ou désinstaller le complément Power Pivot pour SharePoint &#40;SharePoint 2013&#41;](../../../analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013.md)   
 [Mettre à niveau les classeurs et l’actualisation planifiée des données & #40 ; SharePoint 2013 & #41 ;](../../../analysis-services/instances/install-windows/upgrade-workbooks-and-scheduled-data-refresh-sharepoint-2013.md)  
  
  
