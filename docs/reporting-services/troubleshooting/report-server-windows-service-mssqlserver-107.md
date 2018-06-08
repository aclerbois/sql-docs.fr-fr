---
title: Service Report Server Windows (MSSQLServer) 107 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: troubleshooting
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- MSSQLServer 107 error
ms.assetid: 52b5704b-27f9-400a-a821-d8fa0786afe4
caps.latest.revision: 20
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 9dab16fea328f96642214c19e6e5f709eb364c73
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-server-windows-service-mssqlserver-107"></a>Service Report Server Windows (MSSQLServer) 107
    
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID d'événement|107|  
|Source de l'événement|Report Server Windows Service|  
|Composant|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|  
|Texte du message|Le service Report Server Windows (MSSQLSERVER) ne peut pas se connecter à la base de données du serveur de rapports.|  
  
## <a name="explanation"></a>Explication  
 Le service Report Server [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas se connecter à la base de données du serveur de rapports. Cette erreur se produit au cours du redémarrage d'un service si une connexion à la base de données du serveur de rapports ne peut pas être établie. Les conditions dans lesquelles cette erreur se produit incluent :  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] Le service n’est pas en cours d’exécution lorsque le service Report Server démarre.  
  
-   La connexion au service du [!INCLUDE[ssDE](../../includes/ssde-md.md)] échoue, car les connexions distantes ou le protocole TCP/IP ne sont pas activés.  
  
-   La base de données du serveur de rapports n'est pas configurée correctement.  
  
-   Le compte de service n'est pas configuré correctement ou le compte n'a plus d'autorisations sur la base de données du serveur de rapports. Cela peut se produire si vous n'utilisez pas l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour configurer le compte ou la base de données du serveur de rapports.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Démarrez le service du [!INCLUDE[ssDE](../../includes/ssde-md.md)] s'il ne s'exécute pas et vérifiez que les connexions distantes sont activées pour le protocole TCP/IP.  
  
 Utilisez l’outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour configurer la base de données du serveur de rapports et le compte de service.  
  
## <a name="internal-only"></a>Interne uniquement  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer le compte de service Report Server &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [Gestionnaire de configuration de Reporting Services &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Démarrer et arrêter le service du serveur de rapports](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)  
  
  
