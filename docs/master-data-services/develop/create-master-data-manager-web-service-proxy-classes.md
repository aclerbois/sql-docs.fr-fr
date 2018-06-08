---
title: Créer des classes proxy de service web Master Data Manager | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: mds
ms.component: develop
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
ms.assetid: 8bdab026-a0c0-41f3-9d36-f3919c23247f
caps.latest.revision: 8
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: aa01beb7c14c342cb40843affd76a06163184bf3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-master-data-manager-web-service-proxy-classes"></a>Créer des classes proxy de service Web Master Data Manager

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Le service Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] vous permet de programmer les fonctionnalités de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] à partir de n'importe quel ordinateur pouvant accéder à votre site Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]. Avant de commencer à écrire du code pour accéder au service Web, vous devez générer des classes proxy. La classe proxy principale que vous utilisez pour effectuer des opérations de service Web est la classe <xref:Microsoft.MasterDataServices.ServiceClient>, qui implémente l'interface <xref:Microsoft.MasterDataServices.IService>.  
  
## <a name="enable-web-service-metadata-publishing"></a>Activer la publication de métadonnées de service Web  
 Avant de pouvoir générer des classes proxy, vous devez activer la publication de métadonnées de service Web. Pour cela, effectuez les étapes suivantes :  
  
1.  Ouvrez le fichier Web.config [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] dans un éditeur de texte. Ce fichier se trouve dans le dossier WebApplication du chemin d'installation de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)].  
  
2.  Rechercher la section **mdsWsHttpBehavior** sous **\<serviceBehaviors>**. Pour l’élément **\<serviceMetadata>**, définissez **httpGetEnabled** sur **true**.  
  
    > [!NOTE]  
    >  Si vous souhaitez activer des services web via SSL (Secure Sockets Layer), définissez **httpsGetEnabled** sur **true** dans la section **mdsWsHttpBehavior** du fichier web.config. Vous devez également changer **mdsWsHTTPBinding** afin qu’il soit configuré pour SSL et mettre en commentaire la section non-SSL.  
  
3.  Enregistrez les modifications apportées au fichier.  
  
4.  Testez les métadonnées de la publication en naviguant jusqu’à l’URL du service, par exemple : `http://yourserver/MDS/service/service.svc`. Si la publication de métadonnées est activée, une page s'affiche, commençant par :   
    « Vous avez créé un service. »  
  
## <a name="creating-proxy-classes-by-using-visual-studio"></a>Création de classes proxy à l'aide de Visual Studio  
 Si vous avez installé Visual Studio 2010, la méthode la plus simple de générer des classes proxy est d’ajouter une **Référence de service** à votre projet. L'adresse de la référence de service est l'URL de l'application Web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], suivie de /service/service.svc. Par exemple : `http://yourserver/MDS/service/service.svc`. Pour plus d’informations, consultez [Guide pratique pour ajouter, mettre à jour ou supprimer une référence de service](http://go.microsoft.com/fwlink/?LinkId=221167).  
  
## <a name="creating-proxy-classes-by-using-svcutilexe"></a>Création de classes proxy à l'aide de Svcutil.exe  
 Vous devez avoir installé [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] ou le kit [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows SDK pour avoir Svcutil.exe sur votre ordinateur. Si vous utilisez [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], vous devez utiliser l'invite de commandes [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] pour exécuter la commande. Pour plus d’informations, consultez [Outil Service Model Metadata Tool (Svcutil.exe)](http://go.microsoft.com/fwlink/?LinkId=165027) et [Génération d’un client WCF à partir de métadonnées de service](http://go.microsoft.com/fwlink/?LinkId=164821).  
  
 Pour créer un jeu de classes proxy C# à l'aide de Svcutil.exe, utilisez une commande telle que :  
  
```  
svcutil.exe http://<server_name:port>/<virtual_path>/Service/Service.svc   
/out:<proxy_name>.cs /messageContract /tcv:Version35   
/noconfig /ct:System.Collections.ObjectModel.Collection`1   
/namespace:*,Microsoft.MasterDataServices  
```  
  
 Où :  
  
-   *servername*:*port* désignent le nom d’ordinateur et le numéro de port de l’ordinateur qui héberge [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].  
  
-   *virtual_path* désigne le chemin virtuel de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] dans IIS (Internet Information Services).  
  
-   *proxy_name* désigne le nom du fichier proxy généré.  
  
## <a name="see-also"></a> Voir aussi  
 [Opérations de service web par catégorie &#40;Master Data Services&#41;](../../master-data-services/develop/categorized-web-service-operations-master-data-services.md)  
  
  
