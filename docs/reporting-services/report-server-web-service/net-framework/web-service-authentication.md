---
title: Authentification du service web | Microsoft Docs
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
- Web service [Reporting Services], authentication
- XML Web service [Reporting Services], authentication
- Report Server Web service, authentication
ms.assetid: 852b4947-a090-4e54-8555-5a503945ceab
caps.latest.revision: 33
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 70fd7737c23b84d9bda87c93d1781bb12e35a299
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="web-service-authentication"></a>Authentification du service web
  Vous pouvez utiliser l'authentification Windows ou l'authentification de base pour authentifier les appels effectués auprès du service Web Report Server. Tout client qui effectue des demandes SOAP au serveur de rapports doit implémenter la partie cliente de l'un des protocoles d'authentification pris en charge. Si vous utilisez [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], vous pouvez utiliser les classes HTTP du code managé pour implémenter l’authentification. L'utilisation de ces API simplifie l'envoi des informations d'authentification et des demandes SOAP.  
  
 Si vous n'avez pas les informations d'identification appropriées avant d'effectuer un appel auprès du service Web Report Server, l'appel échoue. Au moment de l’exécution, vous pouvez passer les informations d’identification au service web en définissant la propriété **Credentials** de l’objet côté client qui représente le service web avant d’appeler ses méthodes.  
  
 Les sections suivantes contiennent un exemple de code qui envoie des informations d'identification à l'aide de [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)].  
  
## <a name="windows-authentication"></a>Authentification Windows  
 Le code suivant passe des informations d'identification Windows au service Web.  
  
```vb  
Dim rs As New ReportingService()  
rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
```  
  
```csharp  
ReportingService rs = new ReportingService();  
rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
```  
  
## <a name="basic-authentication"></a>Authentification de base  
 Le code suivant passe des informations d'identification de base au service Web.  
  
```vb  
Dim rs As New ReportingService()  
rs.Credentials = New System.Net.NetworkCredential("username", "password", "domain")  
```  
  
```csharp  
ReportingService service = new ReportingService();  
service.Credentials = new System.Net.NetworkCredential("username", "password", "domain");  
```  
  
 Les informations d'identification doivent être définies avant d'appeler chacune des méthodes du service Web Report Server. Si vous ne définissez pas ces informations, vous recevez le code d'erreur HTTP 401 : Accès refusé. Vous devez authentifier le service avant de l’utiliser, mais après avoir défini les informations d’identification, il n’est pas nécessaire de les définir à nouveau si vous continuez à utiliser la même variable de service (telle que *rs*).  
  
## <a name="custom-authentication"></a>Authentification personnalisée  
 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] inclut une API de programmation qui permet aux développeurs de concevoir et de développer des extensions d'authentification personnalisées appelées extensions de sécurité. Pour plus d’informations, consultez [Implémentation d’une extension de sécurité](../../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Création d’applications à l’aide du service web et du .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Service Web des serveurs de rapports](../../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
  
