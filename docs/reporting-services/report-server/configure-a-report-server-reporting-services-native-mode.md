---
title: Configurer un serveur de rapports (mode natif de Reporting Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- report server configuration
- report servers [Reporting Services], configuring
ms.assetid: ef84ce9d-9156-48e9-8073-7e0535476932
caps.latest.revision: 17
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 175688c6892a0cb16613a55d168e971681d5ea0e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-a-report-server-reporting-services-native-mode"></a>Configurer un serveur de rapports (mode natif de Reporting Services)
  Selon les options que vous avez sélectionnées pendant l'installation, le serveur de rapports peut requérir une configuration supplémentaire avant de pouvoir fonctionner. Dans sa configuration minimale, un serveur de rapports se compose des éléments suivants :  
  
-   Un compte de service Report Server (configuré au cours de l'installation).  
  
-   Un URL du service Web qui permet d'accéder au serveur de rapports.  
  
-   Une base de données du serveur de rapports qui stocke des données d'application, des rapports et d'autres éléments.  
  
 Le programme d'installation fournit la configuration minimale si vous sélectionnez les options d'installation suivantes : configuration par défaut en mode natif ou configuration par défaut en mode intégré SharePoint. Si vous avez installé le serveur de rapports en mode de fichiers uniquement (option **Installer mais ne pas configurer** dans l’Assistant Installation), seul le compte de service est configuré. L'URL du service Web et la base de données du serveur de rapports doivent être configurés à l'issue de l'installation.  
  
 Le Gestionnaire de rapports est une fonctionnalité en option pour un serveur de rapports en mode natif, mais il vous est recommandé de configurer le Gestionnaire de rapports pour vous permettre d'accorder à l'utilisateur l'accès au serveur de rapports et gérer le contenu de ce serveur. Si vous déployez un serveur de rapports en mode intégré SharePoint, utilisez le frontal Web d'un serveur SharePoint pour accorder l'accès.  
  
 Des fonctionnalités supplémentaires, telles que la messagerie électronique de serveur de rapports et le compte d'exécution sans assistance, peuvent être configurées si nécessaire. Pour plus d’informations, consultez [Gérer un serveur de rapports Reporting Services (SSRS) en mode natif](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md).  
  
 Pour configurer un serveur de rapports, utilisez l'outil de configuration de Reporting Services.  
  
### <a name="to-minimally-configure-a-report-server-installation"></a>Pour installer la configuration minimale d'un serveur de rapports  
  
1.  Démarrez l'outil de configuration de Reporting Services, puis connectez-vous à l'instance du serveur de rapports. Pour obtenir des instructions, consultez [Gestionnaire de configuration de Reporting Services &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2.  Cliquez sur **URL du service Web** pour ouvrir la page de configuration d'une URL pour le serveur de rapports. Pour obtenir des instructions sur la définition de l’URL, consultez [Configurer une URL &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md).  
  
3.  Cliquez sur **Base de données** pour créer la base de données du serveur de rapports. Pour obtenir des instructions, consultez [Créer une base de données du serveur de rapports en mode natif &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  
  
4.  Revenez à la page **URL du service Web** et cliquez sur l'URL pour vérifier son fonctionnement.  
  
5.  Suivez les instructions dans « Étapes suivantes » pour achever votre déploiement.  
  
## <a name="next-steps"></a>Next Steps  
 Pour achever votre déploiement, vous devez configurer le Gestionnaire de rapports ou l'intégration SharePoint. Pour plus d’informations, consultez [Configurer le Gestionnaire de rapports &#40;mode natif&#41;](../../reporting-services/report-server/configure-report-manager-native-mode.md).  
  
 Si le Pare-feu Windows est activé, le port que le serveur de rapports est configuré pour utiliser est très probablement fermé. Un port peut être fermé si une page vide apparaît lorsque vous essayez d'ouvrir le Gestionnaire de rapports à partir d'un ordinateur client distant. Pour plus d'informations sur la configuration du pare-feu, consultez [Configure a Firewall for Report Server Access](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
 Si vous utilisez Windows Vista ou Windows Server 2008, des étapes supplémentaires peuvent être requises avant d'ouvrir le Gestionnaire de rapports en local. Pour plus d’informations, consultez [Configurer un serveur de rapports en mode natif pour l’administration locale &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  
  
 Vérifiez votre installation en créant des dossiers, en téléchargeant des éléments, et en exécutant des rapports. Suivez les instructions de la rubrique [Vérifier une installation de Reporting Services](../../reporting-services/install-windows/verify-a-reporting-services-installation.md) pour vérifier votre installation.  
  
## <a name="see-also"></a> Voir aussi  
 [Gérer un serveur de rapports Reporting Services (SSRS) en mode natif](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
 [Configurer un pare-feu pour accéder au serveur de rapports](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)   
 [Configurer un serveur de rapports en mode natif pour l’administration locale &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md)   
 [Configurer un serveur de rapports pour l’administration à distance](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md)   
 [Gestionnaire de configuration de Reporting Services &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
  
  
