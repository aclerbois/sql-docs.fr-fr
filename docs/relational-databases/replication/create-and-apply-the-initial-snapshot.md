---
title: Créer et appliquer l’instantané initial | Microsoft Docs
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
- snapshots [SQL Server replication], creating
- snapshot replication [SQL Server], initial snapshots
ms.assetid: 742727a1-5189-44ec-b3ae-6fd7aa1f5347
caps.latest.revision: 44
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: dca228017f3ef4efff0309ac5f6b1af0a9006e97
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-and-apply-the-initial-snapshot"></a>Créer et appliquer l'instantané initial
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment créer et appliquer l'instantané initial dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou des objets RMO (Replication Management Objects). Les publications de fusion qui utilisent des filtres paramétrés nécessitent un instantané en deux parties. Pour plus d'informations, voir [Créer un instantané d’une publication de fusion avec des filtres paramétrés](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
 **Dans cette rubrique**  
  
-   **Pour créer et appliquer l'instantané initial à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Par défaut, si l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution, un instantané est généré par l'Agent d'instantané immédiatement après la création d'une publication par l'Assistant Nouvelle publication. Il est ensuite appliqué par défaut par l'Agent de distribution (pour la réplication d'instantané et transactionnelle) ou l'Agent de fusion (pour les abonnements de fusion) pour tous les abonnements. Il est également possible de générer un instantané à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et du Moniteur de réplication. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-create-a-snapshot-in-management-studio"></a>Pour créer un instantané dans Management Studio  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Cliquez avec le bouton droit sur la réplication pour laquelle vous souhaitez créer un instantané, puis cliquez sur **Afficher l'état de l'Agent d'instantané**.  
  
4.  Dans la boîte de dialogue **Afficher l’état de l’Agent d’instantané - \<Publication>**, cliquez sur **Démarrer**.  
  
 Lorsque l'Agent d'instantané termine la génération de l'instantané, un message comme celui-ci s'affiche : « [100 %] Un instantané de 17 article(s) a été généré ».  
  
#### <a name="to-create-a-snapshot-in-replication-monitor"></a>Pour créer un instantané dans le Moniteur de réplication  
  
1.  Dans le moniteur de réplication, développez un groupe de serveurs de publication dans le volet gauche, puis développez un serveur de publication.  
  
2.  Cliquez avec le bouton droit sur la réplication pour laquelle vous souhaitez créer un instantané, puis cliquez sur **Générer l'instantané**.  
  
3.  Pour afficher le statut de l'Agent d'instantané, cliquez sur l'onglet **Agents** . Pour des informations plus détaillées, cliquez avec le bouton droit sur l'Agent d'instantané dans la grille, puis cliquez sur **Afficher les détails**.  
  
#### <a name="to-apply-a-snapshot"></a>Pour appliquer un instantané  
  
1.  Une fois que l'instantané est généré, il est appliqué en synchronisant l'abonnement avec l'Agent de distribution ou l'Agent de fusion :  
  
    -   Si l'Agent est paramétré pour une exécution en continu (par défaut pour la réplication transactionnelle); l'instantané est appliqué automatiquement après avoir été généré.  
  
    -   Si l'Agent est paramétré pour une exécution planifiée, l'instantané est appliqué à la prochaine exécution planifiée de l'Agent.  
  
    -   Si l'Agent est paramétré pour une exécution à la demande, l'instantané est appliqué la prochaine fois que vous exécutez l'Agent.  
  
     Pour plus d'informations sur la synchronisation des abonnements, consultez [Synchroniser un abonnement par émission (Push](../../relational-databases/replication/synchronize-a-push-subscription.md) et [Synchroniser un abonnement par extraction (Pull)](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Les instantanés initiaux peuvent être créés par programme en créant et exécutant un travail de l'Agent d'instantané ou en exécutant le fichier exécutable de l'Agent d'instantané à partir d'un fichier de commandes. Une fois qu'un instantané initial a été généré, il est transféré et appliqué sur l'Abonné lorsque l'abonnement est synchronisé pour la première fois. Si vous exécutez l'Agent d'instantané à partir d'une invite de commandes ou d'un fichier de commandes, vous devrez exécuter de nouveau l'agent chaque fois que l'instantané existant deviendra non valide.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
#### <a name="to-create-and-run-a-snapshot-agent-job-to-generate-the-initial-snapshot"></a>Pour créer et exécuter un travail de l'Agent d'instantané pour générer l'instantané initial  
  
1.  Créez une publication d'instantané, transactionnelle ou de fusion. Pour plus d’informations, consultez [Créer une publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Exécutez [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Spécifiez **@publication** et les paramètres suivants :  
  
    -   **@job_login, qui spécifie** les informations d’identification de l’authentification Windows qu’utilise l’Agent d’instantané pour s’exécuter sur le serveur de distribution.  
  
    -   **@job_password**, qui correspond au mot de passe pour les informations d’identification Windows fournies.  
  
    -   (Facultatif) La valeur **0** pour **@publisher_security_mode** si l'agent utilisera l'authentification SQL Server lors de la connexion au serveur de publication. Dans ce cas, vous devez également spécifier les informations de connexion d'authentification SQL Server pour **@publisher_login** et **@publisher_password**.  
  
    -   (Facultatif) Une planification de synchronisation pour le travail de l'Agent d'instantané. Pour plus d'informations, voir [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
3.  Ajoutez des articles à la publication. Pour plus d'informations, voir [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
4.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_startpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-startpublication-snapshot-transact-sql.md), en spécifiant la valeur **@publication** créée à l’étape 1.  
  
#### <a name="to-run-the-snapshot-agent-to-generate-the-initial-snapshot"></a>Pour exécuter l'Agent d'instantané afin de générer l'instantané initial  
  
1.  Créez une publication d'instantané, transactionnelle ou de fusion. Pour plus d’informations, voir [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Ajoutez des articles à la publication. Pour plus d'informations, voir [Define an Article](../../relational-databases/replication/publish/define-an-article.md).  
  
3.  À partir de l'invite de commandes ou d'un fichier de commandes, démarrez l' [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md) en exécutant **snapshot.exe**, en spécifiant les arguments de ligne de commande suivants :  
  
    -   **-Publication**  
  
    -   **-Publisher**  
  
    -   **-Distributor**  
  
    -   **-PublisherDB**  
  
    -   **-ReplicationType**  
  
     Si vous utilisez l'authentification SQL Server, vous devez également spécifier les arguments suivants :  
  
    -   **-DistributorLogin**  
  
    -   **-DistributorPassword**  
  
    -   **-DistributorSecurityMode** = **0**  
  
    -   **-PublisherLogin**  
  
    -   **-PublisherPassword**  
  
    -   **-PublisherSecurityMode** = **0**  
  
###  <a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Cet exemple montre comment créer une publication transactionnelle et ajouter un travail de l'Agent d'instantané pour la nouvelle publication (à l'aide des variables de script **sqlcmd** ). L'exemple démarre également le travail.  
  
 [!code-sql[HowTo#sp_trangenerate_snapshot](../../relational-databases/replication/codesnippet/tsql/create-and-apply-the-ini_1.sql)]  
  
 Cet exemple crée une publication de fusion et ajoute un travail de l'Agent d'instantané (à l'aide des variables **sqlcmd** ) pour la publication. Cet exemple démarre également le travail.  
  
 [!code-sql[HowTo#sp_mergegenerate_snapshot](../../relational-databases/replication/codesnippet/tsql/create-and-apply-the-ini_2.sql)]  
  
 Les arguments de ligne de commande suivants démarrent l'Agent d'instantané pour générer l'instantané pour une publication de fusion.  
  
> [!NOTE]  
>  Des sauts de ligne ont été ajoutés pour améliorer la lisibilité. Dans un fichier de commandes, les commandes doivent figurer sur une seule ligne.  
  
```  
  
REM -- Declare variables  
SET Publisher=%InstanceName%  
SET PublicationDB=AdventureWorks2012   
SET Publication=AdvWorksSalesOrdersMerge   
  
REM --Start the Snapshot Agent to generate the snapshot for AdvWorksSalesOrdersMerge.  
"C:\Program Files\Microsoft SQL Server\120\COM\SNAPSHOT.EXE" -Publication %Publication%   
-Publisher %Publisher% -Distributor %Publisher% -PublisherDB %PublicationDB%   
-ReplicationType 2 -OutputVerboseLevel 1 -DistributorSecurityMode 1  
  
```  
  
##  <a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 L'Agent d'instantané génère des instantanés après qu'une publication a été créée. Vous pouvez générer ces instantanés par programme en utilisant les Replication Management Objects et l'accès direct par code managé aux fonctionnalités de l'Agent de réplication. Les objets à utiliser dépendent du type de réplication. L'Agent d'instantané peut être démarré de façon synchrone à l'aide de l'objet <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> ou de façon asynchrone à l'aide du travail de l'agent. Une fois l'instantané initial généré, il est transféré et appliqué sur l'Abonné lorsque l'abonnement est synchronisé pour la première fois. Vous devrez exécuter de nouveau l'agent chaque fois que l'instantané existant ne contiendra plus de données valides à jour. Pour plus d’informations, consultez [Gestion des publications](../../relational-databases/replication/publish/maintain-publications.md).  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez stocker des informations d'identification, utilisez les [Services de chiffrement](http://go.microsoft.com/fwlink/?LinkId=34733) fournis par [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-generate-the-initial-snapshot-for-a-snapshot-or-transactional-publication-by-starting-the-snapshot-agent-job-asynchronous"></a>Pour générer l'instantané initial pour une publication transactionnelle ou d'instantané en démarrant le travail de l'Agent d'instantané (asynchrone)  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPublication> . Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publication, et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec la connexion créée à l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour charger les propriétés restantes de l'objet. Si cette méthode retourne **false**, soit les propriétés de la publication ont été définies de manière incorrecte à l'étape 2, soit la publication n'existe pas.  
  
4.  Si <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> a la valeur **false**, appelez <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> pour créer le travail de l'Agent d'instantané pour cette publication.  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> pour démarrer le travail de l'agent qui génère l'instantané pour cette publication.  
  
6.  (Facultatif) Lorsque <xref:Microsoft.SqlServer.Replication.TransPublication.SnapshotAvailable%2A> a la valeur **true**, l'instantané est à la disposition des Abonnés.  
  
#### <a name="to-generate-the-initial-snapshot-for-a-snapshot-or-transactional-publication-by-running-the-snapshot-agent-synchronous"></a>Pour générer l'instantané initial pour une publication transactionnelle ou d'instantané en exécutant l'Agent d'instantané (synchrone)  
  
1.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> et définissez les propriétés requises suivantes :  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> – nom du serveur de publication  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> – nom de la base de données de publication  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> – nom de la publication  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> – nom du serveur de distribution  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> – valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> pour utiliser l'authentification Windows lors de la connexion au serveur de publication ou valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> et valeurs pour <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherLogin%2A> et <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherPassword%2A> pour utiliser l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la connexion au serveur de publication. L'authentification Windows est recommandée.  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> – valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> pour utiliser l'authentification Windows lors de la connexion au serveur de distribution ou valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> et valeurs pour <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorLogin%2A> et <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorPassword%2A> pour utiliser l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la connexion au serveur de distribution. L'authentification Windows est recommandée.  
  
2.  Spécifiez la valeur <xref:Microsoft.SqlServer.Replication.ReplicationType.Transactional> ou <xref:Microsoft.SqlServer.Replication.ReplicationType.Snapshot> pour <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> .  
  
#### <a name="to-generate-the-initial-snapshot-for-a-merge-publication-by-starting-the-snapshot-agent-job-asynchronous"></a>Pour générer l'instantané initial pour une publication de fusion en démarrant le travail de l'Agent d'instantané (asynchrone)  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication> . Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publication, et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec la connexion créée à l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour charger les propriétés restantes de l'objet. Si cette méthode retourne **false**, soit les propriétés de la publication ont été définies de manière incorrecte à l'étape 2, soit la publication n'existe pas.  
  
4.  Si <xref:Microsoft.SqlServer.Replication.Publication.SnapshotAgentExists%2A> a la valeur **false**, appelez <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> pour créer le travail de l'Agent d'instantané pour cette publication.  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Publication.StartSnapshotGenerationAgentJob%2A> pour démarrer le travail de l'agent qui génère l'instantané pour cette publication.  
  
6.  (Facultatif) Lorsque <xref:Microsoft.SqlServer.Replication.MergePublication.SnapshotAvailable%2A> a la valeur **true**, l'instantané est à la disposition des Abonnés.  
  
#### <a name="to-generate-the-initial-snapshot-for-a-merge-publication-by-running-the-snapshot-agent-synchronous"></a>Pour générer l'instantané initial pour une publication de fusion en exécutant l'Agent d'instantané (synchrone)  
  
1.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent> et définissez les propriétés requises suivantes :  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publisher%2A> – nom du serveur de publication  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherDatabase%2A> – nom de la base de données de publication  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Publication%2A> – nom de la publication  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.Distributor%2A> – nom du serveur de distribution  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherSecurityMode%2A> – valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> pour utiliser l'authentification Windows lors de la connexion au serveur de publication ou valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> et valeurs pour <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherLogin%2A> et <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.PublisherPassword%2A> pour utiliser l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la connexion au serveur de publication. L'authentification Windows est recommandée.  
  
    -   <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorSecurityMode%2A> – valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Integrated> pour utiliser l'authentification Windows lors de la connexion au serveur de distribution ou valeur <xref:Microsoft.SqlServer.Replication.SecurityMode.Standard> et valeurs pour <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorLogin%2A> et <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.DistributorPassword%2A> pour utiliser l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la connexion au serveur de distribution. L'authentification Windows est recommandée.  
  
2.  Spécifiez la valeur <xref:Microsoft.SqlServer.Replication.ReplicationType.Merge> pour <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.ReplicationType%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.SnapshotGenerationAgent.GenerateSnapshot%2A> .  
  
###  <a name="PShellExample"></a> Exemples (RMO)  
 Cet exemple exécute de façon synchrone l'Agent d'instantané pour générer l'instantané initial pour une publication transactionnelle.  
  
 [!code-cs[HowTo#rmo_GenerateSnapshot](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatesnapshot)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateSnapshot](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatesnapshot)]  
  
 Cet exemple démarre de façon asynchrone le travail de l'agent pour générer l'instantané initial pour une publication transactionnelle.  
  
 [!code-cs[HowTo#rmo_GenerateSnapshot_WithJob](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_generatesnapshot_withjob)]  
  
 [!code-vb[HowTo#rmo_vb_GenerateSnapshot_WithJob](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_generatesnapshot_withjob)]  
  
## <a name="see-also"></a> Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md)   
 [Créer et appliquer l’instantané](../../relational-databases/replication/create-and-apply-the-snapshot.md)   
 [Initialiser un abonnement avec un instantané](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)   
 [Concepts liés à RMO (Replication Management Objects)](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Utiliser sqlcmd avec des variables de script](../../relational-databases/scripting/sqlcmd-use-with-scripting-variables.md)  
  
  
