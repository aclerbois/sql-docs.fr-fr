---
title: Gérer un serveur de rapports Reporting Services (SSRS) en mode natif | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services Configuration tool
- configuration options [Reporting Services]
- report servers [Reporting Services], configuring
ms.assetid: 6ca03a09-d6a8-4c93-ba12-1c99dcbfb618
caps.latest.revision: 10
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: b6322c4aca621aa668286f82bf144ec579657cc3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-a-reporting-services-native-mode-report-server"></a>Gérer un serveur de rapports Reporting Services (SSRS) en mode natif
  Cette section contient des procédures qui indiquent comment configurer une instance de serveur de rapports en mode natif à l'aide du Gestionnaire de configuration de Reporting Services.  
  
## <a name="in-this-section"></a>Dans cette section  
 Les rubriques de cette section sont organisées en catégories afin que vous puissiez rechercher plus aisément les instructions que vous souhaitez. La première section contient les rubriques pour les tâches de configuration de base pour un serveur de rapports en mode natif. La deuxième section contient les rubriques de configuration avancées. La troisième section contient les rubriques pour configurer un serveur de rapports afin qu'il s'exécute en mode intégré SharePoint.  
  
### <a name="basic-configuration"></a>Configuration de base  
 [Gestionnaire de configuration de Reporting Services &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
 Indique la procédure à suivre pour démarrer l'outil de configuration de Reporting Services.  
  
 [Configurer un compte de service &#40;Gestionnaire de configuration de SSR&#41;](http://msdn.microsoft.com/library/25000ad5-3f80-4210-8331-d4754dc217e0)  
 Explique comment spécifier des informations de compte et de mot de passe pour le service Web Report Server.  
  
 [Inscrire un nom de principal du service &#40;SPN&#41; pour un serveur de rapports](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md)  
 Explique comment enregistrer manuellement un SPN pour un serveur de rapports qui s'exécute sous un compte d'utilisateur de domaine sur un réseau qui utilise l'authentification Kerberos.  
  
 [Configurer une URL &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)  
 Explique comment établir une ou plusieurs URL utilisées pour accéder au service Web Report Server et au Gestionnaire de rapports.  
  
 [Créer une base de données du serveur de rapports en mode natif &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md)  
 Indique la procédure à suivre pour créer une base de données du serveur de rapports. Cette étape est nécessaire pour déployer une installation de Reporting Services.  
  
### <a name="advanced-or-optional-configuration"></a>Configuration avancée ou facultative  
 [Configurer un déploiement par montée en puissance parallèle de serveurs de rapports en mode natif &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)  
 Indique la procédure à suivre pour configurer plusieurs serveurs de rapports afin qu'ils partagent une base de données du serveur de rapports.  
  
 [Configurer un serveur de rapports pour la remise par messagerie (Gestionnaire de configuration de SSRS)](http://msdn.microsoft.com/en-us/b838f970-d11a-4239-b164-8d11f4581d83)  
 Fournit des instructions de configuration d'un serveur de rapports pour la distribution par messagerie.  
  
 [Configurer un pare-feu pour accéder au serveur de rapports](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)  
 Explique comment ouvrir les ports utilisés pour les demandes entrantes et les réponses sortantes d'un serveur de rapports.  
  
 [Configurer un serveur de rapports en mode natif pour l’administration locale &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md)  
 Décrit les étapes supplémentaires requises pour se connecter au Gestionnaire de rapports ou à un serveur de rapports en utilisant `http://localhost`.  
  
 [Configurer un serveur de rapports pour l'administration à distance](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md)  
 Explique comment configurer une instance de serveur de rapports distante afin que vous puissiez vous y connecter et le configurer à partir d'un autre ordinateur.  
  
 [Activer ou désactiver les fonctionnalités Reporting Services](../../reporting-services/report-server/turn-reporting-services-features-on-or-off.md)  
 Explique comment supprimer les fonctionnalités inutilisées dans une installation Reporting Services.  
  
 [Activer les erreurs distantes &#40;Reporting Services&#41;](../../reporting-services/report-server/enable-remote-errors-reporting-services.md)  
 Explique comment définir des propriétés de serveur sur un serveur de rapports de façon à retourner des informations supplémentaires concernant les conditions d'erreur qui se produisent sur des serveurs distants.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer et administrer un serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)   
 [Configuration et administration d’un serveur de rapports &#40;mode SharePoint de Reporting Services&#41;](../../reporting-services/report-server-sharepoint/configuration-and-administration-of-a-report-server.md)  
  
  
