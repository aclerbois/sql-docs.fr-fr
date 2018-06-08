---
title: Créer un abonnement pouvant être mis à jour pour une publication transactionnelle | Microsoft Docs
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
- updateable transactional subscriptions, T-SQL
ms.assetid: a6e80857-0a69-4867-b6b7-f3629d00c312
caps.latest.revision: 6
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 8f65e800ebc68f635bbfbbf5d8cd0c8e0acdfbf7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-updatable-subscription-to-transactional-publication"></a>Créer un abonnement pouvant être mis à jour pour une publication transactionnelle
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
> [!NOTE]  
>  Cette fonctionnalité reste prise en charge dans les versions de [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] (2012 à 2016).  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
 
La réplication transactionnelle permet de propager sur le serveur de publication les modifications apportées au niveau d'un Abonné en utilisant des abonnements avec mise à jour immédiate ou mise à jour en file d'attente. Vous pouvez créer par programme un abonnement avec mise à jour en utilisant des procédures stockées de réplication. (Voir également [Créer un abonnement pouvant être mis à jour pour une publication transactionnelle (Management Studio)](../../../relational-databases/replication/publish/create-an-updatable-subscription-to-a-transactional-publication.md).) 

## <a name="to-create-an-immediate-updating-pull-subscription"></a>Pour créer un abonnement par extraction avec mise à jour immédiate ##

1. Sur le serveur de publication, vérifiez que la publication prend en charge les abonnements avec mise à jour immédiate en exécutant [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). 

    * Si la valeur de `allow_sync_tran` dans le jeu de résultats est `1`, la publication prend en charge les abonnements avec mise à jour immédiate.

    * Si la valeur de `allow_sync_tran` dans le jeu de résultats est `0`, la publication doit être recréée en activant la prise en charge des abonnements avec mise à jour immédiate.

2. Sur le serveur de publication, vérifiez que la publication prend en charge les abonnements par extraction en exécutant [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). 

    * Si la valeur de `allow_pull` dans le jeu de résultats est `1`, la publication prend en charge les abonnements par extraction.

    * Si la valeur de `allow_pull` est `0`, exécutez [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md), en spécifiant `allow_pull` pour `@property` , et `true` pour `@value`. 

3. Sur l’Abonné, exécutez [sp_addpullsubscription](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md). Spécifiez `@publisher` et `@publication`, et l’une des valeurs suivantes pour `@update_mode`:

    * `sync tran` - active l’abonnement pour la mise à jour immédiate.

    * `failover` - active l’abonnement pour la mise à jour immédiate avec mise à jour en file d’attente sous forme d’option de basculement.

    > [!NOTE]  
>  `failover` requiert que la publication soit également activée pour les abonnements avec mise à jour en file d’attente. 
 
