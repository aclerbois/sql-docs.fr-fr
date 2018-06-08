---
title: Configurer des connexions SSL sur un serveur de rapports en mode natif | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: security
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Secure Sockets Layer (SSL)
ms.assetid: 212f2042-456a-4c0a-8d76-480b18f02431
caps.latest.revision: 34
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 0eead8d73153621430037141e0a509adac4cd1fa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-ssl-connections-on-a-native-mode-report-server"></a>Configurer des connexions SSL sur un serveur de rapports en mode natif
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Le mode natif utilise le service HTTP SSL (Secure Sockets Layer) pour définir des connexions chiffrées à un serveur de rapports. Si le fichier de certificat (.cer) est installé dans un magasin de certificats local sur le serveur de rapports, vous pouvez lier le certificat à une réservation d’URL [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour prendre en charge des connexions de serveur de rapports sur un canal chiffré.  
  
> [!TIP]  
>  Pour plus d'informations, consultez la documentation SharePoint si vous utilisez le mode [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint. Par exemple, [How to enable SSL on a SharePoint 2010 web application (Procédure pour activer SSL sur une application Web SharePoint 2010) (http://blogs.msdn.com/b/sowmyancs/archive/2010/02/12/how-to-enable-ssl-on-a-sharepoint-web-application.aspx)](http://blogs.msdn.com/b/sowmyancs/archive/2010/02/12/how-to-enable-ssl-on-a-sharepoint-web-application.aspx).  
  
 Comme Internet Information Services utilise aussi HTTP SSL, cela entraîne des problèmes d’interopérabilité significatifs que vous devez connaître si vous exécutez IIS et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur le même ordinateur. Veillez à consulter la section Problèmes d'interopérabilité avec IIS pour des informations sur la manière de traiter ces problèmes.  
  
## <a name="server-certificate-requirements"></a>Conditions requises des certificats de serveur  
 Vous devez installer un certificat de serveur sur l'ordinateur (les certificats clients ne sont pas pris en charge). Les services de rapports n'offrent pas de fonctionnalité pour demander, générer, télécharger ou installer un certificat. [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] fournit un composant logiciel enfichable de certificat que vous pouvez utiliser pour demander un certificat à une autorité de certification approuvée.  
  
 Vous pouvez créer un certificat localement à des fins de tests. Si vous employez l’utilitaire **MakeCert** et l’exemple de commande comme modèle, veillez à spécifier le nom de votre serveur comme hôte et à supprimer tous les sauts de ligne avant d’exécuter la commande. Si vous exécutez la commande dans une fenêtre DOS, vous devrez peut-être augmenter la taille de la mémoire tampon de la fenêtre pour prendre en compte toute la commande.  
  
 Si vous exécutez IIS et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ensemble sur le même ordinateur, utilisez l’application de console [!INCLUDE[iismgr](../../includes/iismgr-md.md)] pour obtenir le certificat installé sur votre ordinateur. [!INCLUDE[iismgr](../../includes/iismgr-md.md)] comprend des options pour la création et l’empaquetage d’un fichier de demande de certificat (.crt) pour les traitements suivants par une autorité de certification approuvée. L'autorité de certification que vous utilisez génère un fichier de certificat (.cer) et vous le renvoie. Vous pouvez utiliser la console de gestion IIS pour installer le fichier de certificat dans le magasin local. Pour plus d’informations, consultez [Using SSL to Encrypt Confidential Data](http://go.microsoft.com/fwlink/?LinkId=71123) (Utilisation de SSL pour chiffrer des données confidentielles) sur le site TechNet.  
  
## <a name="interoperability-issues-with-iis"></a>Problèmes d'interopérabilité avec IIS  
 La présence d'IIS sur le même ordinateur que [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] affecte considérablement les connexions SSL à un serveur de rapports :  
  
-   Si IIS est installé, le service World Wide Web (W3SVC) doit toujours être en cours d'exécution. Le service HTTP SSL établit une dépendance sur IIS s'il détecte que le service est en cours d'exécution. Cela signifie que le service World Wide Web (W3SVC) doit être en cours d’exécution à chaque fois qu’IIS et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sont installés sur le même ordinateur et que vous configurez des URL du serveur de rapports pour des connexions SSL.  
  
-   La désinstallation d'IIS peut interrompre temporairement le service à une URL du serveur de rapports liée à SSL. C'est pourquoi il est fortement recommandé de redémarrer l'ordinateur après avoir désinstallé IIS.  
  
     Il est nécessaire de redémarrer l'ordinateur pour effacer toutes les sessions SSL du cache. Certains systèmes d'exploitation mettent en cache des sessions SSL pendant 10 heures, ce qui prolonge le fonctionnement d'une URL https:// même après la suppression de la liaison SSL de la réservation d'URL dans HTTP.SYS. Le redémarrage de l'ordinateur ferme toutes les connexions ouvertes qui utilisent le canal.  
  
## <a name="bind-ssl-to-a-reporting-services-url-reservation"></a>Lier SSL à une réservation d'URL Reporting Services  
 Les étapes suivantes n'incluent pas d'instructions pour demander, générer, télécharger, ou installer un certificat. Un certificat doit être installé et disponible. Les propriétés de certificat que vous spécifiez, l'autorité de certification émettrice, et les outils et utilitaires que vous utilisez pour demander et installer le certificat relèvent entièrement de votre choix.  
  
 Vous pouvez utiliser l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour lier le certificat. Si le certificat est installé correctement dans le magasin de l’ordinateur local, l’outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] le détecte et l’affiche dans la liste **Certificats SSL** dans les pages **URL du service Web** et **URL du Gestionnaire de rapports** .  
  
