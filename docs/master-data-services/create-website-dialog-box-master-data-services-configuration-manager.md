---
title: Boîte de dialogue Créer un site web (Gestionnaire de configuration Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.mds.configmanager.createsite.f1
ms.assetid: 179c9c1e-3b06-421b-b71b-1cb64d104f5e
caps.latest.revision: 10
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 940d6c966eb92ab9070f654298e7471fad2af791
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-website-dialog-box-master-data-services-configuration-manager"></a>Boîte de dialogue Créer un site Web (Gestionnaire de configuration Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Utilisez la boîte de dialogue **Créer un site web** pour créer un site web sur l’ordinateur local. Lorsque vous créez un site web dans [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], le site est ajouté à Internet Information Services (IIS) sur l’ordinateur local avec une application racine configurée comme application web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . Un pool d'applications est également créé et l'application web est placée dans ce pool d'applications.  
  
## <a name="web-site"></a>Site web  
  
|Nom du contrôle|Description|  
|------------------|-----------------|  
|**Nom du site web**|Tapez un nom pour le site Web ou utilisez le nom par défaut. Ce nom est un nom convivial utilisé uniquement pour identifier le site dans IIS. Il n'est pas utilisé pour accéder au site à partir d'un navigateur Web.<br /><br /> Le nom doit être unique parmi tous les sites dans IIS sur l'ordinateur local.|  
|**Protocole**|Affiche le protocole **http**. Utilisez le protocole HTTP lorsque vous n'exigez pas que les communications entre le client et le serveur se déroulent sur un canal chiffré.<br /><br /> **Remarque**: Vous ne pouvez pas créer de site HTTPS dans [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]. HTTPS est le protocole HTTP qui utilise SSL (Secure Sockets Layer) et il est utile lors de l'échange de données confidentielles ou personnelles, ou lorsque vous souhaitez que les utilisateurs confirment l'identité du serveur avant de transmettre des informations personnelles. Si vous avez besoin de transférer des informations entre le serveur et un client sur un canal chiffré, vous devez utiliser un outil IIS, tel que le Gestionnaire des services IIS, pour configurer le site avec une liaison HTTPS et associer la liaison du site Web avec un certificat de serveur ; c'est indispensable pour pouvoir ouvrir correctement le Site Web dans un navigateur Web. Pour plus d’informations sur les certificats de serveur, consultez [Configuration des certificats de serveur dans IIS 7.0](http://go.microsoft.com/fwlink/?LinkId=163220) sur [!INCLUDE[msCoName](../includes/msconame-md.md)] TechNet.|  
|**Adresse IP**|Sélectionnez une adresse IP que les utilisateurs peuvent utiliser pour accéder au site. Par défaut, la valeur **Non assigné** est sélectionnée. À moins que vous n'ayez une raison d'utiliser une adresse IPv4 ou IPv6 spécifique, utilisez la valeur par défaut.<br /><br /> Avec la valeur **Non assigné**, ce site répond aux demandes pour toutes les adresses IP sur le port et le nom d’hôte facultatif que vous spécifiez. Si un autre site sur le serveur a une liaison sur le même port, mais avec une adresse IP spécifique, ce site reçoit des requêtes HTTP sur ces port et adresse IP spécifiques, et le site dont l’adresse IP a la valeur **Non assigné** reçoit toutes les autres requêtes HTTP sur ce port et les autres adresses IP.|  
|**Port**|Tapez le port pour les demandes adressées à ce site Web. Si vous sélectionnez le protocole HTTP, le port par défaut est 80. Si vous spécifiez un port différent des ports par défaut, les clients doivent spécifier le numéro de port pour se connecter au site Web.<br /><br /> **Remarque**: Le **site web par défaut** dans IIS est configuré pour utiliser le protocole HTTP sur le port 80 avec toutes les adresses IP non assignées. Si vous essayez de créer le site Web dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] avec les informations de liaison par défaut, vous recevez une erreur qui indique qu'une liaison en double existe. Vous devez modifier les informations de liaison pour le site dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]ou pour le site Web par défaut à l'aide d'un outil IIS, tel que le Gestionnaire des services IIS. Vous pouvez aussi spécifier un en-tête d'hôte pour permettre à IIS d'identifier le site Web de façon unique. Assurez-vous de configurer votre pare-feu pour accepter le trafic sur le port que vous spécifiez.|  
|**En-tête de l'hôte**|Valeur facultative. Tapez un nom d'en-tête de l'hôte. Utilisez-le lorsque vous souhaitez affecter un nom d'hôte, également appelé nom de domaine, à un ordinateur qui utilise un port ou une adresse IP unique. Lorsque vous spécifiez un nom d'hôte, les clients doivent utiliser ce nom au lieu de l'adresse IP pour accéder au site Web. Lorsque vous configurez un nom d'hôte, vous ne pouvez pas ouvrir le site dans un navigateur Web tant que votre serveur DNS n'a pas d'entrée pour ce nom d'hôte.<br /><br /> Par exemple, si vous souhaitez que les utilisateurs accèdent à votre site à l’adresse `http://www.contoso.com/`, vous devez spécifier www.contoso.com comme nom d’hôte et le serveur DNS doit avoir une entrée associée à ce nom.<br /><br /> Si votre site web est disponible sur un intranet, vous n’avez pas à spécifier un nom d’hôte si les utilisateurs tapent le nom du serveur dans un navigateur, par exemple `http://server_name`. Toutefois, si le serveur DNS dans votre environnement est configuré pour stocker d'autres noms pour ce serveur Web, vous pouvez créer une liaison séparée pour chaque nom d'hôte afin que les utilisateurs puissent utiliser les autres noms stockés par le serveur DNS. Si vous devez configurer plusieurs noms d'hôtes pour votre site, utilisez un outil IIS, tel que le Gestionnaire des services IIS, pour ajouter des liaisons de site supplémentaires.|  
  
## <a name="application-pool"></a>Pool d'applications  
  
|Nom du contrôle|Description|  
|------------------|-----------------|  
|**Nom**|Tapez un nom convivial unique pour un nouveau pool d'applications ou utilisez le nom par défaut fourni. L’application web racine pour ce site web s’exécute dans ce pool d’applications.<br /><br /> Les pools d'applications fournissent des limites qui empêchent les applications dans un pool d'applications d'affecter les applications dans un autre pool d'applications.|  
|**User name**|Tapez un domaine et un nom d'utilisateur à partir d'Active Directory. Ce compte est l'identité du pool d'applications dans lequel s'exécute l'application Web.<br /><br /> Ce compte est ajouté au rôle de base de données mds_exec dans la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] pour l'accès à la base de données. Pour plus d’informations, consultez [Connexions, utilisateurs et rôles de base de données &#40;Master Data Services&#41;](../master-data-services/database-logins-users-and-roles-master-data-services.md). Il est également ajouté à un groupe Windows [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], **MDS_ServiceAccounts**, qui est autorisé à accéder au répertoire de compilation temporaire, **MDSTempDir**, dans le système de fichiers. Pour plus d’informations, consultez [Autorisations d’accès aux dossiers et aux fichiers &#40;Master Data Services&#41;](../master-data-services/folder-and-file-permissions-master-data-services.md).|  
|**Mot de passe**|Tapez le mot de passe du compte d'utilisateur spécifié.|  
|**Confirmer le mot de passe**|Retapez le mot de passe du compte d'utilisateur spécifié. Les champs **Mot de passe** et **Confirmer le mot de passe** doivent contenir le même mot de passe.|  
  
## <a name="see-also"></a> Voir aussi  
 [Page Configuration web &#40;Gestionnaire de configuration de Master Data Services&#41;](../master-data-services/web-configuration-page-master-data-services-configuration-manager.md)   
[Installation et configuration de Master Data Services](../master-data-services/master-data-services-installation-and-configuration.md) [Configuration requise pour l’application web &#40;Master Data Services&#41;](../master-data-services/install-windows/web-application-requirements-master-data-services.md)   
 [Créer une application web Master Data Manager &#40;Master Data Services&#41;](../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md)  
  
  
