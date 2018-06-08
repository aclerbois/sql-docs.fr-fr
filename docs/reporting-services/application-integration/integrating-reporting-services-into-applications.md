---
title: Intégration de Reporting Services à des applications | Microsoft Docs
ms.custom: ''
ms.date: 10/19/2017
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
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 4b880674bb1a98c455f991218a21c9f39e9afe53
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integrating-reporting-services-into-applications"></a>Intégration de Reporting Services dans les applications

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../../includes/ssrs-appliesto-not-pbirs.md)])

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est une plate-forme de création de rapports ouverte et extensible conçue pour fournir aux développeurs un jeu complet d'API  pour développer des solutions.

> [!NOTE]
> À partir de SQL Server 2017 Reporting Services, l’accès de l’API REST est disponible pour le développement de solutions. L’accès de l’API SOAP a été déprécié. Pour plus d’informations, consultez [Développer avec les API REST pour Reporting Services](../developer/rest-api.md).
  
 Il existe trois options d’intégration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à des applications personnalisées : le service Web Report Server, également appelé API SOAP de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], les contrôles ReportViewer pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] et l’accès URL. Chaque option représente une approche différente de l'intégration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans vos applications.
  
## <a name="report-server-web-service"></a>Service web Report Server

 Le service Web Report Server constitue la principale interface de développement avec [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Que vous développiez du code pour gérer votre catalogue de rapports ou pour effectuer le rendu de rapports dans un format pris en charge, le service Web propose toutes les méthodes nécessaires pour intégrer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans vos applications. Ainsi, par exemple, le Gestionnaire de rapports, qui est inclus dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], utilise le service web pour gérer la base de données du serveur de rapports.  
  
## <a name="reportviewer-controls-for-visual-studio"></a>Contrôles ReportViewer pour Visual Studio

 Les contrôles ReportViewer disponibles pour [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] sont utilisés pour intégrer la consultation de rapports à vos applications. Il existe deux contrôles : un pour les applications Windows Forms et un autre pour les applications Web Forms. Chaque contrôle propose une fonctionnalité de consultation des rapports qui ont été déployés sur un serveur de rapports et permet d'effectuer le rendu de rapports dans un environnement où un aucun serveur de rapports n'est installé.  
  
## <a name="url-access"></a>accès URL  
 L'accès URL est une autre option d'intégration de la consultation de rapports dans vos applications si les contrôles ReportViewer ne sont pas proposés en option. Par ailleurs, l'accès URL est utile pour envoyer aux utilisateurs des liens vers des rapports par courrier électronique.  
  
## <a name="in-this-section"></a>Contenu de cette section

 [Intégration de Reporting Services à l'aide de SOAP](../../reporting-services/application-integration/integrating-reporting-services-using-soap.md)  
 Décrit comment intégrer la navigation et la gestion des rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans vos applications de gestion existantes à l'aide du service Web Report Server.  
  
 [Intégration de Reporting Services à l’aide des contrôles ReportViewer](../../reporting-services/application-integration/integrating-reporting-services-using-reportviewer-controls.md)  
 Décrit comment intégrer la consultation de rapports dans vos applications existantes à l'aide de contrôles ReportViewer.  
  
 [Intégration de Reporting Services à l'aide de l'accès URL](../../reporting-services/application-integration/integrating-reporting-services-using-url-access.md)  
 Décrit comment intégrer la navigation entre les rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans vos applications de gestion existantes à l'aide de l'accès URL.  
  
## <a name="next-steps"></a>Étapes suivantes

Pour savoir si vous devez utiliser l’accès URL ou les API SOAP, consultez [Choix entre l’accès URL et SOAP dans Reporting Services](choosing-between-url-access-and-soap.md).

Pour plus d’informations sur l’API REST SQL Server 2017 Reporting Services, consultez [Développer avec les API REST pour Reporting Services](../developer/rest-api.md).

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
