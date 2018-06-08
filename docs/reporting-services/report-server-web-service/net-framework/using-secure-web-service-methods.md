---
title: Utilisation des méthodes de service web sécurisées | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
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
- SOAP [Reporting Services], secure connections
- Web service [Reporting Services], SOAP
- Report Server Web service, SOAP
- XML Web service [Reporting Services], SOAP
ms.assetid: 87329299-c2ea-4517-9148-d855726768a9
caps.latest.revision: 36
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 0595f9df191bb1a4ad53c6934d85c701a80520bd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-secure-web-service-methods"></a>Utilisation des méthodes de service Web sécurisées
  Certaines méthodes de service Web Report Server peuvent requérir une connexion sécurisée lorsque vous les appelez. Ces méthodes qui exigent une connexion sécurisée sont déterminées par le paramètre **SecureConnectionLevel** dans le fichier RSReportServer.config. La valeur du paramètre est une valeur entière avec une plage valide supérieure ou égale à 0. Le tableau suivant décrit ces valeurs.  
  
|Level|Description|  
|-----------|-----------------|  
|**0**|Non sécurisé. Les appels effectués auprès de l'API SOAP de Reporting Services ne requièrent pas de connexion sécurisée.|  
|Supérieur à **0**|Sécurisé. Les appels effectués auprès de l'API SOAP de Reporting Services requièrent une connexion sécurisée.|  
  
 Vous pouvez utiliser la méthode <xref:ReportExecution2005.ReportExecutionService.ListSecureMethods%2A> du service Web pour renvoyer une liste des méthodes de service Web qui requièrent une connexion sécurisée d'après la configuration actuelle du serveur de rapports. Dans un scénario SSL, vous devez évaluer la liste des méthodes qui sont renvoyées par <xref:ReportExecution2005.ReportExecutionService.ListSecureMethods%2A> et remplacer le nom de méthode de l'URI de service Web par "https" ou "http" selon la méthode appelée.  
  
## <a name="see-also"></a> Voir aussi  
 [Création d’applications à l’aide du service web et du .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Service Web des serveurs de rapports](../../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
  
