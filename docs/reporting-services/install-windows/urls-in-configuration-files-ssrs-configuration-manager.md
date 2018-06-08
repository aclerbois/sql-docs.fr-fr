---
title: URL des fichiers de configuration (Gestionnaire de configuration de SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 05/18/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- URL configuration [Reporting Services]
ms.assetid: 4f5e7fe0-b5b1-4665-93d4-80dce12d6b14
caps.latest.revision: 9
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 5bc15384a80a29bed2b70ba9036f354fb0d11693
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="urls-in-configuration-files--ssrs-configuration-manager"></a>URL des fichiers de configuration (Gestionnaire de configuration de SSRS)
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] stocke les paramètres d’application dans un fichier RSReportServer.config. Ce fichier contient des paramètres de configuration pour les URL et pour les réservations d'URL. Ces paramètres de configuration ont des règles de modification et des objectifs très différents. Si vous êtes habitué à la modification de fichiers de configuration pour régler un déploiement, cette rubrique peut vous aider à comprendre comment chaque paramètre URL est utilisé.  
  
## <a name="url-settings-in-rsreportserverconfig-file"></a>Paramètres d'URL du fichier RSReportServer.config  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] stocke les URL pour l’accès aux applications et aux rapports, et pour connecter les composants web frontaux à un serveur de rapports principal.  
  
#### <a name="urls-for-application-access"></a>URL d'accès aux applications  
 Les URL permettent d’accéder au service Web Report Server et au [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]. Pour configurer les URL, vous devez utiliser l'outil de configuration de Reporting Services. L’outil crée les réservations d’URL pour chaque application dans HTTP.SYS et ajoute des entrées pour les URL dans la section **URLReservations** de RSReportServer.config.  
  
-   Pour afficher la description de chaque élément dans la section **URLReservations** , consultez [Fichier de configuration RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md) dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Pour plus d’informations sur la syntaxe de l’élément **UrlString**, consultez [Syntaxe de réservation d’URL &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/url-reservation-syntax-ssrs-configuration-manager.md).  
  
-   Pour obtenir des instructions sur la configuration des URL pour l’accès aux applications, consultez [Configurer une URL &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md).  
  
#### <a name="urls-for-report-access"></a>URL d'accès aux rapports  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] inclut une extension de remise par messagerie de serveur de rapports que vous pouvez utiliser pour envoyer les liens de rapport ou les pièces jointes. Un lien de rapport est construit lorsque le rapport est remis. L’extension de remise par messagerie de serveur de rapports utilise le paramètre **UrlRoot** du fichier de configuration pour créer le lien. **UrlRoot** est également utilisé pour résoudre les liens d’un rapport rendu et généré par le biais d’un traitement des rapports sans assistance.  
  
 **UrlRoot** est spécifié automatiquement dans le fichier RSReportServer.config quand vous configurez les URL pour l’accès aux applications. Si vous modifiez cette valeur dans le fichier de configuration, vous devez spécifier l'adresse URL valide d'un service Web Report Server connecté à une base de données de serveur de rapports qui contient les rapports que vous souhaitez remettre. Vous pouvez spécifier un seul élément **UrlRoot** pour une instance de serveur de rapports unique ; une seule entrée **UrlRoot** peut exister dans le fichier RSReportServer.config pour toute instance de serveur de rapports donnée. Si vous avez plusieurs URL réservées pour le service Web Report Server, vous devez choisir l’une des valeurs disponibles pour **UrlRoot**.  
  
 Dans la plupart des cas, vous n’avez pas besoin de modifier **UrlRoot**. Toutefois, si le serveur de rapports est accessible par le biais d’une URL complète et que vous n’avez pas configuré une URL qui utilise l’en-tête de l’hôte sur le nom de site complet, vous devez modifier le fichier RSReportServer.config manuellement pour définir **UrlRoot** avec l’URL complète du serveur de rapports qui sera utilisée pour restituer le rapport (par exemple, https://www.adventure-works.com/mywebapp/reportserver)).  
  
#### <a name="urls-connecting-the-includessrswebportalincludesssrswebportalmd-and-web-parts-to-the-report-server-web-service"></a>URL connectant le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] et les composants WebPart au service Web Report Server  
 Le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] et les composants WebPart SharePoint 2.0 pour Reporting Services sont les composants web frontaux qui se connectent à un serveur de rapports. Les URL utilisées pour se connecter à un serveur de rapports principal sont les suivantes :  
  
