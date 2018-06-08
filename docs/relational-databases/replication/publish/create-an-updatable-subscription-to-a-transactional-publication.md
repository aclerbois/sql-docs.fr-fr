---
title: Créer un abonnement pouvant être mis à jour pour une publication transactionnelle | Microsoft Docs
ms.custom: ''
ms.date: 07/21/2016
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
- updatable transactional subscriptions
- updateable transactional subscriptions, SSMS
ms.assetid: f9ef89ed-36f6-431b-8843-25d445ec137f
caps.latest.revision: 51
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 24ad78478cfeaae74792bc129baf675614aad737
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-an-updatable-subscription-to-a-transactional-publication"></a>Créer un abonnement pouvant être mis à jour pour une publication transactionnelle
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
> [!NOTE]  
>  Cette fonctionnalité reste prise en charge dans les versions de [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] (2012 à 2016).  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
 
Configurez des abonnements pouvant être mis à jour dans la page **Abonnements pouvant être mis à jour** de l’**Assistant Nouvel abonnement**. Cette page est disponible seulement si vous avez activé une publication transactionnelle pour les abonnements pouvant être mis à jour. Pour plus d’informations sur l’activation des abonnements pouvant être mis à jour, consultez [Activer la mise à jour d’abonnements pour les publications transactionnelles](../../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md).   
  
## <a name="to-configure-an-updatable-subscription-from-the-publisher"></a>Pour configurer un abonnement pouvant être mis à jour à partir du serveur de publication  

1. Connectez-vous au serveur de publication dans Microsoft SQL Server Management Studio, puis développez le nœud du serveur.

2. Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .

3. Cliquez avec le bouton droit sur une publication transactionnelle activée pour la mise à jour d’abonnements, puis cliquez sur **Nouveaux abonnements**.

4. Suivez les pages de l'Assistant pour spécifier les options de l'abonnement, par exemple où l'Agent de distribution doit s'exécuter.

5. Dans la page **Abonnements pouvant être mis à jour** de l’**Assistant Nouvel abonnement**, vérifiez que **Répliquer** est sélectionné.

6. Sélectionnez une option dans la liste déroulante **Valider sur le serveur de publication** :

    * Pour utiliser des abonnements mis à jour immédiatement, sélectionnez **Enregistrer les modifications simultanément**. Si vous sélectionnez cette option et que la publication autorise les abonnements mis à jour en attente (l’option par défaut pour les publications créées avec l’Assistant Nouvelle publication), la propriété d’abonnement **update_mode** a la valeur **failover**. Ce mode vous permet de passer ultérieurement en mise à jour en attente si nécessaire.

    * Pour utiliser des abonnements mis à jour en attente, sélectionnez **Mettre les modifications en file d’attente et valider dès que possible**. Si vous sélectionnez cette option et que la publication autorise les abonnements mis à jour immédiatement (l’option par défaut pour les publications créées avec l’Assistant Nouvelle publication), et si l’abonné exécute SQL Server 2005 ou une version ultérieure, la propriété d’abonnement **update_mode** a la valeur queued failover. Ce mode vous permet de passer ultérieurement en mise à jour immédiate si nécessaire.

    Pour plus d’informations sur le basculement entre les modes de mise à jour, consultez [Basculer entre les modes de mise à jour d’un abonnement transactionnel pouvant être mis à jour](../../../relational-databases/replication/administration/switch-between-update-modes-for-an-updatable-transactional-subscription.md).

