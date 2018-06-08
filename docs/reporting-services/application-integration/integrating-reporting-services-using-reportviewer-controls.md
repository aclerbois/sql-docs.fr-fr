---
title: Intégration de Reporting Services à l’aide des contrôles ReportViewer | Microsoft Docs
ms.custom: ''
ms.date: 09/06/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: application-integration
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- ReportViewer controls
- integrating reports [Reporting Services]
ms.assetid: 3ba47fb4-73a9-4059-89fd-329adebe94a8
caps.latest.revision: 26
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: bf99e8bbb0286c66ca0b16cea0ccb8ff8a1817cd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integrating-reporting-services-using-reportviewer-controls"></a>Intégration de Reporting Services à l’aide des contrôles ReportViewer
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio 2015 propose deux contrôles ReportViewer permettant d’intégrer des fonctionnalités d’affichage des rapports à vos applications. Il existe une version pour les applications Windows Forms et une version pour les applications Web Forms. Chaque contrôle offre des fonctionnalités similaires, mais chacun est conçu pour un environnement particulier. Les deux contrôles peuvent traiter des rapports qui ont été déployés sur un serveur de rapports (mode de traitement à distance) ou qui ont été copiés sur un ordinateur où [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] n’a pas été installé (mode de traitement local).  
  
 Le contrôle ReportViewer n'offre pas la prise en charge intégrée de l'adaptation dynamique à différents appareils avec différentes résolutions d'écran.  
  
## <a name="remote-processing-mode"></a>Mode de traitement à distance  
 Le mode de traitement à distance est la méthode recommandée pour consulter les rapports qui ont été déployés sur un serveur de rapports. Le mode de traitement à distance offre les avantages suivants :  
  
-   Le traitement à distance offre une solution optimisée pour l'exécution de rapports car le rapport est traité par le serveur de rapports.  
  
-   Dans la mesure où l'ensemble du traitement est géré par le serveur de rapports, une demande de rapport peut être traitée par plusieurs serveurs de rapports dans un déploiement avec montée en puissance parallèle ou par un serveur doté de plusieurs processeurs dans un déploiement avec montée en puissance par unité.  
  
 Par ailleurs, les rapports exécutés en mode à distance peuvent utiliser l'ensemble des fonctionnalités du serveur de rapports, notamment toutes les extensions de rendu et de données.  
  
> [!NOTE]  
>  La liste des extensions disponibles pour le contrôle ReportViewer lorsqu'il s'exécute en mode de traitement à distance dépend de l'édition de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui est installée sur le serveur de rapports.  
  
## <a name="local-processing-mode"></a>Mode de traitement local  
 Le mode de traitement local est une autre méthode d'affichage et de rendu des rapports qui peut être utilisée lorsque [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] n'est pas installé. Contrairement au traitement à distance, seul un sous-ensemble des fonctionnalités fournies par le serveur de rapports est disponible dans le contrôle. En mode de traitement local, le traitement des données n'est pas géré par le contrôle, mais implémenté par l'application hôte. Cependant, le traitement des rapports est géré par le contrôle lui-même. En mode de traitement local, seules les extensions de rendu PDF, Excel, Word et Image sont disponibles.  
  
## <a name="see-also"></a> Voir aussi  
 [Intégration de Reporting Services dans des applications](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [Utilisation du contrôle WebForms ReportViewer](../../reporting-services/application-integration/using-the-webforms-reportviewer-control.md)   
 [Utilisation du contrôle WinForms ReportViewer](../../reporting-services/application-integration/using-the-winforms-reportviewer-control.md)  

  
  
