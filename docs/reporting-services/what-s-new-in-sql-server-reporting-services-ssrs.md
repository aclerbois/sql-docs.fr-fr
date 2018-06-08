---
title: Nouveautés de SQL Server Reporting Services (SSRS) | Microsoft Docs
ms.date: 03/16/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.custom: ''
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: get-started-article
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: a279a399575f6272b3cefc22a2a319637de6c7b5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="whats-new-in-sql-server-reporting-services-ssrs"></a>Nouveautés de SQL Server Reporting Services (SSRS)

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../includes/ssrs-appliesto-not-pbirs.md)])

Découvrez les nouveautés de SQL Server [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]. Cette rubrique, qui aborde les fonctionnalités majeures, est mise à jour à mesure de la sortie de nouveaux éléments.

  Pour plus d’informations sur les nouveautés concernant d’autres aspects de SQL Server, consultez [Nouveautés de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md) ou [Nouveautés de SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md).

 **Télécharger** ![download](../analysis-services/media/download.png "download")

- Pour télécharger SQL Server 2017 Reporting Services, accédez au **[Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=55252)**.

Pour obtenir les dernières notes de publication, consultez [Notes de publication de SQL Server 2017](../sql-server/sql-server-2017-release-notes.md). 

Pour plus d’informations sur Power BI Report Server, consultez [Prise en main de Power BI Report Server](https://powerbi.microsoft.com/documentation/reportserver-get-started/).

## <a name="whats-new-in-sql-server-2017"></a>Nouveautés de SQL Server 2017

### <a name="comments-on-reports"></a>Commentaires sur les rapports

Des commentaires sont maintenant disponibles pour les rapports, pour ajouter une perspective et collaborer avec d’autres utilisateurs. Vous pouvez également inclure des pièces jointes avec des commentaires.

![Commentaires contenus dans un serveur de rapports](media/what-s-new-in-sql-server-reporting-services-ssrs/report-server-comments.png)

Pour plus d’informations, consultez [Ajouter des commentaires à un rapport dans un serveur de rapports](https://powerbi.microsoft.com/documentation/reportserver-add-comments/).

### <a name="dax-queries-in-reporting-tools"></a>Requêtes DAX dans les outils de création de rapports

Dans les dernières versions du Générateur de rapports et SQL Server Data Tools, vous pouvez créer des requêtes DAX natives sur des modèles de données tabulaires SQL Server Analysis Services pris en charge en faisant glisser les champs voulus dans les concepteurs de requêtes. Consultez le [blog de Reporting Services](https://blogs.msdn.microsoft.com/sqlrsteamblog/2017/03/09/query-designer-support-for-dax-now-available-in-report-builder-and-sql-server-data-tools/).

### <a name="rest-api-support"></a>Prise en charge des API REST

Pour permettre le développement d’applications modernes et la personnalisation, SQL Server Reporting Services prend désormais en charge une API RESTful entièrement conforme à OpenAPI. La spécification et la documentation complètes de l’API se trouvent désormais sur [swaggerhub](https://app.swaggerhub.com/apis/microsoft-rs/SSRS/2.0).

## <a name="query-designer-support-for-dax-now-in-report-builder-and-sql-server-data-tools"></a>Prise en charge du Concepteur de requêtes pour DAX désormais disponible dans le Générateur de rapports et SQL Server Data Tools

Dans les dernières versions du Générateur de rapports et de SQL Server Data Tools (version Release Candidate), vous pouvez maintenant créer des requêtes DAX natives sur des modèles de données tabulaires SQL Server Analysis Services pris en charge. Vous pouvez utiliser le Concepteur de requêtes dans ces deux outils pour faire glisser et déplacer les champs souhaités et faire en sorte que la requête DAX soit générée au lieu de l’écrire vous-même.  
 
Des informations supplémentaires sont disponibles dans le [blog de Reporting Services](https://blogs.msdn.microsoft.com/sqlrsteamblog/2017/03/09/query-designer-support-for-dax-now-available-in-report-builder-and-sql-server-data-tools/).

* Téléchargez le [Générateur de rapports Microsoft SQL Server 2016](https://go.microsoft.com/fwlink/?LinkId=734968).
* Téléchargez [SSDT (SQL Server Data Tools) - Version Release Candidate](https://docs.microsoft.com/sql/ssdt/sql-server-data-tools-ssdt-release-candidate).

> **Remarque** : Vous pouvez utiliser le Concepteur de requêtes pour DAX uniquement avec des sources de données tabulaires SSAS générés dans SQL Server 2016+.
 
## <a name="whats-new-in-sql-server-2016"></a>Nouveautés de SQL Server 2016
  
### <a name="reporting-services-includessrswebportal-non-markdownincludesssrswebportal-non-markdown-mdmd"></a>[!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)]  
 Un nouveau [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] est disponible. Il s’agit d’un portail mis à jour et moderne qui inclut des indicateurs de performance clés, des rapports mobiles, des rapports paginés, ainsi que des fichiers Excel et Power BI Desktop. Le [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] remplace le Gestionnaire de rapports des versions antérieures. Vous pouvez également télécharger l’Éditeur de rapports mobiles et le Générateur de rapports à partir du [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] sans avoir besoin de la technologie ClickOnce.
 
 Pour créer des rapports mobiles, vous avez besoin de l’ [!INCLUDE[SS_MobileReptPub_Short](../includes/ss-mobilereptpub-short-md.md)].  
  
 Pour plus d’informations sur le [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)], consultez l’article [Portail web (SSRS en mode natif)](../reporting-services/web-portal-ssrs-native-mode.md).  
  
 ![ssRSPortal](../reporting-services/media/ssrsportal.png "ssRSPortal")  
 
 #### <a name="custom-branding-for-the-includessrswebportal-non-markdownincludesssrswebportal-non-markdown-mdmd"></a>Personnaliser le [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 
  Vous pouvez personnaliser le [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] avec les couleurs et le logo de votre organisation au moyen d’un pack de personnalisation.  
  
  Pour plus d’informations sur la personnalisation, consultez [Personnalisation du portail web](http://msdn.microsoft.com/6dac97f7-02a6-4711-81a3-e850a6b40bf1).
 
 #### <a name="key-performance-indicators-kpi-in-the-includessrswebportal-non-markdownincludesssrswebportal-non-markdown-mdmd"></a>Indicateurs de performance clé (KPI) dans le [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 

Vous pouvez créer directement dans le [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] des indicateurs de performance clés ayant pour contexte le dossier dans lequel vous vous trouvez. Quand vous créez des indicateurs de performance clés, vous pouvez choisir les champs du dataset et synthétiser ces valeurs. Vous pouvez également sélectionner le contenu associé pour explorer au niveau du détail.
  
 ![ssrs-webportal-kpi](../reporting-services/media/ssrs-webportal-kpi.png)
 
 Pour plus d’informations, consultez [Utilisation des indicateurs de performance clés dans le portail web](http://msdn.microsoft.com/a28cf500-6d47-4268-a248-04837e7a09eb).
  
 
 ### <a name="mobile-reports"></a>Rapports mobiles
 
Les rapports mobiles Reporting Services sont des rapports dédiés qui sont optimisés pour un grand nombre de facteurs de forme et qui permettent aux utilisateurs d’accéder de manière optimale aux rapports sur les appareils mobiles. Les rapports mobiles sont dotés d’un large éventail de visualisations, allant des graphiques de temps, à catégories et de comparaison aux cartes personnalisées, en passant par les arborescences. Connectez vos rapports mobiles à de nombreuses sources de données, notamment des données multidimensionnelles et tabulaires SQL Server Analysis Services locales. Disposez vos rapports mobiles sur une aire de conception avec des lignes et des colonnes de grille réglables et des éléments de rapport mobile flexibles bien adaptés à la taille de votre écran. Ensuite, enregistrez ces rapports mobiles sur un serveur Reporting Services, puis affichez-les et exploitez-les dans un navigateur ou dans l’application mobile Power BI sur un appareil iPad, iPhone, Windows 10 ou sur un téléphone Android.
  
#### <a name="mobile-report-publisher"></a>Éditeur de rapports mobiles  
 L’ [!INCLUDE[SS_MobileReptPub_Long](../includes/ss-mobilereptpub-long-md.md)]vous permet de créer et de publier des rapports mobiles SQL Server sur votre [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)].  
  
 ![SS_MRP_LayoutTabSmall](../reporting-services/media/ss-mrp-layouttabsm.png "SS_MRP_LayoutTabSmall")  
  
 Pour plus d’informations, consultez [Créer des rapports mobiles avec l’Éditeur de rapports mobiles SQL Server](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md).  
  
#### <a name="sql-server-mobile-reports-hosted-in-reporting-services-available-in-power-bi-mobile-app"></a>Rapports mobiles SQL Server hébergés dans Reporting Services disponibles dans l’application Power BI Mobile  
 L’application Power BI Mobile pour iOS sur iPad et iPhone peut désormais afficher les rapports mobiles SQL Server hébergés sur votre serveur de rapports local.  
  
 ![SS_MRP_iPad_HomeSm](../reporting-services/media/ss-mrp-ipad-homesm.png "SS_MRP_iPad_HomeSm")  
  
 Par défaut, vous ne pouvez pas vous connecter sans apporter certaines modifications à la configuration. Pour plus d’informations sur la façon d’autoriser l’application Power BI Mobile à se connecter à votre serveur de rapports, consultez [Enable a report server for Power BI Mobile access](../reporting-services/report-server/enable-a-report-server-for-power-bi-mobile-access.md).
  
### <a name="support-of-sharepoint-mode-and-sharepoint-2016"></a>Prise en charge du mode SharePoint et de SharePoint 2016  
 [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] prend en charge l’intégration à SharePoint 2013 et SharePoint 2016.
 
Pour plus d'informations, consultez :  
  
-   [Combinaisons de serveur et complément SharePoint et Reporting Services prises en charge &#40;SQL Server 2016&#41;](../reporting-services/install-windows/supported-combinations-of-sharepoint-and-reporting-services-server.md)  
  
-   [Où trouver le complément Reporting Services pour les produits SharePoint](../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)  
  
-   [Installer le mode SharePoint de Reporting Services](../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)  

### <a name="microsoft-net-framework-4-support"></a>Prise en charge de Microsoft .NET Framework 4  
 [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] prend en charge les versions actuelles de Microsoft .NET Framework 4, dont 4.0 et 4.5.1. Si aucune version de .NET Framework 4 n’est installée, le programme d’installation [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] installe NET 4.0 pendant l’installation des fonctionnalités.  

### <a name="report-improvements"></a>Améliorations apportées aux rapports

**Moteur de rendu HTML 5 :** nouveau moteur de rendu HTML5 qui cible le mode standard web moderne « complet » et les navigateurs modernes.  Le nouveau moteur de rendu n’utilise plus le mode excentrique utilisé par certains navigateurs plus anciens.
  
 Pour plus d’informations sur la prise en charge des navigateurs, consultez [Prise en charge des navigateurs pour Reporting Services et Power View](../reporting-services/browser-support-for-reporting-services-and-power-view.md).  

**Rapports paginés modernes :** concevez de magnifiques rapports paginés modernes avec les nouveaux styles modernes pour les graphiques, jauges, cartes et autres visualisations de données.
  
**Graphiques de compartimentage et en rayons de soleil :** Améliorez vos rapports avec des graphiques de compartimentage ![ssrs_treemap_icon](../reporting-services/media/ssrs-treemap-icon.png "ssrs_treemap_icon") et en rayons de soleil ![ssrs_sunburst_icon](../reporting-services/media/ssrs-sunburst-icon.png "ssrs_sunburst_icon") très utiles pour afficher des données hiérarchiques. Pour plus d’informations, consultez [Tree Map and Sunburst Charts in Reporting Services](../reporting-services/report-design/tree-map-and-sunburst-charts-in-reporting-services.md).  

**Incorporation des rapports :** vous pouvez désormais incorporer des rapports mobiles et paginés dans d’autres pages web et des applications en utilisant un iframe avec les paramètres d’URL.  

**Épingler des éléments de rapport à un tableau de bord Power BI :** quand vous affichez un rapport dans le [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], vous pouvez sélectionner des éléments de rapport et les épingler à un tableau de bord [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] .   Vous pouvez épingler les éléments suivants : graphiques, jauges, cartes et images. Vous pouvez **(1)** sélectionner le groupe qui contient le tableau de bord sur lequel vous souhaitez épingler l’élément, **(2)** sélectionner le tableau de bord sur lequel vous souhaitez épingler l’élément et **(3)** sélectionner la fréquence à laquelle vous voulez que la vignette soit mise à jour sur le tableau de bord.   ![remarque](../analysis-services/instances/install-windows/media/ssrs-fyi-note.png "remarque") L’actualisation est gérée par les abonnements [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] et, une fois l’élément épinglé, vous pouvez modifier l’abonnement et configurer une autre planification de l’actualisation.  
  
 ![ssRS_Pin_to_PowerBI](../reporting-services/media/ssrs-pin-to-powerbi.png) 
  
 Pour plus d’informations, consultez [Intégration de Power BI Report Server BI &#40;Gestionnaire de configuration&#41;](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md) et [Épingler des éléments Reporting Services aux tableaux de bord Power BI](../reporting-services/pin-reporting-services-items-to-power-bi-dashboards.md).  
 
 **Exportation et rendu PowerPoint :** e format Microsoft PowerPoint (PPTX) est une nouvelle extension de rendu [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] . Vous pouvez exporter des rapports au format PPTX à partir des applications habituelles : le Générateur de rapports, le Concepteur de rapports (dans SSDT) et le [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]. À titre d’exemple, l’illustration suivante montre le menu d’exportation du [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]. 
  
 ![ssrs-export-powerpoint](../reporting-services/media/ssrs-export-powerpoint.png) 
  
 Vous pouvez également sélectionner le format PPTX pour la sortie de l’abonnement et utiliser l’accès URL du serveur de rapports pour afficher et exporter un rapport. À titre d’exemple, la commande URL suivante dans votre navigateur exporte un rapport à partir d’une instance nommée du serveur de rapports.  
  
```  
http://servername/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  
  
 Pour plus d’informations, consultez [Export a Report Using URL Access](../reporting-services/export-a-report-using-url-access.md).  
 
 **Le format PDF remplace ActiveX pour l’impression à distance :** l’expérience d’impression ActiveX de la barre d’outils de la visionneuse de rapports a été remplacée par une version PDF moderne qui fonctionne sur un large éventail de navigateurs pris en charge, notamment Microsoft Edge. Il n’est plus nécessaire de télécharger des contrôles ActiveX Selon le navigateur que vous utilisez et les applications et services d’affichage PDF que vous avez installés, [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] ouvre une boîte de dialogue d’impression pour imprimer votre rapport ou vous invite à télécharger le fichier .pdf associé.  En tant qu’administrateur, vous pouvez toujours désactiver l’impression côté client à partir de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]. Pour plus d’informations, consultez [Activer et désactiver l’impression côté client pour Reporting Services](../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md).

![ssrs-pdf-printing](../reporting-services/media/ssrs-pdf-printing.png)

### <a name="subscription-improvements"></a>Améliorations apportées aux abonnements  
 
|Fonctionnalité|Mode serveur pris en charge|  
|-------------|---------------------------|  
|**Activation et désactivation des abonnements**. De nouvelles options de l’interface utilisateur permettant de rapidement désactiver et activer les abonnements. Les abonnements désactivés conservent leurs autres propriétés de configuration comme la planification, et peuvent être facilement activés.<br /><br /> ![ssrs-enable-disable-subscriptions](../reporting-services/media/ssrs-enable-disable-subscriptions.png)<br /><br /> Pour plus d’informations, consultez [Disable or Pause Report and Subscription Processing](../reporting-services/subscriptions/disable-or-pause-report-and-subscription-processing.md).|en mode natif|  
|**Description de l’abonnement**. Lorsque vous créez un abonnement, vous pouvez désormais inclure une description du rapport au sein des propriétés d’abonnement. La description est incluse dans la page de résumé de l’abonnement.|SharePoint et mode natif|  
|**Modification du propriétaire de l’abonnement**. Interface utilisateur améliorée permettant de rapidement changer le propriétaire d’un abonnement. Les versions précédentes de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] permettent aux administrateurs de modifier les propriétaires d’abonnements à l’aide d’un script. Depuis la version [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] , vous pouvez modifier les propriétaires d’abonnements à l’aide de l’interface utilisateur ou d’un script. La modification du propriétaire de l’abonnement est une tâche administrative courante lorsque les utilisateurs quittent ou modifient des rôles dans votre organisation.|SharePoint et mode natif|  
|**Partage des informations d’identification pour les abonnements aux partages de fichiers**. Les abonnements aux partages de fichiers [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] incluent désormais deux flux de travail :<br /><br /> (Nouveauté de cette version) Votre administrateur [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] peut configurer un compte de partage de fichiers unique, utilisé pour un ou plusieurs abonnements. Le compte de partage de fichiers est configuré dans le gestionnaire de configuration en mode natif [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] **Spécifier un compte de partage de fichiers**, puis dans la page de configuration de l’abonnement, les utilisateurs sélectionnent **Utiliser le compte de partage de fichiers**.<br /><br /> Configurez les abonnements individuels avec des informations d’identification spécifiques pour le partage de fichiers de destination.<br /><br /> Vous pouvez également combiner les deux approches de sorte que certains abonnements de partage de fichiers utilisent le compte de partage de fichiers central tandis que d’autres abonnements utilisent des informations d’identification spécifiques.|en mode natif|  

### <a name="sql-server-data-tools-ssdt"></a>Outils de données SQL Server (SSDT)  
 La nouvelle version de SSDT inclut les modèles de projet pour [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)]: Assistant Projet Report Server et Projet Report Server. Pour plus d’informations sur le téléchargement de SSDT, consultez [SQL Server Data Tools for Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=827542).  

### <a name="report-builder-improvements"></a>Améliorations du Générateur de rapports

**Nouvelle interface utilisateur du Générateur de rapports :** la principale interface utilisateur du [!INCLUDE[ssRBnoversion](../includes/ssrbnoversion-md.md)] a maintenant une apparence moderne avec des éléments simplifiés.  
  
|||  
|-|-|  
|Nouvelle|Previous|  
|![ssrs_rbfacelift_new](../reporting-services/media/ssrs-rbfacelift-new.png "ssrs_rbfacelift_new")|![ssrs_rbfacelift_old](../reporting-services/media/ssrs-rbfacelift-old.png "ssrs_rbfacelift_old")|  

**Volet Paramètres personnalisé :** vous pouvez maintenant personnaliser le volet Paramètres. Vous pouvez utiliser l’aire de conception du Générateur de rapports pour faire glisser un paramètre vers une colonne et une ligne spécifiques du volet Paramètres. Vous pouvez ajouter et supprimer des colonnes pour modifier la disposition du volet.   Pour plus d’informations, consultez [Personnaliser le volet Paramètres dans un rapport &#40;Générateur de rapports&#41;](../reporting-services/report-design/customize-the-parameters-pane-in-a-report-report-builder.md).  
  
 ![Liste des paramètres dans le volet des données du rapport et dans le volet des paramètres](../reporting-services/media/ssrs-customizeparameter-parameterlist-reportdatapane.png "Liste des paramètres dans le volet des données du rapport et dans le volet des paramètres")  

  
**Prise en charge des résolutions élevées :** [!INCLUDE[ssRBnoversion](../includes/ssrbnoversion-md.md)] prend en charge les appareils et la mise à l’échelle haute résolution.  Pour plus d’informations sur la haute résolution, consultez les documents suivants :  
  
-   [Windows 8.1 DPI Scaling Enhancements (Améliorations apportées à la mise à l’échelle PPP dans Windows 8.1)](https://blogs.windows.com/windowsexperience/2013/07/15/windows-8-1-dpi-scaling-enhancements/)  
  
-   [High DPI and Windows 8.1 (Windows 8.1 et la haute résolution)](http://technet.microsoft.com/library/dn528848.aspx)  

## <a name="next-steps"></a>Étapes suivantes

[Nouveautés d’Analysis Services](http://msdn.microsoft.com/aa69c299-b8f4-4969-86d8-b3292fe13f08)  
[Notes de publication de SQL Server 2016](../sql-server/sql-server-2016-release-notes.md)   
[Compatibilité descendante](http://msdn.microsoft.com/675b0e0e-cfee-4790-9675-80fc3ea6d30f)   
[Fonctionnalités Reporting Services prises en charge par les éditions de SQL Server 2016](http://msdn.microsoft.com/39f03d2d-6e48-4b34-a9d3-07f86313b937)   
[Mettre à niveau et migrer Reporting Services](../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[Reporting Services](../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
