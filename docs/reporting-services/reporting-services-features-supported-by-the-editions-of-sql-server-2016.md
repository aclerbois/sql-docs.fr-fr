---
title: Fonctionnalités Reporting Services prises en charge par les éditions de SQL Server 2016 | Microsoft Docs
ms.custom: ''
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: get-started-article
ms.assetid: 39f03d2d-6e48-4b34-a9d3-07f86313b937
caps.latest.revision: 3
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 4fe2db69e67e0d0e7630bd34c1d926be49e7bbe2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reporting-services-features-supported-by-the-editions-of-sql-server-2016"></a>Fonctionnalités Reporting Services prises en charge par les éditions de SQL Server 2016

Cette rubrique fournit des détails sur les fonctionnalités prises en charge par les différentes éditions de SQL Server 2016.  
  
 La version d’évaluation de SQL Server est disponible pendant une période d’évaluation de 180 jours.  
  
 Pour obtenir les dernières notes de publication, voir [SQL Server 2016 Release Notes](../sql-server/sql-server-2016-release-notes.md). Pour plus d’informations sur les nouveautés, consultez [Nouveautés de Reporting Services (SSRS)](~/reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).
    
 **Essayez SQL Server 2016 !**    
    
 > [![Télécharger à partir du Centre d’évaluation](../analysis-services/media/download.png)](https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016) **[Télécharger SQL Server 2016 à partir du Centre d’évaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016)**    
    