7. La page **Nom d’accès aux abonnements pouvant être mis à jour** est affichée pour les abonnements qui utilisent la mise à jour immédiate ou dont la propriété **update_mode** a la valeur **queued failover**. Dans la page **Nom d’accès aux abonnements pouvant être mis à jour**, spécifiez un serveur lié via lequel sont effectuées les connexions au serveur de publication pour les abonnements mis à jour immédiatement. Les connexions sont utilisées par les déclencheurs qui s'exécutent pour l'abonné et propagent les modifications sur le serveur de publication. Sélectionnez l'une des options suivantes :

    * **Créer un serveur lié qui se connecte par Authentification SQL Server**. Sélectionnez cette option si vous n'avez pas défini un serveur distant ou un serveur lié entre l'Abonné et le serveur de publication. La réplication crée un serveur lié pour vous. Le compte que vous spécifiez doit déjà exister sur le serveur de publication.

    * **Utiliser un serveur lié ou un serveur distant que vous avez déjà défini.** Sélectionnez cette option si vous avez défini un serveur distant ou un serveur lié entre l’abonné et le serveur de publication à l’aide de [sp_addserver (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md), [sp_addlinkedserver (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md), SQL Server Management Studio ou d’une autre méthode.

    Pour plus d’informations sur les autorisations requises par le compte du serveur lié, consultez la section **Abonnements mis à jour en attente** de [saisissez la description de lien ici](../../../relational-databases/replication/security/secure-the-subscriber.md).

8. Terminez l'Assistant.

## <a name="to-configure-an-updatable-subscription-from-the-subscriber"></a>Pour configurer un abonnement pouvant être mis à jour à partir de l'Abonné


1. Connectez-vous à l’abonné dans SQL Server Management Studio, puis développez le nœud du serveur.

2. Développez le dossier **Réplication** .

3. Cliquez avec le bouton droit sur le dossier **Abonnements locaux** , puis cliquez sur **Nouveaux abonnements**.

4. Dans la page **Publication** de l’**Assistant Nouvel abonnement**, sélectionnez **Rechercher un serveur de publication SQL Server** dans la liste déroulante **Serveur de publication**.

5. Connectez-vous au serveur de publication dans la boîte de dialogue **Se connecter au serveur** .

6. Sélectionnez une publication transactionnelle activée pour la mise à jour d’abonnements dans la page **Publication**.

7. Suivez les pages de l'Assistant pour spécifier les options de l'abonnement, par exemple où l'Agent de distribution doit s'exécuter.

8. Dans la page **Abonnements pouvant être mis à jour** de l’Assistant Nouvel abonnement, vérifiez que **Répliquer** est sélectionné.

9. Sélectionnez une option dans la liste déroulante **Valider sur le serveur de publication** :

    * Pour utiliser des abonnements mis à jour immédiatement, sélectionnez **Enregistrer les modifications simultanément**. Si vous sélectionnez cette option et que la publication autorise les abonnements mis à jour en attente (l’option par défaut pour les publications créées avec l’Assistant Nouvelle publication), la propriété d’abonnement **update_mode** a la valeur **failover**. Ce mode vous permet de passer ultérieurement en mise à jour en attente si nécessaire.

    * Pour utiliser des abonnements mis à jour en attente, sélectionnez **Mettre les modifications en file d’attente et valider dès que possible**. Si vous sélectionnez cette option et que la publication autorise les abonnements mis à jour immédiatement (l’option par défaut pour les publications créées avec l’Assistant Nouvelle publication), et si l’abonné exécute SQL Server 2005 ou une version ultérieure, la propriété d’abonnement **update_mode** a la valeur queued **failover**. Ce mode vous permet de passer ultérieurement en mise à jour immédiate si nécessaire.

    Pour plus d’informations sur le basculement entre les modes de mise à jour, consultez [Basculer entre les modes de mise à jour d’un abonnement transactionnel pouvant être mis à jour](../../../relational-databases/replication/administration/switch-between-update-modes-for-an-updatable-transactional-subscription.md).

10. La page **Nom d’accès aux abonnements pouvant être mis à jour** est affichée pour les abonnements qui utilisent la mise à jour immédiate ou dont la propriété **update_mode** a la valeur queued **failover**. Dans la page **Nom d’accès aux abonnements pouvant être mis à jour**, spécifiez un serveur lié via lequel sont effectuées les connexions au serveur de publication pour les abonnements mis à jour immédiatement. Les connexions sont utilisées par les déclencheurs qui s'exécutent pour l'abonné et propagent les modifications sur le serveur de publication. Sélectionnez l'une des options suivantes :

    * **Créer un serveur lié qui se connecte par Authentification SQL Server**. Sélectionnez cette option si vous n'avez pas défini un serveur distant ou un serveur lié entre l'Abonné et le serveur de publication. La réplication crée un serveur lié pour vous. Le compte que vous spécifiez doit déjà exister sur le serveur de publication.

    * **Utiliser un serveur lié ou un serveur distant que vous avez déjà défini.** Sélectionnez cette option si vous avez défini un serveur distant ou un serveur lié entre l’abonné et le serveur de publication à l’aide de [sp_addserver (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md), [sp_addlinkedserver (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md), SQL Server Management Studio ou d’une autre méthode.

    Pour plus d’informations sur les autorisations requises par le compte du serveur lié, consultez la section **Abonnements mis à jour en attente** de [saisissez la description de lien ici](../../../relational-databases/replication/security/secure-the-subscriber.md).

11. Terminez l'Assistant.

## <a name="see-also"></a> Voir aussi

[Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)

[Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)

[Créer un abonnement pouvant être mis à jour pour une publication transactionnelle à l’aide de Transact-SQL](../../../relational-databases/replication/publish/create-updatable-subscription-to-transactional-publication.md) 

