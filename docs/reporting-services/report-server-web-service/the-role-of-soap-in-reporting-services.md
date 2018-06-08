---
title: Rôle de SOAP dans Reporting Services | Microsoft Docs
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
- Web service [Reporting Services], SOAP
- SOAP [Reporting Services], role in Reporting Services
- Report Server Web service, SOAP
- XML Web service [Reporting Services], SOAP
ms.assetid: f229c3ef-f2ca-448f-98f1-b8df350b9992
caps.latest.revision: 34
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5137b89878092328e0b809a988d1255e3dc13c30
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="the-role-of-soap-in-reporting-services"></a>Rôle de SOAP dans Reporting Services
  Le service Web Report Server utilise la messagerie SOAP (Simple Object Access Protocol) pour envoyer des commandes textuelles sur un réseau. Ces commandes prennent la forme de texte XML envoyé sur le Web à l'aide du protocole HTTP. En utilisant SOAP en tant que protocole de communication, le service Web Report Server permet aux applications et aux composants d'échanger des données avec le serveur de rapports à l'aide d'une infrastructure ouverte et largement reconnue. La norme SOAP est définie sur le site www.w3.org/TR/SOAP.  
  
 Toute application cliente peut jouer le rôle de client SOAP dans la mesure où elle prend en charge le protocole SOAP et où elle peut envoyer des demandes SOAP. Le Gestionnaire de rapports est l'un de ces clients SOAP. Il fournit une interface à la base de données du serveur de rapports dans laquelle tous les rapports et leur contenu associé sont stockés. Les utilisateurs finals peuvent utiliser l'application pour parcourir et gérer des rapports et des dossiers dans l'espace de noms du serveur de rapports. Le Gestionnaire de rapports repose sur l'infrastructure du service Web Report Server.  
  
 Un serveur de rapports joue le rôle de serveur SOAP, un service prenant en charge le protocole SOAP qui peut accepter des demandes en provenance de clients SOAP et créer des réponses appropriées. Le serveur gère les demandes et renvoie des réponses encodées au client.  
  
 Les messages SOAP dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] prennent de nombreuses formes différentes, selon le type de demande faite par le client. L'exemple suivant représente une requête simple d'un client SOAP consistant à supprimer un élément de la base de données du serveur de rapports.  
  
```  
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <soap:Body>  
        <DeleteItem xmlns="http://www.microsoft.com/sql/ReportingServer">  
            <item>/Samples/Report1</item>  
        </DeleteItem>  
    </soap:Body>  
</soap:Envelope>  
```  
  
 Le protocole SOAP exige que les messages soient placés dans un élément **Envelope**, avec l’essentiel du message à l’intérieur d’un élément **Body**. Dans cet exemple, le corps contient un appel de la méthode <xref:ReportService2010.ReportingService2010.DeleteItem%2A>, qui prend un paramètre de chaîne représentant le chemin d'accès de l'élément à supprimer. Vous pouvez créer une classe proxy de client [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] qui encapsule toutes les opérations SOAP dans des méthodes. La méthode [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[csprcs](../../includes/csprcs-md.md)] suivante représente l’exemple SOAP donné précédemment.  
  
```  
public void DeleteItem(string item);  
```  
  
 La réponse du serveur peut ressembler à ce qui suit :  
  
```  
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <soap:Body>  
        <DeleteItemResponse xmlns="http://www.microsoft.com/sql/ReportingServer" />  
    </soap:Body>  
</soap:Envelope>  
```  
  
 La méthode <xref:ReportService2010.ReportingService2010.DeleteItem%2A> n'a aucune valeur de retour, donc une réponse vide est retournée.  
  
## <a name="see-also"></a> Voir aussi  
 [Accès à l’API SOAP](../../reporting-services/report-server-web-service/accessing-the-soap-api.md)   
 [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896)   
 [Serveur de rapports Reporting Services](../../reporting-services/report-server-sharepoint/reporting-services-report-server.md)   
 [Service Web des serveurs de rapports](../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
  
