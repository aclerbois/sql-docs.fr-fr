---
title: Analyse des performances d’un serveur de rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- report servers [Reporting Services], performance
- counters [Reporting Services]
- monitoring performance [Reporting Services]
- SQL Server Reporting Services, performance
- performance [Reporting Services]
- Reporting Services, performance
ms.assetid: c1bc13d4-8297-4daf-bb19-4c1e5ba292a6
caps.latest.revision: 64
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 02dfceaa1a720e7aa2855c10a1e51ea757e06ce9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="monitoring-report-server-performance"></a>Analyse des performances d'un serveur de rapports
  Utilisez les outils d'analyse des performances sur un serveur de rapports pour évaluer l'activité du serveur, observer les tendances, diagnostiquer les goulots d'étranglement du système ou collecter des données permettant de déterminer si la configuration actuelle est suffisante. Pour optimiser les performances du serveur, vous pouvez spécifier la fréquence de recyclage du domaine d'application du serveur de rapports. Pour plus d’informations, consultez [Configurer la mémoire disponible pour les applications du serveur de rapports](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).  
  
## <a name="sources-of-performance-data"></a>Sources des données de performances  
 Utilisez une combinaison de technologies et d'outils pour obtenir des informations exhaustives sur les performances du système. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Les systèmes d’exploitation Windows Server fournissent des informations sur les performances au moyen des outils suivants :  
  
-   Gestionnaire des tâches  
  
-   Observateur d'événements  
  
-   Console de performance  
  
 Le Gestionnaire des tâches fournit des informations sur les programmes et les processus en cours d'exécution sur votre ordinateur. Il vous permet de surveiller les indicateurs clés des performances de votre serveur de rapports. Vous pouvez également évaluer l'activité des processus en cours d'exécution et afficher des graphiques et des données sur l'utilisation de l'UC et de la mémoire. Pour plus d'informations sur l'utilisation du Gestionnaire des tâches, consultez la documentation du produit [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 Vous pouvez utiliser la Console de performance et l'Observateur d'événements pour créer des journaux et des alertes à propos du traitement des rapports et de la consommation de ressources. Pour plus d’informations sur les événements Windows générés par [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Journal des applications Windows](../../reporting-services/report-server/windows-application-log.md). Pour plus d'informations sur la Console de performance, consultez la section « Compteurs de performances Windows » plus loin dans cette rubrique.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les utilitaires fournissent également des informations sur la base de données du serveur de rapports et sur les bases de données temporaires utilisées pour la gestion des sessions et de la mise en cache.  
  
## <a name="windows-performance-counters"></a>Compteurs de performances Windows  
 La surveillance de compteurs de performances spécifiques vous permet de :  
  
-   estimer la configuration requise nécessaire pour prendre en charge une charge de travail prédite ;  
  
-   créer une ligne de base des performances pour mesurer l'effet de modifications apportées à une configuration ou de mises à niveau d'applications ;  
  
-   observer les performances des applications sous certaines charges créées réellement ou artificiellement ;  
  
-   vérifier que les mises à niveau matérielles ont l'effet escompté sur les performances ;  
  
-   Valider que les modifications qui ont été apportées à la configuration du système ont l'effet souhaité sur les performances.  
  
## <a name="reporting-services-performance-objects"></a>Objets de performance de Reporting Services  
 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] comprend les objets de performance suivants :  
  
-   **Service Web MSRS 2011** et **Service Web MSRS 2011 en mode SharePoint** pour analyser les performances d’un serveur de rapports. Ces objets de performance incluent une collection de compteurs utilisée pour suivre le traitement du serveur de rapports initialisé en général via des opérations de consultation du rapport interactives. Ces compteurs sont réinitialisés à chaque interruption du service Web Report Server par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .  
  
-   **Service Windows MSRS 2011** et **Service Windows MSRS 2011 en mode SharePoint** pour surveiller les opérations planifiées et la remise des rapports. Ces objets de performance incluent une collection de compteurs utilisée pour suivre le traitement des rapports initialisé via des opérations planifiées. Les opérations planifiées englobent l'abonnement et la remise, les instantanés d'exécution de rapport et l'historique de rapport.  
  
-   **ReportServer:Service** et **ReportServerSharePoint:Service** pour surveiller des événements liés à HTTP et la gestion de la mémoire. Ces compteurs sont spécifiques à [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], et ils suivent les événements liés à HTTP pour le serveur de rapports, notamment les demandes, les connexions et les tentatives d’ouverture de session. Cet objet de performance inclut également des compteurs liés à la gestion de la mémoire.  
  
 Si vous possédez plusieurs instances de serveurs de rapports sur un seul ordinateur, vous pouvez les analyser collectivement ou individuellement. Choisissez les instances à inclure au moment où vous ajoutez un compteur. Pour plus d’informations sur l’utilisation de la Console de performances (perfmon.msc) et l’ajout de compteurs, consultez la documentation du produit [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
## <a name="other-performance-counters"></a>Autres compteurs de performance  
 Les compteurs de performances [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] personnalisés sont fournis uniquement pour le **Service Web MSRS 2008**, le **Service Windows MSRS 2008**et **ReportServer:Service**. Les objets de performance suivants fournissent des données d'analyse des performances supplémentaires pour le serveur de rapports.  
  
|Objet de performance|Remarques|  
|------------------------|-----------|  
|**Données CLR .NET** et **mémoire CLR .NET**|Le Gestionnaire de rapports utilise les compteurs de performances d' [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] . Pour plus d'informations, consultez la rubrique relative à l'amélioration des performances et de l'évolutivité des applications .NET, « Improving .NET Application Performance and Scalability » (en anglais) sur MSDN.|  
|**Traiter**|Ajoutez les compteurs de performances **Temps écoulé** et **ID de processus** pour une instance ReportingServicesService pour suivre le temps de fonctionnement de processus par ID de processus.|  
  
## <a name="sharepoint-events"></a>Événements SharePoint  
 En plus des objets de performance [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vous pouvez également vouloir configurer des événements SharePoint si vous exécutez un serveur de rapports en mode intégré SharePoint et avez configuré votre environnement de création de rapports de façon à utiliser un produit SharePoint. Dans cette section, utilisez les événements d'un serveur de rapports en mode intégré SharePoint pour examiner les événements de diagnostic qui peuvent fournir des informations utiles si votre environnement de création de rapports est intégré SharePoint.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Compteurs de performances du service Web MSRS 2011 et des objets de performance du service Windows MSRS 2011 &#40;mode natif&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)  
 Décrit les compteurs de performances utilisés par le service Web Report Server.  
  
 [Compteurs de performances du service Web MSRS 2011 en mode SharePoint et des objets de performance du service Windows MSRS 2011 en mode SharePoint &#40;mode SharePoint&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
 Décrit les compteurs de performances utilisés par le service Windows Report Server.  
  
 [Performance Counters for the ReportServer:Service  and ReportServerSharePoint:Service Performance Objects](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
 Décrit les compteurs de performance liés à HTTP et relatifs à la mémoire dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Événements pour un serveur de rapports en mode intégré SharePoint  
 Décrit les événements de diagnostic utiles pour enregistrer lorsque vous exécutez un environnement de création de rapports avec un produit SharePoint.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer la mémoire disponible pour les applications du serveur de rapports](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)   
 [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Outils de Reporting Services](../../reporting-services/tools/reporting-services-tools.md)  
  
  
