---
title: Autorisations d’accès aux dossiers et aux fichiers (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- security [Master Data Services], file and folder
- folders [Master Data Services]
- files [Master Data Services]
ms.assetid: 6402d81d-7349-47b1-95ca-99b0c0f4f373
caps.latest.revision: 10
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: b4460ef0b2c620bbfe97a69d04f23e63dde04cea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="folder-and-file-permissions-master-data-services"></a>Autorisations d'accès aux dossiers et aux fichiers (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Lorsque vous installez [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], des dossiers et des fichiers sont installés dans le système de fichiers dans le chemin d’installation que vous spécifiez pour les fonctionnalités partagées [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Si vous utilisez le chemin d’installation par défaut pour les fonctionnalités partagées [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , le chemin d’installation de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] est *lecteur*:\Program Files\Microsoft SQL Server\130\Master Data Services. Vous pouvez modifier le chemin d’installation des fonctionnalités partagées, mais tenez compte des autorisations héritées du dossier parent et des autorisations définies explicitement pour [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
## <a name="inherited-permissions"></a>Autorisations héritées  
 Le dossier **Microsoft SQL Server** , le dossier **Master Data Services** et la plupart des sous-dossiers et des fichiers héritent les autorisations du dossier parent spécifié lors de l’exécution du programme d’installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Si vous choisissez l’emplacement d’installation par défaut, le dossier parent duquel les autorisations sont héritées est *lecteur*:\Program Files. Le tableau suivant décrit les autorisations par défaut du dossier **Program Files**.  
  
> [!NOTE]  
>  Si vous modifiez les autorisations par défaut du dossier **Program Files**, ou si vous choisissez un emplacement d’installation différent, les dossiers et les fichiers [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] héritent les autorisations de leur dossier parent et, par conséquent, les autorisations peuvent différer de celles décrites dans le tableau suivant.  
  
###### <a name="program-files-default-permissions"></a>Autorisations par défaut du dossier Program Files  
  
|Nom de groupe ou de compte|Autorisations|  
|---------------------------|-----------------|  
|CREATOR OWNER|Autorisations spéciales|  
|SYSTEM|Autorisations spéciales|  
|Administrateurs|Autorisations spéciales|  
|Utilisateurs|Lire & exécuter, répertorier le contenu des dossiers, lire|  
|TrustedInstaller|Répertorier le contenu des dossiers, autorisations spéciales|  
  
## <a name="explicit-permissions"></a>Autorisations explicites  
 Le dossier **MDSTempDir** et le fichier [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Web.config (dans le dossier **WebApplication** ) n’héritent pas des autorisations. Ils ont des autorisations définies explicitement lorsque vous installez [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], indépendamment du chemin d'installation que vous choisissez. Ne modifiez pas ces autorisations.  
  
###### <a name="mdstempdir-permissions"></a>Autorisations MDSTempDir  
  
|Nom de groupe ou de compte|Autorisations|  
|---------------------------|-----------------|  
|SYSTEM|Modifier, lire & exécuter, répertorier le contenu des dossiers, lire, écrire|  
|Administrateurs|Modifier, lire & exécuter, répertorier le contenu des dossiers, lire, écrire|  
|MDS_ServiceAccounts|Modifier, lire & exécuter, répertorier le contenu des dossiers, lire, écrire|  
  
###### <a name="webconfig-permissions"></a>Autorisations Web.config  
  
|Nom de groupe ou de compte|Autorisations|  
|---------------------------|-----------------|  
|SYSTEM|Contrôle total, modifier, lire & exécuter, lire, écrire|  
|Administrateurs|Contrôle total, modifier, lire & exécuter, lire, écrire|  
|MDS_ServiceAccounts|Lire & exécuter, lire|  
  
 Pour plus d’informations sur le contenu de le fichier [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Web.config, consultez [Référence de configuration web &#40;Master Data Services&#41;](../master-data-services/web-configuration-reference-master-data-services.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Installer Master Data Services](../master-data-services/install-windows/install-master-data-services.md)  
  
  
