---
title: Désactiver la publication et la distribution | Microsoft Docs
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
- disabling publishing
- publishing [SQL Server replication], disabling
- distribution disabling [SQL Server replication]
- removing replication
- replication [SQL Server], removing
- disabling replication
- disabling distribution
ms.assetid: 6d4a1474-4d13-4826-8be2-80050fafa8a5
caps.latest.revision: 41
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 1ba752dbcf4131864b3ce8e3891723bc1ac6096f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="disable-publishing-and-distribution"></a>Désactiver la publication et la distribution
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment désactiver la publication et la distribution dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).  
  
 Vous pouvez effectuer les opérations suivantes :  
  
-   Supprimer toutes les bases de données de distribution sur le serveur de distribution.  
  
-   Désactiver tous les serveurs de publication utilisant le serveur de distribution et supprimer toutes les publications situées sur ces serveurs de publication.  
  
-   Supprimer tous les abonnements aux publications. Les données des bases de données de publication et d'abonnement ne sont pas supprimées ; elles perdent toutefois leur lien de synchronisation aux bases de données de publication. Si vous souhaitez supprimer les données de l'Abonné, faites-le manuellement.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Conditions préalables](#Prerequisites)  
  
-   **Pour désactiver la publication et la distribution à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
  
-   Pour désactiver la publication et la distribution, toutes les bases de données de distribution et de publication doivent être en ligne. S'il existe des *instantanés de base de données* pour les bases de données de distribution ou de publication, vous devez les supprimer avant de désactiver la publication et la distribution. Un instantané de base de données est une copie hors ligne en lecture seule d'une base de données et n'a pas de lien avec un instantané de réplication. Pour plus d’informations, consultez [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Désactivez la publication et la distribution à l'aide de l'Assistant Désactivation de publication et de distribution.  
  
#### <a name="to-disable-publishing-and-distribution"></a>Pour désactiver la publication et la distribution  
  
1.  Connectez-vous au serveur de publication ou au serveur de distribution que vous souhaitez désactiver dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2.  Cliquez avec le bouton droit sur le dossier **Réplication** , puis cliquez sur **Désactiver la publication et la distribution**.  
  
3.  Exécutez les étapes de l'Assistant Désactivation de la publication et de la distribution.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 La publication et la distribution peuvent être désactivées par programme à l'aide des procédures stockées de réplication.  
  
#### <a name="to-disable-publishing-and-distribution"></a>Pour désactiver la publication et la distribution  
  
1.  Arrêtez tous les travaux liés à la réplication. Pour obtenir la liste des noms de travaux, consultez la section « Sécurité de l'Agent dans l'Agent SQL Server » de [Modèle de sécurité de l'Agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
2.  Exécutez [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) sur la base de données d'abonnement de chaque Abonné pour supprimer des objets de réplication de la base de données. Cette procédure stockée ne supprimera pas les travaux de réplication sur le serveur de distribution.  
  
3.  Exécutez [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) sur la base de données de publication du serveur de publication pour supprimer des objets de réplication de la base de données.  
  
4.  Si le serveur de publication utilise un serveur de distribution distant, exécutez [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md).  
  
5.  Sur le serveur de distribution, exécutez [sp_dropdistpublisher](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md). Cette procédure stockée doit être exécutée une fois pour chaque serveur de publication inscrit sur le serveur de distribution.  
  
6.  Sur le serveur de distribution, exécutez [sp_dropdistributiondb](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md) pour supprimer la base de données de distribution. Cette procédure stockée doit être exécutée une fois pour chaque base de données de distribution sur le serveur de distribution. Cela supprime également tous les travaux de l'Agent de lecture de la file d'attente associés à la base de données de distribution.  
  
7.  Sur le serveur de distribution, exécutez [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md) pour supprimer la désignation de serveur de distribution du serveur.  
  
    > [!NOTE]  
    >  Si tous les objets de publication et de distribution de la réplication ne sont pas supprimés avant l'exécution de [sp_dropdistpublisher](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md) et [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md), ces procédures retourneront une erreur. Pour supprimer tous les objets liés à la réplication lorsqu'un serveur de publication ou un serveur de distribution est supprimé, le paramètre **@no_checks** doit avoir la valeur **1**. Si un serveur de publication ou un serveur de distribution est hors connexion ou inaccessible, la valeur **@ignore_distributor** peut être affectée au paramètre **1** afin qu'ils puissent être supprimés ; toutefois, tous les objets de publication et de distribution restants doivent être supprimés manuellement.  
  
###  <a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Cet exemple de script supprime des objets de réplication de la base de données d'abonnement.  
  
 [!code-sql[HowTo#sp_removedbreplication](../../relational-databases/replication/codesnippet/tsql/disable-publishing-and-d_1.sql)]  
  
 Cet exemple de script désactive la publication et la distribution sur un serveur faisant office de serveur de publication et de serveur de distribution et supprime la base de données de distribution.  
  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/disable-publishing-and-d_2.sql)]  
  
##  <a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
  
#### <a name="to-disable-publishing-and-distribution"></a>Pour désactiver la publication et la distribution  
  
1.  Supprimez tous les abonnements aux publications qui utilisent le serveur de distribution. Pour plus d'informations, consultez [Delete a Pull Subscription](../../relational-databases/replication/delete-a-pull-subscription.md) et [Delete a Push Subscription](../../relational-databases/replication/delete-a-push-subscription.md).  
  
2.  Supprimez toutes les publications qui utilisent le serveur de distribution et désactivez la publication pour toutes les bases de données si le serveur de publication et le serveur de distribution se trouvent sur le même serveur. Pour plus d'informations, voir [Delete a Publication](../../relational-databases/replication/publish/delete-a-publication.md).  
  
3.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
4.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.DistributionPublisher> . Spécifiez la propriété <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> et passez l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l'étape 3.  
  
5.  (Facultatif) Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet et vérifier que le serveur de publication existe. Si cette méthode retourne **false**, le nom du serveur de publication défini à l'étape 4 est incorrect ou le serveur de publication n'est pas utilisé par ce serveur de distribution.  
  
6.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Remove%2A> . Passez la valeur **true** pour *force* si le serveur de publication et le serveur de distribution se trouvent sur des serveurs différents, et lorsque le serveur de publication doit être désinstallé au niveau du serveur de distribution sans commencer par vérifier que les publications n'existent plus sur le serveur de publication.  
  
7.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationServer> . Passez l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l'étape 3.  
  
8.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationServer.UninstallDistributor%2A> . Passez la valeur **true** pour *force* pour supprimer tous les objets de réplication au niveau du serveur de distribution sans commencer par vérifier que toutes les bases de données de publication locales ont été désactivées et que les bases de données de distribution ont été désinstallées.  
  
###  <a name="PShellExample"></a> Exemples (RMO)  
 Cet exemple supprime l'inscription du serveur de publication au niveau du serveur de distribution, supprime la base de données de distribution et désinstalle le serveur de distribution.  
  
 [!code-cs[HowTo#rmo_DropDistPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropdistpub)]  
  
 [!code-vb[HowTo#rmo_vb_DropDistPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropdistpub)]  
  
 Cet exemple désinstalle le serveur de distribution sans commencer par désactiver les bases de données de publication locales ni supprimer la base de données de distribution.  
  
 [!code-cs[HowTo#rmo_DropDistPubForce](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropdistpubforce)]  
  
 [!code-vb[HowTo#rmo_vb_DropDistPubForce](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropdistpubforce)]  
  
## <a name="see-also"></a> Voir aussi  
 [Concepts liés à RMO (Replication Management Objects)](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Concepts liés aux procédures stockées système de réplication](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
  
