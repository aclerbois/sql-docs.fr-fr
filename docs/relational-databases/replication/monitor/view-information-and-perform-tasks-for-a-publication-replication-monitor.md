---
title: Afficher des informations et effectuer des tâches pour une publication (moniteur de réplication) | Microsoft Docs
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
- viewing publication information
- publications [SQL Server replication], viewing information
- publications [SQL Server replication], Replication Monitor tasks
ms.assetid: 92e28a07-d6a7-461b-a0b3-bd9bc6afcbe5
caps.latest.revision: 39
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: fbe11687aefa08102d1b8a74534b53d31f561fe8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-information-and-perform-tasks-for-a-publication-replication-monitor"></a>Afficher des informations et effectuer des tâches relatives à une publication (moniteur de réplication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le moniteur de réplication comporte plusieurs onglets contenant des informations relatives à la publication sélectionnée :  
  
-   **Tous les abonnements**  
  
     Cet onglet montre des informations sur tous les abonnements à la publication sélectionnée.  
  
-   **Agents**  
  
     Cet onglet affiche les informations sur tous les agents utilisés par une publication :  
  
    -   Agent d'instantané, utilisé par toutes les publications  
  
    -   Agent de lecture du journal, utilisé par toutes les publications transactionnelles  
  
    -   Agent de lecture de la file d'attente, utilisé par les publications transactionnelles qui possèdent des abonnements mis à jour en attente  
  
-   **Avertissements** (pour les serveurs de distribution qui exécutent [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et ultérieurement)  
  
    -   Cet onglet permet de spécifier des avertissements et des alertes pour les agents.  
  
-   **Jetons de suivi** (uniquement pour la réplication transactionnelle, sur les serveurs de distribution qui exécutent [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et version ultérieure)  
  
     Cet onglet permet de mesurer la latence, à savoir le temps écoulé entre la validation d'une transaction sur le serveur de publication et la validation de la transaction correspondante sur l'Abonné.  
  
 Pour plus d'informations sur les options de chaque onglet, cliquez sur l'onglet dans le volet de droite puis cliquez sur l'option **Aide** dans la barre de menus. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="to-view-information-and-perform-tasks-for-a-publication"></a>Pour afficher des informations et effectuer des tâches relatives à une publication  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.  
  
2.  Pour afficher et modifier les propriétés de la publication, cliquez avec le bouton droit sur celle-ci puis cliquez sur **Propriétés**.  
  
3.  Pour afficher des informations sur les abonnements, cliquez sur l'onglet **Tous les abonnements** .  
  
     Pour afficher et modifier les propriétés de l'abonnement, cliquez avec le bouton droit sur celui-ci puis cliquez sur **Propriétés**. Vous pouvez également accéder à des informations plus détaillées et effectuer certaines tâches dans cet onglet. Pour plus d’informations, consultez [Afficher des informations et effectuer des tâches pour les agents d’abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
4.  Pour consulter les informations sur les agents, cliquez sur l'onglet **Agents** . Vous pouvez également accéder à des informations plus détaillées et effectuer certaines tâches dans cet onglet. Pour plus d’informations, consultez [Afficher des informations et effectuer des tâches pour les agents de publication &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-publication-agents.md).  
  
5.  Pour afficher les informations sur les avertissements d'agent et les seuils, cliquez sur l'onglet **Avertissements** . Pour plus d’informations, voir [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
  
6.  Pour afficher des informations sur les jetons de suivi, cliquez sur l'onglet **Jetons de suivi** . Pour plus d'informations sur l'utilisation des jetons de suivi, consultez [Mesurer la latence et valider les connexions pour la réplication transactionnelle](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md)  
  
  
