---
title: Planifier la conception de rapports et le déploiement de rapports | Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: get-started-article
ms.assetid: 1c1e265e-52a2-4de3-96fd-ca4abae01c02
caps.latest.revision: 19
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 912d8f3035df4c5698deb9dc78321770f23fdaf2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="plan-for-report-design-and-report-deployment--reporting-services"></a>Planifier la conception de rapports et le déploiement de rapports | Reporting Services
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] offre plusieurs approches pour créer et déployer des rapports paginés. Découvrez comment planifier un outil de création de rapports et un environnement de serveur de rapports qui fonctionnent ensemble.

Cette rubrique offre une vue d'ensemble de la prise en charge de la définition de rapport par les composants [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . Une définition de rapport est un fichier XML écrit dans le langage RDL (Report Definition Language) ou RDLC (Report Definition Language for Clients). Chaque définition de rapport est conforme à une version de schéma spécifique qui est indiquée au début du fichier.  
  
 Les fichiers RDL sont créés dans le Concepteur de rapports dans les projets [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] et dans le Générateur de rapports. Les fichiers RDLC sont créés à l'aide des contrôles ReportViewer qui sont inclus dans [!INCLUDE[vsprvs](../includes/vsprvs-md.md)].
  
##  <a name="bkmk_rdl_schema_versions"></a> Versions de schéma RDL  
 Le tableau suivant répertorie chaque version disponible du schéma et l'abréviation utilisée dans le reste de cette rubrique :  
  
|Abréviation|Version de schéma|  
|------------------|--------------------|  
|2016 RDL|`http://schemas.microsoft.com/sqlserver/reporting/2016/01/reportdefinition`|
|RDL 2010|`http://schemas.microsoft.com/sqlserver/reporting/2010/01/reportdefinition`|  
|RDL 2008|`http://schemas.microsoft.com/sqlserver/reporting/2008/01/reportdefinition`|  
|RDL 2005<br /><br /> RDLC 2005|`http://schemas.microsoft.com/sqlserver/reporting/2005/01/reportdefinition`|  
|RDL 2000|`http://schemas.microsoft.com/sqlserver/reporting/2003/10/reportdefinition`|  
  
 Pour plus d'informations sur RDL et les schémas RDL, reportez-vous aux rubriques suivantes :  
  
