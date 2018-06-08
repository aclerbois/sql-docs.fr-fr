---
title: Propriétés du serveur (page Général) | Microsoft Docs
ms.custom: ''
ms.date: 06/08/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.reportserver.serverproperties.general.f1
ms.assetid: 23537d52-4356-450f-a671-5921cef2431f
caps.latest.revision: 34
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 617abb0133a09cb348571509e105b1795be258cc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-server-properties-general-page"></a>Propriétés du serveur de rapports (page Général)
  Utilisez cette page pour afficher ou modifier le titre utilisé dans le Gestionnaire de rapports, activer ou désactiver Mes rapports, sélectionner une définition de rôle pour la sécurité de Mes rapports, et activer ou désactiver le contrôle d'impression du client.  
  
 **Pour ouvrir cette page**
 1) Démarrer [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]
 2) Connectez-vous à une instance de serveur de rapports.
 3) Cliquez avec le bouton droit sur le nom du serveur de rapports et sélectionnez **Propriétés**.  
  
 Le mode serveur détermine les propriétés de serveur que vous pouvez définir. Si vous gérez un serveur de rapports configuré pour le mode intégré SharePoint, vous ne pouvez pas activer Mes rapports ou définir le titre du portail web.  
  
## <a name="options"></a>Options  
 **Nom**  
 Tapez un nom qui apparaît en haut du portail web. Par défaut, il s’agit de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Le nom que vous spécifiez s'affiche uniquement dans le Gestionnaire de rapports.  
  
 **Version**  
 Cette propriété est en lecture seule. Spécifie la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que vous utilisez.  
  
 **Édition**  
 Cette propriété est en lecture seule. Spécifie l'instance de serveur de rapports actuelle. Le Gestionnaire de rapports n’est pas disponible dans certaines éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 **Mode d'authentification**  
 Cette propriété est en lecture seule. Elle identifie les types de demandes d'authentification acceptés par l'instance de serveur de rapports. Pour changer le mode d’authentification, vous devez modifier le fichier **RSReportServer.config** . Pour plus d’informations, consultez [Authentification avec le serveur de rapports](../../reporting-services/security/authentication-with-the-report-server.md).  
  
 **URL**  
 Cette propriété est en lecture seule. Indique l'URL vers le service Web Report Server. Cette valeur est spécifiée dans l’outil de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez [Configurer une URL &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md).  
  
 **Activer un dossier Mes rapports pour chaque utilisateur**  
 Mettez **Mes rapports** à la disposition des utilisateurs. Cette option est disponible uniquement pour les serveurs de rapports en mode natif.  
  
 **Sélectionner le rôle à appliquer à chaque dossier Mes Rapports**  
 Spécifiez une définition de rôle à utiliser pour la sécurité de Mes rapports. La définition de rôle identifie l'ensemble des tâches qui sont prises en charge dans chaque dossier Mes rapports.  

  
## <a name="see-also"></a> Voir aussi  
 [Définir les propriétés du serveur de rapports &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md)   
 [Se connecter à un serveur de rapports dans Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [Activer et désactiver Mes rapports](../../reporting-services/report-server/enable-and-disable-my-reports.md)   
 [Aide du serveur de rapports dans Management Studio accessible par la touche F1](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)   
 [Sécuriser Mes Rapports](../../reporting-services/security/secure-my-reports.md)  
  
  

