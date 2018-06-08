---
title: Gestion des exceptions dans Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: report-server-web-service-net-framework-exception-handling
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- SOAP [Reporting Services], exceptions
- .NET Framework [Reporting Services]
- exceptions [Reporting Services], about exception handling
- SoapException object
ms.assetid: 1a443432-2db5-48c5-bc29-433b4688082f
caps.latest.revision: 31
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 75bc7d7f057c9207a0f70525acf61072b82f3469
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="handling-exceptions-in-reporting-services"></a>Gestion des exceptions dans Reporting Services
  Lorsqu'une demande de client de l'API SOAP Reporting Services ne peut pas être exécutée, le serveur de rapports retourne une erreur au lieu des résultats attendus de l'appel. Quand un appel ne peut pas être passé, une erreur pour le service web Report Server est retournée sous la forme d’élément XML **Fault** SOAP. L’élément descriptif principal de l’erreur est l’élément **detail**, qui inclut toutes les informations sur l’erreur fournies par le serveur de rapports, ainsi que d’éventuelles informations supplémentaires sur l’erreur du service web. L’information essentielle de l’élément **detail** est le code d’erreur du serveur de rapports. En fonction du message et du code d'erreur, vous pouvez déterminer l'action appropriée suivante à prendre dans vos applications. Pour plus d'informations sur les erreurs SOAP, consultez le site Web du W3C (World Wide Consortium) à l'adresse http://www.w3.org/TR/SOAP.  
  
## <a name="soap-faults-and-the-net-framework"></a>Erreurs SOAP et le .NET Framework  
 Dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], si une erreur se produit dans une requête de client au service web, le serveur de rapports communique l’erreur au code client qui appelle le service web en levant un objet **SoapException**. L’objet **SoapException** inclut dans un wrapper les informations contenues dans une erreur SOAP. La propriété **Detail** de **SoapException** est mappée à l’élément **detail** dans l’erreur SOAP. Les applications doivent intercepter l’objet **SoapException** avec un bloc try/catch et utiliser la propriété **Detail** de **SoapException** pour prendre la mesure appropriée. Pour plus d’informations sur la classe **SoapException** et la propriété **Detail** dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [SoapException, classe Reporting Services](../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/reporting-services-soapexception-class.md). Pour plus d’informations sur la classe **SoapException**, consultez la documentation du SDK [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
## <a name="see-also"></a> Voir aussi  
 [Detail, propriété](../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/detail-property.md)   
 [Présentation de la gestion des exceptions dans Reporting Services](../../reporting-services/report-server-web-service-net-framework-exception-handling/introducing-exception-handling-in-reporting-services.md)   
 [Classe SoapException Reporting Services](../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/reporting-services-soapexception-class.md)  
  
  
