---
title: Méthodes des services web Report Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: report-server-web-service
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- Report Server Web service, methods
- Web service [Reporting Services], methods
- XML Web service [Reporting Services], features
- Web service [Reporting Services], features
- Report Server Web service, features
- XML Web service [Reporting Services], methods
ms.assetid: ce5afa27-e90c-44a7-b204-098a065b3665
caps.latest.revision: 49
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 6f0730ea9ab0ea2a79e6716922739203568977d4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-server-web-service-methods"></a>Méthodes des services Web Report Server
  Les services Web Report Server comportent plusieurs catégories de méthodes basées sur les fonctionnalités des composants. Ces méthodes sont fournies à l'aide de plusieurs points de terminaison de service Web (trois permettant de gérer les rapports, un autre de les exécuter), lesquels sont exposés comme membres des classes <xref:ReportService2010.ReportingService2010> et <xref:ReportExecution2005.ReportExecutionService>. Ces classes peuvent être générées à l’aide d’un outil de classe proxy tel que wsdl.exe. Ce dernier est d’ailleurs inclus dans le SDK du [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]. Pour plus d’informations sur les services web Report Server et le [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], consultez [Génération d’applications à l’aide du service web et du .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md).  
  
## <a name="endpoints-and-methods"></a>Points de terminaison et méthodes  
 Le tableau suivant dresse la liste des points de terminaison du service Web Report Server et des catégories de méthodes fournies par le point de terminaison <xref:ReportService2010.ReportingService2010>. Pour plus d’informations sur les méthodes disponibles dans les autres points de terminaison, consultez [Informations techniques de référence &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md).  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Points de terminaison du service Web des serveurs de rapports](../../../reporting-services/report-server-web-service/methods/report-server-web-service-endpoints.md)|Décrit les points de terminaison de chaque service Web Report Server utilisés pour la gestion et l'exécution des rapports.|  
|[Méthodes de gestion des espaces de noms des serveurs de rapports](../../../reporting-services/report-server-web-service/methods/report-server-namespace-management-methods.md)|Décrit les méthodes que vous pouvez utiliser pour gérer la base de données du serveur de rapports. Plus précisément, ces méthodes vous permettent de gérer des dossiers et des ressources, et de définir des propriétés d'élément.|  
|[Méthodes d’autorisation](../../../reporting-services/report-server-web-service/methods/authorization-methods.md)|Décrit les méthodes que vous pouvez utiliser pour gérer des tâches, des rôles et des stratégies.|  
|[Sources de données et méthodes de connexion](../../../reporting-services/report-server-web-service/methods/data-sources-and-connection-methods.md)|Décrit les méthodes que vous pouvez utiliser pour définir et gérer la connexion aux sources de données et les informations d'identification des rapports.|  
|[Méthodes des paramètres des rapports](../../../reporting-services/report-server-web-service/methods/report-parameters-methods.md)|Décrit les méthodes que vous pouvez utiliser pour définir et extraire des paramètres destinés aux rapports.|  
|[Méthodes de modèles - service web des serveurs de rapports](../../../reporting-services/report-server-web-service/methods/model-methods-report-server-web-service.md)|Décrit les méthodes que vous pouvez utiliser pour gérer des modèles.|  
|[Méthodes de rendu et d'exécution](../../../reporting-services/report-server-web-service/methods/rendering-and-execution-methods.md)|Décrit les méthodes que vous pouvez utiliser pour gérer l'exécution, le rendu et la mise en cache des rapports.|  
|[Méthodes d'historique des rapports](../../../reporting-services/report-server-web-service/methods/report-history-methods.md)|Décrit les méthodes que vous pouvez utiliser pour créer des instantanés d'historique de rapport et les gérer.|  
|[Méthodes de planification](../../../reporting-services/report-server-web-service/methods/scheduling-methods.md)|Décrit les méthodes que vous pouvez utiliser pour créer et gérer des planifications partagées et des plans d'actualisation du cache, utilisées ensuite par le serveur de rapports.|  
|[Méthodes d'abonnement et de remise](../../../reporting-services/report-server-web-service/methods/subscription-and-delivery-methods.md)|Décrit les méthodes que vous pouvez utiliser pour créer des abonnements et générer des remises de rapport, puis gérer ces événements.|  
|[Méthodes relatives aux rapports liés](../../../reporting-services/report-server-web-service/methods/linked-reports-methods.md)|Décrit les méthodes que vous pouvez utiliser pour créer et gérer des rapports liés.|  
  
## <a name="see-also"></a> Voir aussi  
 [Accès à l’API SOAP](../../../reporting-services/report-server-web-service/accessing-the-soap-api.md)   
 [Création d’applications à l’aide du service web et du .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Service web Report Server](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [Informations techniques de référence &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md)  
  
  
