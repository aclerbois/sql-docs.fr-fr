---
title: Analyser les performances avec le moniteur de réplication | Microsoft Docs
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
- Log Reader Agent, monitoring
- Replication Monitor, performance
- Merge Agent, monitoring
- Queue Reader Agent, monitoring
- Snapshot Agent, monitoring
- Distribution Agent, monitoring
- monitoring performance [SQL Server replication]
ms.assetid: f212397d-1bfd-496b-a246-668952891d09
caps.latest.revision: 36
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: b32f10e884b501eec72b526dce06b703089a046a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="monitor-performance-with-replication-monitor"></a>Analyser les performances avec le Moniteur de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le moniteur de réplication de[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vous permet d'analyser les performances de la réplication transactionnelle et de la réplication de fusion via les fonctionnalités suivantes :  
  
-   Définition d'alertes et de seuils  
  
-   Affichage de mesures de performance  
  
-   Détermination de la latence avec des jetons de suivi (réplication transactionnelle)  
  
-   Affichage de statistiques de synchronisation détaillées (réplication de fusion)  
  
-   Affichage des transactions et du temps de remise (réplication transactionnelle)  
  
## <a name="set-warnings-and-thresholds"></a>Définir des seuils et des avertissements  
 Le moniteur de réplication vous permet d'activer des avertissements pour plusieurs conditions de performances. Quand vous activez un avertissement, vous spécifiez un seuil. Quand ce seuil est atteint ou dépassé, un avertissement est affiché dans la colonne **État** pour l'abonnement et la publication avec laquelle il se synchronise (à moins qu'un problème d'une priorité plus élevée doive être affiché). L'atteinte d'un seuil déclenche un avertissement dans le Moniteur de réplication, mais également une alerte. Vous pouvez activer des avertissements pour les conditions de performances suivantes :  
  
-   Dépassement de la latence spécifiée (la quantité de temps qui s'écoule entre la validation d'une transaction sur le serveur de publication et la validation de la transaction correspondante sur l'Abonné).  
  
     Ceci s'applique à la réplication transactionnelle. Si le seuil spécifié est atteint ou dépassé, l'état affiché est **Critique pour les performances**.  
  
-   Dépassement du temps de synchronisation spécifié.  
  
     Ceci s'applique à la réplication de fusion. Si le seuil spécifié est atteint ou dépassé, l'état affiché est **Fusion longue**. Vous pouvez spécifier des seuils différents pour les connexions d'accès à distance et pour les connexions LAN (Local Area Network).  
  
-   Impossibilité de traiter le nombre de lignes spécifié dans un intervalle de temps donné.  
  
     Ceci s'applique à la réplication de fusion. Si le seuil spécifié est atteint ou dépassé, l'état affiché est **Critique pour les performances**. Vous pouvez spécifier des seuils différents pour les connexions d'accès à distance et pour les connexions LAN.  
  
 Pour plus d’informations, voir [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
  
## <a name="view-performance-measurements"></a>Afficher des mesures de performance  
 Le Moniteur de réplication affiche des valeurs de qualité des performances pour la réplication transactionnelle et la réplication de fusion dans les colonnes **Performance moyenne actuelle** et **Pire performance actuelle** pour les publications, et dans la colonne **Performance** pour les abonnements. Les valeurs sont :  
  
-   Excellent  
  
-   Bonne  
  
-   Correcte  
  
-   Médiocre  
  
-   Critique (réplication transactionnelle seulement)  
  
 Les valeurs sont déterminées comme suit :  
  
-   Pour la réplication transactionnelle, la qualité des performances est déterminée par le seuil de latence. Si le seuil n'est pas défini, aucune valeur n'est affichée. Le tableau suivant montre la corrélation entre le seuil et la valeur de la qualité des performances. Par exemple, si le seuil est défini à 60 secondes et que la latence réelle est de 30 secondes, la latence représente 50% du seuil, ce qui correspond à la valeur « Bonne ».  
  
    |Excellent|Bonne|Correcte|Médiocre|Critique|  
    |---------------|----------|----------|----------|--------------|  
    |0 – 34%|35 – 59%|60 – 84%|85 – 99%|100% +|  
  
-   Pour la réplication de fusion, la qualité des performances est indépendante des seuils (le seuil de traitement des lignes détermine si la valeur **Critique pour les performances** est affichée dans la colonne **État** ). La qualité des performances est déterminée en comparant la performance d'abonnements individuels à la performance historique moyenne des abonnements à la publication avec le même type de connexion (d'accès distant ou LAN). Le moniteur de réplication affiche une valeur après cinq synchronisations avec au moins 50 modifications chacune via le même type de connexion. S'il y a eu moins de cinq synchronisations comprenant 50 modifications ou plus ou que la synchronisation la plus récente inclut moins de 50 modifications, il n'affiche pas de valeur.  
  
     Le tableau suivant montre la corrélation entre la performance moyenne et la valeur de la qualité des performances. Par exemple, si dix Abonnés se sont synchronisés via une connexion LAN avec un taux moyen de 100 lignes par seconde, et que l'un des abonnements se synchronise ensuite à un taux de 125 lignes par seconde, la performance pour la synchronisation de cet Abonné est à 125% de la moyenne, ce qui correspond à la valeur « Bonne ».  
  
    |Excellent|Bonne|Correcte|Médiocre|  
    |---------------|----------|----------|----------|  
    |151+%|76 – 150%|26 – 75%|0 – 25%|  
  
 Pour plus d’informations, consultez [Afficher des informations et effectuer des tâches pour un abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-subscription-replication-monitor.md).  
  
## <a name="determine-latency-with-tracer-tokens"></a>Déterminer la latence avec des jetons de suivi  
 La réplication transactionnelle vous permet de mesurer la latence dans un système en insérant un jeton (une petite quantité de données) dans le journal des transactions de la base de données de publication et en enregistrant le temps qu'il met pour arriver au serveur de distribution et aux Abonnés. Le jeton vous permet aussi de détecter si des données n'atteignent par le serveur de distribution ou l'Abonné. Pour plus d’informations, voir [Measure Latency and Validate Connections for Transactional Replication](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md).  
  
## <a name="view-detailed-synchronization-performance-for-merge-replication"></a>Afficher les performances de synchronisation détaillées pour la réplication de fusion  
 Pour la réplication de fusion, le moniteur de réplication affiche des statistiques détaillées pour chaque article traité lors de la synchronisation, notamment la quantité de temps passé dans chaque phase du traitement (chargement des modifications, téléchargement des modifications, etc.). Il peut être utile d'identifier les tables spécifiques qui provoquent les ralentissements ; il s'agit du meilleur observatoire pour résoudre les problèmes de performance avec les abonnements de fusion. Pour plus d’informations sur l’affichage de statistiques détaillées, consultez [Afficher des informations et effectuer des tâches pour les agents d’abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
## <a name="view-transactions-and-delivery-time-for-transactional-replication"></a>Afficher les transactions et les temps de remise pour la réplication transactionnelle  
 Pour la réplication transactionnelle, le moniteur de réplication affiche des informations sur le nombre de transactions dans la base de données de distribution qui n'ont pas encore été distribuées à un Abonné et sur le temps estimé pour distribuer ces transactions. Pour plus d’informations, consultez [Afficher des informations et effectuer des tâches pour les agents d’abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md)   
 [Définir des seuils et des avertissements dans le moniteur de réplication](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)  
  
  
