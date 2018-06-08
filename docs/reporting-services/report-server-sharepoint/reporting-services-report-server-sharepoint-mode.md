---
title: Serveur de rapports Reporting Services (mode SharePoint) | Microsoft Docs
ms.custom: ''
ms.date: 09/26/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server-sharepoint
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 8bb9d1a2949a838c579322a8e0b6fcd523c9f2eb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reporting-services-report-server-sharepoint-mode"></a>Serveur de rapports Reporting Services (mode SharePoint)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  Un serveur de rapports Reporting Services configuré pour le **mode SharePoint** peut s’exécuter au sein d’un déploiement d’un produit SharePoint. Un serveur de rapports en mode SharePoint peut utiliser les fonctionnalités de collaboration et de gestion des rapports SharePoint et d'autres types de contenus [!INCLUDE[ssRSnfoversion_md](../../includes/ssrsnoversion-md.md)] . Le mode SharePoint requiert l’installation de la version appropriée du complément Reporting Services pour les produits SharePoint sur vos serveurs web frontaux SharePoint.  
  
> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

 Pour plus d'informations sur l'installation et la configuration, consultez les rubriques suivantes.  
  
-   [Installer le mode SharePoint de Reporting Services pour SharePoint 2010](http://msdn.microsoft.com/47efa72e-1735-4387-8485-f8994fb08c8c).  
  
-   [Ajouter un serveur de rapports supplémentaire à une batterie](../../reporting-services/install-windows/add-an-additional-report-server-to-a-farm-ssrs-scale-out.md).  
  
## <a name="feature-summary"></a>Synthèse des fonctionnalités

 Configurer un serveur de rapports pour qu'il s'exécute en mode intégré SharePoint fournit les fonctionnalités supplémentaires ci-dessous, qui sont disponibles uniquement lorsque vous déployez un serveur de rapports dans ce mode :  
  
-   Utilisez les fonctionnalités de SharePoint de gestion de documents et de collaboration, y compris les alertes. Un site SharePoint fournit un portail unifié permettant d'accéder à tous les éléments de rapport et de les gérer dans un seul emplacement.  
  
-   Utilisez les autorisations SharePoint et les fournisseurs d'authentification pour contrôler l'accès aux rapports, aux modèles et aux autres éléments.  
  
-   Utilisez les topologies de déploiement SharePoint pour distribuer des rapports via une connexion Internet en dehors du pare-feu. Un serveur de rapports fournit des services de traitement de rapports et de données dans le cadre d'un déploiement SharePoint plus vaste, configuré pour l'accès à Internet.  
  
-   Gérez des rapports, des modèles, des sources de données, des planifications et un historique de rapport dans des pages d'application personnalisées sur un site SharePoint. Vous pouvez définir des propriétés, des planifications et des abonnements, ainsi que créer et gérer un historique de rapport sur un site SharePoint de la même manière que vous les créez et gérez à partir d'autres outils dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Publiez ou téléchargez des rapports, des modèles de rapport, des ressources et des fichiers de sources de données partagées dans une bibliothèque SharePoint, y compris Report Center dans Office SharePoint Server.  
  
     Utilisez le Concepteur de rapports, le Générateur de modèles et le Générateur de rapports pour créer des rapports et des sources de données à publier directement dans une bibliothèque SharePoint. Vous pouvez également utiliser l'action Télécharger sur un site SharePoint pour ajouter des définitions de rapport et des modèles de rapport quelconques dans une bibliothèque SharePoint.  
  
     Comme le serveur de rapports traite les définitions de rapport de la même manière, quel que soit le mode serveur utilisé, les données et la mise en page des rapports ne sont pas affectées par le mode serveur. Tout rapport que vous pouvez exécuter sur un serveur de rapports en mode natif peut être exécuté sur un serveur de rapports configuré pour le mode intégré SharePoint.  
  
-   Abonnez-vous à des rapports et livrez-les dans une bibliothèque SharePoint à l'aide d'une nouvelle extension de remise SharePoint. Vous pouvez également livrer des rapports par courrier électronique ou dans un dossier partagé. Les extensions de remise du serveur de rapports sont utilisées pour livrer les rapports. Vous pouvez créer des abonnements pilotés par les données pour une distribution de rapport à grande échelle à l'aide de données d'abonné interrogées au moment de l'exécution.  
  
-   Vous pouvez ajouter aux pages SharePoint un composant WebPart Visionneuse de rapports pour afficher un rapport à l’intérieur de votre application web SharePoint. Ce composant WebPart inclut des fonctionnalités de navigation entre les pages, de recherche, d’impression et d’exportation.  
  
-   Programmez un nouveau point de terminaison SOAP pour créer des applications personnalisées qui s'intègrent à un site SharePoint. Vous pouvez également utiliser le fournisseur WMI (Windows Management Instrumentation) mis à jour pour configurer par programmation une instance de serveur de rapports qui s'exécute en mode intégré SharePoint.  
  
-   Rapports de services Microsoft Access en mode connecté.  
  
-   Zones AAM (mappages d'accès de substitution), déploiements exposés à Internet et jetons utilisateur SharePoint pour les listes SharePoint.  
  
## <a name="connected-mode-and-local-mode"></a>Mode connecté et mode local

 La version SQL Server 2008 R2 a introduit un nouveau *mode local* pour consulter des rapports d’un serveur SharePoint 2010 qui inclut le complément Microsoft SQL Server 2008 R2 Reporting Services (ou version ultérieure) pour les produits SharePoint 2010 installés.  
  
-   *Mode local* : ce mode permet un rendu local des rapports à partir de la bibliothèque de documents SharePoint sans intégration avec un serveur de rapports Reporting Services. Le complément Reporting Services pour les produits SharePoint est requis, mais un serveur de rapports Reporting Services ne l’est pas. Le complément peut être installé de différentes façons, notamment avec l'Outil de préparation des produits SharePoint 2010. Pour plus d’informations sur le mode local, consultez [Rapports en mode local et rapports en mode connecté dans la Visionneuse de rapports](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md) et [Où trouver le complément Reporting Services pour les produits SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md).  
  
-   *Mode connecté* : ce mode est pris en charge en intégrant un serveur de rapports Reporting Services dans la batterie de serveurs SharePoint à l’aide de l’Administration centrale de SharePoint. L'intégration à un serveur de rapports permet la création de rapports complète de bout en bout, l'utilisation des fonctionnalités de collaboration de SharePoint 2010 et des fonctionnalités de serveur d'un serveur de rapports notamment : abonnements, instantanés et traitement par le serveur.  
  
## <a name="unsupported-sharepoint-features"></a>Fonctionnalités SharePoint non prises en charge

 Certaines fonctionnalités SharePoint ne sont pas disponibles en mode intégré. La liste suivante présente les fonctionnalités SharePoint avec lesquelles Reporting Services ne s’intègre pas directement :  
  
-   Service Banque d'informations sécurisé.  
  
-   Vous ne pouvez pas utiliser l'intégration du calendrier Outlook dans SharePoint ni la planification SharePoint pour les fichiers de Reporting Services dans une bibliothèque de documents.  
  
-   Catalogue de données métier SharePoint.  
  
-   La personnalisation SharePoint n’est pas prise en charge sur les pages de Reporting Services. L'intégration Report Server n'est pas prise en charge si l'application Web SharePoint est activée pour l'accès anonyme.  
  
-   SQL Server Reporting Services ne prend **pas** en charge le contrôle de version de bibliothèque de documents SharePoint. Si vous enregistrez des éléments de rapport dans une bibliothèque de documents pour laquelle l'option Historique des versions du document est activée, les fonctionnalités de Reporting Services ne fonctionnent pas correctement et génèrent des erreurs dans le journal ULS. L'exemple suivant montre une erreur dans le journal ULS :  
  
    -   « …une source de données associée au rapport a été désactivée ».  
  
     L'historique des versions de la bibliothèque de documents est configuré dans la page Paramètres de contrôle de version de Paramètres de la bibliothèque.  
  
## <a name="supported-combinations-of-the-sharepoint-add-in-and-report-server"></a>Combinaisons du complément SharePoint et du serveur de rapports prises en charge

 Certaines fonctionnalités ne sont pas prises en charge dans toutes les combinaisons de serveur de rapports, complément Reporting Services pour SharePoint et produits SharePoint. Pour plus d’informations, consultez [Combinaisons de serveurs et compléments SharePoint et Reporting Services prises en charge](../../reporting-services/install-windows/supported-combinations-of-sharepoint-and-reporting-services-server.md).  
  
> [!NOTE]  
>  La version correcte du complément Reporting Services doit être utilisée avec la version correspondante des produits SharePoint.  
  
## <a name="components-that-provide-integration"></a>Composants permettant l’intégration

 Pour combiner les serveurs dans un déploiement unique, vous intégrez une installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Reporting Services avec une instance de produits SharePoint.  
  
 L’intégration est fournie par le biais de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et du complément Reporting Services pour les produits SharePoint. Le complément Reporting Services est un composant en distribution libre que vous pouvez télécharger, puis installer sur un serveur qui exécute la version appropriée de SharePoint.  
  
> [!TIP]  
>  Certaines fonctionnalités ne sont pas prises en charge dans toutes les combinaisons de serveur de rapports, complément Reporting Services pour SharePoint et produits SharePoint. Pour plus d’informations, consultez [Combinaisons de serveurs et compléments SharePoint et Reporting Services prises en charge](../../reporting-services/install-windows/supported-combinations-of-sharepoint-and-reporting-services-server.md).  
  
-   Dans SharePoint, le complément Reporting Services fournit le point de terminaison proxy ReportServer, un composant WebPart Visionneuse de rapports et des pages d’application afin que vous puissiez afficher, stocker et gérer le contenu du serveur de rapports sur un site ou une batterie de serveurs SharePoint.  
  
-   Sur Reporting Services, le complément Reporting Services fournit des fichiers programme mis à jour, un point de terminaison SOAP, ainsi que des extensions de sécurité et de remise personnalisées. Le serveur de rapports doit être configuré pour s'exécuter en mode intégré SharePoint, dédié exclusivement à la prise en charge de la remise des rapports et de l'accès aux rapports par le biais de votre site SharePoint.  
  
 Une fois que vous avez installé le complément le complément Reporting Services dans SharePoint et configuré les deux serveurs pour l’intégration, vous pouvez télécharger ou publier les types de contenu du serveur de rapports vers une bibliothèque SharePoint, puis afficher et gérer ces documents à partir d’un site SharePoint. Le téléchargement ou la publication du contenu du serveur de rapports constitue une première étape importante. Le composant WebPart et les pages deviennent accessibles lorsque vous sélectionnez des définitions de rapport (.rdl), des modèles de rapport (.smdl) et des sources de données partagées (.rsds) sur un site SharePoint.  
  
##  <a name="language-considerations"></a>Observations relatives au langage

 [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] et [!INCLUDE[SPS2010](../../includes/sps2010-md.md)] sont disponibles dans beaucoup plus de langues que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 Lorsque vous configurez un serveur de rapports pour qu'il s'exécute dans un déploiement d'un produit SharePoint, vous pouvez éventuellement constater une combinaison de langues. L'interface utilisateur, la documentation et les messages apparaissent dans les langues suivantes :  
  
-   L’ensemble des pages, des outils, des erreurs, des avertissements et des messages de l’application qui proviennent Reporting Services apparaissent dans la langue utilisée par l’instance de Reporting Services dans l’une des langues de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Les pages d’application que vous ouvrez sur un site SharePoint, le composant WebPart Visionneuse de rapports et le Générateur de rapports s’afficheront dans l’une des langues prises en charge pour le complément Reporting Services. Pour parcourir la liste des langues prises en charge, consultez [Téléchargements SQL Server](http://msdn.microsoft.com/sql/downloads/) et recherchez la page de téléchargement pour le complément SQL Server 2016 Reporting Services.  
  
-   Les sites SharePoint, l'administration centrale de SharePoint, l'aide en ligne et les messages sont disponibles dans les langues prises en charge par les produits Office Server.  
  
 Si la langue de votre produit ou technologie SharePoint diffère de la langue du serveur de rapports, Reporting Services essaie d’utiliser une langue de la même famille de langues qui correspond le mieux. Si aucun substitut proche n'est disponible, le serveur de rapports utilise l'anglais.  
  
## <a name="related-tasks"></a>Tâches associées

 Le tableau suivant récapitule les tâches liées à un serveur de rapports Reporting Services en mode SharePoint :  
  
|**Tâche**|**Lien**|  
|--------------|--------------|  
|Procédures détaillées pour installer et configurer Reporting Services en mode SharePoint.|[Installer le mode SharePoint de Reporting Services pour SharePoint 2010](http://msdn.microsoft.com/47efa72e-1735-4387-8485-f8994fb08c8c) et [Ajouter un serveur de rapports supplémentaire à une batterie](../../reporting-services/install-windows/add-an-additional-report-server-to-a-farm-ssrs-scale-out.md).|  
|Augmenter la puissance de votre déploiement de Reporting Services SharePoint en ajoutant des serveurs de rapports supplémentaires.|[Ajouter un serveur de rapports supplémentaire à une batterie](../../reporting-services/install-windows/add-an-additional-report-server-to-a-farm-ssrs-scale-out.md) et [Topologies de déploiement pour les fonctionnalités SQL Server BI dans SharePoint](http://msdn.microsoft.com/library/39f76bc7-94e6-4dbc-bfa5-d56f4430bb26).|  
|Ajouter des serveurs web frontaux SharePoint supplémentaires sur lesquels sont installés les composants Reporting Services pour l’affichage et les éléments de rapport.|[Ajouter un serveur web frontal Reporting Services supplémentaire à une batterie](../../reporting-services/install-windows/add-an-additional-reporting-services-web-front-end-to-a-farm.md)|  
|Configurer l’e-mail de votre serveur de rapports dans SharePoint.|[Configurer l’e-mail d’une application de service Reporting Services](../install-windows/configure-e-mail-for-a-reporting-services-service-application.md)|
|Les informations récentes pour cette version s'appuient sur TechNet Wiki.|[Conseils, astuces et dépannage pour SQL Server 2012 Reporting Services](http://go.microsoft.com/fwlink/?LinkId=221297).|  

## <a name="next-steps"></a>Étapes suivantes

[Installer ou désinstaller le complément Reporting Services pour SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)   
[Composant WebPart Visionneuse de rapports sur un site SharePoint](../../reporting-services/report-server-sharepoint/report-viewer-web-part-on-a-sharepoint-site.md)   
[Quiz : Configuration de SQL Server Reporting Services 2012 pour l’intégration SharePoint](http://go.microsoft.com/fwlink/?LinkId=306443)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
