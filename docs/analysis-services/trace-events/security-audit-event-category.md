---
title: Catégorie d’événement d’Audit de sécurité | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: trace-events
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8d4524c7c723c48f4acae72178dc58f5a858cc05
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="security-audit-event-category"></a>Catégorie d'événement Audit de sécurité
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La catégorie d'événement Audit de sécurité contient les classes d'événements décrites dans le tableau ci-dessous.  
  
|Classe d'événements|ID d'événement|Description|  
|-----------------|--------------|-----------------|  
|Audit Login|1|Enregistre tous les événements de connexion depuis le début de la trace, tels qu’une demande de connexion d’un client à un serveur qui exécute une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
|Audit Logout|2|Enregistre tous les nouveaux événements de déconnexion depuis le début de la trace, tels que l'émission par un client d'une commande de déconnexion.|  
|Audit Server Starts And Stops|4|Enregistre l'arrêt, le début et la suspension des activités des services.|  
|Audit Object Permission Event|18|Enregistre toutes les modifications d'autorisations sur les objets.|  
|Audit Admin Operations Event|19|Enregistre des opérations de serveur pour la sauvegarde, la restauration, la synchronisation, l'attachement, le détachement, ainsi que pour le chargement et la sauvegarde d'images.|  
  
 Pour plus d’informations sur les colonnes associées à chaque classe d’événements Security Audit, consultez [Colonnes de données Audit de sécurité](../../analysis-services/trace-events/security-audit-data-columns.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Événements de Trace Analysis Services](../../analysis-services/trace-events/analysis-services-trace-events.md)  
  
  
