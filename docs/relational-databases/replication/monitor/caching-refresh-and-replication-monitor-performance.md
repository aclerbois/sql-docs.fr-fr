---
title: Mise en cache, actualisation et performances du moniteur de réplication | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- monitoring performance [SQL Server replication], Replication Monitor
- cache [SQL Server], replication
- Replication Monitor, caching
- refreshing data
- Replication Monitor, refreshing
ms.assetid: a2d8b666-ed41-4f86-b2b8-c8e118416ab7
caps.latest.revision: 12
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: cfc8cfc20db38bafc60b1aabfd588ddd1c259833
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="caching-refresh-and-replication-monitor-performance"></a>Mise en cache, actualisation et performances du moniteur de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le moniteur de réplication[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est conçu pour analyser efficacement un grand nombre d'ordinateurs dans un système de production. Les requêtes utilisées par le moniteur de réplication pour effectuer des calculs et collecter des données sont mises en cache et actualisées de façon périodique. La mise en cache réduit le nombre de requêtes et de calculs requis lorsque vous affichez différentes pages du moniteur de réplication et permet d'adapter l'analyse à de nombreux utilisateurs.  
  
 L'actualisation du cache est gérée par un travail de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent, l' **Actualisateur d'analyse de réplication pour la distribution**. Le travail s'exécute en permanence mais la planification de l'actualisation du cache prévoit l'attente d'un certain délai après l'actualisation précédente :  
  
-   En cas de modifications de l'historique de l'Agent depuis la dernière création du cache, le délai d'attente est la période la plus courte entre 4 secondes et le temps nécessaire à la création du cache précédent.  
  
-   En l'absence de modifications de l'historique de l'Agent depuis la dernière création du cache (d'autres modifications sont possibles), le délai d'attente est la période la plus longue entre 30 secondes et le temps nécessaire à la création du cache précédent.  
  
## <a name="refreshing-the-replication-monitor-user-interface"></a>Actualisation de l'interface utilisateur du moniteur de réplication  
 L'interface utilisateur du moniteur de réplication peut être actualisée de plusieurs façons :  
  
-   La fenêtre principale du moniteur de réplication (y compris tous les onglets) est automatiquement actualisée toutes les cinq secondes. Les actualisations automatiques ne forcent pas une actualisation du cache ; l'interface utilisateur affiche la version la plus récente des données du cache. Vous pouvez personnaliser la fréquence d'actualisation utilisée pour toutes les fenêtres associées à un serveur de publication en modifiant les paramètres de ce dernier. Vous pouvez aussi désactiver les actualisations automatiques d'un serveur de publication.  
  
-   Par défaut, les fenêtres de détails lancées à partir du moniteur de réplication ne sont pas automatiquement actualisées, à l'exception des fenêtres liées aux abonnements de fusion qui font l'objet d'une synchronisation. Si vous spécifiez que les fenêtres de détails doivent être automatiquement actualisées, elles le sont à la même fréquence que celle planifiée pour la fenêtre principale du moniteur de réplication.  
  
-   Toutes les fenêtres peuvent être actualisées manuellement en appuyant sur la touche F5 ou en cliquant avec le bouton droit dans l'arborescence du moniteur de réplication et en cliquant sur **Actualiser**. Les actualisations manuelles forcent celle du cache.  
  
 Pour plus d’informations, consultez [Actualiser des données dans le moniteur de réplication](../../../relational-databases/replication/monitor/refresh-data-in-replication-monitor.md).  
  
## <a name="performance-considerations"></a>Considérations relatives aux performances  
 Même si le moniteur de réplication est conçu pour s'exécuter efficacement, prenez en compte les points suivants :  
  
-   Si vous possédez un grand nombre de publications ou d'abonnements, envisagez de définir une planification d'actualisation automatique moins fréquente pour l'interface utilisateur.  
  
-   Évitez d'exécuter simultanément plusieurs instances du moniteur de réplication.  
  
-   Évitez d'inscrire un grand nombre de serveurs de distribution et de configurer le moniteur de réplication pour qu'il se connecte automatiquement à tous.  
  
## <a name="see-also"></a> Voir aussi  
 [Exécuter des travaux de maintenance de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/administration/run-replication-maintenance-jobs-sql-server-management-studio.md)   
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md)  
  
  
