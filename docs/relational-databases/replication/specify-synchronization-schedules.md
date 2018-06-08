---
title: Spécifier des planifications de synchronisation | Microsoft Docs
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
- subscriptions [SQL Server replication], synchronizing
- scheduling synchronization [SQL Server replication]
- synchronization [SQL Server replication], schedules
- replication [SQL Server], synchronization
ms.assetid: 97f2535b-ec19-4973-823d-bcf3d5aa0216
caps.latest.revision: 40
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 05e5e55f02573bc73ac13a411ebb03671bf9bba2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-synchronization-schedules"></a>Spécifier des planifications de synchronisation
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment spécifier des planifications de synchronisation dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects). Lorsque vous créez un abonnement, vous pouvez définir une planification de synchronisation qui contrôle à quel moment l'agent de réplication de l'abonnement s'exécute. Si aucun paramètre de planification n'est spécifié, l'abonnement utilise la planification par défaut.  
  
 Les abonnements sont synchronisés par l'Agent de distribution (pour la réplication transactionnelle et d'instantané) ou l'Agent de fusion (pour la réplication de fusion). Les agents peuvent s'exécuter en continu, à la demande ou selon une planification.  
  
 **Dans cette rubrique**  
  
-   **Pour spécifier des planifications de synchronisation à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Spécifiez des planifications de synchronisation dans la page **Planification de synchronisation** dans l'Assistant Nouvel abonnement. Pour plus d'informations sur l'accès à cet Assistant, consultez [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md) et [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
 Modifiez les planifications de synchronisation dans la boîte de dialogue **Propriétés de la planification du travail** , accessible à partir du dossier **Travaux** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et de la fenêtre des détails de l'agent dans le Moniteur de réplication. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
 Si vous définissez des planifications à partir du dossier **Travaux** , utilisez le tableau suivant pour déterminer le nom du travail de l'Agent.  
  
|Agent|Nom du travail|  
|-----------|--------------|  
|Agent de fusion pour les abonnements extraits|**\<Serveur_Publication>-\<Base_de_données_Publication>-\<Publication>-\<Abonné>-\<Base_de_données_Abonnement>-\<entier>**|  
|Agent de fusion pour abonnements par envoi de données (push)|**\<ServeurPublication>-\<BasededonnéesPublication>-\<Publication>-\<Abonné>-\<entier>**|  
|Agent de distribution pour abonnements par envoi de données (push)|**\<Serveur_Publication>-\<Base_de_données_Publication>-\<Publication>-\<Abonné>-\<entier>** <sup>1</sup>|  
|Agent de distribution pour abonnements par extraction de données (pull)|**\<Serveur_Publication>-\<Base_de_données_Publication>-\<Publication>-\<Abonné>-\<Base_de_données_Abonnement>-\<GUID>** <sup>2</sup>|  
|Agent de distribution pour les abonnements par envoi de données aux Abonnés non SQL Server|**\<Serveur_Publication>-\<Base_de_données_Publication>-\<Publication>-\<Abonné>-\<entier>**|  
  
 <sup>1</sup> Pour les abonnements par émission de données aux publications Oracle, il s’agit de **\<Serveur_Publication>-\<Serveur_Publication**> au lieu de **\<Serveur_Publication>-\<Base_de_données_Publication>**  
  
 <sup>2</sup> Pour les abonnements par extraction aux publications Oracle, il s’agit de **\<Serveur_Publication>-\<Base_de_données_Distribution**> au lieu de **\<Serveur_Publication>-\<Base_de_données_Publication>**  
  
#### <a name="to-specify-synchronization-schedules"></a>Pour spécifier des planifications de synchronisation  
  
1.  Dans la page **Planification de synchronisation** de l’Assistant Nouvel abonnement, sélectionnez l’une des valeurs suivantes dans la liste déroulante **Planification de l’agent** pour chaque abonnement que vous créez :  
  
    -   **Exécuter en continu**  
  
    -   **Exécuter à la demande uniquement**  
  
    -   **\<Définir la planification...>**  
  
2.  Si vous sélectionnez l’option **\<Définir la planification...>**, spécifiez une planification dans la boîte de dialogue **Propriétés de la planification du travail**, puis cliquez sur **OK**.  
  
3.  Terminez l'Assistant.  
  
#### <a name="to-modify-a-synchronization-schedule-for-a-push-subscription-in-replication-monitor"></a>Pour modifier une planification de synchronisation d'un abonnement envoyé dans le moniteur de réplication  
  
1.  Développez un groupe Serveur de publication dans le volet gauche du moniteur de réplication, développez un serveur de publication puis cliquez sur une publication.  
  
2.  Cliquez sur l'onglet **Tous les abonnements** .  
  
3.  Cliquez avec le bouton droit sur un abonnement, puis cliquez sur **Afficher les détails**.  
  
