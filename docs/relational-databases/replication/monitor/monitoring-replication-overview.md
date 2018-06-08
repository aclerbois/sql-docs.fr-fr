---
title: Surveillance de la réplication | Microsoft Docs
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
- Replication Monitor, about Replication Monitor
ms.assetid: 81f596d2-27a5-489d-bf8d-0f4361decd02
caps.latest.revision: 37
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 657895d87dc7a2ebcb3e60b47426cf343cc30e0a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="monitoring-replication-overview"></a>Vue d'ensemble sur la surveillance de la réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le Moniteur de réplication[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est un outil graphique vous permettant d'analyser la santé globale d'une topologie de réplication. Le Moniteur de réplication fournit des informations détaillées sur l'état et les performances des publications et abonnements, vous permettant de répondre aux questions les plus fréquentes :  
  
-   L'intégrité de mon système de réplication est-elle de bonne qualité ?  
  
-   Quels sont les abonnements lents ?  
  
-   Mon abonnement transactionnel est-il en retard ?  
  
-   Combien de temps mettra une transaction validée maintenant pour atteindre un Abonné avec la réplication transactionnelle ?  
  
-   Pourquoi mon abonnement de fusion est-il lent ?  
  
-   Pourquoi un Agent de fonctionne pas ?  
  
 Pour analyser la réplication, un utilisateur doit être membre du rôle serveur fixe **sysadmin** sur le serveur de distribution ou membre du rôle de base de données fixe **replmonitor** dans la base de données de distribution. Un administrateur système peut ajouter n'importe quel utilisateur au rôle **replmonitor** , ce qui lui permet de visionner l'activité de réplication dans le Moniteur de réplication ; cependant, l'utilisateur ne peut pas administrer la réplication.  
  
## <a name="in-this-section"></a>Dans cette section  
 Les rubriques suivantes fournissent des informations sur les fonctionnalités du Moniteur de réplication.  
  
 [Présentation de l’interface du moniteur de réplication](../../../relational-databases/replication/monitor/overview-of-the-replication-monitor-interface.md)  
 Décrit chaque fenêtre et onglet du Moniteur de réplication et fournit des informations sur la façon de répondre aux questions listées ci-dessus.  
  
 [Démarrer le moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md)  
 Explique comment démarrer le Moniteur de réplication.  
  
 [Autoriser des non-administrateurs à utiliser le moniteur de réplication](../../../relational-databases/replication/monitor/allow-non-administrators-to-use-replication-monitor.md)  
 Explique comment affecter des autorisations aux non-administrateurs afin qu'ils puissent utiliser le Moniteur de réplication.  
  
 [Ajouter et supprimer des serveurs de publication à partir du moniteur de réplication](../../../relational-databases/replication/monitor/add-and-remove-publishers-from-replication-monitor.md)  
 Explique comment ajouter ou supprimer des serveurs de publication à partir du Moniteur de réplication.  
  
 [Actualiser des données dans le moniteur de réplication](../../../relational-databases/replication/monitor/refresh-data-in-replication-monitor.md)  
 Explique comment actualiser des données dans le Moniteur de réplication.  
  
 [Analyser les performances avec le moniteur de réplication](../../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)  
 Décrit comment analyser les performances dans le Moniteur de réplication, y compris les informations sur le paramétrage des seuils de performance. Inclut des informations sur les statistiques, qui fournissent un affichage détaillé du traitement, au niveau des articles pour la réplication de fusion.  
  
 [Mesurer la latence et valider les connexions pour la réplication transactionnelle](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
 Décrit les jetons de suivi qui vous permettent de mesurer les performances d'une topologie de réplication transactionnelle.  
  
 [Surveiller les agents de réplication](../../../relational-databases/replication/monitor/monitor-replication-agents.md)  
 Décrit comment rechercher des informations sur chaque Agent de réplication.  
  
 [Définir des seuils et des avertissements dans le moniteur de réplication](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)  
 Décrit les avertissements, seuils et alertes que vous pouvez définir dans le Moniteur de réplication. Il est recommandé d'activer les avertissements pour votre topologie, afin que vous soyez informés de l'état et des performances en temps voulu.  
  
 [Mise en cache, actualisation et performances du moniteur de réplication](../../../relational-databases/replication/monitor/caching-refresh-and-replication-monitor-performance.md)  
 Décrit la façon dont le Moniteur de réplication met les données et les calculs dans le cache afin d'améliorer les performances ; explique le lien entre l'actualisation de l'interface utilisateur et du cache.  
  
 [Afficher l’état des publications et des abonnements dans le moniteur de réplication](../../../relational-databases/replication/monitor/view-publication-and-subscription-status-in-replication-monitor.md)  
 Explique comment afficher les informations d'état d'une publication ou d'un abonnement à l'aide du Moniteur de réplication.  
  
 [Afficher des informations et effectuer des tâches pour un serveur de publication &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-publisher-replication-monitor.md)  
 Explique comment afficher des informations et exécuter des tâches pour un serveur de publication à l'aide du Moniteur de réplication.  
  
 [Afficher des informations et effectuer des tâches pour une publication &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-publication-replication-monitor.md)  
 Explique comment afficher des informations et exécuter des tâches pour une publication à l'aide du Moniteur de réplication.  
  
 [Afficher des informations et effectuer des tâches pour les agents associés à une publication &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-publication-agents.md)  
 Explique comment afficher des informations et effectuer des tâches pour les agents associés à une publication à l'aide du Moniteur de réplication.  
  
 [Afficher des informations et effectuer des tâches pour un abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-subscription-replication-monitor.md)  
 Explique comment afficher des informations et exécuter des tâches pour un abonnement à l'aide du Moniteur de réplication.  
  
 [Afficher des informations et exécuter des tâches pour les agents associés à un abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md)  
 Explique comment afficher des informations et effectuer des tâches pour les agents associés à un abonnement à l'aide du Moniteur de réplication.  
  
  
