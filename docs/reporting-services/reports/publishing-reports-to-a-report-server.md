---
title: Publication de rapports sur un serveur de rapports | Microsoft Docs
ms.custom: ''
ms.date: 06/01/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reports
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- production environments [Reporting Services]
- report projects [Reporting Services]
- Debug configuration [Reporting Services]
- report publishing [Reporting Services]
- publishing reports [Reporting Services]
- report properties [Reporting Services]
- Report Designer [Reporting Services], deploying reports
- Production configuration [Reporting Services]
- publishing reports [Reporting Services], production environments
- DebugLocal configuration [Reporting Services]
- deploying [Reporting Services], reports
- Report Designer [Reporting Services], publishing reports
ms.assetid: bd7aa5e0-61ce-43fd-8f74-5d1aeed078bb
caps.latest.revision: 47
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 9a69b67fcbc9d047526ff0c30883731543a826a7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="publishing-reports-to-a-report-server"></a>Publication de rapports sur un serveur de rapports
  Après avoir conçu et testé un rapport ou ensemble de rapports, vous pouvez utiliser les fonctionnalités de déploiement de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour publier les rapports sur un serveur de rapports. Vous pouvez publier des rapports individuels ou un projet Report Server qui peut inclure plusieurs rapports et sources de données. La publication d'un projet Report Server est la méthode la plus simple pour publier plusieurs rapports. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] utilise le terme *déployer*à la place du terme *publier*. Les deux termes sont interchangeables.  
  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] fournit des configurations de projet pour la gestion de la publication de rapports. La configuration spécifie l'emplacement du serveur de rapports, la version de SQL Server Reporting Services installée sur le serveur de rapports, si les sources de données publiées sur le serveur de rapports sont remplacées, etc. Par exemple, la configuration Debug peut publier sur un autre serveur que la configuration Release. Outre les configurations fournies par [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] , vous pouvez créer des configurations supplémentaires.  
 
## <a name="requirements-to-publish"></a>Exigences en matière de publication
L'autorisation est déterminée via la sécurité basée sur les rôles définie par votre administrateur du serveur de rapports. Les opérations de publication sont généralement autorisées par le biais du **rôle Serveur de publication**.  
  
## <a name="project-configurations"></a>Configurations de projet  
 Plusieurs serveurs de rapports et versions différentes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peuvent être installés dans votre environnement de création de rapports. Vous pouvez créer plusieurs configurations, puis en utiliser une différente selon le scénario de déploiement. Les configurations de projet incluent des propriétés pour la génération de rapports, telles que le dossier dans lequel les rapports créés doivent être stockés temporairement, et le mode de gestion des problèmes de génération. Les configurations ont également des propriétés que vous utilisez pour spécifier l'emplacement et la version du serveur de rapports, les dossiers sur le serveur de rapports.  
  
 Par défaut, [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] fournit trois configurations de projet : **DebugLocal**, **Debug**et **Release**. La configuration par défaut est DebugLocal. Vous utilisez en général la configuration DebugLocal pour afficher un aperçu des rapports dans une fenêtre d’aperçu locale, la configuration Debug pour publier des rapports sur un serveur de test et la configuration Release pour publier des rapports sur un serveur de production. La liste déroulante des configurations de solution dans la barre d'outils Standard affiche la configuration active. Pour utiliser une configuration différente, sélectionnez-la dans la liste.  
  
 ![ssrs_project_properties](../../reporting-services/reports/media/ssrs-project-properties.png) 
  
 Pour plus d’informations, consultez les rubriques suivantes :
 + [Pages de propriétés du projet, boîte de dialogue](../../reporting-services/tools/project-property-pages-dialog-box.md)
 + [Deployment and Version Support in SQL Server Data Tools](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)
 + [Définir les propriétés de déploiement pour les projets Reporting Services dans SSDT](../../reporting-services/tools/set-deployment-properties-reporting-services.md)
  
## <a name="to-publish-all-reports-in-a-project"></a>Pour publier tous les rapports d'un projet  
  
Dans le menu **Générer**, cliquez sur **Déployer \<nom_projet_rapport>**. Vous pouvez également, dans l’Explorateur de solutions, cliquer avec le bouton droit sur le projet de rapport, puis cliquer sur **Déployer**. Vous pouvez consulter l'état du processus de publication dans la fenêtre Sortie.  
  
Lorsque vous déployez un projet Report Server, les sources de données partagées dans le projet de rapport sont également déployées. Tous les rapports sont déployés à l'aide de la même configuration de projet : sur le même serveur de rapports, le même dossier sur le serveur, et ainsi de suite. Pour publier des rapports sur différents serveurs, publiez-les un par un ou incluez uniquement les rapports que vous souhaitez dans le projet Report Server. Une solution peut inclure plusieurs projets Report Server et l'utilisation de plusieurs projets peut simplifier la gestion du déploiement des rapports, car vous pouvez utiliser une configuration différente pour déployer des projets différents. 
  
## <a name="to-publish-a-single-report"></a>Pour publier un seul rapport  
  
Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le rapport, puis cliquez sur **Déployer**. Vous pouvez consulter l'état du processus de publication dans la fenêtre Sortie.  
  
 Lorsque vous publiez un rapport, vous devez également déployer les sources de données partagées que le rapport utilise.   
 Si vous ne souhaitez pas publier tous les rapports d'un projet, vous pouvez choisir de n'en publier qu'un seul. Pour cela, sélectionnez une configuration qui déploie le rapport (par exemple, Release), cliquez avec le bouton droit sur ce rapport, puis cliquez sur **Déployer**.  
  
 Si un rapport utilise une source de données partagée, vous devez également la déployer ou le rapport déployé ne s'exécutera pas. Cliquez avec le bouton droit sur la source de données partagée, puis cliquez sur **Déployer**.  
  
 L'URL de serveur cible du serveur de rapports doit être spécifiée et vous pouvez modifier les dossiers par défaut vers lesquels les rapports et sources de données partagées sont déployés.  

  
## <a name="see-also"></a> Voir aussi  
 [Pages de propriétés du projet, boîte de dialogue](../../reporting-services/tools/project-property-pages-dialog-box.md)   
 [Gestion du contenu du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [Mettre à niveau des rapports](../../reporting-services/install-windows/upgrade-reports.md)  
  
  