4. Sur l’Abonné, exécutez [sp_addpullsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Spécifiez les éléments suivants :

    * Les paramètres `@publisher`, `@publisher_db`et `@publication` . 

    * Les informations d’identification Microsoft Windows sous lesquelles l’Agent de distribution est exécuté sur l’abonné pour `@job_login` et `@job_password`. 

    > [!NOTE]  
>  Les connexions établies à l’aide de l’authentification intégrée Windows utilisent toujours les informations d’identification Windows spécifiées par `@job_login` et `@job_password`. L'Agent de distribution établit toujours la connexion locale à l'Abonné à l'aide de l'authentification intégrée Windows. Par défaut, l'agent se connecte au serveur de distribution à l'aide de l'authentification intégrée Windows. 
 
    * (Facultatif) Une valeur de `0` pour `@distributor_security_mode` et les informations de connexion Microsoft SQL Server pour `@distributor_login` et `@distributor_password`, si vous devez utiliser l’authentification SQL Server lors de la connexion du distributeur. 

    * Planification du travail de l'Agent de distribution pour cet abonnement. 

5. Dans la base de données d’abonnement de l’Abonné, exécutez [sp_link_publication](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md). Spécifiez `@publisher`, `@publication`, le nom de la base de données de publication pour `@publisher_db`, et l’une des valeurs suivantes pour `@security_mode`: 

    * `0` - Utiliser l’authentification SQL Server pour effectuer des mises à jour sur le serveur de publication. Avec cette option, vous devez spécifier une connexion valide sur le serveur de publication pour `@login` et `@password`.

    * `1` - Utiliser le contexte de sécurité de l’utilisateur qui apporte des modifications à l’Abonné lors de la connexion au serveur de publication. Consultez [sp_link_publication](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md) pour connaître les restrictions en rapport avec ce mode de sécurité.

    * `2` - Utiliser une connexion de serveur lié existante, définie par l’utilisateur, créée à l’aide de [sp_addlinkedserver](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).

6. Sur le serveur de publication, exécutez [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) en spécifiant `@publication`, `@subscriber`, `@destination_db`, une valeur d’extraction pour `@subscription_type`, et la valeur spécifiée à l’étape 3 pour `@update_mode`.

L'abonnement par extraction est alors inscrit sur le serveur de publication. 


## <a name="to-create-an-immediate-updating-push-subscription"></a>Pour créer un abonnement par émission de données avec mise à jour immédiate ##

1. Sur le serveur de publication, vérifiez que la publication prend en charge les abonnements avec mise à jour immédiate en exécutant [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). 

    * Si la valeur de `allow_sync_tran` dans le jeu de résultats est `1`, la publication prend en charge les abonnements avec mise à jour immédiate.

    * Si la valeur de `allow_sync_tran` dans le jeu de résultats est `0`, la publication doit être recréée en activant la prise en charge des abonnements avec mise à jour immédiate.

2. Sur le serveur de publication, vérifiez que la publication prend en charge les abonnements par émission de données en exécutant [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). 

    * Si la valeur de `allow_push` dans le jeu de résultats est `1`, la publication prend en charge les abonnements par émission de données.

    * Si la valeur de `allow_push` est `0`, exécutez [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md), en spécifiant `allow_push` pour `@property` , et `true` pour `@value`. 

3. Sur le serveur de publication, exécutez [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Spécifiez `@publication`, `@subscriber`, `@destination_db`, et l’une des valeurs suivantes pour `@update_mode`:

    * `sync tran` - active la prise en charge de la mise à jour immédiate.

    * `failover` - active la prise en charge de la mise à jour immédiate avec mise à jour en file d’attente comme option de basculement.

    > [!NOTE]  
>  `failover` requiert que la publication soit également activée pour les abonnements avec mise à jour en file d’attente. 
 
4. Sur le serveur de publication, exécutez [sp_addpushsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md). Spécifiez les paramètres suivants :

    * `@subscriber`, `@subscriber_db`et `@publication`. 

    * Informations d’identification Windows sous lesquelles l’Agent de distribution s’exécute sur le serveur de distribution pour `@job_login` et `@job_password`. 

    > [!NOTE]  
>  Les connexions établies à l’aide de l’authentification intégrée Windows utilisent toujours les informations d’identification Windows spécifiées par `@job_login` et `@job_password`. L'Agent de distribution crée toujours la connexion locale au serveur de distribution à l'aide de l'authentification intégrée Windows. Par défaut, l'agent se connecte à l'Abonné à l'aide de ces informations. 

    * (Facultatif) Une valeur de `0` pour `@subscriber_security_mode` et les informations de connexion SQL Server pour `@subscriber_login` et `@subscriber_password`, si vous devez utiliser l’authentification SQL Server lors de la connexion de l’abonné. 

    * Planification du travail de l'Agent de distribution pour cet abonnement.

5. Dans la base de données d’abonnement de l’Abonné, exécutez [sp_link_publication](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md). Spécifiez `@publisher`, `@publication`, le nom de la base de données de publication pour `@publisher_db`, et l’une des valeurs suivantes pour `@security_mode`: 

     * `0` - Utiliser l’authentification SQL Server pour effectuer des mises à jour sur le serveur de publication. Avec cette option, vous devez spécifier une connexion valide sur le serveur de publication pour `@login` et `@password`.

     * `1` - Utiliser le contexte de sécurité de l’utilisateur qui apporte des modifications à l’Abonné lors de la connexion au serveur de publication. Consultez [sp_link_publication](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md) pour connaître les restrictions en rapport avec ce mode de sécurité.

     * `2` - Utiliser une connexion de serveur lié existante, définie par l’utilisateur, créée à l’aide de [sp_addlinkedserver](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).


## <a name="to-create-a-queued-updating-pull-subscription"></a>Pour créer un abonnement par extraction avec mise à jour en file d'attente ##

1. Sur le serveur de publication, vérifiez que la publication prend en charge les abonnements avec mise à jour en file d’attente en exécutant [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). 

    * Si la valeur de `allow_queued_tran` dans le jeu de résultats est `1`, la publication prend en charge les abonnements avec mise à jour immédiate.

    * Si la valeur de `allow_queued_tran` dans le jeu de résultats est `0`, la publication doit être recréée en activant la prise en charge des abonnements avec mise à jour en file d’attente activée.

2. Sur le serveur de publication, vérifiez que la publication prend en charge les abonnements par extraction en exécutant [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). 

    * Si la valeur de `allow_pull` dans le jeu de résultats est `1`, la publication prend en charge les abonnements par extraction.

    * Si la valeur de `allow_pull` est `0`, exécutez [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md), en spécifiant `allow_pull` pour `@property` , et `true` pour `@value`. 

3. Sur l’Abonné, exécutez [sp_addpullsubscription](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md). Spécifiez `@publisher` et `@publication`, et l’une des valeurs suivantes pour `@update_mode`:

    * `queued tran` - active l’abonnement pour la mise à jour en attente.

    * `queued failover` - active la prise en charge de la mise à jour en file d’attente avec mise à jour immédiate comme option de basculement.

    > [!NOTE]  
>  `queued failover` requiert que la publication soit également activée pour les abonnements avec mise à jour immédiate. Pour basculer sur la mise à jour immédiate, vous devez utiliser [sp_link_publication](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md) pour définir les informations d’identification sous lesquelles les modifications au niveau de l’Abonné sont répliquées sur le serveur de publication.
 
4. Sur l’Abonné, exécutez [sp_addpullsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Spécifiez les paramètres suivants :

    * @publisher, `@publisher_db`et `@publication`. 

    * Les informations d’identification Windows sous lesquelles l’Agent de distribution est exécuté sur l’abonné pour `@job_login` et `@job_password`. 

    > [!NOTE]  
>  Les connexions établies à l’aide de l’authentification intégrée Windows utilisent toujours les informations d’identification Windows spécifiées par `@job_login` et `@job_password`. L'Agent de distribution établit toujours la connexion locale à l'Abonné à l'aide de l'authentification intégrée Windows. Par défaut, l'agent se connecte au serveur de distribution à l'aide de l'authentification intégrée Windows. 
 
    * (Facultatif) Une valeur de `0` pour `@distributor_security_mode` et les informations de connexion SQL Server pour `@distributor_login` et `@distributor_password`, si vous devez utiliser l’authentification SQL Server lors de la connexion du distributeur. 

    * Planification du travail de l'Agent de distribution pour cet abonnement.

5. Sur le serveur de publication, exécutez [sp_addsubscriber](../../../relational-databases/system-stored-procedures/sp-addsubscriber-transact-sql.md) pour enregistrer l’abonné sur le serveur de publication en spécifiant `@publication`, `@subscriber`, `@destination_db`, une valeur d’extraction pour `@subscription_type`, et la valeur spécifiée à l’étape 3 pour `@update_mode`.

L'abonnement par extraction est alors inscrit sur le serveur de publication. 


## <a name="to-create-a-queued-updating-push-subscription"></a>Pour créer un abonnement par émission de données avec mise à jour en file d'attente ##

1. Sur le serveur de publication, vérifiez que la publication prend en charge les abonnements avec mise à jour en file d’attente en exécutant [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). 

    * Si la valeur de allow_queued_tran dans le jeu de résultats est 1, la publication prend en charge les abonnements avec mise à jour immédiate.

    * Si la valeur de allow_queued_tran dans le jeu de résultats est 0, la publication doit être recréée en activant la prise en charge des abonnements avec mise à jour en file d’attente. Pour plus d’informations, voir Procédure : activer les abonnements avec mise à jour pour les publications transactionnelles (programmation Transact-SQL de la réplication).

2. Sur le serveur de publication, vérifiez que la publication prend en charge les abonnements par émission de données en exécutant [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). 

    * Si la valeur de `allow_push` dans le jeu de résultats est `1`, la publication prend en charge les abonnements par émission de données.

    * Si la valeur de `allow_push` est `0`, exécutez [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md), en spécifiant allow_push pour `@property` , et `true` pour `@value`. 

3. Sur le serveur de publication, exécutez [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Spécifiez `@publication`, `@subscriber`, `@destination_db`, et l’une des valeurs suivantes pour `@update_mode`:

    * `queued tran` - active l’abonnement pour la mise à jour en attente.

    * `queued failover` - active la prise en charge de la mise à jour en file d’attente avec mise à jour immédiate comme option de basculement.

    > [!NOTE]  
>  L’option queued failover requiert que la publication soit également activée pour les abonnements avec mise à jour immédiate. Pour basculer sur la mise à jour immédiate, vous devez utiliser [sp_link_publication](../../../relational-databases/system-stored-procedures/sp-link-publication-transact-sql.md) pour définir les informations d’identification sous lesquelles les modifications au niveau de l’Abonné sont répliquées sur le serveur de publication.

4. Sur le serveur de publication, exécutez [sp_addpushsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md). Spécifiez les paramètres suivants :

    * `@subscriber`, `@subscriber_db`et `@publication`. 

    * Informations d’identification Windows sous lesquelles l’Agent de distribution s’exécute sur le serveur de distribution pour `@job_login` et `@job_password`. 

    > [!NOTE]  
>  Les connexions établies à l’aide de l’authentification intégrée Windows utilisent toujours les informations d’identification Windows spécifiées par `@job_login` et `@job_password`. L'Agent de distribution crée toujours la connexion locale au serveur de distribution à l'aide de l'authentification intégrée Windows. Par défaut, l’agent se connecte à l’Abonné à l’aide de ces informations. 
 
    * (Facultatif) Une valeur de `0` pour `@subscriber_security_mode` et les informations de connexion SQL Server pour `@subscriber_login` et `@subscriber_password`, si vous devez utiliser l’authentification SQL Server lors de la connexion de l’abonné. 

    * Planification du travail de l'Agent de distribution pour cet abonnement.


## <a name="example"></a> Exemple ##

Cet exemple crée un abonnement par extraction avec mise à jour immédiate à une publication qui prend en charge les abonnements avec mise à jour immédiate. Les valeurs de connexion et le mot de passe sont fournis lors de l'exécution à l'aide des variables de script sqlcmd.

> [!NOTE]  
>  Ce script utilise des variables de script sqlcmd. Elles sont au format `$(MyVariable)`. Pour plus d’informations sur l’utilisation des variables de script sur la ligne de commande et dans SQL Server Management Studio, consultez la section **Exécution de scripts de réplication** dans la rubrique [Concepts liés aux procédures stockées système de réplication](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md).

```
-- Execute this batch at the Subscriber.
DECLARE @publication AS sysname;
DECLARE @publicationDB AS sysname;
DECLARE @publisher AS sysname;
DECLARE @login AS sysname;
DECLARE @password AS nvarchar(512);
SET @publication = N'AdvWorksProductTran';
SET @publicationDB = N'AdventureWorks2008R2';
SET @publisher = $(PubServer);
SET @login = $(Login);
SET @password = $(Password);

-- At the subscription database, create a pull subscription to a transactional 
-- publication using immediate updating with queued updating as a failover.
EXEC sp_addpullsubscription 
    @publisher = @publisher, 
    @publication = @publication, 
    @publisher_db = @publicationDB, 
    @update_mode = N'failover', 
    @subscription_type = N'pull';

-- Add an agent job to synchronize the pull subscription, 
-- which uses Windows Authentication when connecting to the Distributor.
EXEC sp_addpullsubscription_agent 
    @publisher = @publisher, 
    @publisher_db = @publicationDB, 
    @publication = @publication,
    @job_login = @login,
    @job_password = @password; 

-- Add a Windows Authentication-based linked server that enables the 
-- Subscriber-side triggers to make updates at the Publisher. 
EXEC sp_link_publication 
    @publisher = @publisher, 
    @publication = @publication,
    @publisher_db = @publicationDB, 
    @security_mode = 0,
    @login = @login,
    @password = @password;
GO

USE AdventureWorks2008R2;
GO

-- Execute this batch at the Publisher.
DECLARE @publication AS sysname;
DECLARE @subscriptionDB AS sysname;
DECLARE @subscriber AS sysname;
SET @publication = N'AdvWorksProductTran'; 
SET @subscriptionDB = N'AdventureWorks2008R2Replica'; 
SET @subscriber = $(SubServer);

-- At the Publisher, register the subscription, using the defaults.
USE [AdventureWorks2008R2]
EXEC sp_addsubscription 
    @publication = @publication, 
    @subscriber = @subscriber, 
    @destination_db = @subscriptionDB, 
    @subscription_type = N'pull', 
    @update_mode = N'failover';
GO
```

## <a name="see-also"></a> Voir aussi ##

[Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)

[Utiliser sqlcmd avec des variables de script](../../../relational-databases/scripting/sqlcmd-use-with-scripting-variables.md)

[Créer un abonnement pouvant être mis à jour pour une publication transactionnelle (Management Studio)](../../../relational-databases/replication/publish/create-an-updatable-subscription-to-a-transactional-publication.md)

