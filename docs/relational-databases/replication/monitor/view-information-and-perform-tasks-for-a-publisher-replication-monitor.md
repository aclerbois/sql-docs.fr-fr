---
title: Afficher des informations et effectuer des tâches pour un serveur de publication (moniteur de réplication) | Microsoft Docs
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
- Publishers [SQL Server replication], Replication Monitor tasks
- viewing Publisher information
- Publishers [SQL Server replication], viewing information
ms.assetid: 1e777e95-377a-4de3-b965-867464aadaaf
caps.latest.revision: 37
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 57f47623fa4d75588eb5f882fa96da97a755131e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-information-and-perform-tasks-for-a-publisher-replication-monitor"></a>Afficher des informations et exécuter des tâches pour un serveur de publication (moniteur de réplication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le moniteur de réplication contient les onglets ci-après qui donnent des informations sur le serveur de publication sélectionné :  
  
-   **Publications**  
  
     Cet onglet donne des informations sur toutes les publications sur le serveur de publication sélectionné.  
  
-   **Liste de suivi des abonnements**  
  
     Cet onglet est destiné à afficher des informations sur les abonnements de toutes les publications disponibles sur le serveur de publication sélectionné, qui ont des erreurs, des avertissements ou les performances les plus faibles. Cet onglet n'est pas affiché pour les serveurs de distribution exécutant des versions antérieures à [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
-   Onglet**Agents**   
  
     Cet onglet affiche les informations détaillées sur les agents et les travaux utilisés par tous les types de réplication. L'onglet permet également de démarrer et d'arrêter chaque agent ou travail.  
  
 Pour plus d'informations sur les options proposées dans chaque onglet, cliquez sur l'onglet dans le volet droit, puis sur **Aide** dans la barre de menus. Pour plus d’informations sur le démarrage du moniteur de réplication, consultez [Démarrer le moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="to-view-information-and-perform-tasks-for-a-publisher"></a>Pour afficher des informations et effectuer des tâches pour un serveur de publication  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, puis développez un serveur de publication.  
  
2.  Pour afficher des informations sur toutes les publications, cliquez sur l'onglet **Publications** .  
  
3.  Pour consulter des informations sur les abonnements, cliquez sur l'onglet **Liste de suivi des abonnements** . Vous pouvez également accéder à des informations plus détaillées et effectuer des tâches à partir de cet onglet :  
  
    -   Pour afficher des informations détaillées sur l'agent associé à un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher les détails**.  
  
    -   Pour afficher les propriétés d'un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Propriétés**.  
  
    -   Pour synchroniser un abonnement par envoi de données (push), cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Démarrer la synchronisation**.  
  
    -   Pour réinitialiser un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Réinitialiser l'abonnement**.  
  
4.  Pour consulter les informations sur les agents, cliquez sur l'onglet **Agents** . Vous pouvez aussi accéder à des informations plus détaillées et effectuer des tâches sur cet onglet :  
  
    -   Pour afficher des informations détaillées sur un agent (telles que des messages d'informations et des messages d'erreur), cliquez avec le bouton droit sur l'agent, puis cliquez sur **Afficher les détails**.  
  
    -   Pour afficher des informations détaillées sur le travail qui exécute l'agent (par exemple, des détails sur la planification, les étapes de travail, etc.), cliquez avec le bouton droit sur l'agent, puis cliquez sur **Propriétés**.  
  
    -   Pour gérer des profils pour l'agent, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Profils d'agent**. Pour plus d’informations, voir [Utiliser des profils d’Agent de réplication](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  
  
    -   Pour démarrer un agent qui n'est pas en cours d'exécution, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Démarrer l'agent**.  
  
    -   Pour arrêter un agent qui est en cours d'exécution, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Arrêter l'agent**.  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md)  
  
  