4.  Dans la fenêtre **Abonnement < NomAbonnement>**, cliquez sur **Action**, puis sur **Propriétés du travail - \<NomAgent>**.  
  
5.  Dans la page **Planifications** de la boîte de dialogue **Propriétés du travail - \<NomTravail>**, cliquez sur **Modifier**.  
  
6.  Dans la boîte de dialogue **Propriétés de la planification du travail** , sélectionnez une valeur dans la liste déroulante **Type de planification** :  
  
    -   Pour spécifier que l'agent doit s'exécuter en continu, sélectionnez **Lancer automatiquement au démarrage de l'Agent SQL Server**.  
  
    -   Pour spécifier une exécution planifiée de l'Agent, sélectionnez **Périodique**.  
  
    -   Pour définir une exécution à la demande de l'Agent, sélectionnez **Une fois**.  
  
7.  Si vous sélectionnez **Périodique**, spécifiez une planification de l'Agent.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-modify-a-synchronization-schedule-for-a-push-subscription-in-management-studio"></a>Pour modifier une planification de synchronisation d'un abonnement envoyé dans Management Studio  
  
1.  Connectez-vous au serveur de distribution dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Agent SQL Server** puis développez le dossier **Travaux** .  
  
3.  Cliquez avec le bouton droit du travail de l'Agent de distribution ou de fusion associé à l'abonnement puis cliquez sur **Propriétés**.  
  
4.  Dans la page **Planifications** de la boîte de dialogue **Propriétés du travail - \<NomTravail>**, cliquez sur **Modifier**.  
  
5.  Dans la boîte de dialogue **Propriétés de la planification du travail** , sélectionnez une valeur dans la liste déroulante **Type de planification** :  
  
    -   Pour spécifier que l'agent doit s'exécuter en continu, sélectionnez **Lancer automatiquement au démarrage de l'Agent SQL Server**.  
  
    -   Pour spécifier une exécution planifiée de l'Agent, sélectionnez **Périodique**.  
  
    -   Pour définir une exécution à la demande de l'Agent, sélectionnez **Une fois**.  
  
6.  Si vous sélectionnez **Périodique**, spécifiez une planification de l'Agent.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-modify-a-synchronization-schedule-for-a-pull-subscription-in-management-studio"></a>Pour modifier une planification de synchronisation d'un abonnement extrait dans Management Studio  
  
1.  Connectez-vous à l'Abonné dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Agent SQL Server** puis développez le dossier **Travaux** .  
  
3.  Cliquez avec le bouton droit du travail de l'Agent de distribution ou de fusion associé à l'abonnement puis cliquez sur **Propriétés**.  
  
4.  Dans la page **Planifications** de la boîte de dialogue **Propriétés du travail - \<NomTravail>**, cliquez sur **Modifier**.  
  
5.  Dans la boîte de dialogue **Propriétés de la planification du travail** , sélectionnez une valeur dans la liste déroulante **Type de planification** :  
  
    -   Pour spécifier que l'agent doit s'exécuter en continu, sélectionnez **Lancer automatiquement au démarrage de l'Agent SQL Server**.  
  
    -   Pour spécifier une exécution planifiée de l'Agent, sélectionnez **Périodique**.  
  
    -   Pour définir une exécution à la demande de l'Agent, sélectionnez **Une fois**.  
  
6.  Si vous sélectionnez **Périodique**, spécifiez une planification de l'Agent.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Vous pouvez définir des planifications de synchronisation par programme en utilisant des procédures stockées de réplication. Les procédures stockées que vous utilisez dépendent du type de réplication et du type d'abonnement (par extraction ou par émission de données).  
  
 Une planification est définie par les paramètres de planification suivants dont le comportement est hérité de [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md) :  
  
-   **@frequency_type** - le type de fréquence utilisé lors de la planification de l'agent.  
  
-   **@frequency_interval** - le jour de la semaine où l'agent est exécuté.  
  
-   **@frequency_relative_interval** – semaine d'un mois donné lorsque l'agent est planifié pour s'exécuter chaque mois ;  
  
-   **@frequency_recurrence_factor** – nombre d'unités de type de fréquence entre deux synchronisations ;  
  
-   **@frequency_subday** – unité de fréquence lorsque l'agent s'exécute plusieurs fois par jour ;  
  
-   **@frequency_subday_interval** – nombre d'unités de fréquence entre deux exécutions lorsque l'agent s'exécute plusieurs fois par jour ;  
  
-   **@active_start_time_of_day** - la première heure d'un jour donné à laquelle l'exécution d'un agent débutera.  
  
-   **@active_end_time_of_day** - la dernière heure d'un jour donné à laquelle l'exécution d'un agent débutera.  
  
-   **@active_start_date** - le premier jour où la planification de l'agent sera appliquée.  
  
