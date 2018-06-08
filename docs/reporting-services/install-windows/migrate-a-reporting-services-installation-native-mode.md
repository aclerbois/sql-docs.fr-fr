---
title: Migrer une installation Reporting Services (mode natif) | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- manual Reporting Services migrations
- Report Server Windows service
- custom Reporting Services installations
- automatic Reporting Services migrations
- Reporting Services, upgrades
- upgrading Reporting Services
- migrating Reporting Services
ms.assetid: a6fc56c1-c504-438d-a2b0-5ed29c24e7d6
caps.latest.revision: 54
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 6502c85bdf016f325058c69fc5ae01f8d2553e87
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34550650"
---
# <a name="migrate-a-reporting-services-installation-native-mode"></a>Migrer une installation Reporting Services (mode natif)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

Cette rubrique fournit des instructions détaillées de migration de l’une des versions prises en charge suivantes d’un déploiement en mode natif de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vers une nouvelle instance de SQL Server Reporting Services :  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
-   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  

Pour plus d’informations sur la migration d’un déploiement en mode SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , consultez [Migrer une installation Reporting Services &#40;mode SharePoint&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md).  
  
 La migration s’entend ici comme étant le déplacement de fichiers de données d’application vers une nouvelle instance de SQL Server. Voici quelques raisons courantes pour lesquelles vous devez migrer votre installation :  
  
-   Vous disposez d'un déploiement à grande échelle ou d'un temps d'exécution spécifique.  
  
-   Vous modifiez le matériel ou la topologie de votre installation.  
  
-   Vous rencontrez un problème qui empêche la mise à niveau.

## <a name="bkmk_nativemode_migration_overview"></a> Présentation de la migration en mode natif

 Le processus de migration pour [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] comprend des étapes manuelles et des étapes automatiques. Les tâches suivantes sont inhérentes au processus de migration d'un serveur de rapports :  
  
-   Sauvegardez les fichiers de base de données, de configuration et d'application.  
  
-   Sauvegardez la clé de chiffrement.  
  
-   Installez une nouvelle instance de SQL Server. Si vous utilisez le même matériel, vous pouvez installer SQL Server côte à côte avec votre installation existante s’il s’agit d’une des versions prises en charge.  
  
    > [!TIP]  
    >  Une installation côte à côte peut nécessiter l’installation de SQL Server comme instance nommée.  
  
-   Déplacez la base de données du serveur de rapports et les autres fichiers d’application de votre installation existante vers votre nouvelle installation de SQL Server.  
  
-   Déplacez tous les fichiers d'application personnalisés vers la nouvelle installation.  
  
-   Configurez le serveur de rapports.  
  
-   Modifiez **RSReportServer.config** pour y inclure tous les paramètres personnalisés de votre installation précédente.  
  
-   Le cas échéant, configurez des listes de contrôle d’accès (ACL, Access Control List) personnalisées pour le nouveau groupe du service Windows [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   Supprimez les applications et outils inutiles après avoir vérifié que la nouvelle instance est complètement opérationnelle.  
  
 Des restrictions s'appliquent pour les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui hébergent la base de données du serveur de rapports. Consultez la rubrique suivante si vous réutilisez une base de données du serveur de rapports créée dans une installation précédente.  
  
-   [Créer une base de données du serveur de rapports](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
  
##  <a name="bkmk_fixed_database_name"></a> Nom de base de données fixe  
 Vous ne pouvez pas renommer la base de données du serveur de rapports. L'identité de la base de données est enregistrée dans des procédures stockées du serveur de rapports lors de la création de la base de données. Renommer les bases de données primaires ou temporaires du serveur de rapports provoque des erreurs lors de l'exécution des procédures, invalidant alors votre installation du serveur de rapports.  
  
 Si le nom de la base de données de l'installation existante ne convient pas pour la nouvelle installation, vous devez envisager de créer une base de données portant le nom souhaité, puis de charger les données d'application existantes à l'aide des techniques énumérées ci-dessous :  
  
-   Écrivez un script [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] qui appelle des méthodes SOAP du service Web Report Server pour copier des données entre des bases de données. Vous pouvez utiliser l'utilitaire RS.exe pour exécuter le script. Pour plus d’informations sur cette approche, consultez [Scripts et PowerShell avec Reporting Services](../../reporting-services/tools/scripting-and-powershell-with-reporting-services.md).  
  
-   Écrivez du code qui appelle le fournisseur WMI pour copier des données entre des bases de données. Pour plus d’informations sur cette approche, consultez [Accès au fournisseur WMI de Reporting Services](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  
  
-   Si vous avez seulement quelques éléments, vous pouvez publier de nouveau les rapports, les modèles de rapport et les sources de données partagées du Concepteur de rapports, du Générateur de modèles et du Générateur de rapports sur le nouveau serveur de rapports. Vous devez recréer les attributions de rôles, les abonnements, les planifications partagées, les planifications d'instantanés de rapports, les propriétés personnalisées que vous définissez sur les rapports ou d'autres éléments, la sécurité des éléments de modèle et les propriétés que vous définissez sur le serveur de rapports. L'historique de rapport et les données du journal des exécutions des rapports seront perdus.  
  
##  <a name="bkmk_before_you_start"></a> Avant de commencer  
 Même si vous effectuez une migration et non une mise à niveau de l'installation, pensez à exécuter le Conseiller de mise à niveau sur votre installation existante ; cela vous permettra d'identifier les problèmes susceptibles d'affecter la migration. Cette étape est particulièrement utile si vous migrez un serveur de rapports que vous n'avez pas installé ou configuré vous-même. En exécutant le Conseiller de mise à niveau, vous pouvez trouver des informations sur des paramètres personnalisés qui peuvent ne pas être pris en charge dans une nouvelle installation de SQL Server.  
  
 Vous devez en outre prendre connaissance de plusieurs changements importants introduits dans SQL Server Reporting Services et décrites ci-dessous, qui affectent la manière dont vous migrez votre installation :
 
- Le nouveau [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] a remplacé le Gestionnaire de rapports.
  
- Depuis [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], IIS n'est plus indispensable. Si vous effectuez une migration d'une installation du serveur de rapports vers un nouvel ordinateur, il n'est pas nécessaire d'ajouter le rôle de serveur Web. De plus, la procédure de configuration des URL et de l'authentification diffère par rapport à la précédente version, de même que les techniques et outils de diagnostic et de résolution des problèmes.  
  
- Le service web Report Server, le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]et le service Windows Report Server s’exécutent sous le même compte. Toutes les trois lisent les paramètres de configuration du fichier RSReportServer.config. 
  
- Le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] et SQL Server Management Studio sont conçus de manière à supprimer les fonctionnalités à double emploi. Chaque outil prend en charge un ensemble de tâches distinct.
  
- Les filtres ISAPI ne sont pas pris en charge dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et versions ultérieures. Si vous utilisez des filtres ISAPI, vous devez reconcevoir votre solution de création de rapports avant la migration.  
  
- Les restrictions d’adresse IP ne sont pas prises en charge dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et versions ultérieures. Si vous utilisez des restrictions d'adresse IP, vous devez revoir votre solution de création de rapports avant la migration ou utiliser une technologie telle qu'un pare-feu, un routeur ou un traducteur d'adresses réseau (NAT, Network Address Translation) pour configurer des adresses restreintes pouvant accéder au serveur de rapports.  
  
- Les certificats SSL (Secure Sockets Layer) clients ne sont pas pris en charge dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et versions ultérieures. Si vous utilisez des certificats SSL clients, vous devez reconcevoir votre solution de création de rapports avant la migration.  
  
- Si vous utilisez un type d’authentification autre que l’authentification intégrée de Windows, vous devez mettre à jour l’élément `<AuthenticationTypes>` dans le fichier **RSReportServer.config** avec un type d’authentification pris en charge. Les types d'authentification pris en charge sont NTLM, Kerberos, Negotiate et Basic. Les authentifications Digest, .NET Passport et anonyme ne sont pas prises en charge dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et versions ultérieures.  
  
- Si vous utilisez des feuilles de style en cascade personnalisées dans votre environnement de création de rapports, elles ne seront pas migrées. Vous devez les déplacer manuellement après la migration.  
  
Pour plus d’informations sur les changements effectués dans SQL Server Reporting Services, consultez la documentation relative au Conseiller de mise à niveau et [Nouveautés de Reporting Services](../../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).  

## <a name="bkmk_backup"></a> Sauvegarde des fichiers et des données

 Avant d'installer une nouvelle instance de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], assurez-vous de sauvegarder tous les fichiers de votre installation actuelle.  
  
1.  Sauvegardez la clé de chiffrement de la base de données du serveur de rapports. Cette étape est cruciale pour le succès de la migration. En effet, à un stade plus avancé du processus de migration, vous devrez la restaurer pour rendre au serveur de rapports l'accès aux données chiffrées. Pour sauvegarder la clé, utilisez le Gestionnaire de configuration de Reporting Services.  
  
2.  Sauvegardez la base de données du serveur de rapports en utilisant l'une des méthodes prises en charge pour la sauvegarde d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez les instructions relatives à la sauvegarde de la base de données du serveur de rapports dans [Déplacement des bases de données du serveur de rapports vers un autre ordinateur &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md).  
  
3.  Sauvegardez les fichiers de configuration du serveur de rapports. Les fichiers à sauvegarder sont les suivants :  
  
    1.  RSReportServer.config  
  
    2.  Rswebapplication.config  
  
    3.  Rssvrpolicy.config  
  
    4.  Rsmgrpolicy.config  
  
    5.  Reportingservicesservice.exe.config  
  
    6.  Web.config pour l’application [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] de serveur de rapports.  
  
    7.  Machine.config pour [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] si vous l'avez modifié pour les opérations du serveur de rapports.  

## <a name="bkmk_install_ssrs"></a> Installation de SQL Server Reporting Services

 Installez une nouvelle instance du serveur de rapports en mode fichiers uniquement ; vous pourrez ainsi la configurer pour une utilisation de valeurs autres que celles définies par défaut. Pour une installation via la ligne de commande, utilisez l’argument **FilesOnly** . Dans l’Assistant Installation, sélectionnez l’option **Installer, mais ne pas configurer le serveur de rapports**.  
  
 Cliquez sur l'un des liens suivants pour obtenir des instructions sur l'installation d'une nouvelle instance de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]:  
  
-   [Installer SQL Server 2016 avec l’Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)  
  
-   [Installer SQL Server 2016 à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md)  

## <a name="bkmk_move_database"></a> Déplacement de la base de données du serveur de rapports

 La base de données du serveur de rapports contient des rapports publiés, des modèles, des sources de données partagées, des planifications, des ressources, des abonnements et des dossiers. Elle contient également des propriétés système et d'élément, ainsi que les autorisations d'accès au contenu du serveur de rapports.  
  
 Si votre migration comprend l'utilisation d'une autre instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] , vous devez déplacer la base de données du serveur de rapports vers la nouvelle instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Si vous utilisez la même instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] , passez à la section [Déplacement des assemblys ou extensions personnalisés](#bkmk_move_custom).  
  
 Pour déplacer la base de données du serveur de rapports, procédez comme suit :  
  
1.  Choisissez l'instance [!INCLUDE[ssDE](../../includes/ssde-md.md)] à utiliser. SQL Server Reporting Services exige que vous utilisiez l’une des versions suivantes pour héberger la base de données du serveur de rapports :  
  
    -   SQL Server 2016  
  
    -   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
    -   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
    -   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
    -   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
2.  Démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
3.  Créez le rôle **RSExecRole** dans les bases de données système si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n’a jamais hébergé une base de données du serveur de rapports. Pour plus d’informations, consultez [Créer le rôle RSExecRole](../../reporting-services/security/create-the-rsexecrole.md).  
  
4.  Suivez les instructions fournies dans [Déplacement des bases de données du serveur de rapports vers un autre ordinateur &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md).  
  
 N'oubliez pas que la base de données du serveur de rapports et la base de données temporaire dépendent l'une de l'autre et doivent être déplacées simultanément. Ne copiez pas les bases de données ; la copie ne transfère pas tous les paramètres de sécurité vers la nouvelle installation. Ne déplacez pas de travaux de l'Agent SQL Server pour les opérations planifiées du serveur de rapports. Le serveur de rapports recrée automatiquement ces travaux.  

## <a name="bkmk_move_custom"></a> Déplacement des assemblys ou extensions personnalisés

 Si votre installation comprend des éléments de rapport, des assemblys ou des extensions personnalisés, vous devez redéployer les composants personnalisés. Si vous n’utilisez pas des composants personnalisés, passez à la section [Configuration du serveur de rapports](#bkmk_configure_reportserver).  
  
 Pour redéployer les composants personnalisés, procédez comme suit :  
  
1.  Déterminez si les assemblys sont pris en charge ou doivent être recompilés :

    -  Les extensions de sécurité personnalisées doivent être réécrites à l’aide de l’interface [IAuthenticationExtension2](https://msdn.microsoft.com/library/microsoft.reportingservices.interfaces.iauthenticationextension2.aspx) .
  
    -   Les extensions de rendu personnalisées pour [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] doivent être réécrites à l’aide du modèle objet de rendu.  
  
    -   Les convertisseurs HTML 3.2 et HTML OWC ne sont pas pris en charge dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et versions ultérieures.  
  
    -   Les autres assemblys personnalisés ne devraient pas nécessiter de recompilation.  
  
2.  Déplacez les assemblys dans le nouveau dossiers \bin du serveur de rapports. Dans SQL Server, les binaires de serveur de rapports se trouvent à l’emplacement suivant pour l’instance de serveur de rapports par défaut :  
  
     `\Program files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer\bin`  
  
3.  Modifiez les fichiers de configuration de façon à y ajouter des entrées pour votre composant personnalisé. Les entrées à ajouter dépendent du type d'assembly que vous utilisez. Pour obtenir des instructions sur l'endroit où placer des fichiers et ajouter des entrées de configuration, consultez les rubriques suivantes :  
  
    1.  [Déploiement d'un assembly personnalisé](../../reporting-services/custom-assemblies/deploying-a-custom-assembly.md)  
  
    2.  [Procédure : déployer un élément de rapport personnalisé](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
  
    3.  [Déploiement d'une extension pour le traitement des données](../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)  
  
    4.  [Déploiement d'une extension de remise](../../reporting-services/extensions/delivery-extension/deploying-a-delivery-extension.md)  
  
    5.  [Déploiement d'une extension de rendu](../../reporting-services/extensions/rendering-extension/deploying-a-rendering-extension.md)  
  
    6.  [Implémentation d'une extension de sécurité](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)  

## <a name="bkmk_configure_reportserver"></a> Configuration du serveur de rapports

 Configurez les URL du service Web Report Server et du portail web, ainsi que la connexion à la base de données du serveur de rapports.  
  
 Si vous migrez un déploiement avec montée en puissance parallèle, mettez tous les nœuds du serveur de rapports hors connexion et migrez les serveurs un par un. Une fois que le premier serveur de rapports est migré et qu’il se connecte avec succès à la base de données du serveur de rapports, la version de la base de données du serveur de rapports est automatiquement mise à niveau vers la version de la base de données SQL Server.  
  
> [!IMPORTANT]  
>  Si l'un des serveurs de rapports dans le déploiement avec montée en puissance parallèle est en ligne et n'a pas été migré, il peut rencontrer une exception rsInvalidReportServerDatabase parce qu'il utilise un schéma plus ancien lorsqu'il est connecté aux éléments mis à niveau.  
  
> [!NOTE]  
>  Si le serveur de rapports que vous avez migré a été configuré comme base de données partagée pour un déploiement avec montée en puissance parallèle, vous devez supprimer toutes les anciennes clés de chiffrement de la table **Keys** dans la base de données **ReportServer** avant de configurer le service de serveur de rapports. Si les clés ne sont pas supprimées, le serveur de rapports migré essaiera de s'initialiser en mode de déploiement avec montée en puissance parallèle. Pour plus d’informations, consultez [Ajouter et supprimer des clés de chiffrement pour un déploiement évolutif &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md) et [Configurer et gérer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md).  
>   
>  Les clés de montée en puissance parallèle ne peuvent pas être supprimées à l'aide du Gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Les anciennes clés doivent être supprimées de la table **Keys** dans la base de données **ReportServer** à l’aide de SQL Server Management Studio. Supprimez toutes les lignes dans la table Keys. Cela effacera la table et la préparera en vue de la restauration de la clé symétrique uniquement, comme documenté dans les étapes suivantes.  
>   
>  Avant de supprimer les clés, il est recommandé de d'abord sauvegarder la clé de chiffrement symétrique. Vous pouvez utiliser le Gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour sauvegarder la clé. Ouvrez le Gestionnaire de configuration, cliquez sur l’onglet **Clés de chiffrement** , puis sur le bouton **Sauvegarder** . Vous pouvez également écrire un script de commandes WMI afin de sauvegarder la clé de chiffrement. Pour plus d’informations sur WMI, consultez [Méthode BackupEncryptionKey &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-backupencryptionkey.md).  
  
1.  Démarrez le Gestionnaire de configuration de Reporting Services et connectez-vous à l'instance de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que vous venez d'installer. Pour plus d’informations, consultez [Gestionnaire de configuration de Reporting Services &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2.  Configurez les URL du serveur de rapports et du portail web. Pour plus d’informations, consultez [Configurer une URL &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md).  
  
3.  Configurez la base de données du serveur de rapports, en sélectionnant la base de données du serveur de rapports de votre installation précédente. Une fois la configuration réussie, les services de serveur de rapports redémarreront et, une fois la connexion établie avec la base de données du serveur de rapports, la base de données sera mise à niveau automatiquement vers Server Reporting Services. Pour plus d’informations sur l’exécution de l’Assistant Modification de base de données que vous utilisez pour créer ou sélectionner une base de données du serveur de rapports, consultez [Créer une base de données du serveur de rapports en mode natif](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  
  
4.  Restaurez les clés de chiffrement. Cette étape est indispensable pour activer le chiffrement réversible sur les chaînes de connexion préexistantes et les informations d'identification déjà présentes dans la base de données du serveur de rapports. Pour plus d’informations, consultez [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
5.  Si vous avez installé le serveur de rapports sur un nouvel ordinateur et que vous utilisez le Pare-feu Windows, assurez-vous que le port TCP sur lequel le serveur de rapports est à l'écoute est ouvert. Par défaut, il s'agit du port 80. Pour plus d’informations, consultez [Configurer un pare-feu pour accéder au serveur de rapports](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
6.  Si vous souhaitez administrer votre serveur de rapports en mode natif localement, vous devez configurer le système d’exploitation pour permettre l’administration locale à l’aide du portail web. Pour plus d’informations, consultez [Configurer un serveur de rapports en mode natif pour l’administration locale](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  

## <a name="bkmk_copy_custom_config"></a> Copie des paramètres de configuration personnalisés dans le fichier RSReportServer.config

Si vous avez modifié le fichier RSReportServer.config ou RSWebApplication.config dans l'installation précédente, vous devez apporter les mêmes modifications au nouveau fichier RSReportServer.config. La liste suivante résume certaines des raisons pour lesquelles vous avez pu modifier le fichier de configuration précédent et fournit des liens vers des informations supplémentaires sur la manière de configurer les mêmes paramètres dans SQL Server 2016.  
  
|Personnalisation|Informations|  
|-------------------|-----------------|  
|Remise du courrier électronique du serveur de rapports avec des paramètres personnalisés|[Paramètres d’e-mail - Mode natif de Reporting Services](../../reporting-services/install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md).|  
|Paramètres d'informations de périphérique|[Personnaliser les paramètres d'extension de rendu dans RSReportServer.Config](../../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)|

## <a name="bkmk_windowsservice_group"></a> Groupe de service Windows et ACL de sécurité

 Dans [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)], il existe un groupe de service, le groupe de service Windows [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], utilisé pour créer des listes de contrôle d’accès (ACL) de sécurité pour toutes les clés de Registre, les fichiers et les dossiers installés avec SQL Server Reporting Services. Ce nom de groupe Windows apparaît au format SQLServerReportServerUser$\<*nom_ordinateur*>$\<*nom_instance*>.  

## <a name="bkmk_verify"></a> Vérification de votre déploiement

1.  Testez les répertoires virtuels du serveur de rapports et de [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] en ouvrant un navigateur et en tapant une adresse URL dans le champ approprié. Pour plus d’informations, consultez [Vérifier une installation de Reporting Services](../../reporting-services/install-windows/verify-a-reporting-services-installation.md).  
  
2.  Testez les rapports et assurez-vous qu'ils contiennent les données attendues. Passez en revue les informations de la source de données pour vérifier si ses informations de connexion sont toujours spécifiées. Le serveur de rapports utilise le modèle objet des rapports pour le traitement et le rendu des rapports, mais il ne remplace pas les constructions [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ni [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] par de nouveaux éléments RDL (Report Definition Language). Pour en savoir plus sur l’exécution de rapports existants sur une nouvelle version de votre serveur de rapports, consultez [Rapports de mise à niveau](../../reporting-services/install-windows/upgrade-reports.md).  

## <a name="bkmk_remove_unused"></a> Suppression des programmes et fichiers inutiles

Une fois que vous avez effectué avec succès une migration de votre serveur de rapports vers une nouvelle instance, vous pouvez éventuellement effectuer les étapes suivantes pour supprimer des programmes et des fichiers qui ne sont plus nécessaires.  
  
1.  Désinstallez la version précédente de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] si elle n'est plus nécessaire. Cette étape ne supprime pas les éléments suivants, mais vous pouvez les supprimer manuellement si vous n'en avez plus besoin :  
  
    -   Ancienne base de données du serveur de rapports  
  
    -   Rôle RsExec  
  
    -   Comptes de service du serveur de rapports  
  
    -   Pool d'applications du service Web Report Server  
  
    -   Répertoires virtuels pour le serveur de rapports et le Gestionnaire de rapports  
  
    -   Fichiers journaux du serveur de rapports  
  
2.  Supprimez IIS si vous n'en avez plus besoin sur cet ordinateur.

## <a name="next-steps"></a>Étapes suivantes

[Migrer une installation Reporting Services](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md)   
[Base de données du serveur de rapports](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
[Mettre à niveau et migrer Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[Compatibilité descendante de Reporting Services](../../reporting-services/reporting-services-backward-compatibility.md)   
[Gestionnaire de configuration de Reporting Services](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
