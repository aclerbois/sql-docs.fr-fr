---
title: Mettre à niveau les scripts de réplication (programmation Transact-SQL de la réplication) | Microsoft Docs
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
- scripts [SQL Server replication], upgrading
- upgrading SQL Server, replicated databases
- upgrading replication applications
- replication [SQL Server], scripting
- replication [SQL Server], upgrading
- upgrading replicated databases
ms.assetid: 0b8720bd-f339-4842-bc8f-b35a46f6d3ee
caps.latest.revision: 41
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 19e2adba0742454e039f106f1ed5c3cd693384f5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="upgrade-replication-scripts-replication-transact-sql-programming"></a>Mettre à niveau les scripts de réplication (programmation Transact-SQL de la réplication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[tsql](../../../includes/tsql-md.md)] Les fichiers de script peuvent être utilisés pour configurer une topologie de réplication par programmation. Pour plus d’informations, consultez [Concepts liés aux procédures stockées système de réplication](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md).  
  
> [!IMPORTANT]  
>  Bien que vous ne soyez pas tenu de mettre à niveau les scripts exécutés par les membres du rôle **sysadmin** , nous vous recommandons de modifier les scripts existants comme décrit dans cette rubrique. Spécifiez un compte qui possède les autorisations minimales pour chaque agent de réplication comme décrit dans la section « Autorisations requises par les Agents » de la rubrique [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
 Ces améliorations de sécurité, qui offrent un contrôle accru sur les autorisations en vous permettant de spécifier explicitement les comptes Windows [!INCLUDE[msCoName](../../../includes/msconame-md.md)] sous lesquels les travaux de l'Agent de réplication sont exécutés, affectent les procédures stockées suivantes dans les scripts existants :  
  
-   **sp_addpublication_snapshot**:  
  
     Vous devez maintenant fournir les informations d’identification Windows comme **@job_login** et **@job_password** lors de l’exécution de [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md) pour créer le travail sous lequel l’Agent d’instantané s’exécute sur le serveur de distribution.  
  
-   **sp_addpushsubscription_agent**:  
  
     Vous devez maintenant exécuter [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md) pour ajouter un travail explicitement et fournir les informations d’identification Windows (**@job_login** et **@job_password**) sous lesquelles le travail de l’Agent de distribution s’exécute sur le serveur de distribution. Dans les versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], ces actions étaient exécutées automatiquement lors de la création d'un abonnement par émission de données.  
  
-   **sp_addmergepushsubscription_agent**:  
  
     Vous devez maintenant exécuter [sp_addmergepushsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md) pour ajouter un travail explicitement et fournir les informations d’identification Windows (**@job_login** et **@job_password**) sous lesquelles le travail de l’Agent de fusion s’exécute sur le serveur de distribution. Dans les versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], ces actions étaient exécutées automatiquement lors de la création d'un abonnement par émission de données.  
  
-   **sp_addpullsubscription_agent**:  
  
     Vous devez maintenant fournir les informations d’identification Windows comme **@job_login** et **@job_password** lors de l’exécution de [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md) pour créer le travail sous lequel l’Agent de distribution s’exécute sur l’Abonné.  
  
-   **sp_addmergepullsubscription_agent**:  
  
     Vous devez maintenant fournir les informations d’identification Windows comme **@job_login** et **@job_password** lors de l’exécution de [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) pour créer le travail sous lequel l’Agent de fusion s’exécute sur l’abonné.  
  
-   **sp_addlogreader_agent**:  
  
     Vous devez maintenant exécuter [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md) pour ajouter manuellement le travail et fournir les informations d’identification Windows sous lesquelles l’Agent de lecture du journal s’exécute sur le serveur de distribution. Dans les versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], ces actions étaient exécutées automatiquement lors de la création d'une publication transactionnelle.  
  
