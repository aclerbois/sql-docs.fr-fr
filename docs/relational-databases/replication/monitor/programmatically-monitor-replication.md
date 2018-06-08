---
title: Surveiller la réplication par programmation | Microsoft Docs
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
dev_langs:
- TSQL
helpviewer_keywords:
- sp_replmonitorhelppublisher
- sp_replmonitorhelpmergesessiondetail
- monitoring performance [SQL Server replication], publication status
- sp_replmonitorhelpmergesession
- sp_replmonitorhelppublicationthresholds
- monitoring performance [SQL Server replication], subscription status
- monitoring performance [SQL Server replication], Transact-SQL programming
- sp_replmonitorsubscriptionpendingcmds
- sp_replmonitorchangepublicationthreshold
- transactional replication, monitoring
- sp_replmonitorhelppublication
- sp_replmonitorhelpsubscription
- monitoring performance [SQL Server replication], thresholds and warnings
- merge replication monitoring [SQL Server replication]
- snapshot replication [SQL Server], monitoring
ms.assetid: e8bf8850-8da5-4a4f-a399-64232b4e476d
caps.latest.revision: 34
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: ebc5ab13c00b5af13a68325fc50c252f0ca68f4d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="programmatically-monitor-replication"></a>Surveiller la réplication par programmation
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le moniteur de réplication est un outil graphique permettant d'analyser une topologie de réplication. Vous pouvez accéder par programmation aux mêmes données d'analyse en utilisant les procédures stockées de réplication [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou les objets RMO (Replication Management Objects). Ces objets permettent de programmer les tâches suivantes :  
  
-   Analyser l'état des serveurs de publication, des publications et des abonnements.  
  
-   Analyser les sessions de l'Agent de fusion sur un ou plusieurs Abonnés.  
  
-   Analyser les commandes transactionnelles en attente d'application sur un ou plusieurs Abonnés.  
  
-   Définir la métrique de seuil qui détermine quand une publication nécessite une intervention.  
  
-   Analyser l'état des jetons de suivi.  
  
 **Dans cette rubrique :**  
  
 [Transact-SQL](#Tsql)  
  
 [Objets RMO (Replication Management Objects)](#RMO)  
  
##  <a name="Tsql"></a> Transact-SQL  
  
#### <a name="to-monitor-publishers-publications-and-subscriptions-from-the-distributor"></a>Analyser les serveurs de publication, les publications et les abonnements du serveur de distribution.  
  
1.  Sur le serveur de distribution de la base de données de distribution, exécutez [sp_replmonitorhelppublisher](../../../relational-databases/system-stored-procedures/sp-replmonitorhelppublisher-transact-sql.md). Les informations d'analyse pour tous les serveurs de publication utilisant ce serveur de distribution sont ainsi retournées. Pour limiter le jeu de résultats à un seul serveur de publication, spécifiez **@publisher**.  
  
2.  Sur le serveur de distribution de la base de données de distribution, exécutez [sp_replmonitorhelppublication](../../../relational-databases/system-stored-procedures/sp-replmonitorhelppublication-transact-sql.md). Les informations d'analyse pour tous les serveurs de publication utilisant ce serveur de distribution sont ainsi retournées. Pour limiter le jeu de résultats à un serveur de publication, une publication ou une base de données publiée, spécifiez respectivement **@publisher**, **@publication**ou **@publisher_db**.  
  
3.  Sur le serveur de distribution de la base de données de distribution, exécutez [sp_replmonitorhelpsubscription](../../../relational-databases/system-stored-procedures/sp-replmonitorhelpsubscription-transact-sql.md). Les informations d'analyse pour tous les abonnements utilisant ce serveur de distribution sont ainsi retournées. Pour limiter le jeu de résultats aux abonnements appartenant à un serveur de publication, une publication ou une base de données publiée, spécifiez respectivement **@publisher**, **@publication**ou **@publisher_db**.  
  
#### <a name="to-monitor-transactional-commands-waiting-to-be-applied-at-the-subscriber"></a>Pour analyser les commandes transactionnelles en attente d'application sur l'Abonné  
  
1.  Sur le serveur de distribution de la base de données de distribution, exécutez [sp_replmonitorsubscriptionpendingcmds](../../../relational-databases/system-stored-procedures/sp-replmonitorsubscriptionpendingcmds-transact-sql.md). Les informations d'analyse pour toutes les commandes en attente de tous les abonnements utilisant ce serveur de distribution sont ainsi retournées. Pour limiter le jeu de résultats aux commandes en attente pour les abonnements appartenant à un serveur de publication, un Abonné, une publication ou une base de données publiée, spécifiez respectivement **@publisher**, **@subscriber**, **@publication**ou **@publisher_db**.  
  
#### <a name="to-monitor-merge-changes-waiting-to-be-uploaded-or-downloaded"></a>Pour analyser les modifications de fusion en attente de chargement ou de téléchargement  
  
1.  Dans la base de données de publication du serveur de publication, exécutez [sp_showpendingchanges](../../../relational-databases/system-stored-procedures/sp-showpendingchanges-transact-sql.md). Retourne un jeu de résultats affichant des informations sur les modifications en attente de réplication sur les Abonnés. Pour limiter le jeu de résultats aux modifications qui appartiennent à une publication ou un article unique, spécifiez respectivement **@publication** ou **@article**.  
  
2.  Sur un Abonné de la base de données d'abonnement, exécutez [sp_showpendingchanges](../../../relational-databases/system-stored-procedures/sp-showpendingchanges-transact-sql.md). Retourne un jeu de résultats affichant des informations sur les modifications en attente de réplication sur le serveur de publication. Pour limiter le jeu de résultats aux modifications qui appartiennent à une publication ou un article unique, spécifiez respectivement **@publication** ou **@article**.  
  
#### <a name="to-monitor-merge-agent-sessions"></a>Pour analyser les sessions de l'Agent de fusion  
  
1.  Sur le serveur de distribution de la base de données de distribution, exécutez [sp_replmonitorhelpmergesession](../../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesession-transact-sql.md). Cela retourne les informations d'analyse, y compris **Session_id**, sur toutes les sessions de l'Agent de fusion pour tous les abonnements utilisant ce serveur de distribution. Vous pouvez également obtenir **Session_id** en interrogeant la table système [MSmerge_sessions](../../../relational-databases/system-tables/msmerge-sessions-transact-sql.md) .  
  
2.  Sur le serveur de distribution de la base de données de distribution, exécutez [sp_replmonitorhelpmergesessiondetail](../../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesessiondetail-transact-sql.md). Spécifiez une valeur **Session_id** de l'étape 1 pour **@session_id**. Les informations d'analyse détaillées sur la session sont ainsi affichées.  
  
3.  Répétez l'étape 2 pour chaque session digne d'intérêt.  
  
#### <a name="to-monitor-merge-agent-sessions-for-pull-subscriptions-from-the-subscriber"></a>Pour analyser les sessions de l'Agent de fusion pour les abonnements par extraction de l'Abonné  
  
1.  Sur l'Abonné de la base de données d'abonnement, exécutez [sp_replmonitorhelpmergesession](../../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesession-transact-sql.md). Pour un abonnement donné, spécifiez **@publisher**, **@publication**et le nom de la base de données de publication pour **@publisher_db**. Cela retourne des informations d'analyse sur les cinq dernières sessions de l'Agent de fusion pour cet abonnement. Notez la valeur de la colonne **Session_id** pour toutes les sessions dignes d'intérêt dans le jeu de résultats.  
  
2.  Sur l'Abonné de la base de données d'abonnement, exécutez [sp_replmonitorhelpmergesessiondetail](../../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesessiondetail-transact-sql.md). Spécifiez une valeur **Session_id** de l'étape 1 pour **@session_id**. Les informations d'analyse détaillées sur la session sont ainsi affichées.  
  
3.  Répétez l'étape 2 pour chaque session digne d'intérêt.  
  
#### <a name="to-view-and-modify-the-monitor-threshold-metrics-for-a-publication"></a>Pour afficher et modifier la métrique du seuil d'analyse pour une publication  
  
1.  Sur le serveur de distribution de la base de données de distribution, exécutez [sp_replmonitorhelppublicationthresholds](../../../relational-databases/system-stored-procedures/sp-replmonitorhelppublicationthresholds-transact-sql.md). Les seuils d'analyse définis pour toutes les publications utilisant ce serveur de distribution sont ainsi retournées. Pour limiter le jeu de résultats aux seuils d'analyse des publications appartenant à un serveur de publication, une publication ou une base de données publiée, spécifiez respectivement **@publisher**, **@publisher_db**ou **@publication**. Notez la valeur de **Metric_id** pour les seuils qui doivent être changés. Pour plus d’informations, voir [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
  
2.  Sur le serveur de distribution de la base de données de distribution, exécutez [sp_replmonitorchangepublicationthreshold](../../../relational-databases/system-stored-procedures/sp-replmonitorchangepublicationthreshold-transact-sql.md). Spécifiez les éléments suivants si nécessaire :  
  
    -   La valeur **Metric_id** obtenue dans l'étape 1 pour **@metric_id**.  
  
    -   Une nouvelle valeur pour le métrique du seuil d'analyse pour **@value**.  
  
    -   Une valeur de **1** pour **@shouldalert** pour une alerte à enregistrer lorsque ce seuil est atteint, ou une valeur de **0** si une alerte n'est pas nécessaire.  
  
    -   Une valeur de **1** pour **@mode** afin d'activer la métrique du seuil d'analyse ou valeur de **2** pour la désactiver.  
  
##  <a name="RMO"></a> Objets RMO (Replication Management Objects)  
  
#### <a name="to-monitor-a-subscription-to-a-merge-publication-at-the-subscriber"></a>Pour surveiller un abonnement à une publication de fusion sur l'Abonné  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor> et définissez les propriétés <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.Publisher%2A>, <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.Publication%2A>, <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.PublisherDB%2A>, <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.SubscriberDB%2A> de l'abonnement, puis attribuez à la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> la valeur <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée dans l'étape 1.  
  
3.  Appelez l'une des méthodes suivantes pour retourner les informations sur les sessions de l'Agent de fusion de cet abonnement :  
  
    -   <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.GetSessionsSummary%2A> - retourne un tableau d'objets <xref:Microsoft.SqlServer.Replication.MergeSessionSummary> avec des informations sur les cinq dernières sessions au plus de l'Agent de fusion. Notez la valeur <xref:Microsoft.SqlServer.Replication.MergeSessionSummary.SessionId%2A> pour toutes les sessions dignes d'intérêt.  
  
    -   <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.GetSessionsSummary%2A> - retourne un tableau d'objets <xref:Microsoft.SqlServer.Replication.MergeSessionSummary> avec les informations sur les sessions de l'Agent de fusion qui se sont produites pendant le nombre passé d'heures transmis comme paramètre *hours* (jusqu'aux cinq dernières sessions au plus). Notez la valeur <xref:Microsoft.SqlServer.Replication.MergeSessionSummary.SessionId%2A> pour toutes les sessions dignes d'intérêt.  
  
    -   <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.GetLastSessionSummary%2A> - retourne un objet <xref:Microsoft.SqlServer.Replication.MergeSessionSummary> avec les informations sur la dernière session de l'Agent de fusion. Notez la valeur <xref:Microsoft.SqlServer.Replication.MergeSessionSummary.SessionId%2A> de cette session.  
  
    -   <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.GetSessionsSummaryDataSet%2A> - retourne un objet <xref:System.Data.DataSet> avec les informations sur les cinq dernières sessions au plus de l'Agent de fusion, une session par ligne. Notez la valeur de la colonne **Session_id** pour toutes les sessions dignes d'intérêt.  
  
    -   <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.GetLastSessionSummaryDataRow%2A> - retourne un objet <xref:System.Data.DataRow> avec les informations sur la dernière session de l'Agent de fusion. Notez la valeur de la colonne **Session_id** pour cette session.  
  
4.  (Facultatif) Appelez <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.RefreshSessionSummary%2A> pour actualiser les données de l'objet <xref:Microsoft.SqlServer.Replication.MergeSessionSummary> transmis comme *mss,* ou appelez <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.RefreshSessionSummary%2A> pour actualiser les données de l'objet <xref:System.Data.DataRow> transmis comme *drRefresh*.  
  
5.  À l'aide de l'ID de session obtenu à l'étape 3, appelez l'une des méthodes suivantes pour retourner les informations sur les détails d'une session particulière :  
  
    -   <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.GetSessionDetails%2A> - retourne un tableau d'objets <xref:Microsoft.SqlServer.Replication.MergeSessionDetail> pour le *SessionId*.  
  
    -   <xref:Microsoft.SqlServer.Replication.MergeSubscriberMonitor.GetSessionDetailsDataSet%2A> - retourne un objet <xref:System.Data.DataSet> avec les informations pour le *SessionId*.  
  
#### <a name="to-monitor-replication-properties-for-all-publications-at-a-distributor"></a>Pour surveiller les propriétés de réplication de toutes les publications sur un serveur de distribution  
  
1.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationMonitor> .  
  
3.  Définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec le <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé au cours de l'étape 1.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet.  
  
5.  Exécutez une ou plusieurs des méthodes suivantes pour retourner les informations de réplication de tous les serveurs de publication qui utilisent ce serveur de distribution.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.EnumDistributionAgents%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur tous les Agents de distribution de ce serveur de distribution.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.EnumErrorRecords%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur les erreurs stockées sur le serveur de distribution.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.EnumLogReaderAgents%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur tous les Agents de lecture du journal de ce serveur de distribution.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.EnumMergeAgents%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur tous les Agents de fusion de ce serveur de distribution.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.EnumMiscellaneousAgents%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur tous les autres agents de réplication de ce serveur de distribution.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.EnumPublishers%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur tous les serveurs de publication de ce serveur de distribution.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.EnumPublishers2%2A> - retourne un objet <xref:System.Data.DataSet> qui contient les serveurs de publication qui utilisent ce serveur de distribution.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.EnumQueueReaderAgents%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur tous les Agents de lecture de la file d'attente de ce serveur de distribution.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.EnumQueueReaderAgentSessionDetails%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des détails sur l'Agent de lecture de la file d'attente et la session spécifiés.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.EnumQueueReaderAgentSessions%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations de session sur l'Agent de lecture de la file d'attente spécifié.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.EnumSnapshotAgents%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur tous les Agents d'instantané de ce serveur de distribution.  
  
#### <a name="to-monitor-publication-properties-for-a-specific-publisher-at-the-distributor"></a>Pour analyser les propriétés de publication d'un serveur de publication spécifique d'un serveur de distribution  
  
1.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Récupérez un objet <xref:Microsoft.SqlServer.Replication.PublisherMonitor> par l'un ou l'autre de ces moyens.  
  
    -   Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.PublisherMonitor> . Définissez la propriété <xref:Microsoft.SqlServer.Replication.PublisherMonitor.Name%2A> pour le serveur de publication et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec le <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé au cours de l'étape 1. Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, le nom du serveur de publication n'a pas été correctement défini ou la publication n'existe pas.  
  
    -   Depuis le <xref:Microsoft.SqlServer.Replication.PublisherMonitorCollection> accessible au moyen de la propriété <xref:Microsoft.SqlServer.Replication.ReplicationMonitor.PublisherMonitors%2A> d'un objet <xref:Microsoft.SqlServer.Replication.ReplicationMonitor> existant.  
  
3.  Exécutez une ou plusieurs des méthodes suivantes pour retourner les informations de réplication de toutes les publications qui appartiennent à ce serveur de publication.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumDistributionAgentSessionDetails%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des détails sur l'Agent de distribution et la session spécifiés.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumDistributionAgentSessions%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations de session sur l'Agent de distribution spécifié.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumErrorRecords%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations d'enregistrement d'erreur sur l'erreur spécifiée.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumLogReaderAgentSessionDetails%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des détails sur l'Agent de lecture du journal et la session spécifiés.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumLogReaderAgentSessions%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations de session sur l'Agent de lecture du journal spécifié.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumMergeAgentSessionDetails%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des détails sur l'Agent de fusion et la session spécifiés.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumMergeAgentSessionDetails2%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des détails supplémentaires sur l'Agent de fusion et la session spécifiés.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumMergeAgentSessions%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations de session sur l'Agent de fusion spécifié.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumMergeAgentSessions2%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations de session supplémentaires sur l'Agent de fusion spécifié.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumPublications%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur toutes les publications de ce serveur de distribution.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumPublications2%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations supplémentaires sur toutes les publications de ce serveur de distribution.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumSnapshotAgentSessionDetails%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des détails sur la session et l'Agent d'instantané spécifiés.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumSnapshotAgentSessions%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations de session sur l'Agent d'instantané spécifié.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublisherMonitor.EnumSubscriptions%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations supplémentaires sur tous les abonnements aux publications de ce serveur de distribution.  
  
#### <a name="to-monitor-properties-for-a-specific-publication-at-the-distributor"></a>Pour analyser les propriétés d'une publication spécifique du serveur de distribution  
  
1.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Récupérez un objet <xref:Microsoft.SqlServer.Replication.PublicationMonitor> par l'un ou l'autre de ces moyens.  
  
    -   Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.PublicationMonitor> . Définissez les propriétés <xref:Microsoft.SqlServer.Replication.PublicationMonitor.DistributionDBName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublisherName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublicationDBName%2A>et <xref:Microsoft.SqlServer.Replication.PublicationMonitor.Name%2A> de la publication et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec le <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé au cours de l'étape 1. Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, les propriétés de publication n'ont pas été correctement définies ou la publication n'existe pas.  
  
    -   Depuis le <xref:Microsoft.SqlServer.Replication.PublicationMonitorCollection> accessible au moyen de la propriété <xref:Microsoft.SqlServer.Replication.PublisherMonitor.PublicationMonitors%2A> d'un objet <xref:Microsoft.SqlServer.Replication.PublisherMonitor> existant.  
  
3.  Exécutez une ou plusieurs des méthodes suivantes pour retourner des informations sur cette publication.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumErrorRecords%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des enregistrements d'erreur sur l'erreur spécifiée.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumLogReaderAgent%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur l'Agent de lecture du journal de cette publication.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumMonitorThresholds%2A> - retourne un objet <xref:System.Data.DataSet> qui contient les informations sur les seuils d'avertissement du moniteur pour cette publication.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumQueueReaderAgent%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur l'Agent de lecture de la file d'attente utilisé par cette publication.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumSnapshotAgent%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur l'Agent d'instantané de cette publication.  
  
    -   <xref:Microsoft.SqlServer.Replication.Publication.EnumSubscriptions%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur les abonnements à cette publication.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumSubscriptions2%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations supplémentaires sur les abonnements à cette publication en fonction du <xref:Microsoft.SqlServer.Replication.SubscriptionResultOption>fourni.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokenHistory%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations de latence sur le jeton de suivi spécifié.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokens%2A> - retourne un objet <xref:System.Data.DataSet> qui contient des informations sur tous les jetons de suivi insérés dans cette publication.  
  
#### <a name="to-monitor-transactional-commands-that-are-waiting-to-be-applied-at-the-subscriber"></a>Pour analyser les commandes transactionnelles en attente d'application sur l'Abonné  
  
1.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Récupérez un objet <xref:Microsoft.SqlServer.Replication.PublicationMonitor> par l'un ou l'autre de ces moyens.  
  
    -   Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.PublicationMonitor> . Définissez les propriétés <xref:Microsoft.SqlServer.Replication.PublicationMonitor.DistributionDBName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublisherName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublicationDBName%2A>et <xref:Microsoft.SqlServer.Replication.PublicationMonitor.Name%2A> de la publication et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec le <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé au cours de l'étape 1. Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, les propriétés de publication n'ont pas été correctement définies ou la publication n'existe pas.  
  
    -   Depuis le <xref:Microsoft.SqlServer.Replication.PublicationMonitorCollection> accessible au moyen de la propriété <xref:Microsoft.SqlServer.Replication.PublisherMonitor.PublicationMonitors%2A> d'un objet <xref:Microsoft.SqlServer.Replication.PublisherMonitor> existant.  
  
3.  Exécutez la méthode <xref:Microsoft.SqlServer.Replication.PublicationMonitor.TransPendingCommandInfo%2A> , qui retourne un objet <xref:Microsoft.SqlServer.Replication.PendingCommandInfo> .  
  
4.  Utilisez les propriétés de cet objet <xref:Microsoft.SqlServer.Replication.PendingCommandInfo> pour déterminer le nombre estimé de commandes en attente et la durée nécessaire à la remise complète de ces commandes.  
  
#### <a name="to-set-the-monitor-warning-thresholds-for-a-publication"></a>Pour définir les seuils d'avertissement du moniteur pour une publication  
  
1.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Récupérez un objet <xref:Microsoft.SqlServer.Replication.PublicationMonitor> par l'un ou l'autre de ces moyens.  
  
    -   Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.PublicationMonitor> . Définissez les propriétés <xref:Microsoft.SqlServer.Replication.PublicationMonitor.DistributionDBName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublisherName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublicationDBName%2A>et <xref:Microsoft.SqlServer.Replication.PublicationMonitor.Name%2A> de la publication et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec le <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé au cours de l'étape 1. Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, les propriétés de publication n'ont pas été correctement définies ou la publication n'existe pas.  
  
    -   Depuis le <xref:Microsoft.SqlServer.Replication.PublicationMonitorCollection> accessible au moyen de la propriété <xref:Microsoft.SqlServer.Replication.PublisherMonitor.PublicationMonitors%2A> d'un objet <xref:Microsoft.SqlServer.Replication.PublisherMonitor> existant.  
  
3.  Exécutez la méthode <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumMonitorThresholds%2A> . Notez les paramètres de seuil en cours dans le <xref:System.Collections.ArrayList> retourné des objets <xref:Microsoft.SqlServer.Replication.MonitorThreshold> .  
  
4.  Exécutez la méthode <xref:Microsoft.SqlServer.Replication.PublicationMonitor.ChangeMonitorThreshold%2A> . Transmettez les paramètres suivants :  
  
    -   *metricID* - une valeur <xref:System.Int32> qui représente le seuil d'analyse métrique de la table suivante :  
  
        |Valeur|Description|  
        |-----------|-----------------|  
        | 1|**expiration** : contrôle l'expiration imminente des abonnements aux publications transactionnelles.|  
        |2|**latency** : contrôle les performances des abonnements aux publications transactionnelles.|  
        |4|**mergeexpiration** : contrôle l'expiration imminente des abonnements aux publications de fusion.|  
        |5|**mergeslowrunduration** : contrôle la durée des synchronisations de fusion sur les connexions à faible bande passante (accès à distance).|  
        |6|**mergefastrunduration** : contrôle la durée des synchronisations de fusion sur les connexions haut débit (LAN).|  
        |7|**mergefastrunspeed** - supervise le taux de synchronisation des synchronisations de fusion sur des connexions à bande passante élevée (LAN).|  
        |8|**mergeslowrunspeed** : contrôle la vitesse de synchronisation des synchronisations de fusion sur les connexions lentes (accès distant).|  
  
    -   *enable* - <xref:System.Boolean> qui indique si le métrique est activé pour la publication.  
  
    -   *thresholdValue* - valeur entière qui définit le seuil.  
  
    -   *shouldAlert* - entier qui indique si ce seuil doit générer une alerte.  
  
  
