---
title: Afficher l’état des publications et des abonnements dans le Moniteur de réplication | Microsoft Docs
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
- Log Reader Agent, monitoring
- Merge Agent, monitoring
- Queue Reader Agent, monitoring
- publications [SQL Server replication], viewing information
- Snapshot Agent, monitoring
- Distribution Agent, monitoring
- monitoring performance [SQL Server replication], publication status
- monitoring performance [SQL Server replication], subscription status
- subscriptions [SQL Server replication], viewing status
- Replication Monitor, publication and subscription status
ms.assetid: 16590771-9867-463e-a973-36a5c145ac16
caps.latest.revision: 34
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 24360f72317bd0f4d6193405c4b271914cb86d33
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-publication-and-subscription-status-in-replication-monitor"></a>Afficher l'état des publications et des abonnements dans le Moniteur de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le moniteur de réplication[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] présente des informations sur l'état des publications et des abonnements :  
  
-   L'état d'une publication est déterminé par l'état de la priorité la plus élevée de ses abonnements. Par exemple, si un abonnement à une publication comporte une erreur et qu'un autre présente un problème de performances, un état d'erreur est affiché pour la publication.  
  
-   L'état d'une publication est déterminé par l'état des agents qui servent l'abonnement. Pour la réplication de fusion, il s'agit de l'Agent de fusion. Pour la réplication transactionnelle, il s'agit de l'Agent de lecture du journal ou de l'Agent de distribution (l'état de la priorité la plus élevée est affiché ; l'état peut également être déterminé par l'Agent de lecture de la file d'attente en cas d'utilisation d'abonnements de mise à jour en attente). Pour la réplication d'instantané, il s'agit de l'Agent d'instantané ou de l'Agent de distribution (l'état de priorité la plus élevée est affiché).  
  
 Les tableaux présentés dans les sections suivantes répertorient les valeurs d'état possibles pour les publications et les abonnements. Trois valeurs d'état sont affichées uniquement si un seuil est atteint ou dépassé :  
  
-   Expiration de l'abonnement  
  
     Cette valeur d'état s'applique à tous les types de réplication. Pour plus d’informations, voir [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
  
-   Critique pour les performances  
  
     Cette valeur d'état s'applique à la réplication transactionnelle et à la réplication de fusion. Pour plus d’informations, consultez [Analyser les performances avec le Moniteur de réplication](../../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md).  
  
-   Fusion longue  
  
     Cette valeur d'état s'applique à la réplication de fusion. Pour plus d’informations, consultez [Analyser les performances avec le Moniteur de réplication](../../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md).  
  
 Outre l'état des publications et des abonnements, la réplication de fusion fournit des statistiques au niveau de l'article qui donnent des informations détaillées sur : la durée d'achèvement d'une phase de fusion, le temps passé pour traiter un article donné, le type de connexion utilisé par un Abonné et bien d'autres informations encore. Les statistiques sont affichées dans la fenêtre de l'Agent de fusion dans le moniteur de réplication. La réplication d'instantané et la réplication transactionnelle fournissent des informations détaillées sur le traitement de l'Agent de distribution.  
  
 **Pour afficher l'état des publications et des abonnements**  
  
-   Moniteur de réplication : [Afficher des informations et exécuter des tâches pour un serveur de publication &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-publication-replication-monitor.md) et [Afficher des informations et exécuter des tâches pour un abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-subscription-replication-monitor.md)  
  
 **Pour afficher des informations détaillées pour les agents**  
  
-   Moniteur de réplication : [Afficher des informations et exécuter des tâches pour les agents associés à un serveur de publication &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-publication-agents.md) et [Afficher des informations et exécuter des tâches pour les agents associés à un abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
## <a name="publication-status-values"></a>Valeurs d'état des publications  
 Le tableau suivant montre les valeurs d'état des publications et leurs icônes correspondantes dans l'ordre de la priorité.  
  
|État|Icône|  
|------------|----------|  
|Error|![Icône d’interface utilisateur : erreur](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "Icône d’interface utilisateur : erreur")|  
|Critique pour les performances|![Icône d’interface utilisateur : avertissement](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icône d’interface utilisateur : avertissement")|  
|Nouvelle tentative de la commande qui a échoué|![Icône d’interface utilisateur : nouvelle tentative de l’agent de réplication](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "Icône d’interface utilisateur : nouvelle tentative de l’agent de réplication")|  
|OK|aucun|  
  
## <a name="subscription-status-values"></a>Valeurs d'état des abonnements  
 Les tableaux suivants montrent les valeurs d'état des abonnements et leurs icônes correspondantes dans l'ordre de la priorité. Un abonnement peut avoir deux états en même temps (par exemple, **Expire bientôt/Expiré** et **Nouvelle tentative de la commande qui a échoué**) ; l'état de haute priorité est affiché.  
  
 Les valeurs d'état **Critique pour les performances**, **Expire bientôt/Expiré**et **Non initialisé** sont des avertissements. Lorsqu'un avertissement est affiché, le moniteur de réplication indique également si un agent est en cours d'exécution. Par exemple, l'état peut être **En cours d'exécution, Critique pour les performances**.  
  
### <a name="transactional-subscriptions"></a>Abonnements transactionnels  
  
|État|Icône|  
|------------|----------|  
|Error|![Icône d’interface utilisateur : erreur](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "Icône d’interface utilisateur : erreur")|  
|Critique pour les performances|![Icône d’interface utilisateur : avertissement](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icône d’interface utilisateur : avertissement")|  
|Expire bientôt/Expiré|![Icône d’interface utilisateur : avertissement](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icône d’interface utilisateur : avertissement")|  
|Abonnement non initialisé|![Icône d’interface utilisateur : avertissement](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icône d’interface utilisateur : avertissement")|  
|Nouvelle tentative de la commande qui a échoué|![Icône d’interface utilisateur : nouvelle tentative de l’agent de réplication](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "Icône d’interface utilisateur : nouvelle tentative de l’agent de réplication")|  
|Non exécuté|![Icône d’interface utilisateur : agent de réplication arrêté](../../../relational-databases/replication/monitor/media/repl-icon-stopped.gif "Icône d’interface utilisateur : agent de réplication arrêté")|  
|Exécution en cours|![Icône d’interface utilisateur : agent de réplication en cours d’exécution](../../../relational-databases/replication/monitor/media/repl-icon-running.gif "Icône d’interface utilisateur : agent de réplication en cours d’exécution")|  
  
### <a name="merge-subscriptions"></a>Abonnements de fusion  
  
|État|Icône|  
|------------|----------|  
|Error|![Icône d’interface utilisateur : erreur](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "Icône d’interface utilisateur : erreur")|  
|Critique pour les performances|![Icône d’interface utilisateur : avertissement](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icône d’interface utilisateur : avertissement")|  
|Fusion longue|![Icône d’interface utilisateur : avertissement](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icône d’interface utilisateur : avertissement")|  
|Expire bientôt/Expiré|![Icône d’interface utilisateur : avertissement](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icône d’interface utilisateur : avertissement")|  
|Abonnement non initialisé|![Icône d’interface utilisateur : avertissement](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icône d’interface utilisateur : avertissement")|  
|Nouvelle tentative de la commande qui a échoué|![Icône d’interface utilisateur : nouvelle tentative de l’agent de réplication](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "Icône d’interface utilisateur : nouvelle tentative de l’agent de réplication")|  
|Synchronisation|![Icône d’interface utilisateur : agent de réplication en cours d’exécution](../../../relational-databases/replication/monitor/media/repl-icon-running.gif "Icône d’interface utilisateur : agent de réplication en cours d’exécution")|  
|Sans synchronisation|![Icône d’interface utilisateur : agent de réplication arrêté](../../../relational-databases/replication/monitor/media/repl-icon-stopped.gif "Icône d’interface utilisateur : agent de réplication arrêté")|  
  
### <a name="snapshot-subscriptions"></a>Abonnements d'instantanés  
  
|État|Icône|  
|------------|----------|  
|Error|![Icône d’interface utilisateur : erreur](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "Icône d’interface utilisateur : erreur")|  
|Expire bientôt/Expiré|![Icône d’interface utilisateur : avertissement](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icône d’interface utilisateur : avertissement")|  
|Abonnement non initialisé|![Icône d’interface utilisateur : avertissement](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icône d’interface utilisateur : avertissement")|  
|Nouvelle tentative de la commande qui a échoué|![Icône d’interface utilisateur : nouvelle tentative de l’agent de réplication](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "Icône d’interface utilisateur : nouvelle tentative de l’agent de réplication")|  
|Synchronisation|![Icône d’interface utilisateur : agent de réplication en cours d’exécution](../../../relational-databases/replication/monitor/media/repl-icon-running.gif "Icône d’interface utilisateur : agent de réplication en cours d’exécution")|  
|Sans synchronisation|![Icône d’interface utilisateur : agent de réplication arrêté](../../../relational-databases/replication/monitor/media/repl-icon-stopped.gif "Icône d’interface utilisateur : agent de réplication arrêté")|  
  
## <a name="see-also"></a> Voir aussi  
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md)  
  
  