-   **sp_addqreader_agent**:  
  
     Vous devez maintenant exécuter [sp_addqreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md) pour ajouter manuellement le travail et fournir les informations d’identification Windows sous lesquelles l’Agent de lecture de la file d’attente s’exécute sur le serveur de distribution. Dans les versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], ces actions étaient exécutées automatiquement lors de la création d'une publication transactionnelle prenant en charge la mise à jour en file d'attente.  
  
 Dans le modèle de sécurité introduit dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], les agents de réplication établissent toujours des connexions à l'instance locale de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec l'authentification Windows à l'aide des informations d'identification fournies dans **@job_name** et **@job_password**. Pour plus d'informations sur la configuration requise des comptes Windows lors de l'exécution des travaux de l'Agent de réplication, consultez [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous stockez les informations d'identification dans un fichier de script, assurez-vous que le fichier lui-même est sécurisé.  
  
### <a name="to-upgrade-scripts-that-configure-a-snapshot-or-transactional-publication"></a>Pour mettre à niveau les scripts qui configurent une publication transactionnelle ou d'instantané  
  
1.  Dans le script existant, avant [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md), exécutez [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md) sur le serveur de publication sur la base de données de publication. Spécifiez les informations d'identification Windows sous lesquelles l'Agent de lecture du journal s'exécute pour **@job_name** et **@job_password**. Si l'agent doit utiliser l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] lors de la connexion au serveur de publication, vous devez aussi spécifier la valeur **0** pour **@publisher_security_mode** et les informations de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour **@publisher_login** et **@publisher_password**. Il s'ensuit la création d'un travail de l'Agent de lecture du journal pour la base de données de publication.  
  
    > [!NOTE]  
    >  Cette étape concerne uniquement les publications transactionnelles et n'est pas requise pour les publications d'instantané.  
  
2.  (Facultatif) Avant [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md), exécutez [sp_addqreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md) sur la base de données de distribution sur le serveur de distribution. Spécifiez les informations d'identification Windows sous lesquelles l'Agent de lecture de la file d'attente s'exécute pour **@job_name** et **@job_password**. Il s'ensuit la création d'un travail de l'Agent de lecture de la file d'attente pour le serveur de distribution.  
  
    > [!NOTE]  
    >  Cette étape n'est requise que pour les publications transactionnelles qui prennent en charge les Abonnés de mise à jour en file d'attente.  
  
3.  (Facultatif) Mettez à jour l’exécution de [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) pour spécifier les valeurs non définies par défaut des paramètres qui implémentent les nouvelles fonctionnalités de réplication.  
  
4.  Après [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md), exécutez [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md) au niveau du serveur de publication sur la base de données de publication. Spécifiez **@publication** et les informations d'identification Windows sous lesquelles l'Agent d'instantané s'exécute pour **@job_name** et **@job_password**. Si l'agent doit utiliser l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] lors de la connexion au serveur de publication, vous devez aussi spécifier la valeur **0** pour **@publisher_security_mode** et les informations de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour **@publisher_login** et **@publisher_password**. Il s'ensuit la création d'un travail de l'Agent d'instantané pour la publication.  
  
5.  (Facultatif) Mettez à jour l’exécution de [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) pour spécifier les valeurs non définies par défaut des paramètres qui implémentent les nouvelles fonctionnalités de réplication.  
  
### <a name="to-upgrade-scripts-that-add-subscriptions-to-a-snapshot-or-transactional-publication"></a>Pour mettre à niveau les scripts qui ajoutent des abonnements à une publication transactionnelle ou d'instantané  
  
1.  Après avoir exécuté la procédure stockée qui crée l'abonnement, veillez bien à exécuter celle qui crée un travail de l'Agent de distribution pour synchroniser l'abonnement. La procédure stockée que vous utilisez dépend du type d'abonnement.  
  
    -   Pour un abonnement par extraction, mettez à jour l’exécution de [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md) afin de fournir les informations d’identification Windows sous lesquelles l’Agent de distribution s’exécute sur l’Abonné pour **@job_name** et **@job_password**. Cette opération s'effectue après l'exécution de [sp_addpullsubscription](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md). Pour plus d’informations, consultez [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md).  
  
    -   Pour un abonnement par émission de données, exécutez [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md) sur le serveur de publication. Spécifiez **@subscriber**, **@subscriber_db**, **@publication**et les informations d'identification Windows sous lesquelles l'Agent de distribution s'exécute sur le serveur de distribution pour **@job_name** et **@job_password**, ainsi que la planification du travail de l'agent. Pour plus d'informations, voir [Specify Synchronization Schedules](../../../relational-databases/replication/specify-synchronization-schedules.md). Cette opération s'effectue après l'exécution de [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Pour plus d'informations, voir [Create a Push Subscription](../../../relational-databases/replication/create-a-push-subscription.md).  
  