-   [Schémas XML SQL Server](http://go.microsoft.com/fwlink/?LinkId=31850)  
  
-   [Spécifications RDL (Report Language Definition)](http://go.microsoft.com/fwlink/?linkid=116865)  
  
-   [Langage de définition de rapport &#40;SSRS, Report Definition Language&#41;](../reporting-services/reports/report-definition-language-ssrs.md)  
  
 Pour plus d’informations sur les contrôles ReportViewer, consultez [Contrôles ReportViewer (Visual Studio)](http://msdn.microsoft.com/library/ms251671.aspx).  
  
##  <a name="bkmk_report_server_rdl_schema_support"></a> Prise en charge du serveur de rapports et du schéma RDL  
 Un fichier de définition de rapport peut être déployé sur un serveur de rapports [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] des manières suivantes :  
  
-   **Concepteur de rapports :** déployez un rapport du Concepteur de rapports dans [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)].  
  
-   **Générateur de rapports :** enregistrez un rapport sur le serveur de rapports à partir du Générateur de rapports.  
  
-   **Portail web :** téléchargez un rapport vers un serveur de rapports configuré en mode natif à partir du [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)].  
  
-   **SharePoint :** téléchargez un rapport vers un site SharePoint configuré avec un serveur de rapports en mode SharePoint.  
  
-   **Par programmation :** publiez par programmation un rapport à l'aide des interfaces SOAP API vers un serveur de rapports. Pour plus d'informations, consultez [Report Server Web Service](../reporting-services/report-server-web-service/report-server-web-service.md).  
  
 Le tableau suivant indique version par version le schéma RDL pris en charge pour le serveur de rapports.  
  
|Version de serveur de rapports|Version de schéma RDL|  
|---------------------------|------------------------|  
|SQL Server 2016|2016 RDL<br /><br />RDL 2010<br /><br /> RDL 2008<br /><br /> RDL 2005<br /><br /> RDL 2000
|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]<br /><br /> ou<br /><br /> [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]<br /><br /> ou<br /><br /> [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|RDL 2010<br /><br /> RDL 2008<br /><br /> RDL 2005<br /><br /> RDL 2000|  
|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]|RDL 2008<br /><br /> RDL 2005<br /><br /> RDL 2000|  
  
 Lorsque vous téléchargez ou publiez une définition de rapport vers le serveur de rapports ou mettez à niveau un serveur de rapports qui contient des rapports, le serveur de rapports conserve la définition de rapport dans le format d'origine. **Lors de la première utilisation**, le serveur de rapports met à niveau le rapport dans la base de données du serveur de rapports dans un format binaire qui est conservé pour les vues suivantes. La définition de rapport (.rdl) proprement dite n'est pas mise à niveau.  
  
 Vous pouvez extraire du serveur de rapports une copie en lecture seule du fichier de définition de rapport (.rdl). Sur un serveur de rapports en mode natif, accédez au [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], sélectionnez le rapport et cliquez sur **Télécharger**. Dans un déploiement en mode SharePoint, accédez à la bibliothèque de documents, sélectionnez le rapport et cliquez sur **Télécharger une copie**.  
  
 Pour mettre à niveau la définition de rapport, vous devez ouvrir le rapport dans un environnement de création de rapports, comme SQL Server Data Tools, et l’enregistrer.  
  
 Pour plus d’informations sur les mises à niveau de rapports et les versions de schéma prises en charge, consultez [Mettre à niveau des rapports](../reporting-services/install-windows/upgrade-reports.md).  
  
##  <a name="bkmk_report_authoring_and_deployment"></a> Prise en charge de la création et le déploiement de rapports  
 Les environnements de création de rapports sont le Concepteur de rapports dans les projets [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] et le Générateur de rapports. Les environnements de création de rapports fournissent de nombreuses prises en charge pour la mise à niveau de rapport, la conception de rapport, l'aperçu de rapport en mode local, l'aperçu de rapport sur le serveur de rapports et le déploiement de rapport.  
  
 Le tableau suivant récapitule la prise en charge de la création et du déploiement de définitions de rapport pour les différentes versions de schéma :  
  
|Environnement de création|Version RDL créée|Version RDL de déploiement|Versions de déploiement sur le serveur de rapports|  
|---------------------------|--------------------------|------------------------|--------------------------------------|  
|Générateur de rapports SQL Server 2016|Crée RDL 2016<br /><br /> Mettre à niveau les anciennes versions RDL vers 2016 RDL|2016 RDL|SQL Server 2016|
|Concepteur de rapports dans SQL Server 2016 Data Tools - Business Intelligence pour Microsoft Visual Studio 2015|Crée RDL 2016<br /><br /> Mettre à niveau les anciennes versions RDL vers 2016 RDL|2016 RDL|SQL Server 2016|
|Concepteur de rapports dans SQL Server 2014 Data Tools - Business Intelligence pour Microsoft Visual Studio 2012<br /><br /> ou<br /><br /> Concepteur de rapports dans SQL Server 2012 Data Tools - Business Intelligence pour Microsoft Visual Studio 2012<br /><br /> ou<br /><br /> Concepteur de rapports dans [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] Data Tools, inclus dans [!INCLUDE[ssSQL11](../includes/sssql11-md.md)].|Crée RDL 2010<br /><br /> Mettra à niveau les anciennes versions RDL vers RDL 2010|RDL 2010|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]<br /><br /> [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]<br /><br /> [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|  
|Concepteur de rapports dans [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] Business Intelligence Development Studio|Crée RDL 2010<br /><br /> Mettra à niveau les anciennes versions RDL vers RDL 2010|RDL 2010|[!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|  
|Concepteur de rapports dans [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] Business Intelligence Development Studio|Crée RDL 2008<br /><br /> Mettra à niveau les anciennes versions RDL vers RDL 2008|RDL 2008|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]|
  
 Pour plus d’informations sur SQL Server Data Tools (SSDT), reportez-vous aux sites suivants :  
  
-   [Déploiement et prise en charge des versions dans les outils de données SQL Server &#40;SSRS&#41;](../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)  
  
-   [SQL Server Data Tools pour Visual Studio 2015](../ssdt/download-sql-server-data-tools-ssdt.md)  
  
##  <a name="bkmk_reportviewer"></a> Contrôles ReportViewer  
 Le contrôle ReportViewer [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] peut d'afficher un rapport .rdlc en mode de prévisualisation local ou distant. Il peut afficher un fichier .rdl hébergé sur un serveur de rapports [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . Le tableau suivant fournit la liste des versions RDL prises en charge par les contrôles de ReportViewer pour le traitement local (.rdlc). La prise en charge RDL côté serveur est résumée dans la section [Prise en charge du serveur de rapports et du schéma RDL](#bkmk_report_server_rdl_schema_support).  
  
|Contrôle ReportViewer du produit|Version de RDL pour la prévisualisation locale|  
|-------------------------------------|--------------------------------------|  
|[!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2015 <br/><br/>ou<br/><br/>[!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2013<br /><br /> ou<br /><br /> [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2012<br /><br /> ou<br /><br /> [!INCLUDE[vs_dev10_long](../includes/vs-dev10-long-md.md)]|RDL 2008|  
|[!INCLUDE[vsprvslong](../includes/vsprvslong-md.md)]<br /><br /> ou<br /><br /> [!INCLUDE[vsOrcas](../includes/vsorcas-md.md)]|RDL 2005|  
  
 Pour plus d'informations, consultez les documents suivants :  
  
-   [Conversion de fichiers RDLC en fichiers RDL](http://msdn.microsoft.com/library/ms252109.aspx)  
  
-   [Contrôles ReportViewer (Visual Studio)](http://msdn.microsoft.com/library/ms251671.aspx)  
  
-   [Ajout et configuration de contrôles ReportViewer](http://msdn.microsoft.com/library/ms252104.aspx)  
  
## <a name="see-also"></a> Voir aussi  
 [Rapports, parties de rapports et définitions de rapports &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/reports-report-parts-and-report-definitions-report-builder-and-ssrs.md)   
 [Outils de Reporting Services](../reporting-services/tools/reporting-services-tools.md)   
 [RDL (Report Definition Language) &#40;SSRS&#41;](../reporting-services/reports/report-definition-language-ssrs.md)  
  
  
