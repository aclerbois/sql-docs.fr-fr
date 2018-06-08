---
title: Basculer entre les modes de mise à jour d’un abonnement transactionnel pouvant être mis à jour | Microsoft Docs
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
- transactional replication, updatable subscriptions
- updatable subscriptions, update modes
- subscriptions [SQL Server replication], updatable
ms.assetid: ab5ebab1-7ee4-41f4-999b-b4f0c420c921
caps.latest.revision: 38
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 4bf0695819d8f4a75a974a0235de092976281a51
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="switch-between-update-modes-for-an-updatable-transactional-subscription"></a>Basculer entre les modes de mise à jour d'un abonnement transactionnel pouvant être mis à jour
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment basculer entre les modes de mise à jour d'un abonnement transactionnel pouvant être mis à jour dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Spécifiez le mode des abonnements pouvant être mis à jour à l'aide de l'Assistant Nouvel abonnement. Pour plus d’informations sur la définition de ce mode lors de l’utilisation de cet Assistant, consultez [Afficher et modifier les propriétés d’un abonnement par extraction](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
-   **Pour basculer entre les modes de mise à jour d'un abonnement transactionnel pouvant être mis à jour à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   Vous pouvez basculer à tout instant d’une mise à jour immédiate vers une mise à jour en file d’attente. Après que vous avez basculé, cependant, vous ne pouvez pas repasser en mise à jour immédiate avant que l'abonné et le serveur de publication ne soient connectés et que l'Agent de lecture de la file d'attente n'ait appliqué tous les messages en attente dans la file d'attente sur le serveur de publication.  
  
###  <a name="Recommendations"></a> Recommandations  
  
-   Lorsqu'un abonnement avec mise à jour à une publication transactionnelle prend en charge le basculement d'un mode de mise à jour vers un autre, vous pouvez basculer par programmation les modes de mise à jour pour gérer les situations où la connectivité change pendant une courte période de temps. Le mode de mise à jour peut être défini par programmation et à la demande à l'aide de procédures stockées de réplication. Pour plus d’informations, consultez [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
> [!NOTE]  
>  Pour modifier le mode de mise à jour après que l'abonnement a été créé, vous devez affecter à la propriété **update_mode** la valeur **failover** (qui permet de basculer de la mise à jour immédiate vers la mise à jour en attente) ou **queued failover** (qui permet de basculer de la mise à jour en attente vers la mise à jour immédiate) lors de la création de l'abonnement. Ces propriétés sont automatiquement définies dans l'Assistant Nouvel abonnement.  
  
#### <a name="to-set-the-updating-mode-for-a-push-subscription"></a>Pour définir le mode de mise à jour d'un abonnement envoyé  
  
1.  Connectez-vous à l'Abonné dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Abonnements locaux** .  
  
3.  Cliquez avec le bouton droit sur l'abonnement dont vous voulez définir le mode de mise à jour puis cliquez sur **Définir la méthode de mise à jour**.  
  
4.  Dans la boîte de dialogue **Définir la méthode de mise à jour - \<Abonné> : \<Base_de_données_d’abonnement>**, sélectionnez **Mise à jour immédiate** ou **Mise à jour en attente**.  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-set-the-updating-mode-for-a-pull-subscription"></a>Pour définir le mode de mise à jour d'un abonnement extrait  
  
1.  Dans la boîte de dialogue **Propriétés de l’abonnement - \<Serveur_de_publication> : \<Base_de_données_de_publication>**, sélectionnez une valeur **Répliquer les modifications immédiatement** ou **Mettre les modifications en file d’attente** pour l’option **Méthode de mise à jour de l’Abonné**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
 Pour plus d’informations sur l’accès à la boîte de dialogue **Propriétés de l’abonnement - \<Serveur_de_publication>: \<Base_de_données_de_publication>**, consultez [Afficher et modifier les propriétés d’un abonnement par extraction](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-switch-between-update-modes"></a>Pour basculer d'un mode de mise à jour vers un autre  
  
1.  Vérifiez que l'abonnement prend en charge le basculement en exécutant [sp_helppullsubscription](../../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md) pour un abonnement par extraction ou [sp_helpsubscription](../../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md) pour un abonnement par émission de données. Si la valeur de **mode de mise à jour** dans le jeu de résultats est **3** ou **4**, le basculement est pris en charge.  
  
2.  Sur l'Abonné de la base de données d'abonnement, exécutez [sp_setreplfailovermode](../../../relational-databases/system-stored-procedures/sp-setreplfailovermode-transact-sql.md). Spécifiez **@publisher**, **@publisher_db**, **@publication**, et l'une des valeurs suivantes pour **@failover_mode**:  
  
    -   **mis en file d'attente** - basculement sur la mise à jour en attente lorsque la connectivité a été perdue temporairement.  
  
    -   **immédiat** - basculement sur la mise à jour immédiate lorsque la connectivité a été restaurée.  
  
## <a name="see-also"></a> Voir aussi  
 [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)  
  
  