> ![Petite machine virtuelle Azure](../analysis-services/media/azure-virtual-machine-small.png) **[Faites tourner une machine virtuelle sur laquelle SQL Server 2016 est déjà installé](https://azure.microsoft.com/en-us/marketplace/partners/microsoft/sqlserver2016rtmenterprisewindowsserver2012r2/?wt.mc_id=sqL16_vm)**    

Pour les fonctionnalités prises en charge par les éditions Evaluation et Developer, consultez SQL Server Entreprise Edition.

Pour accéder au tableau d’une technologie SQL Server, cliquez sur son lien :  

-   [Reporting Services](#SSRS)  
  
-   [Clients Business Intelligence](#BIC)  

##  <a name="SSRS"></a> Reporting Services  
  
|Nom de la fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express with Tools|Express|Développeur|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|-------------|---------------|  
|Rapports mobiles et indicateurs de performance clés|Oui||||||Oui|  
|Édition [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prise en charge pour la base de données du catalogue|Standard ou supérieur|Standard ou supérieur|Web|Express|||Standard ou supérieur|  
|Sources de données prises en charge par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Toutes les éditions   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Toutes les éditions [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Web|Express|||Toutes les éditions [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|  
|Serveur de rapports|Oui|Oui|Oui|Oui|||Oui|  
|Concepteur de rapports|Oui|Oui|Oui|Oui|||Oui|  
|Portail web Concepteur de rapports|Oui|Oui|Oui|Oui|||Oui|  
|Sécurité basée sur les rôles|Oui|Oui|Oui|Oui|||Oui|  
|Exporter vers Excel, PowerPoint, Word, PDF et images|Oui|Oui|Oui|Oui|||Oui|  
|Jauges et graphiques améliorés|Oui|Oui|Oui|Oui|||Oui|  
|Épingler des éléments de rapport vers des tableaux de bord [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)]|Oui|Oui|Oui|Oui|||Oui|  
|Authentification personnalisée|Oui|Oui|Oui|Oui|||Oui|  
|Rapport en tant que flux de données|Oui|Oui|Oui|Oui|||Oui|  
|Prise en charge des modèles|Oui|Oui|Oui||||Oui|  
|Créer des rôles personnalisés pour la sécurité basée sur les rôles|Oui|Oui|||||Oui|  
|Sécurité de l'élément de modèle|Oui|Oui|||||Oui|  
|Rapport généré interactif infini|Oui|Oui|||||Oui|  
|Bibliothèque de composants partagés|Oui|Oui|||||Oui|  
|Abonnements et planifications par partage de fichiers et messagerie électronique|Oui|Oui|||||Oui|  
|Historique de rapport, exécution d'instantanés et mise en cache|Oui|Oui|||||Oui|  
|Intégration SharePoint|Oui|Oui|||||Oui|  
|Prise en charge de sources de données distantes et non-SQL<sup>1</sup>|Oui|Oui|||||Oui|  
|Source de données, remise et rendu, extensibilité RDCE|Oui|Oui|||||Oui|  
|Options de personnalisation|Oui||||||Oui|  
|Abonnement aux rapports pilotés par les données|Oui||||||Oui|  
|Déploiement avec montée en puissance parallèle (batteries de serveurs Web)|Oui||||||Oui|  
|Alerte<sup>2</sup>|Oui||||||Oui|  
|[!INCLUDE[ssCrescent](../includes/sscrescent-md.md)] <sup>2</sup>|Oui||||||Oui|  
  
 <sup>1</sup> Pour plus d’informations sur les sources de données prises en charge dans SQL Server 2016 Reporting Services (SSRS), consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
 <sup>2</sup> Exige Reporting Services en mode SharePoint. Pour plus d’informations, consultez [Installer le mode SharePoint de Reporting Services](../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md).  
  
## <a name="report-server-database-server-edition-requirements"></a>Conditions requises pour l'édition SQL Server de la base de données du serveur de rapports  
 Lors de la création d'une base de données de serveur de rapports, toutes les éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne peuvent pas être utilisées pour héberger la base de données. Le tableau suivant répertorie les éditions du [!INCLUDE[ssDE](../includes/ssde-md.md)] que vous pouvez utiliser pour les éditions spécifiques de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)].  
  
|Pour cette édition de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Reporting Services|Utilisez cette édition de l'instance du moteur de base de données pour héberger la base de données|  
|----------------------------------------------------------------------|---------------------------------------------------------------------------|  
|Enterprise|Éditions Standard ou Enterprise (locales ou distantes)|  
|Standard|Éditions Standard ou Enterprise (locales ou distantes)|  
|Web|Web Edition (locale uniquement)|  
|Express with Advanced Services|Express with Advanced Services (local uniquement).|  
|Evaluation|Evaluation|  
  
##  <a name="BIC"></a> Clients Business Intelligence  
 Les applications logicielles clientes suivantes sont disponibles dans le Centre de téléchargement Microsoft ; elles ont pour but de vous aider à créer des documents décisionnels qui s’exécutent sur une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Lorsque vous hébergez ces documents dans un environnement serveur, utilisez une édition de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui prend en charge ce type de document. Le tableau suivant identifie quelle édition de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] contient les fonctionnalités de serveur obligatoires pour héberger les documents créés dans ces applications clientes.  
  
|Nom de l’outil|Enterprise|Standard|Web|Express with Advanced Services|Express with Tools|Express|Développeur|  
|---------------|----------------|--------------|---------|------------------------------------|------------------------|-------------|---------------|  
|[!INCLUDE[ssRBnoversion](../includes/ssrbnoversion-md.md)] (.rdl et .rds)|Oui|Oui|||||Oui|  
|[!INCLUDE[SS_MobileReptPub_Long](../includes/ss-mobilereptpub-long-md.md)] (.rsmobile)|Oui||||||Oui|  
|Applications Power BI pour appareils mobiles (iOS, Windows 10, Android) (.rsmobile)|Oui||||||Oui|  
  
> [!NOTE]  
> 1.  Le tableau ci-dessus identifie les éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] requises pour activer ces outils clients ; toutefois, ces outils peuvent accéder aux données hébergées sur n’importe quelle édition de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
> 2.  [!INCLUDE[SS_MobileReptPub_Long](../includes/ss-mobilereptpub-long-md.md)] est le seul point de création de rapports mobiles. Connectez-vous à un serveur [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] pour accéder aux sources de données et créer des rapports. Ensuite, publiez-les sur le serveur [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] pour que d’autres utilisateurs de l’organisation puissent y accéder, soit sur le serveur, soit sur des appareils mobiles. Vous pouvez également utiliser le [!INCLUDE[SS_MobileReptPub_Long](../includes/ss-mobilereptpub-long-md.md)] autonome avec des sources de données locales.  
> 3.  Si vous utilisez  [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] localement, [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] dans le cloud ou les deux pour votre solution de remise de rapports, vous avez besoin d’une seule application mobile pour accéder aux tableaux de bord et aux rapports mobiles sur des appareils mobiles. Les applications [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] sont disponibles au téléchargement à partir des App Store Windows, iOS et Android.  

## <a name="next-steps"></a>Étapes suivantes

[Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)  
[Spécifications de produit pour SQL Server 2016](http://msdn.microsoft.com/library/6445fd53-6844-4170-a86b-7fe76a9f64cb)  
[Installation de SQL Server 2016](../database-engine/install-windows/installation-for-sql-server-2016.md) 

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
