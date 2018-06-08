---
title: Afficher des informations et effectuer des tâches pour les agents d’abonnement | Microsoft Docs
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
- agents [SQL Server replication], viewing information
- viewing replication agent information
- agents [SQL Server replication], tasks in Replication Monitor
ms.assetid: fbb59d31-2424-4552-9195-0da8d83e755f
caps.latest.revision: 37
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: f98dd86491561fb67197a02e228ec1b151e6e3ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-information-and-perform-tasks-for-subscription-agents"></a>Afficher des informations et effectuer des tâches pour les agents d’abonnement
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le moniteur de réplication contient deux onglets qui permettent d'accéder à des informations sur le ou les agents associés à un abonnement :  
  
-   **Tous les abonnements**  
  
     Cet onglet montre des informations sur tous les abonnements à la publication sélectionnée.  
  
-   **Liste de suivi des abonnements**  
  
     Cet onglet est destiné à afficher des informations sur les abonnements de toutes les publications disponibles sur le serveur de publication sélectionné, qui ont des erreurs, des avertissements ou les performances les plus faibles. Cet onglet n'est pas affiché pour les serveurs de distribution exécutant des versions antérieures à [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
 Pour plus d'informations sur les options de chaque onglet, cliquez sur l'onglet dans le volet de droite puis cliquez sur l'option **Aide** dans la barre de menus. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="to-view-information-and-perform-tasks-for-the-agents-associated-with-a-subscription-all-subscriptions-tab"></a>Pour afficher des informations et effectuer des tâches pour les agents associés à un abonnement (Onglet Tous les abonnements)  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.  
  
2.  Cliquez sur l'onglet **Tous les abonnements** pour afficher des informations sur les abonnements. Vous pouvez aussi accéder à des informations plus détaillées et effectuer des tâches sur cet onglet :  
  
    -   Pour afficher des informations détaillées sur l'agent associé à un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher les détails**. Ces informations détaillées sont les suivantes : messages d'historique et d'erreur de l'agent, statistiques de performances pour la réplication transactionnelle et statistiques de synchronisation au niveau des articles pour la réplication de fusion.  
  
         Les onglets de la fenêtre des détails qui est ouverte dépendent du type d'abonnement : pour les abonnements d'instantané, l'onglet est **Historique du serveur de distribution vers l'Abonné**; pour les abonnements transactionnels, les onglets sont **Historique du serveur de publication vers le serveur de distribution**, **Historique du serveur de distribution vers l'Abonné**et **Commandes non distribuées**; pour les abonnements de fusion, l'onglet est **Historique de synchronisation**.  
  
    -   Pour synchroniser un abonnement par envoi de données (push), cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Démarrer la synchronisation**.  
  
    -   Pour réinitialiser un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Réinitialiser l'abonnement**.  
  
    -   Pour valider un seul abonnement de fusion, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Valider l'abonnement**. Pour valider tous les abonnements à une publication de fusion, cliquez avec le bouton droit sur la publication, puis cliquez sur **Valide tous les abonnements**; pour valider tous les abonnements pour une publication transactionnelle, cliquez avec le bouton droit sur la publication, puis cliquez sur **Valider les abonnements**.  
  
    -   Pour gérer des profils pour l'agent, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Profils d'agent**. Pour plus d’informations, voir [Utiliser des profils d’Agent de réplication](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  
  
### <a name="to-view-information-and-perform-tasks-for-the-agents-associated-with-a-subscription-subscription-watch-list-tab"></a>Pour afficher des informations et effectuer des tâches pour les agents associés à un abonnement (Onglet Liste de suivi des abonnements)  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, puis développez un serveur de publication.  
  
2.  Cliquez sur l'onglet **Liste de suivi des abonnements** pour afficher des informations sur les abonnements. Vous pouvez aussi accéder à des informations plus détaillées et effectuer des tâches sur cet onglet :  
  
    -   Pour afficher des informations détaillées sur l'agent associé à un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher les détails**. Ces informations détaillées sont les suivantes : messages d'historique et d'erreur de l'agent, statistiques de performances pour la réplication transactionnelle et statistiques de synchronisation au niveau des articles pour la réplication de fusion.  
  
         Les onglets de la fenêtre des détails qui est ouverte dépendent du type d'abonnement : pour les abonnements d'instantané, l'onglet est **Historique du serveur de distribution vers l'Abonné**; pour les abonnements transactionnels, les onglets sont **Historique du serveur de publication vers le serveur de distribution**, **Historique du serveur de distribution vers l'Abonné**et **Performances**; pour les abonnements de fusion, l'onglet est **Historique de synchronisation**.  
  
    -   Pour synchroniser un abonnement par envoi de données (push), cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Démarrer la synchronisation**.  
  
    -   Pour réinitialiser un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Réinitialiser l'abonnement**.  
  
    -   Pour valider un seul abonnement de fusion, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Valider l'abonnement**. Pour valider tous les abonnements à une publication de fusion, cliquez avec le bouton droit sur la publication, puis cliquez sur **Valide tous les abonnements**; pour valider tous les abonnements pour une publication transactionnelle, cliquez avec le bouton droit sur la publication, puis cliquez sur **Valider les abonnements**.  
  
    -   Pour gérer des profils pour l'agent, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Profils d'agent**. Pour plus d’informations, voir [Utiliser des profils d’Agent de réplication](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher des informations et effectuer des tâches pour un abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-subscription-replication-monitor.md)   
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md)  
  
  
