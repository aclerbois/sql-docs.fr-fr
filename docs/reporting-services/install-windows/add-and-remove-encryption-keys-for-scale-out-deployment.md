---
title: Ajouter et supprimer des clés de chiffrement pour un déploiement évolutif | Microsoft Docs
ms.custom: ''
ms.date: 05/31/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- encryption keys [Reporting Services]
- deleting encryption keys
- removing encryption keys
- adding encryption keys
- rskeymgmt utility
- scale-out deployments [Reporting Services]
ms.assetid: 2da86fb3-4b4d-407f-9825-74dcc42486f5
caps.latest.revision: 10
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 9c2da984572976c511a2d373b7f2d50a923a96ec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-and-remove-encryption-keys-for-scale-out-deployment"></a>Ajouter et supprimer des clés de chiffrement pour un déploiement évolutif
  Vous pouvez exécuter [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans un modèle de déploiement avec montée en puissance parallèle si vous configurez plusieurs serveurs de rapports pour qu'ils utilisent une base de données de serveur de rapports partagée. L'appartenance d'un serveur de rapports au déploiement évolutif dépend si ce serveur a déposé ou non une clé de chiffrement dans la base de données de serveurs de rapports. Vous pouvez contrôler un déploiement évolutif en ajoutant et en supprimant des clés de chiffrement pour des instances de serveurs de rapports spécifiques. Si vous supprimez des nœuds du déploiement, vous pouvez les supprimer dans n'importe quel ordre. Si vous ajoutez des nœuds à un déploiement, vous devez joindre toutes les nouvelles instances à partir d'un serveur de rapports faisant déjà partie du déploiement.  
  
## <a name="using-the-reporting-services-configuration-tool-to-configure-scale-out-deployment"></a>Utilisation de l'outil de configuration de Reporting Services pour configurer un déploiement évolutif  
 Le moyen le plus simple de configurer un déploiement évolutif consiste à utiliser l'outil de configuration de Reporting Services. Pour plus d’informations et pour obtenir des instructions détaillées, consultez [Configurer un déploiement avec montée en puissance parallèle de serveurs de rapports en mode natif &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
## <a name="using-rskeymgmt-to-configure-scale-out-deployment"></a>Utilisation de l'outil Rskeymgmt pour configurer un déploiement évolutif  
 Servez-vous de l’utilitaire **rskeymgmt** pour initialiser une instance de serveur de rapports afin qu’elle utilise une base de données de serveur de rapports partagée. L'ajout d'un serveur de rapports à un déploiement évolutif requiert l'initialisation du serveur de rapports. L'initialisation nécessite des autorisations d'administrateur. Vous devez disposer d'informations d'identification d'administrateur pour l'ordinateur distant qui héberge le serveur de rapports à intégrer au déploiement.  
  
### <a name="how-to-join-a-report-server-to-a-scale-out-deployment-rskeymgmt"></a>Procédure d'intégration d'un serveur de rapports à un déploiement évolutif (rskeymgmt)  
  
1.  Exécutez **rskeymgmt.exe** localement sur l’ordinateur hébergeant un serveur de rapports déjà membre du déploiement par montée en puissance parallèle de serveurs de rapports.  
  
2.  Utilisez l’argument **-j** pour rattacher un serveur de rapports à la base de données de serveur de rapports. Utilisez les arguments **-m** et **-n** pour spécifier l’instance de serveur de rapports distante à ajouter au déploiement. Utilisez les arguments **-u** et **-v** pour spécifier un compte d’administrateur sur l’ordinateur distant. Si vous créez un déploiement évolutif en utilisant plusieurs instances de serveur de rapports sur le même ordinateur, la syntaxe à utiliser est légèrement différente. Pour plus d’informations sur la syntaxe à utiliser, consultez [Utilitaire rskeymgmt &#40;SSRS&#41;](../../reporting-services/tools/rskeymgmt-utility-ssrs.md).  
  
     L'exemple suivant illustre les arguments que vous devez spécifier si vous ajoutez un serveur de rapports distant à un déploiement évolutif (vous pouvez omettre les informations d'identification si vous avez des autorisations d'administrateur sur l'ordinateur distant) :  
  
    ```  
    rskeymgmt -j -m <remotecomputer> -n <namedreportserverinstance> -u <administratoraccount> -v <administratorpassword>  
    ```
3. Redémarrez le service Windows Reporting Services.
  
### <a name="how-to-remove-a-report-server-from-a-scale-out-deployment-rskeymgmt"></a>Procédure de suppression d'un serveur de rapports intégré à un déploiement évolutif (rskeymgmt)  
  
1.  Ouvrez le fichier rsreportserver.config du serveur de rapports que vous souhaitez supprimer et recherchez l'ID d'installation. Par défaut, ce fichier se trouve dans Program Files\Microsoft SQL Server\MSSQL.*n*\Reporting Services\ReportServer).  
  
     Si vous installez une instance unique, il n'y aura qu'un seul fichier rsreportserver.config sur l'ordinateur. Si plusieurs instances de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sont installées, utilisez la page État du serveur de l'outil de configuration de Reporting Services pour rechercher l'identificateur d'instance (par exemple, MSSQL.2) pour le serveur de rapports que vous souhaitez supprimer. Le nom du dossier où sont stockés les fichiers programmes de l'instance du serveur de rapports est basé sur l'identificateur de l'instance (par exemple, Program Files\Microsoft SQL Server\MSSQL.2).  
  
2.  Exécutez **rskeymgmt.exe**. Vous pouvez exécuter cet utilitaire sur n'importe quel serveur de rapports intégré à un déploiement évolutif de serveurs de rapports.  
  
3.  Utilisez l’argument **-r** pour séparer l’instance de serveur de rapports du déploiement évolutif. L'exemple suivant illustre les arguments que vous devez spécifier :  
  
    ```  
    rskeymgmt -r <installation ID>  
    ```  
4. Redémarrez le service Windows Reporting Services.
  
 Ces étapes suppriment le serveur de rapports d'un déploiement avec montée en puissance parallèle, mais ne désinstallent pas l'instance [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur le serveur de rapports. Après avoir supprimé le serveur de rapports du déploiement avec montée en puissance parallèle, vous pouvez désinstaller [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] du serveur si vous n'avez plus besoin de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur ce serveur. Pour plus d’informations, consultez [Désinstaller une instance existante de SQL Server &#40;programme d’installation&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md) dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer et gérer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)   
 [Initialiser un serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md)  
  
  
