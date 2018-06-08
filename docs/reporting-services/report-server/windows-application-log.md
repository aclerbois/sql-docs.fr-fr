---
title: Journal des applications Windows | Microsoft Docs
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
- Windows application logs [Reporting Services]
- logs [Reporting Services], Windows application logs
- application logs [Reporting Services]
ms.assetid: 742fd00e-aa6c-4c8a-b58f-c03c489b1699
caps.latest.revision: 32
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: f5e4aea6d0558e7bbb09b96aad3f3520f9de7add
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="windows-application-log"></a>Journal des applications Windows
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] écrit des messages d’événements dans le journal des applications Windows. Vous pouvez vous servir des informations des messages écrits dans le journal des applications pour rechercher des événements générés par les applications du serveur de rapports s'exécutant sur le système local.  
  
## <a name="viewing-report-server-events"></a>Affichage des événements du serveur de rapports  
 Vous pouvez utiliser l'Observateur d'événements pour afficher le fichier journal et filtrer les messages qu'il contient. Pour plus d’informations sur les messages d’événements, consultez [Références des erreurs et événements &#40;Reporting Services&#41;](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md). Pour plus d'informations sur le journal des applications Windows ou l'Observateur d'événements, voir la documentation du produit Windows.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit trois sources d’événements :  
  
-   Serveur de rapports (service Windows Report Server)  
  
-   Gestionnaire de rapports  
  
-   Processeur de planification et de remise  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] n’offre pas de moyen de désactiver la journalisation des événements d’application pour un serveur de rapports ou de contrôler les événements qui sont consignés dans le journal. Le schéma de description de la journalisation des événements du serveur de rapports est fixe. Vous ne pouvez pas étendre le schéma de manière à intégrer des événements personnalisés.  
  
 Le tableau suivant décrit les types d'événements que le serveur de rapports écrit dans le journal des événements d'application.  
  
|Type d'événement|Description|  
|----------------|-----------------|  
|Informations|Événement décrivant une opération réussie (par exemple, le démarrage des services du serveur de rapports).|  
|Avertissement|Événement indiquant un problème potentiel (par exemple, un faible espace disque).|  
|Error|Événement décrivant un problème significatif (par exemple, le service n'a pas démarré).|  
|Audit des succès|Événement de sécurité décrivant une ouverture de session réussie.|  
|Audit des échecs|Événement consigné en journal en cas d'échec d'une tentative d'ouverture de session.|  
  
## <a name="see-also"></a> Voir aussi  
 [Fichiers journaux et sources de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md)   
 [Guide de référence des erreurs et des événements &#40;Reporting Services&#41;](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
  
  