-   **@active_end_date** - le dernier jour où la planification de l'agent sera appliquée.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-pull-subscription-to-a-transactional-publication"></a>Pour définir la planification de synchronisation pour un abonnement par extraction à une publication transactionnelle  
  
1.  Créez un abonnement par extraction à une publication transactionnelle. Pour plus d’informations, consultez [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  Sur l’Abonné, exécutez [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Spécifiez **@publisher**, de **@publisher_db**, de **@publication**et les informations d'identification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lesquelles l'Agent de distribution s'exécute sur l'Abonné pour **@job_name** et **@password**. Spécifiez les paramètres de synchronisation (détaillés plus haut) qui définissent la planification du travail de l'Agent de distribution qui synchronise l'abonnement.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-push-subscription-to-a-transactional-publication"></a>Pour définir la planification de synchronisation pour un abonnement par émission de données à une publication transactionnelle  
  
1.  Créez un abonnement par émission de données à une publication transactionnelle. Pour plus d’informations, consultez [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  Sur l’Abonné, exécutez [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md). Spécifiez **@subscriber**, de **@subscriber_db**, de **@publication**et les informations d'identification Windows sous lesquelles l'Agent de distribution s'exécute sur l'Abonné pour **@job_name** et **@password**. Spécifiez les paramètres de synchronisation (détaillés plus haut) qui définissent la planification du travail de l'Agent de distribution qui synchronise l'abonnement.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-pull-subscription-to-a-merge-publication"></a>Pour définir la planification de synchronisation pour un abonnement par extraction à une publication de fusion  
  
1.  Créez un abonnement par extraction à une publication de fusion Pour plus d’informations, consultez [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  Sur l'Abonné, exécutez [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Spécifiez **@publisher**, de **@publisher_db**, de **@publication**et les informations d'identification Windows sous lesquelles l'Agent de fusion s'exécute sur l'Abonné pour **@job_name** et **@password**. Spécifiez les paramètres de synchronisation (détaillés plus haut) qui définissent la planification du travail de l'Agent de fusion qui synchronise l'abonnement.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-push-subscription-to-a-merge-publication"></a>Pour définir la planification de synchronisation pour un abonnement par émission de données à une publication de fusion  
  
1.  Créez un abonnement par émission de données à une publication de fusion. Pour plus d’informations, consultez [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  Sur l'Abonné, exécutez [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md). Spécifiez **@subscriber**, de **@subscriber_db**, de **@publication**et les informations d'identification Windows sous lesquelles l'Agent de fusion s'exécute sur l'Abonné pour **@job_name** et **@password**. Spécifiez les paramètres de synchronisation (détaillés plus haut) qui définissent la planification du travail de l'Agent de fusion qui synchronise l'abonnement.  
  
##  <a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 La réplication utilise l'Agent SQL Server pour planifier des travaux pour les activités qui se produisent périodiquement, telles que la génération d'instantanés et la synchronisation d'abonnements. Vous pouvez utiliser les Replication Management Objects par programme pour spécifier des planifications pour les travaux des agents de réplication.  
  
> [!NOTE]  
>  Lorsque vous créez un abonnement et spécifiez la valeur **false** pour **CreateSyncAgentByDefault** (comportement par défaut pour les abonnements par extraction), le travail de l'agent n'est pas créé et les propriétés de planification sont ignorées. Dans ce cas, la planification de la synchronisation doit être déterminée par l'application. Pour plus d'informations, consultez [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) et [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-push-subscription-to-a-transactional-publication"></a>Pour définir une planification de l'Agent de réplication lorsque vous créez un abonnement par émission de données à une publication transactionnelle  
  
1.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransSubscription> pour l'abonnement que vous créez. Pour plus d’informations, consultez [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  Avant d'appeler <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>, définissez un ou plusieurs des champs suivants de la propriété <xref:Microsoft.SqlServer.Replication.Subscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> – type de fréquence (tel que quotidien ou hebdomadaire) que vous utilisez pour planifier l'agent ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> – jour de la semaine où un agent s'exécute ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> – semaine d'un mois donné lorsque l'agent est planifié pour s'exécuter chaque mois ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> – nombre d'unités de type de fréquence entre deux synchronisations ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> – unité de fréquence lorsque l'agent s'exécute plusieurs fois par jour ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> – nombre d'unités de fréquence entre deux exécutions lorsque l'agent s'exécute plusieurs fois par jour ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> – première heure d'un jour donné à laquelle l'exécution de l'agent commence ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> – heure la plus tardive d'un jour donné à laquelle l'exécution de l'agent commence ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> – premier jour d'application de la planification de l'agent ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> – dernier jour d'application de la planification de l'agent.  
  
    > [!NOTE]  
    >  Si vous ne spécifiez pas l'une de ces propriétés, une valeur par défaut est définie.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> pour créer l'abonnement.  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-pull-subscription-to-a-transactional-publication"></a>Pour définir une planification de l'Agent de réplication lorsque vous créez un abonnement par extraction à une publication transactionnelle  
  
1.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPullSubscription> pour l'abonnement que vous créez. Pour plus d’informations, consultez [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  Avant d'appeler <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A>, définissez un ou plusieurs des champs suivants de la propriété <xref:Microsoft.SqlServer.Replication.PullSubscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> – type de fréquence (tel que quotidien ou hebdomadaire) que vous utilisez pour planifier l'agent ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> – jour de la semaine où un agent s'exécute ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> – semaine d'un mois donné où l'agent est planifié pour s'exécuter chaque mois ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> – nombre d'unités de type de fréquence entre deux synchronisations ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> – unité de fréquence lorsque l'agent s'exécute plusieurs fois par jour ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> – nombre d'unités de fréquence entre deux exécutions lorsque l'agent s'exécute plusieurs fois par jour ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> – première heure d'un jour donné à laquelle l'exécution de l'agent commence ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> – heure la plus tardive d'un jour donné à laquelle l'exécution de l'agent commence ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> – premier jour d'application de la planification de l'agent ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> – dernier jour d'application de la planification de l'agent.  
  
    > [!NOTE]  
    >  Si vous ne spécifiez pas l'une de ces propriétés, une valeur par défaut est définie.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> pour créer l'abonnement.  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-pull-subscription-to-a-merge-publication"></a>Pour définir une planification de l'Agent de réplication lorsque vous créez un abonnement par extraction à une publication de fusion  
  
1.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePullSubscription> pour l'abonnement que vous créez. Pour plus d’informations, consultez [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  Avant d'appeler <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A>, définissez un ou plusieurs des champs suivants de la propriété <xref:Microsoft.SqlServer.Replication.PullSubscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> – type de fréquence (tel que quotidien ou hebdomadaire) que vous utilisez pour planifier l'agent ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> – jour de la semaine où un agent s'exécute ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> – semaine d'un mois donné où l'agent est planifié pour s'exécuter chaque mois ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> – nombre d'unités de type de fréquence entre deux synchronisations ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> – unité de fréquence lorsque l'agent s'exécute plusieurs fois par jour ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> – nombre d'unités de fréquence entre deux exécutions lorsque l'agent s'exécute plusieurs fois par jour ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> – première heure d'un jour donné à laquelle l'exécution de l'agent commence ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> – heure la plus tardive d'un jour donné à laquelle l'exécution de l'agent commence ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> – premier jour d'application de la planification de l'agent ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> – dernier jour d'application de la planification de l'agent.  
  
    > [!NOTE]  
    >  Si vous ne spécifiez pas l'une de ces propriétés, une valeur par défaut est définie.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> pour créer l'abonnement.  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-push-subscription-to-a-merge-publication"></a>Pour définir une planification de l'Agent de réplication lorsque vous créez un abonnement par émission de données à une publication de fusion  
  
1.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeSubscription> pour l'abonnement que vous créez. Pour plus d’informations, consultez [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  Avant d'appeler <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>, définissez un ou plusieurs des champs suivants de la propriété <xref:Microsoft.SqlServer.Replication.Subscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> – type de fréquence (tel que quotidien ou hebdomadaire) que vous utilisez pour planifier l'agent ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> – jour de la semaine où un agent s'exécute ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> – semaine d'un mois donné où l'agent est planifié pour s'exécuter chaque mois ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> – nombre d'unités de type de fréquence entre deux synchronisations ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> – unité de fréquence lorsque l'agent s'exécute plusieurs fois par jour ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> – nombre d'unités de fréquence entre deux exécutions lorsque l'agent s'exécute plusieurs fois par jour ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> – première heure d'un jour donné à laquelle l'exécution de l'agent commence ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> – heure la plus tardive d'un jour donné à laquelle l'exécution de l'agent commence ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> – premier jour d'application de la planification de l'agent ;  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> – dernier jour d'application de la planification de l'agent.  
  
    > [!NOTE]  
    >  Si vous ne spécifiez pas l'une de ces propriétés, une valeur par défaut est définie.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> pour créer l'abonnement.  
  
###  <a name="PShellExample"></a> Exemple (RMO)  
 Cet exemple crée un abonnement par émission de données à une publication de fusion et spécifie la planification sur laquelle l'abonnement est synchronisé.  
  
 [!code-cs[HowTo#rmo_CreateMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepushsub)]  
  
## <a name="see-also"></a> Voir aussi  
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Synchroniser un abonnement par émission de données](../../relational-databases/replication/synchronize-a-push-subscription.md)   
 [Synchroniser un abonnement par extraction](../../relational-databases/replication/synchronize-a-pull-subscription.md)   
 [Synchroniser les données](../../relational-databases/replication/synchronize-data.md)  
  
  