### <a name="to-configure-a-report-server-url-for-ssl"></a>Pour configurer une URL de serveur de rapports pour SSL  
  
1.  Démarrez l'outil de configuration de Reporting Services, puis connectez-vous au serveur de rapports.  
  
2.  Cliquez sur **URL du service Web**.  
  
3.  Développez la liste des certificats SSL. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] détecte les certificats d’authentification des serveurs dans le magasin local. Si vous avez installé un certificat et qu'il ne figure pas dans la liste, vous devrez peut-être redémarrer le service. Vous pouvez utiliser les boutons **Arrêter** et **Démarrer** dans la page **État de Report Server** dans l’outil de configuration de Reporting Services pour redémarrer le service.  
  
4.  Sélectionnez le certificat.  
  
5.  Cliquez sur **Appliquer**.  
  
6.  Cliquez sur l'URL pour vérifier qu'elle fonctionne.  
  
 Il est nécessaire de configurer la base de données du serveur de rapports pour tester l'URL. Si vous n'avez pas encore créé la base de données du serveur de rapports, faites le avant de tester l'URL.  
  
 Les réservations d'URL pour le service Web Report Server et le Gestionnaire de rapports sont configurées indépendamment. Si vous souhaitez configurer également l'accès au Gestionnaire de rapports à l'aide d'un canal chiffré par SSL, continuez les étapes suivantes :  
  
1.  Cliquez sur **URL du Gestionnaire de rapports**.  
  
2.  Cliquez sur **Avancé**.  
  
3.  Dans **Plusieurs identités SSL pour le Gestionnaire de rapports**, cliquez **Ajouter**.  
  
4.  Sélectionnez le certificat, cliquez sur **OK**, puis sur **Appliquer**.  
  
5.  Cliquez sur l'URL pour vérifier qu'elle fonctionne.  
  
## <a name="how-certificate-bindings-are-stored"></a>Comment les liaisons de certificat sont stockées  
 Les liaisons de certificat sont stockées dans HTTP.SYS. Une représentation des liaisons que vous avez définies est également stockée dans la section **URLReservations** du fichier RSReportServer.config. Les paramètres dans le fichier de configuration sont uniquement une représentation des valeurs réelles spécifiées ailleurs. Ne modifiez pas les valeurs directement dans le fichier de configuration. Les paramètres de configuration apparaissent dans le fichier uniquement si vous avez utilisé l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou le fournisseur WMI (Windows Management Instrumentation) de Report Server pour lier un certificat.  
  
> [!NOTE]  
>  Si vous configurez une liaison avec un certificat SSL dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et que vous souhaitez par la suite supprimer le certificat de l’ordinateur, n’oubliez pas de supprimer la liaison de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] avant de supprimer le certificat de l’ordinateur. Sinon, vous ne pourrez pas supprimer la liaison en utilisant l’outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou WMI, et vous recevrez l’erreur « Paramètre non valide ». Si vous avez déjà supprimé le certificat de l'ordinateur, vous pouvez utiliser l'outil Httpcfg.exe pour supprimer la liaison de HTTP.SYS. Pour plus d'informations sur Httpcfg.exe, consultez la documentation produit de Windows.  
  
 Les liaisons SSL sont une ressource partagée dans Microsoft Windows. Les modifications apportées par le Gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou d’autres outils, tels que le Gestionnaire des services IIS, peuvent avoir une incidence sur d’autres applications sur le même ordinateur. Il est recommandé d'utiliser le même outil pour modifier les liaisons que celui utilisé pour les créer.  Par exemple, si vous avez créé des liaisons SSL à l'aide du Gestionnaire de configuration, il est recommandé d'utiliser cet outil pour gérer le cycle de vie des liaisons. Si vous utilisez le Gestionnaire des services IIS pour créer des liaisons, il est recommandé d'utiliser cet outil pour gérer le cycle de vie des liaisons. Si IIS a été installé sur l'ordinateur avant [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , il est recommandé de vérifier la configuration SSL dans IIS avant de configurer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Si vous supprimez des liaisons SSL pour [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à l’aide du Gestionnaire de configuration Reporting Services, il se peut que SSL ne fonctionne plus pour les sites web sur un serveur exécutant Internet Information Services (IIS) ou sur un autre serveur HTTP.SYS. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Le Gestionnaire de configuration supprime la clé de Registre suivante. Lorsque cette clé de Registre est supprimée, la liaison SSL pour IIS l'est également. Sans cette liaison, SSL n'est pas fourni pour le protocole HTTPS. Pour diagnostiquer ce problème, utilisez le Gestionnaire des services IIS ou l'utilitaire en ligne de commande HTTPCFG.exe. Pour résoudre le problème, restaurez la liaison SSL pour vos sites Web en utilisant le Gestionnaire des services IIS. Pour éviter ce problème dans le futur, utilisez le Gestionnaire des services IIS pour restaurer la liaison pour les sites Web souhaités. Pour plus d’informations, consultez l’article de la Base de connaissances [SSL ne fonctionne plus après la suppression d’une liaison SSL (http://support.microsoft.com/kb/956209/n)](http://support.microsoft.com/kb/956209/n).  
  
## <a name="see-also"></a> Voir aussi  
 [Authentification avec le serveur de rapports](../../reporting-services/security/authentication-with-the-report-server.md)   
 [Configurer et administrer un serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)   
 [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Configurer des URL de serveurs de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)  
  
  
