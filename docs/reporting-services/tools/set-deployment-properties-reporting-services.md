---
title: Définir des propriétés de déploiement (Reporting Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- reports [Reporting Services], deploying
- publishing reports [Reporting Services]
- properties [Reporting Services], deployment
- deploying reports [Reporting Services]
ms.assetid: 18201ca0-bf4a-484f-b3a2-95d1046a6a9b
caps.latest.revision: 44
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 3646d424b9f2f66546369c74a4bb310d0fb6a4cd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-deployment-properties-reporting-services"></a>Définir des propriétés de déploiement (Reporting Services)
  Dans[!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], vous devez spécifier le serveur de rapports et éventuellement les dossiers pour les rapports et les sources de données partagées afin de pouvoir publier les éléments dans un projet Report Server sur un serveur de rapports. Les propriétés et valeurs dont [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] a besoin pour générer, visualiser et déployer des rapports sont stockées dans les configurations de projet du projet Report Server. Vous pouvez créer plusieurs jeux nommés pour ces propriétés de projet afin de pouvoir aisément basculer entre les jeux de propriétés. Chaque jeu de propriétés est une configuration. Par exemple, vous pouvez avoir une configuration pour publier des rapports sur un serveur de test et une configuration différente pour publier des rapports sur un serveur de production.  
  
 Utilisez le Gestionnaire de configuration pour créer et gérer des jeux de propriétés de projet dans les configurations de projet. Le Gestionnaire de configuration est une fonctionnalité prise en charge par [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], sur lequel repose [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] .  
  
> [!NOTE]  
>  Ne confondez pas cette fonctionnalité avec le Gestionnaire de configuration de Reporting Services, utilisé pour configurer Reporting Services après l'installation. Pour plus d’informations, consultez [Configurer et administrer un serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md).  
  
> [!NOTE]  
>  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], l’action de publier des rapports d’une solution ou d’un projet Report Server est connue sous le nom de *déploiement de rapports*.  
  
### <a name="to-set-deployment-properties"></a>Pour définir des propriétés de déploiement  
  
1.  Cliquez avec le bouton droit sur le projet de rapport, puis cliquez sur **Propriétés**.  
  
2.  Dans la boîte de dialogue **Pages de propriétés** du projet, sélectionnez une configuration à modifier dans la liste **Configuration** . Les configurations habituelles sont **DebugLocal**, **Debug**et **Release**.  
  
    > [!NOTE]  
    >  Vous pouvez utiliser plusieurs configurations pour passer rapidement d'un serveur de rapports ou d'un paramètre à l'autre.  
  
3.  Dans la zone de texte **OutputPath**  , tapez ou collez le chemin dans votre système de fichiers local pour stocker la définition de rapport utilisée dans la vérification de build, le déploiement et l’aperçu de rapports. Le chemin d'accès doit être différent de celui que vous utilisez pour le projet et un chemin d'accès relatif qui est un dossier enfant sous le chemin d'accès du projet.  
  
4.  Dans la zone de texte **ErrorLevel**  , tapez la gravité des problèmes de génération signalés comme erreurs. Les problèmes qui se produisent lors de la génération de rapports, de sources de données ou d’autres ressources du projet avec des niveaux de gravité inférieurs ou égaux à la valeur de **ErrorLevel**  sont signalés comme erreurs ; sinon, les problèmes sont signalés comme avertissements. Toute erreur provoquera l'échec de la tâche de génération. Les niveaux de gravité valides sont compris entre 0 et 4. La valeur par défaut est 2.  
  
     **ErrorLevel** peut être utilisé pour augmenter ou diminuer la sensibilité de la génération. Par exemple, lorsqu'un rapport avec une carte est généré pendant le déploiement vers un serveur de rapports [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , une erreur s'affiche par défaut et la génération du rapport échoue. Si vous diminuez la valeur de **ErrorLevel** , la carte est supprimée du rapport, un avertissement s'affiche et la génération du rapport continue.  
  
5.  Dans la liste **StartItem**  , sélectionnez un rapport à afficher dans la fenêtre d’aperçu ou dans une fenêtre de navigateur lors de l’exécution du projet de rapport.  
  
6.  Dans la liste **OverwriteDataSources** , sélectionnez **True** pour remplacer la source de données partagée sur le serveur chaque fois que des sources de données partagées sont publiées ou sélectionnez **False** pour conserver la source de données sur le serveur.  
  
7.  Dans la liste **TargetServerVersion** , sélectionnez la version SQL Server 2016 de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou sélectionnez **Détecter la version** pour déterminer automatiquement la version installée sur le serveur identifié par la propriété **TargetServerURL** . La valeur par défaut est **SQL Server 2016 ou version ultérieure**.  
  
     Utilisez **TargetServerVersion** pour personnaliser les rapports créés, placés dans le chemin d'accès spécifié dans OutputPath, pour la version du serveur de rapports spécifié dans **TargetServerURL**.  
  