### <a name="to-upgrade-scripts-that-configure-a-merge-publication"></a>Pour mettre à niveau les scripts qui configurent une publication de fusion  
  
1.  (Facultatif) Dans le script existant, mettez à jour l’exécution de [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) pour spécifier les valeurs non définies par défaut des paramètres qui implémentent de nouvelles fonctionnalités de réplication.  
  
2.  Après [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md), exécutez [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md) au niveau du serveur de publication sur la base de données de publication. Spécifiez **@publication** et les informations d'identification Windows sous lesquelles l'Agent d'instantané s'exécute pour **@job_name** et **@job_password**. Si l'agent doit utiliser l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] lors de la connexion au serveur de publication, vous devez aussi spécifier la valeur **0** pour **@publisher_security_mode** et les informations de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour **@publisher_login** et **@publisher_password**. Il s'ensuit la création d'un travail de l'Agent d'instantané pour la publication.  
  
3.  (Facultatif) Mettez à jour l’exécution de [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) pour spécifier les valeurs non définies par défaut des paramètres qui implémentent les nouvelles fonctionnalités de réplication.  
  
### <a name="to-upgrade-scripts-that-add-subscriptions-to-a-merge-publication"></a>Pour mettre à niveau les scripts qui ajoutent des abonnements à une publication de fusion  
  