-   **ReportServerUrl** (utilisé par le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)])  
  
-   **ReportServerExternalUrl** (utilisé par les composants WebPart)  
  
> [!NOTE]  
>  Les versions précédentes de Reporting Services incluaient l’élément **ReportServerVirtualDirectory** . Cette valeur est obsolète dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures. Si vous avez mis à niveau une installation existante et utilisez un fichier de configuration qui contient ce paramètre, le serveur de rapports ne lit plus cette valeur.  
  
 Le tableau suivant récapitule toutes les URL pouvant être spécifiées dans un fichier de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
|Paramètre|Utilisation|Description|  
|-------------|-----------|-----------------|  
|**ReportServerUrl**|Facultatif. Cet élément n'est pas inclus dans le fichier RSReportServer.config à moins que vous ne l'ajoutiez vous-même.<br /><br /> Définissez cet élément uniquement si vous configurez l'un des scénarios suivants :<br /><br /> Le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] fournit un accès web frontal à un service Web Report Server qui s’exécute sur un autre ordinateur ou sur une instance différente du même ordinateur.<br /><br /> Quand vous avez plusieurs URL vers un serveur de rapports et que vous souhaitez que le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] utilise une URL spécifique.<br /><br /> Vous avez une URL du serveur de rapports spécifique que vous souhaitez voir utilisée par toutes les connexions du [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] .<br /><br /> Par exemple, vous pouvez activer l’accès du [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] pour tous les ordinateurs du réseau, tout en exigeant que le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] se connecte au serveur de rapports par le biais d’une connexion locale. Dans ce cas, vous pouvez configurer **ReportServerUrl** avec la valeur « `http://localhost/reportserver` ».|Cette valeur spécifie l'URL du service Web Report Server. Cette valeur est lue par l’application [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] au démarrage. Si cette valeur est définie, le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] se connecte au serveur de rapports spécifié dans l’URL.<br /><br /> Par défaut, le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] fournit un accès web frontal à un service Web Report Server qui s’exécute sur la même instance du serveur de rapports que le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]. Toutefois, si vous souhaitez utiliser le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] avec un service Web Report Server qui fait partie d’une autre instance ou qui s’exécute sur une instance d’un autre ordinateur, vous pouvez définir cette URL pour faire en sorte que le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] se connecte au service Web Report Server externe.<br /><br /> Si un certificat SSL (Secure Sockets Layer) est installé sur le serveur de rapports auquel vous êtes connecté, la valeur de l’élément **ReportServerUrl** doit être le nom du serveur inscrit pour ce certificat. Si l’erreur « La connexion sous-jacente a été fermée. Impossible d’établir une relation de confiance pour le canal sécurisé SSL/TLS » s’affiche, affectez à l’élément **ReportServerUrl** le nom de domaine complet du serveur pour lequel le certificat SSL a été émis. Par exemple, si le certificat est inscrit sur **https://adventure-works.com.onlinesales**, l’URL du serveur de rapports serait **https://adventure-works.com.onlinesales/reportserver**.|  
|**ReportServerExternalUrl**|Facultatif. Cet élément n'est pas inclus dans le fichier RSReportServer.config à moins que vous ne l'ajoutiez vous-même.<br /><br /> Définissez cet élément uniquement si vous utilisez les composants WebPart de SharePoint 2.0 et que vous souhaitez que les utilisateurs soient en mesure d'extraire un rapport et de l'ouvrir dans une nouvelle fenêtre de navigateur.<br /><br /> Ajoutez l’élément \<**ReportServerExternalUrl**> sous l’élément \<**ReportServerUrl**>, puis affectez-lui un nom de serveur de rapports complet qui se résout en une instance de serveur de rapports en cas d’accès dans une fenêtre de navigateur distincte. Ne supprimez pas \<**ReportServerUrl**>.<br /><br /> L'exemple suivant illustre la syntaxe :<br /><br /> `<ReportServerExternalUrl>http://myserver/reportserver</ReportServerExternalUrl>`|Cette valeur est utilisée par les composants WebPart de SharePoint 2.0.<br /><br /> Dans les versions précédentes, il était recommandé que vous définissiez cette valeur pour déployer le Générateur de rapport sur un serveur de rapports Internet. Il s'agit d'un scénario de déploiement non testé. Si vous utilisiez ce paramètre par le passé pour gérer l'accès Internet au Générateur de rapports, vous devez prévoir une autre stratégie.|  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer des URL de serveurs de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)   
 [Configurer une URL &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)
