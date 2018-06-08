---
title: Exécuter des travaux de maintenance de réplication (SQL Server Management Studio)| Microsoft Docs
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
- jobs [SQL Server replication]
ms.assetid: 0dc485a0-5a50-41eb-a29d-f2b2fb920174
caps.latest.revision: 38
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: bd22b5fbf516f439d9b5e1d66273dc2919ebf0c0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="run-replication-maintenance-jobs-sql-server-management-studio"></a>Exécuter des travaux de maintenance de réplication (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les travaux de maintenance de la réplication sont les suivants :  
  
-   **Réinitialiser les abonnements présentant des erreurs de validation de données**  
  
-   **Nettoyage de l'historique de l'agent : distribution**  
  
-   **Actualisateur d'analyse de réplication pour la distribution.**  
  
-   **Contrôle des agents de réplication**  
  
-   **Nettoyage de la distribution : distribution**  
  
-   **Nettoyage de l'abonnement expiré**  
  
 Démarrez et arrêtez ces travaux à partir du dossier **Travaux** de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] et à partir de l'onglet **Travaux communs** du moniteur de réplication. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md). Affichez et modifiez les propriétés de chaque travail dans la boîte de dialogue **Propriétés du travail - \<Travail>**, disponible à partir du même dossier et du même onglet.  
  
### <a name="to-start-or-stop-a-replication-maintenance-job-in-management-studio"></a>Pour démarrer ou arrêter un travail de maintenance de réplication dans Management Studio  
  
1.  Connectez-vous au serveur de distribution dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Agent SQL Server** puis développez le dossier **Travaux** .  
  
3.  Cliquez avec le bouton droit sur un travail, puis cliquez sur **Démarrer le travail** ou sur **Arrêter le travail**.  
  
### <a name="to-start-or-stop-a-replication-maintenance-job-in-replication-monitor"></a>Pour démarrer ou arrêter un travail de maintenance de réplication dans le moniteur de réplication  
  
1.  Développez un groupe du serveur de publication dans le moniteur de réplication, puis sélectionnez un serveur de publication.  
  
2.  Cliquez sur l'onglet **Agents** .  
  
3.  Cliquez avec le bouton droit sur un travail dans la grille, puis cliquez sur **Démarrer le travail** ou sur **Arrêter le travail**.  
  
### <a name="to-view-and-modify-properties-for-a-replication-maintenance-job-in-management-studio"></a>Pour afficher et modifier les propriétés d'un travail de maintenance de réplication dans Management Studio  
  
1.  Connectez-vous au serveur de distribution dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Agent SQL Server** puis développez le dossier **Travaux** .  
  
3.  Cliquez avec le bouton droit sur un travail, puis sélectionnez **Propriétés**.  
  
4.  Dans la boîte de dialogue **Propriétés du travail - \<Travail>**, modifiez des propriétés si nécessaire, puis cliquez sur **OK**.  
  
### <a name="to-view-and-modify-properties-for-a-replication-maintenance-job-in-replication-monitor"></a>Pour afficher et modifier les propriétés d'un travail de maintenance de réplication dans le moniteur de réplication  
  
1.  Développez un groupe du serveur de publication dans le moniteur de réplication, puis sélectionnez un serveur de publication.  
  
2.  Cliquez sur l'onglet **Agents** .  
  
3.  Cliquez avec le bouton droit sur un travail dans la grille, puis cliquez sur **Propriétés**.  
  
4.  Dans la boîte de dialogue **Propriétés du travail - \<Travail>**, modifiez des propriétés si nécessaire, puis cliquez sur **OK**.  
  
## <a name="see-also"></a> Voir aussi  
 [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)   
 [Afficher des informations et exécuter des tâches pour un serveur de publication &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-publisher-replication-monitor.md)   
 [Administration de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
