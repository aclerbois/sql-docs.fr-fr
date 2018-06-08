---
title: Configurer des alertes de réplication prédéfinies (SQL Server Management Studio) | Microsoft Docs
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
- alerts [SQL Server replication]
- predefined replication alerts [SQL Server replication]
ms.assetid: c0414147-7ffe-4f9a-908c-71c1b5201584
caps.latest.revision: 24
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e7bfdd311887970d9df6982e102262da03de47ad
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-predefined-replication-alerts-sql-server-management-studio"></a>configurer des alertes de réplication prédéfinies (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La réplication comporte les alertes prédéfinies suivantes, qui peuvent être configurées pour répondre aux événements de réplication :  
  
-   **Réplication : succès de l'agent**  
  
-   **Réplication : échec de l'agent**  
  
-   **Réplication : nouvelle tentative de l'agent**  
  
-   **Réplication : suppression de l'abonnement expiré**  
  
-   **Réplication : abonnement réinitialisé après l'échec de validation**  
  
-   **Réplication : l'Abonné n'a pas réussi la validation des données**  
  
-   **Réplication : l'Abonné a passé la validation des données**  
  
-   **Réplication : arrêt personnalisé de l'Agent**  
  
 Configurez ces alertes à partir du dossier **Alertes[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] dans**  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] ou de l’onglet **Avertissements** dans le moniteur de réplication. Pour plus d’informations sur l’accès à cet onglet, consultez [Afficher des informations et effectuer des tâches pour un abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-subscription-replication-monitor.md).  
  
 En plus de ces alertes, le moniteur de réplication comprend un ensemble d'avertissements et d'alertes liées aux états et aux performances. Pour plus d’informations, voir [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md). Vous pouvez aussi définir des alertes pour d'autres événements de réplication à l'aide de l'infrastructure d'alertes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Créer un événement défini par l’utilisateur](http://msdn.microsoft.com/library/03d71a35-97fa-4bba-aa9a-23ac9c9cf879).  
  
### <a name="to-configure-a-predefined-replication-alert-in-management-studio"></a>Pour configurer une alerte de réplication prédéfinie dans Management Studio  
  
1.  Connectez-vous au serveur de distribution dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Agent SQL Server** , puis développez le dossier **Alertes** .  
  
3.  Cliquez avec le bouton droit sur une alerte de réplication, puis cliquez sur **Propriétés**.  
  
4.  Définissez les options dans la boîte de dialogue **Propriétés de l’alerte \<nom_alerte>**  :  
  
    -   Sur la page **Général** , cliquez sur **Activer**; spécifiez la base de données à laquelle l'alerte doit s'appliquer.  
  
    -   Sur la page **Réponse** , spécifiez si un courrier électronique doit être envoyé et/ou si un travail doit être effectué.  
  
         Si l'alerte est **Réplication : l'Abonné n'a pas réussi la validation des données**, vous pouvez spécifier le travail de réponse que la réplication fournit pour cette alerte : sélectionnez **Exécuter le travail**, puis cliquez sur le bouton Parcourir (**…**). Dans la boîte de dialogue **Localiser le travail** , cliquez sur **Parcourir**. Dans la boîte de dialogue **Rechercher des objets** , sélectionnez **Réinitialiser les abonnements présentant des erreurs de validation de données**. Cliquez sur **OK** dans les deux boîtes de dialogue ouvertes. Lorsque le travail s'exécute, il utilise un appel de procédure distante (RPC) à une procédure stockée qui réinitialise l'abonnement. Si le serveur de publication utilise un serveur de distribution distant, vous devez définir une connexion serveur distante sur le serveur de publication, pour que l'appel de procédure distante puisse être effectué du serveur de distribution au serveur de publication.  
  
    -   Sur la page **Options** , personnalisez le texte de la réponse.  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-configure-an-alert-for-a-threshold-in-replication-monitor"></a>Pour configurer une alerte de seuil dans le moniteur de réplication  
  
1.  Sous l'onglet **Avertissements** , cliquez sur **Configurer des alertes**.  
  
2.  Dans la boîte de dialogue **Configurer des alertes de réplication** , sélectionnez une alerte puis cliquez sur **Configurer**.  
  
3.  Définissez les options dans la boîte de dialogue **Propriétés de l’alerte \<nom_alerte>**  :  
  
    -   Sur la page **Général** , cliquez sur **Activer**; spécifiez la base de données à laquelle l'alerte doit s'appliquer.  
  
    -   Sur la page **Réponse** , spécifiez si un courrier électronique doit être envoyé et/ou si un travail doit être effectué.  
  
         Si l'alerte est **Réplication : l'Abonné n'a pas réussi la validation des données**, vous pouvez spécifier le travail de réponse que la réplication fournit pour cette alerte : sélectionnez **Exécuter le travail**, puis cliquez sur le bouton Parcourir (**…**). Dans la boîte de dialogue **Localiser le travail** , cliquez sur **Parcourir**. Dans la boîte de dialogue **Rechercher des objets** , sélectionnez **Réinitialiser les abonnements présentant des erreurs de validation de données**. Cliquez sur **OK** dans les deux boîtes de dialogue ouvertes. Lorsque le travail s'exécute, il utilise un appel de procédure distante (RPC) à une procédure stockée qui réinitialise l'abonnement. Si le serveur de publication utilise un serveur de distribution distant, vous devez définir une connexion serveur distante sur le serveur de publication, pour que l'appel de procédure distante puisse être effectué du serveur de distribution au serveur de publication.  
  
    -   Sur la page **Options** , personnalisez le texte de la réponse.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Cliquez sur **Fermer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser les alertes pour les événements des agents de réplication](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md)  
  
  
