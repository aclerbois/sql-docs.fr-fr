---
title: Référence de la configuration web (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- web configuration file [Master Data Services]
ms.assetid: b8cc9a35-97ab-4fe0-ab4b-c07f13d9793a
caps.latest.revision: 6
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 0debbd16d8f250d5ee59ef0120a19485d2d5e289
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="web-configuration-reference-master-data-services"></a>Référence de la configuration Web (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] utilise un fichier Web.config pour contenir les paramètres de configuration qui permettent à Internet Information Services (IIS) d’héberger l’application web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] et le service web. Ce fichier Web.config se trouve dans le dossier WebApplication du chemin d'installation de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Pour plus d’informations sur le chemin et les autorisations, consultez [Autorisations d’accès aux dossiers et aux fichiers &#40;Master Data Services&#41;](../master-data-services/folder-and-file-permissions-master-data-services.md).  
  
## <a name="webconfig-elements"></a>Éléments Web.Config  
 Le fichier Web.config contient un élément [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] personnalisé, **\<masterDataServices>**, en plus des éléments de configuration IIS, .NET Framework, ASP.NET et WCF (Windows Communication Foundation) standard. Le tableau suivant décrit les éléments inclus dans le fichier Web.config.  
  
|Élément de configuration|Description|  
|---------------------------|-----------------|  
|**masterDataServices**|Élément personnalisé. Connecte le service Web [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] à une base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .|  
|**connectionStrings**|Élément ASP.NET. Pour plus d’informations, consultez [connectionStrings, élément (Schéma des paramètres ASP.NET)](http://go.microsoft.com/fwlink/?LinkId=178347) dans MSDN Library.|  
|**system.web**|Élément ASP.NET. Pour plus d’informations, consultez [system.web, élément (Schéma des paramètres ASP.NET)](http://go.microsoft.com/fwlink/?LinkId=178348) dans MSDN Library.|  
|**startup**|Élément .NET Framework. Pour plus d’informations, consultez [\<startup>, élément](http://go.microsoft.com/fwlink/?LinkId=178349) dans MSDN Library.|  
|**runtime**|Élément .NET Framework. Pour plus d’informations, consultez [\<runtime>, élément](http://go.microsoft.com/fwlink/?LinkId=178350) dans MSDN Library.|  
|**system.codedom**|Élément .NET Framework. Pour plus d’informations, consultez [\<system.codedom>, élément](http://go.microsoft.com/fwlink/?LinkId=178351) dans MSDN Library.|  
|**system.web.extensions**|Élément ASP.NET. Pour plus d’informations, consultez [system.web.extensions, élément (Schéma des paramètres ASP.NET)](http://go.microsoft.com/fwlink/?LinkId=178352) dans MSDN Library.|  
|**system.webServer**|Groupe de sections qui contient des éléments IIS. Pour plus d’informations, consultez [system.webServer, groupe de sections \[Schéma des paramètres IIS 7\]](http://go.microsoft.com/fwlink/?LinkId=178353) dans MSDN Library.|  
|**system.serviceModel**|Élément WCF. Pour plus d’informations, consultez [\<system.serviceModel>](http://go.microsoft.com/fwlink/?LinkId=178354) dans MSDN Library.|  
|**system.diagnostics**|Élément .NET Framework. Pour plus d’informations, consultez [\<system.diagnostics>, élément](http://go.microsoft.com/fwlink/?LinkId=178355) dans MSDN Library.|  
|**appSettings**|Élément ASP.NET. Pour plus d’informations, consultez [appSettings, élément (Schéma des paramètres généraux)](http://go.microsoft.com/fwlink/?LinkId=178356) dans MSDN Library.|  
  
## <a name="masterdataservices-element"></a>Élément masterDataServices  
 L’élément **\<masterDataServices>** est un élément personnalisé qui permet de connecter un service web [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] à une base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
### <a name="syntax"></a>Syntaxe  
  
```  
<masterDataServices>  
   <instance virtualPath="path" siteName="name" connectionName="name" serviceName="name" />  
</masterDataServices>  
```  
  
### <a name="elements-and-attributes"></a>Éléments et attributs  
  
|Élément|Description|  
|----------|-----------------|  
|**instance**|Élément enfant. Contient des attributs qui spécifient les informations concernant le service Web et la chaîne de connexion à la base de données.|  
|**virtualPath**|Attribut. Spécifie le chemin d'accès virtuel de l'application Web et du service Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . Correspond à l’attribut **path** de l’élément **\<application>** sous l’élément **\<site>** dans le fichier ApplicationHost.config IIS.|  
|**siteName**|Attribut. Spécifie le nom du site qui héberge l'application Web et le service Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . Correspond à l’attribut **name** de l’élément **\<site>** sous l’élément **\<sites>** dans le fichier ApplicationHost.config IIS.|  
|**connectionName**|Attribut. Spécifie le nom de la connexion à utiliser. Correspond à l’attribut **name** de l’élément **\<add>** sous l’élément **\<connectionStrings>** dans Web.config.|  
|**serviceName**|Attribut. Spécifie le nom du service Web. Correspond à l’attribut **name** de l’élément **\<service>** sous l’élément **\<services>** dans Web.config.|  
  
### <a name="example"></a> Exemple  
 L'exemple suivant présente un service nommé MDS1 sur le site Contoso et le chemin d'accès /MDS, utilisant une chaîne de connexion spécifiée par MDSDB.  
  
```  
<masterDataServices>  
   <instance virtualPath="/MDS" siteName="Contoso" connectionName="MDSDB" serviceName="MDS1" />  
</masterDataServices>  
```  
  
  
