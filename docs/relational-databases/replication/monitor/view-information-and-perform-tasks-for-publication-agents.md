---
title: Afficher des informations et effectuer des tâches pour les agents de publication | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
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
ms.assetid: 2a420da2-66f4-4650-9bdd-1992221ed3fd
caps.latest.revision: 39
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 2337a96f2c09070a98baf65e4d85e094615ec2ee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-information-and-perform-tasks-for-publication-agents"></a>Afficher des informations et effectuer des tâches pour les agents de publication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le moniteur de réplication fournit l'onglet **Agents** qui contient des informations sur les agents associés à la publication sélectionnée. L’agent de distribution et l’Agent de fusion sont associés à des abonnement (pour plus d’informations, consultez [Afficher des informations et effectuer des tâches pour les agents d’abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md)).  
  
 Cet onglet affiche des informations sur les agents suivants :  
  
-   Agent d'instantané, utilisé par toutes les publications  
  
-   Agent de lecture du journal, utilisé par toutes les publications transactionnelles  
  
-   Agent de lecture de la file d'attente, utilisé par les publications transactionnelles qui possèdent des abonnements mis à jour en attente  
  
 Pour afficher plus d'informations sur les options de cet onglet, sélectionnez **Aide** sur la barre de menus. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="to-view-information-and-perform-tasks-for-the-agents-associated-with-a-publication"></a>Pour afficher des informations et effectuer des tâches pour les agents associés à une publication  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.  
  
2.  Pour consulter les informations sur les agents, cliquez sur l'onglet **Agents** . Vous pouvez aussi accéder à des informations plus détaillées et effectuer des tâches sur cet onglet :  
  
    -   Pour afficher des informations détaillées sur un agent (telles que des messages d'informations et des messages d'erreur), cliquez avec le bouton droit sur l'agent, puis cliquez sur **Afficher les détails**.  
  
    -   Pour afficher des informations détaillées sur le travail qui exécute l'agent (par exemple, des détails sur la planification, les étapes de travail, etc.), cliquez avec le bouton droit sur l'agent, puis cliquez sur **Propriétés**.  
  
    -   Pour gérer des profils pour l'agent, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Profils d'agent**. Pour plus d’informations, voir [Utiliser des profils d’Agent de réplication](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  
  
    -   Pour démarrer un agent qui n'est pas en cours d'exécution, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Démarrer l'agent**.  
  
    -   Pour arrêter un agent qui est en cours d'exécution, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Arrêter l'agent**.  
  
## <a name="see-also"></a> Voir aussi  
 [Définir des seuils et des avertissements dans le moniteur de réplication](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)   
 [Afficher des informations et effectuer des tâches pour une publication &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-publication-replication-monitor.md)   
 [Administration de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-administration.md)   
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md)  
  
  