1.  Après avoir exécuté la procédure stockée qui crée l'abonnement, veillez bien à exécuter celle qui crée un travail de l'Agent de fusion pour synchroniser l'abonnement. La procédure stockée que vous utilisez dépend du type d'abonnement.  
  
    -   Pour un abonnement par extraction, mettez à jour l’exécution de [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) afin de fournir les informations d’identification Windows sous lesquelles l’Agent de fusion s’exécute sur l’Abonné pour **@job_name** et **@job_password**. Cette opération s'effectue après l'exécution de [sp_addmergepullsubscription](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md). Pour plus d’informations, consultez [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md).  
  
    -   Pour un abonnement par émission de données, exécutez [sp_addmergepushsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md) sur le serveur de publication. Spécifiez **@subscriber**, **@subscriber_db**, **@publication**et les informations d'identification Windows sous lesquelles l'Agent de fusion s'exécute sur le serveur de distribution pour **@job_name** et **@job_password**, ainsi que la planification du travail de l'agent. Pour plus d'informations, voir [Specify Synchronization Schedules](../../../relational-databases/replication/specify-synchronization-schedules.md). Cette opération s'effectue après l'exécution de [sp_addmergesubscription](../../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md). Pour plus d'informations, voir [Create a Push Subscription](../../../relational-databases/replication/create-a-push-subscription.md).  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de script [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] créant une publication transactionnelle pour la table Product. Cette publication prend en charge la mise à jour immédiate avec la mise à jour en file d'attente comme basculement. Les paramètres par défaut ont été supprimés pour des raisons de lisibilité.  
  
 [!code-sql[HowTo#sp_createtranpub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_1.sql)]  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de mise à niveau du précédent script, lequel crée une publication transactionnelle, pour qu'il s'exécute avec succès sous [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et versions ultérieures. Cette publication prend en charge la mise à jour immédiate avec la mise à jour en file d'attente comme basculement. Les valeurs par défaut des nouveaux paramètres ont été déclarées explicitement.  
  
> [!NOTE]  
>  Les informations d'identification Windows sont fournies pendant l'exécution à l'aide des variables de script **Sqlcmd** .  
  
 [!code-sql[HowTo#sp_createtranpub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_2.sql)]  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de script [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] qui crée une publication de fusion pour la table Customers. Les paramètres par défaut ont été supprimés pour des raisons de lisibilité.  
  
 [!code-sql[HowTo#sp_createmergepub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_3.sql)]  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de mise à niveau du précédent script, lequel crée une publication de fusion, pour qu'il s'exécute avec succès sous [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et versions ultérieures. Les valeurs par défaut des nouveaux paramètres ont été déclarées explicitement.  
  
> [!NOTE]  
>  Les informations d'identification Windows sont fournies pendant l'exécution à l'aide des variables de script **Sqlcmd** .  
  
 [!code-sql[HowTo#sp_createmergepub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_4.sql)]  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de script [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] qui crée un abonnement par émission de données à une publication transactionnelle. Les paramètres par défaut ont été supprimés pour des raisons de lisibilité.  
  
 [!code-sql[HowTo#sp_createtranpushsub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_5.sql)]  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de mise à niveau du précédent script, lequel crée un abonnement par émission de données à une publication transactionnelle, pour qu'il s'exécute avec succès sous [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et versions ultérieures. Les valeurs par défaut des nouveaux paramètres ont été déclarées explicitement.  
  
> [!NOTE]  
>  Les informations d'identification Windows sont fournies pendant l'exécution à l'aide des variables de script **Sqlcmd** .  
  
 [!code-sql[HowTo#sp_createtranpushsub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_6.sql)]  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de script [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] qui crée un abonnement par émission de données à une publication de fusion. Les paramètres par défaut ont été supprimés pour des raisons de lisibilité.  
  
 [!code-sql[HowTo#sp_createmergepushsub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_7.sql)]  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de mise à niveau du précédent script, lequel crée un abonnement par émission de données à une publication de fusion, pour qu'il s'exécute avec succès sous [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et versions ultérieures. Les valeurs par défaut des nouveaux paramètres ont été déclarées explicitement.  
  
> [!NOTE]  
>  Les informations d'identification Windows sont fournies pendant l'exécution à l'aide des variables de script **Sqlcmd** .  
  
 [!code-sql[HowTo#sp_createmergepushsub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_8.sql)]  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de script [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] qui crée un abonnement par extraction à une publication transactionnelle. Les paramètres par défaut ont été supprimés pour des raisons de lisibilité.  
  
 [!code-sql[HowTo#sp_createmergepushsub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_7.sql)]  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de mise à niveau du précédent script, lequel crée un abonnement par extraction à une publication transactionnelle, pour qu'il s'exécute avec succès sous [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et versions ultérieures. Les valeurs par défaut des nouveaux paramètres ont été déclarées explicitement.  
  
> [!NOTE]  
>  Les informations d'identification Windows sont fournies pendant l'exécution à l'aide des variables de script **Sqlcmd** .  
  
 [!code-sql[HowTo#sp_createtranpullsub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_9.sql)]  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de script [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] qui crée un abonnement par extraction à une publication de fusion. Les paramètres par défaut ont été supprimés pour des raisons de lisibilité.  
  
 [!code-sql[HowTo#sp_createmergepullsub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_10.sql)]  
  
## <a name="example"></a> Exemple  
 Ce qui suit est un exemple de mise à niveau du précédent script, lequel crée un abonnement par extraction à une publication de fusion, pour qu'il s'exécute avec succès sous [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et versions ultérieures. Les valeurs par défaut des nouveaux paramètres ont été déclarées explicitement.  
  
> [!NOTE]  
>  Les informations d'identification Windows sont fournies pendant l'exécution à l'aide des variables de script **Sqlcmd** .  
  
 [!code-sql[HowTo#sp_createmergepullsub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_11.sql)]  
  
## <a name="see-also"></a> Voir aussi  
 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)   
 [Create a Push Subscription](../../../relational-databases/replication/create-a-push-subscription.md)   
 [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md)   
 [Afficher et modifier les paramètres de sécurité de la réplication](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [MSSQL_ENG021797](../../../relational-databases/replication/mssql-eng021797.md)   
 [MSSQL_ENG021798](../../../relational-databases/replication/mssql-eng021798.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Mettre à niveau des bases de données répliquées](../../../database-engine/install-windows/upgrade-replicated-databases.md)  
  
  
