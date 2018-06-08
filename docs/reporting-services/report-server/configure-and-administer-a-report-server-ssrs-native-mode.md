---
title: Configurer et administrer un serveur de rapports (SSRS en mode natif) | Microsoft Docs
ms.custom: ''
ms.date: 03/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, components
- deploying [Reporting Services], component options
- report servers [Reporting Services], component options
- configuration options [Reporting Services]
- administering Reporting Services
- components [Reporting Services], configuring
- configuring servers [Reporting Services]
ms.assetid: cfec012b-56f1-4346-9814-247acf22351c
caps.latest.revision: 51
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: ca2bbc7549a2616195061a5079918f236e963e28
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34550030"
---
# <a name="configure-and-administer-a-report-server-ssrs-native-mode"></a>Configurer et administrer un serveur de rapports (SSRS en mode natif)
  Cette rubrique récapitule les approches que vous pouvez utiliser pour configurer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Elle inclut également une liste de rubriques qui expliquent comment configurer des composants, des fonctions ou des fonctionnalités de serveur spécifiques. Pour configurer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vous pouvez :  
  
-   utiliser le Gestionnaire de configuration de Reporting Services. Un grand nombre de rubriques de cette section contiennent des informations décrivant comment configurer des fonctions spécifiques par le biais de cet outil ;  
  
-   utiliser [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour personnaliser les propriétés du serveur, activer Mes rapports, d’activer les journaux des traces et définir des valeurs par défaut à l’échelle du site. Pour plus d’informations sur les paramètres de site, consultez [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md) pour Management Studio. Notez que vous pouvez créer et exécuter un script qui définit des propriétés de serveur par programmation. Pour plus d’informations, consultez [Écrire des scripts pour les tâches d’administration et de déploiement](../../reporting-services/tools/script-deployment-and-administrative-tasks.md) et [Propriétés système de Report Server](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md).  
  
-   utiliser le Gestionnaire de rapports pour accorder des autorisations d'accès au serveur de rapports. Les autorisations sont fournies via des attributions de rôles que vous définissez pour chaque compte d'utilisateur ou de groupe. Pour plus d’informations, consultez [Rôles et autorisations &#40;Reporting Services&#41;](../../reporting-services/security/roles-and-permissions-reporting-services.md).  
  
-   modifier éventuellement les fichiers de configuration afin de changer les paramètres d'application. Pour plus d’informations sur chaque fichier ainsi que des instructions pour les modifier, consultez [Reporting Services Configuration Files](../../reporting-services/report-server/reporting-services-configuration-files.md).  
  
## <a name="in-this-section"></a>Dans cette section  
 [Configurer des URL de serveurs de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)  
 Décrit comment définir les URL d'accès au serveur de rapports et au Gestionnaire de rapports.  
  
 [Configurer le compte de service Report Server &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)  
 Fournit des recommandations et des procédures pour la modification du compte de service et du mot de passe.  
  
 [Créer une base de données du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
 Décrit le mode de création d'une base de données du serveur de rapports, tel qu'il est exigé pour le stockage des objets et des métadonnées de serveur.  
  
 [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)  
 Décrit le mode de modification de la chaîne de connexion utilisée par le serveur de rapports pour se connecter à la base de données du serveur de rapports.  
  
 [Configurer un serveur de rapports pour la remise par messagerie (Gestionnaire de configuration de SSRS)](http://msdn.microsoft.com/en-us/b838f970-d11a-4239-b164-8d11f4581d83)  
 Décrit comment configurer un serveur de rapports pour la prise en charge de la distribution des rapports par courrier électronique.  
  
 [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)  
 Décrit comment configurer un compte d'utilisateur de manière à traiter les rapports en mode sans assistance.  
  
## <a name="see-also"></a> Voir aussi  
 [Fichiers de configuration de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Gestionnaire de configuration de Reporting Services &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Sécurité et protection de Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md)   
 [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)  
  
  