8.  Dans la zone de texte **TargetDataSourceFolder** , tapez le nom du dossier situé sur le serveur de rapports dans lequel placer les sources de données partagées publiées. La valeur par défaut pour **TargetDataSourceFolder** est Data Sources. Si vous laissez cette valeur vide, les sources de données seront publiées à l'emplacement spécifié dans **TargetReportFolder**.  
  
9. Dans la zone de texte **TargetReportFolder** , tapez le nom du dossier situé sur le serveur de rapports dans lequel placer les rapports publiés. La valeur par défaut pour **TargetReportFolder**  est le nom du projet de rapport.  
  
    > [!NOTE]  
    >  Pour un serveur de rapports qui s’exécute en mode natif, vous devez disposer des autorisations de **publication** sur le dossier cible pour publier les rapports dans ce dossier. Ces autorisations sont fournies par l'intermédiaire d'une attribution de rôle qui associe votre compte d'utilisateur à un rôle qui inclut des opérations de publication. Pour plus d’informations, consultez [Créer et gérer des attributions de rôles](../../reporting-services/security/create-and-manage-role-assignments.md). Pour un serveur de rapports qui s'exécute en mode intégré SharePoint, vous devez disposer de l'autorisation **Membre** ou **Propriétaire** sur le site SharePoint. Pour plus d’informations, consultez [Article de référence sur les autorisations de site SharePoint et de listes pour les éléments de serveur de rapports](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md).  
  
10. Dans la zone de texte **TargetServerURL** , tapez l'URL du serveur de rapports cible. Avant de publier un rapport, vous devez affecter à cette propriété une URL de serveur de rapports valide. Quand vous publiez sur un serveur de rapports qui s’exécute en mode natif, utilisez l’URL du répertoire virtuel du serveur de rapports (par exemple, http:*//serveur/serveur_rapports* ou https:*//serveur/serveur_rapports)*. Il s'agit du répertoire virtuel du serveur de rapports et non du Gestionnaire de rapports.  
  
     Lors de la publication sur un serveur de rapports s'exécutant en mode intégré SharePoint, utilisez l'URL d'un site de premier niveau ou d'un sous-site SharePoint. Si vous ne spécifiez pas de site, le site de niveau supérieur par défaut est utilisé (par exemple, http://*nom_serveur*, http://*nom_serveur*/*site* ou http://*nom_serveur*/*site*/*sous-site*).  
  
### <a name="to-set-configuration-manager-properties"></a>Pour définir les propriétés du Gestionnaire de configuration  
  
1.  Cliquez avec le bouton droit sur le projet de rapport, puis cliquez sur **Propriétés**.  
  
2.  Dans la boîte de dialogue **Pages de propriétés** du projet, cliquez sur **Gestionnaire de configuration**.  
  
3.  Dans la boîte de dialogue **Gestionnaire de configuration** , sélectionnez la configuration à modifier. La configuration actuellement active apparaît de la façon suivante : **Active(***\<configuration>***)**.  
  
4.  Dans **Contextes des projets**, pour chaque projet de la solution, sélectionnez ou désélectionnez **Générer** ou **Déployer**.  
  
    > [!NOTE]  
    >  Si l'option **Générer** est sélectionnée, le Gestionnaire de rapports génère le projet de rapport et recherche les éventuelles erreurs avant l'aperçu ou la publication sur un serveur de rapports. Si l'option **Déployer** est sélectionnée, le Gestionnaire de rapports publie les rapports sur le serveur de rapports comme défini dans les propriétés de déploiement. Si l'option **Déployer** n'est pas sélectionnée, le Gestionnaire de rapports affiche le rapport spécifié dans la propriété **StartItem** dans une fenêtre d'aperçu locale.  
  
## <a name="see-also"></a> Voir aussi  
 [Publication des sources de données et des rapports](../../reporting-services/reports/publishing-data-sources-and-reports.md)   
 [Aperçu des rapports](../../reporting-services/reports/previewing-reports.md)   
 [Aide sur le Concepteur de rapports accessible à l’aide de la touche F1](../../reporting-services/tools/report-designer-f1-help.md)   
 [Exemples d’URL pour les éléments de rapport publiés sur un serveur de rapports en mode SharePoint &#40;SSRS&#41;](../../reporting-services/tools/url-examples-for-items-on-a-report-server-sharepoint-mode.md)   
 [Pages de propriétés du projet, boîte de dialogue](../../reporting-services/tools/project-property-pages-dialog-box.md)   
 [Publication de rapports sur un serveur de rapports](../../reporting-services/reports/publishing-reports-to-a-report-server.md)  
  
  
