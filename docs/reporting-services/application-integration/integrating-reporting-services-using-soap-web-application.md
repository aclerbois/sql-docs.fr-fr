---
title: Utilisation de l’API SOAP dans une application web | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- SOAP [Reporting Services], Web applications
- impersonation [Reporting Services]
- user impersonation [Reporting Services]
- report servers [Reporting Services], SOAP
- Web applications [Reporting Services]
ms.assetid: e8ca4455-0dc3-4741-8872-3636114938ad
caps.latest.revision: 34
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 601131488db47a48a6151adc38572c1535272ca1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integrating-reporting-services-using-soap---web-application"></a>Intégration de Reporting Services à l’aide de SOAP - Application web
  Vous pouvez accéder aux fonctionnalités complètes du serveur de rapports via l'API SOAP de Reporting Services. Étant donné qu'il s'agit d'un service Web, l'API SOAP est facilement accessible afin de fournir des fonctionnalités de création de rapports d'entreprise à vos applications de gestion personnalisées. Vous accédez au service Web Report Server à partir d'une application Web à peu près de la même manière que vous accédez à l'API SOAP à partir d'une application [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. À l’aide du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], vous pouvez générer une classe proxy qui expose les propriétés et méthodes du service web Report Server et vous permet d’utiliser une infrastructure et des outils familiers pour générer des applications de gestion basées sur la technologie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Les fonctionnalités de gestion de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sont tout aussi faciles d'accès à partir d'une application Web qu'à partir d'une application Windows. À partir d'une application Web, vous pouvez ajouter et supprimer des éléments dans la base de données du serveur de rapports, définir la sécurité des éléments, modifier les éléments de la base de données du serveur de rapports, gérer la planification et la remise, etc.  
  
## <a name="enabling-impersonation"></a>Activation de l'emprunt d'identité  
 La première étape de la configuration de votre application Web consiste à activer l'emprunt d'identité à partir du client de service Web. Avec l'emprunt d'identité, les applications [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] peuvent s'exécuter avec l'identité du client au nom duquel elles s'exécutent. [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] compte sur les services Internet (IIS) de [!INCLUDE[msCoName](../../includes/msconame-md.md)] pour authentifier l'utilisateur et passer soit un jeton authentifié à l'application [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)], soit un jeton non authentifié en cas d'incapacité à authentifier l'utilisateur. Dans les deux cas, l'application [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] emprunte l'identité de n'importe quel jeton reçu si l'emprunt d'identité est activé. Vous pouvez activer l'emprunt d'identité sur le client, en modifiant le fichier Web.config de l'application cliente comme suit :  
  
```  
<!-- Web.config file. -->  
<identity impersonate="true"/>  
```  
  
> [!NOTE]  
>  L'emprunt d'identité est désactivé par défaut.  
  
 Pour plus d’informations sur l’emprunt d’identité [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)], consultez la documentation du SDK [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
## <a name="managing-the-report-server-using-soap-api"></a>Gestion du serveur de rapports à l'aide de l'API SOAP  
 Vous pouvez également utiliser votre application Web pour gérer un serveur de rapports et son contenu. Le Gestionnaire de rapports, inclus dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], est un exemple d'application Web complètement créée à l'aide de [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] et de l'API SOAP de Reporting Services. Vous pouvez ajouter les fonctionnalités de gestion de rapports du Gestionnaire de rapports à vos applications Web personnalisées. Par exemple, vous pouvez souhaiter retourner la liste des rapports disponibles dans la base de données du serveur de rapports afin de les afficher dans un contrôle [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] **Listbox** pour permettre à vos utilisateurs d’en choisir un. Le code suivant permet de se connecter à la base de données du serveur de rapports et de retourner la liste des éléments contenus dans la base de données du serveur de rapports. Les rapports disponibles sont alors ajoutés à un contrôle ListBox, lequel affiche le chemin d'accès de chaque rapport.  
  
```vb  
Private Sub Page_Load(sender As Object, e As System.EventArgs)  
   ' Create a Web service proxy object and set credentials  
   Dim rs As New ReportingService2005()  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
  
   ' Return a list of catalog items in the report server database  
   Dim items As CatalogItem() = rs.ListChildren("/", True)  
  
   ' For each report, display the path of the report in a Listbox  
   Dim ci As CatalogItem  
   For Each ci In  items  
      If ci.Type = ItemTypeEnum.Report Then  
         catalogListBox.Items.Add(ci.Path)  
      End If  
   Next ci  
End Sub ' Page_Load   
```  
  
```csharp  
private void Page_Load(object sender, System.EventArgs e)  
{  
   // Create a Web service proxy object and set credentials  
   ReportingService2005 rs = new ReportingService2005();  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
  
   // Return a list of catalog items in the report server database  
   CatalogItem[] items = rs.ListChildren("/", true);  
  
   // For each report, display the path of the report in a Listbox  
   foreach(CatalogItem ci in items)  
   {  
      if (ci.Type == ItemTypeEnum.Report)  
         catalogListBox.Items.Add(ci.Path);  
   }  
}  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Création d’applications à l’aide du service web et du .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Intégration de Reporting Services dans des applications](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896)   
 [Utilisation de l’API SOAP dans une application Windows](../../reporting-services/application-integration/integrating-reporting-services-using-soap-windows-application.md)  
  
  
