---
title: Stratégies de sauvegarde et de restauration de la réplication transactionnelle et d’instantané | Microsoft Docs
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
- backups [SQL Server replication], snapshot replication
- restoring [SQL Server replication], transactional replication
- snapshot replication [SQL Server], backup and restore
- restoring [SQL Server replication], snapshot replication
- recovery [SQL Server replication], transactional replication
- transactional replication, backup and restore
- recovery [SQL Server replication], snapshot replication
- sync with backup [SQL Server replication]
- backups [SQL Server replication], transactional replication
ms.assetid: a8afcdbc-55db-4916-a219-19454f561f9e
caps.latest.revision: 59
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: bcb4fbdca1bf52c410a9d71e4aaf9b391475024f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication"></a>Stratégies de sauvegarde et de restauration de la réplication transactionnelle et d'instantané
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Lors de la conception d'une stratégie de sauvegarde et de restauration de la réplication transactionnelle et d'instantané, vous devez identifier :  
  
-   les bases de données à sauvegarder ;  
  
-   les paramètres de sauvegarde de la réplication transactionnelle ;  
  
-   les étapes requises pour restaurer une base de données. Celles-ci dépendent du type de réplication et des options choisies.  
  
 Cette rubrique aborde chacun de ces points au cours des trois sections suivantes. Pour plus d’informations sur la sauvegarde et la restauration de la publication Oracle, consultez [Sauvegarde et restauration des serveurs de publication Oracle](../../../relational-databases/replication/non-sql/backup-and-restore-for-oracle-publishers.md).  
  
## <a name="backing-up-databases"></a>Sauvegarde de bases de données  
 Pour la réplication transactionnelle et d'instantané, vous devez sauvegarder régulièrement les bases de données suivantes :  
  
-   Base de données de publication sur le serveur de publication  
  
-   Base de données de distribution sur le serveur de distribution  
  
-   Base de données d'abonnement sur chaque Abonné  
  
-   Bases de données système **master** et **msdb** sur le serveur de publication, sur le serveur de distribution et sur tous les Abonnés. Il est recommandé de sauvegarder ces bases de données en même temps, ainsi que la base de données de réplication appropriée. Par exemple, sauvegardez les bases de données **master** et **msdb** du serveur de publication en même temps que la base de données de publication. Si la base de données de publication est restaurée, assurez-vous que les bases de données **master** et **msdb** sont cohérentes par rapport à la base de données de publication en termes de paramètres de configuration de la réplication.  
  
 Si vous effectuez des sauvegardes régulières des journaux, toutes les modifications liées à la réplication doivent être capturées dans les sauvegardes des journaux. Si vous n'effectuez pas de sauvegardes de fichiers journaux, une sauvegarde doit être effectuée chaque fois qu'une modification portant sur un paramètre de réplication a lieu. Pour en savoir plus, voir [Common Actions Requiring an Updated Backup](../../../relational-databases/replication/administration/common-actions-requiring-an-updated-backup.md).  
  
## <a name="backup-settings-for-transactional-replication"></a>Paramètres de sauvegarde de la réplication transactionnelle  
 La réplication transactionnelle inclut l'utilisation de l'option **sync with backup** , qu'il est possible de définir sur la base de données de distribution et sur celle de publication :  
  
-   Il est recommandé de toujours activer cette option sur la base de données de distribution.  
  
     De cette façon, vous êtes assuré que les transactions du journal de la base de données de publication ne seront pas tronquées tant qu'elles n'auront pas été sauvegardées dans la base de données de distribution. La base de données de distribution peut être restaurée à partir de la dernière sauvegarde. Toutes les transactions manquantes sont remises à la base de données de distribution par la base de données de publication. La réplication se poursuit normalement.  
  
     L'activation de cette option sur la base de données de distribution n'a aucune incidence sur la latence de réplication. Cependant, l'option diffère la troncation du journal sur la base de données de publication jusqu'à ce que les transactions correspondantes de la base de données de distribution aient été sauvegardées. Il en résulte un journal des transactions plus volumineux dans la base de données de publication.  
  
-   Il est recommandé de configurer cette option sur la base de données de publication si votre application peut accepter une latence supplémentaire.  
  
     L'activation de cette option garantit que les transactions ne sont pas remises à la base de données de distribution tant qu'elles ne sont pas sauvegardées dans la base de données de publication. La dernière sauvegarde de la base de données de publication peut ensuite être restaurée sur le serveur de publication en étant assuré que la base de données de distribution ne détient pas des transactions qui n'auraient pas été transmises à la base de données de publication restaurée.  
  
     La latence et le débit sont affectés car les transactions ne peuvent être transmises à la base de données de distribution avant d'avoir été sauvegardées sur le serveur de publication. Si, par exemple, le journal des transactions est sauvegardé toutes les cinq minutes, il existe une latence supplémentaire de cinq minutes entre la validation d'une transaction sur le serveur de publication et la remise de la transaction à la base de données de distribution, puis à l'Abonné.  
  
    > [!NOTE]  
    >  L'option **sync with backup** garantit la cohérence entre les bases de données de publication et de distribution, mais elle ne vous protège pas contre d'éventuelles pertes de données. En cas de perte du journal des transactions par exemple, les transactions validées depuis la dernière sauvegarde de fichier journal ne seront disponibles ni dans la base de données de publication ni dans celle de distribution. Ce comportement est identique à celui d'une base de données non répliquée.  
  
 **Pour définir l'option sync with backup**  
  
-   Programmation [!INCLUDE[tsql](../../../includes/tsql-md.md)]de la réplication : [Activer les sauvegardes coordonnées pour la réplication transactionnelle &#40;programmation Transact-SQL de la réplication&#41;.](../../../relational-databases/replication/administration/enable-coordinated-backups-for-transactional-replication.md)  
  
## <a name="restoring-databases-involved-in-replication"></a>Restauration des bases de données concernées par la réplication  
 Il est possible de restaurer toutes les bases de données d'une topologie de réplication s'il existe des sauvegardes récentes et si vous respectez la procédure indiquée. Les étapes de restauration de la base de données de publication dépendent du type de réplication et des options utilisées, ce qui n'est pas le cas pour toutes les autres bases de données.  
  
 La réplication prend en charge la restauration de bases de données répliquées sur le même serveur et dans la même base de données à partir desquels la sauvegarde a été créée. Si vous restaurez une sauvegarde d'une base de données répliquée sur un autre serveur ou dans une autre base de données, les paramètres de réplication ne peuvent pas être conservés. Dans ce cas, vous devez recréer la totalité des abonnements et des publications une fois les sauvegardes restaurées.  
  
### <a name="publisher"></a>Serveur de publication  
 Des procédures de restauration sont décrites pour les types de réplication suivants :  
  
-   Réplication d'instantané  
  
-   Réplication transactionnelle en lecture seule  
  
-   Réplication transactionnelle avec des abonnements mis à jour  
  
-   Réplication transactionnelle d'égal à égal  
  
 La restauration des bases de données **msdb** et **master** , également décrite dans cette section, est identique pour les quatre types.  
  
#### <a name="publication-database-snapshot-replication"></a>Base de données de publication : réplication d'instantané  
  
1.  Restaurez la dernière sauvegarde de la base de données de publication. Passez à l’étape 2.  
  
2.  Si la sauvegarde de la base de données de publication contient la dernière configuration de tous les abonnements et publications, la restauration est alors terminée. Sinon, passez à l'étape 3.  
  
3.  Supprimez la configuration de la réplication sur le serveur de publication, le serveur de distribution et les Abonnés, puis recréez la configuration. La restauration est terminée.  
  
     Pour plus d’informations sur la suppression de la réplication, consultez [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
#### <a name="publication-database-read-only-transactional-replication"></a>Base de données de publication : réplication transactionnelle en lecture seule  
  
1.  Restaurez la dernière sauvegarde de la base de données de publication. Passez à l’étape 2.  
  
2.  Si l'option **sync with backup** a été activée sur la base de données de publication avant la défaillance, passez à l’étape 3 ; sinon, passez à l’étape 5.  
  
     Si l'option est activée, la requête `SELECT DATABASEPROPERTYEX('<PublicationDatabaseName>', 'IsSyncWithBackup')` retourne la valeur 1.  
  
3.  La sauvegarde restaurée est-elle complète et à jour ? Si elle contient la configuration la plus récente de tous les abonnements et publications, la restauration est alors terminée. Sinon, passez à l’étape 4.  
  
4.  Les informations de configuration dans la base de données de publication restaurée ne sont pas à jour. Par conséquent, vous devez vous assurer que les Abonnés ont toutes les commandes en attente dans la base de données de distribution, puis supprimer et recréer la configuration de la réplication.  
  
    1.  Exécutez l'Agent de distribution jusqu'à ce que tous les Abonnés soient synchronisés avec les commandes non traitées dans la base de données de distribution. Vérifiez que toutes les commandes sont remises aux Abonnés via l'onglet **Commandes non distribuées** du moniteur de réplication ou en interrogeant la vue [MSdistribution_status](../../../relational-databases/system-views/msdistribution-status-transact-sql.md) de la base de données de distribution. Passez à l'étape b.  
  
         Pour plus d’informations sur l’exécution de l’Agent de distribution, consultez [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) ou [Concepts des exécutables de l’Agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
         Pour plus d’informations sur la vérification des commandes, consultez [Afficher les commandes répliquées et autres informations dans la base de données de distribution &#40;Programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/monitor/view-replicated-commands-and-information-in-distribution-database.md) et [Afficher des informations et effectuer des tâches pour les agents d’abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
    2.  Supprimez la configuration de la réplication sur le serveur de publication, le serveur de distribution et les Abonnés, puis recréez la configuration. Lorsque vous recréez les abonnements, spécifiez que l'Abonné possède déjà les données. La restauration est terminée.  
  
         Pour plus d’informations sur la suppression de la réplication, consultez [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
         Pour plus d'informations sur la manière de spécifier que l'Abonné possède déjà les données, consultez [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md).  
  
5.  L'option **sync with backup** n'a pas été définie sur la base de données de publication. Par conséquent, les transactions qui n'ont pas été incluses dans la sauvegarde restaurée ont pu être remises au serveur de distribution et aux Abonnés. Vous devez maintenant vous assurer que les Abonnés possèdent toutes les commandes non traitées de la base de données de distribution, puis appliquer manuellement à la base de données de publication les transactions qui ne sont pas incluses dans la sauvegarde restaurée.  
  
    > [!IMPORTANT]  
    >  L'exécution de cette procédure peut se traduire par une restauration des tables publiées à un point dans le temps plus récent que celui des autres tables non publiées restaurées à partir de la sauvegarde.  
  
    1.  Exécutez l'Agent de distribution jusqu'à ce que tous les Abonnés soient synchronisés avec les commandes non traitées dans la base de données de distribution. Vérifiez que toutes les commandes sont remises aux Abonnés via l'onglet **Commandes non distribuées** du moniteur de réplication ou en interrogeant la vue [MSdistribution_status](../../../relational-databases/system-views/msdistribution-status-transact-sql.md) de la base de données de distribution. Passez à l'étape b.  
  
         Pour plus d’informations sur l’exécution de l’Agent de distribution, consultez [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) ou [Concepts des exécutables de l’Agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
         Pour plus d’informations sur la vérification des commandes, consultez [Afficher les commandes répliquées et autres informations dans la base de données de distribution &#40;Programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/monitor/view-replicated-commands-and-information-in-distribution-database.md) et [Afficher des informations et effectuer des tâches pour les agents d’abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
    2.  Utilisez l' [utilitaire tablediff](../../../tools/tablediff-utility.md) ou un autre outil pour synchroniser manuellement le serveur de publication avec l'Abonné. Cela vous permet de récupérer les données de la base de données d'abonnement qui ne se trouvaient pas dans la sauvegarde de la base de données de publication. Passez à l'étape c.  
  
         Pour plus d’informations sur l’utilitaire **tablediff**, consultez [Comparer des tables répliquées pour identifier les différences &#40;programmation de réplication&#41;](../../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md).  
  
    3.  La sauvegarde restaurée est-elle complète et à jour ? Si elle contient la configuration la plus récente de tous les abonnements et publications, exécutez la procédure stockée [sp_replrestart](../../../relational-databases/system-stored-procedures/sp-replrestart-transact-sql.md) pour resynchroniser les métadonnées du serveur de publication avec celles du serveur de distribution. La restauration est terminée. Sinon, passez à l'étape d.  
  
    4.  Supprimez la configuration de la réplication sur le serveur de publication, le serveur de distribution et les Abonnés, puis recréez la configuration. Lorsque vous recréez les abonnements, spécifiez que l'Abonné possède déjà les données. La restauration est terminée.  
  
         Pour plus d’informations sur la suppression de la réplication, consultez [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
         Pour plus d'informations sur la manière de spécifier que l'Abonné possède déjà les données, consultez [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md).  
  
#### <a name="publication-database-transactional-replication-with-updating-subscriptions"></a>Base de données de publication : réplication transactionnelle avec des abonnements mis à jour  
  
1.  Restaurez la dernière sauvegarde de la base de données de publication. Passez à l’étape 2.  
  
2.  Exécutez l'Agent de distribution jusqu'à ce que tous les Abonnés soient synchronisés avec les commandes non traitées dans la base de données de distribution. Vérifiez que toutes les commandes sont remises aux Abonnés via l'onglet **Commandes non distribuées** du moniteur de réplication ou en interrogeant la vue [MSdistribution_status](../../../relational-databases/system-views/msdistribution-status-transact-sql.md) de la base de données de distribution. Passez à l’étape 3.  
  
     Pour plus d’informations sur l’exécution de l’Agent de distribution, consultez [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) ou [Concepts des exécutables de l’Agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
     Pour plus d’informations sur la vérification des commandes, consultez [Afficher les commandes répliquées et autres informations dans la base de données de distribution &#40;Programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/monitor/view-replicated-commands-and-information-in-distribution-database.md) et [Afficher des informations et effectuer des tâches pour les agents d’abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
3.  Si vous utilisez les abonnements mis à jour en attente, connectez-vous à chaque Abonné et supprimez toutes les lignes de la table [MSreplication_queue &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/msreplication-queue-transact-sql.md) dans la base de données d’abonnement. Passez à l’étape 4.  
  
    > [!NOTE]  
    >  Si vous utilisez des abonnements mis à jour en attente et qu'une table quelconque contient des colonnes d'identité, vous devez vérifier que les plages d'identité correctes sont affectées après une restauration. Pour plus d’informations, consultez [ Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
4.  Vous devez maintenant vous assurer que les Abonnés possèdent toutes les commandes non traitées de la base de données de distribution, puis appliquer manuellement à la base de données de publication les transactions qui ne sont pas incluses dans la sauvegarde restaurée.  
  
    > [!IMPORTANT]  
    >  L'exécution de cette procédure peut se traduire par une restauration des tables publiées à un point dans le temps plus récent que celui des autres tables non publiées restaurées à partir de la sauvegarde.  
  
    1.  Exécutez l'Agent de distribution jusqu'à ce que tous les Abonnés soient synchronisés avec les commandes non traitées dans la base de données de distribution. Vérifiez que toutes les commandes sont remises aux Abonnés à l'aide du moniteur de réplication ou en interrogeant la vue [MSdistribution_status](../../../relational-databases/system-views/msdistribution-status-transact-sql.md) dans la base de données de distribution. Passez à l'étape b.  
  
    2.  Utilisez l' [tablediff Utility](../../../tools/tablediff-utility.md) ou un autre outil pour synchroniser manuellement le serveur de publication avec l'Abonné. Cela vous permet de récupérer les données de la base de données d'abonnement qui ne se trouvaient pas dans la sauvegarde de la base de données de publication. Passez à l'étape c.  
  
         Pour plus d’informations sur l’utilitaire **tablediff**, consultez [Comparer des tables répliquées pour identifier les différences &#40;programmation de réplication&#41;](../../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md).  
  
    3.  La sauvegarde restaurée est-elle complète et à jour ? Si elle contient la configuration la plus récente de tous les abonnements et publications, exécutez la procédure stockée [sp_replrestart](../../../relational-databases/system-stored-procedures/sp-replrestart-transact-sql.md) pour resynchroniser les métadonnées du serveur de publication avec celles du serveur de distribution. La restauration est terminée. Sinon, passez à l'étape d.  
  
    4.  Supprimez la configuration de la réplication sur le serveur de publication, le serveur de distribution et les Abonnés, puis recréez la configuration. Lorsque vous recréez les abonnements, spécifiez que l'Abonné possède déjà les données. La restauration est terminée.  
  
         Pour plus d’informations sur la suppression de la réplication, consultez [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
         Pour plus d'informations sur la manière de spécifier que l'Abonné possède déjà les données, consultez [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md).  
  
#### <a name="publication-database-peer-to-peer-transactional-replication"></a>Base de données de publication : réplication transactionnelle d'égal à égal  
 Dans les étapes suivantes, les bases de données de publication **A**, **B**et **C** font partie d'une topologie de réplication transactionnelle d'égal à égal. Les bases de données **A** et **C** sont en ligne et fonctionnent correctement tandis que la base de données **B** doit être restaurée. Le processus décrit ici, surtout les étapes 7, 10 et 11, est très similaire à celui requis pour ajouter un nœud à une topologie d'égal à égal. Le moyen le plus simple pour effectuer ces étapes consiste à utiliser l'Assistant Configurer la topologie d'égal à égal. Toutefois, vous pouvez également utiliser des procédures stockées.  
  
1.  Exécutez les Agents de distribution pour synchroniser les abonnements sur les bases de données **A** et **C**. Passez à l’étape 2.  
  
     Pour plus d’informations sur l’exécution de l’Agent de distribution, consultez [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) ou [Concepts des exécutables de l’Agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
2.  Si la base de données de distribution utilisée par **B** est toujours accessible, exécutez les Agents de distribution pour synchroniser les abonnements entre les bases de données **B** et **A** et les bases de données B et **C**. Passez à l’étape 3.  
  
3.  Supprimez les métadonnées de la base de données de distribution utilisée par **B** en exécutant [sp_removedistpublisherdbreplication](../../../relational-databases/system-stored-procedures/sp-removedistpublisherdbreplication-transact-sql.md) sur la base de données de distribution de **B**. Passez à l’étape 4.  
  
4.  Dans les bases de données **A** et **C**, supprimez les abonnements à la publication hébergée dans la base de données **B**. Passez à l’étape 5.  
  
     Pour plus d'informations sur la suppression des abonnements, consultez [Subscribe to Publications](../../../relational-databases/replication/subscribe-to-publications.md).  
  
5.  Effectuez une sauvegarde de fichier journal ou une sauvegarde complète de la base de données **A**. Passez à l’étape 6.  
  
6.  Restaurez la sauvegarde de la base de données **A** sur la base de données **B**. La base de données **B** possède maintenant les données de la base de données **A**, mais pas la configuration de la réplication. Lorsque vous restaurez une sauvegarde sur un autre serveur, la réplication est supprimée. La réplication a donc été supprimée de la base de données **B**. Passez à l’étape 7.  
  
7.  Recréez la publication sur la base de données **B**, puis recréez les abonnements entre les bases de données **A** et **B**. (Les abonnements qui impliquent la base de données **C** sont gérés à une étape ultérieure.)  
  
    1.  Recréez la publication sur la base de données **B**. Passez à l'étape b.  
  
    2.  Recréez l'abonnement de la base de données **B** à la publication de la base de données **A**, en spécifiant que l'abonnement doit être initialisé avec une sauvegarde (valeur **initialize with backup** pour le paramètre **@sync_type** de [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)). Passez à l'étape c.  
  
    3.  Recréez l'abonnement de la base de données **A** à la publication de la base de données **B**, en spécifiant que l'abonnement possède déjà les données (valeur **replication support only** pour le paramètre **@sync_type** de [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)). Passez à l’étape 8.  
  
8.  Exécutez les Agents de distribution pour synchroniser les abonnements sur les bases de données **A** et **B**. Si les tables publiées comportent des colonnes d'identité, passez à l'étape 9. Sinon, passez à l’étape 10.  
  
9. Après la restauration, la plage d’identité assignée à chaque table de la base de données **A** est également utilisée dans la base de données **B**. Vérifiez que la base de données **B** restaurée a reçu toutes les modifications de la base de données **B** défaillante qui ont été propagées aux bases de données **A** et **C**, puis réattribuez une valeur de départ à la plage d’identité de chaque table.  
  
    1.  Exécutez [sp_requestpeerresponse](../../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md) sur la base de données **B** et récupérez le paramètre de sortie **@request_id**. Passez à l'étape b.  
  
    2.  Par défaut, l'Agent de distribution est configuré pour s'exécuter en continu ; par conséquent, les jetons doivent être envoyés automatiquement à tous les nœuds. Si l'Agent de distribution ne s'exécute pas en mode continu, exécutez l'Agent. Pour plus d’informations, consultez [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md) ou [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md). Passez à l'étape c.  
  
    3.  Exécutez [sp_helppeerresponses](../../../relational-databases/system-stored-procedures/sp-helppeerresponses-transact-sql.md), en fournissant la valeur **@request_id** récupérée à l'étape b. Attendez que tous les nœuds indiquent qu'ils ont reçu la demande de l'homologue. Passez à l'étape d.  
  
    4.  Utilisez [DBCC CHECKIDENT](../../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md) pour réattribuer une valeur de départ à chaque table de la base de données **B** et vérifier qu'une plage appropriée est utilisée. Passez à l'étape 10.  
  
     Pour plus d’informations sur la gestion des plages d’identité, consultez la section « Attribution de plages pour la gestion manuelle des plages d’identité » dans [Réplication de colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
10. À ce stade, les bases de données **B** et **C** ne sont pas directement connectées, mais elles reçoivent les modifications via la base de données **A**. Si la topologie contient des nœuds qui exécutent [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], passez à l'étape 11 ; sinon, passez à l'étape 12.  
  
11. Suspendez le système, puis recréez l’abonnement entre les bases de données **B** et **C**. Suspendre un système implique d’interrompre toute activité sur les tables publiées de tous les nœuds et de vérifier que chaque nœud a reçu toutes les modifications des autres nœuds.  
  
    1.  Arrêtez toute activité sur les tables publiées dans la topologie d'égal à égal. Passez à l'étape b.  
  
    2.  Exécutez [sp_requestpeerresponse](../../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md) sur la base de données **B** et récupérez le paramètre de sortie **@request_id**. Passez à l'étape c.  
  
    3.  Par défaut, l'Agent de distribution est configuré pour s'exécuter en continu ; par conséquent, les jetons doivent être envoyés automatiquement à tous les nœuds. Si l'Agent de distribution ne s'exécute pas en mode continu, exécutez l'Agent. Passez à l'étape d.  
  
    4.  Exécutez [sp_helppeerresponses](../../../relational-databases/system-stored-procedures/sp-helppeerresponses-transact-sql.md), en fournissant la valeur **@request_id** récupérée à l'étape b. Attendez que tous les nœuds indiquent qu'ils ont reçu la demande de l'homologue. Passez à l'étape e.  
  
    5.  Recréez l'abonnement de la base de données **B** à la publication de la base de données **C**, en spécifiant que l'Abonné possède déjà les données. Passez à l'étape b.  
  
    6.  Recréez l'abonnement de la base de données **C** à la publication de la base de données **B**, en spécifiant que l'Abonné possède déjà les données. Passez à l’étape 13.  
  
12. Recréez l'abonnement entre les bases de données **B** et **C**:  
  
    1.  Sur la base de données **B**, interrogez la table [MSpeer_lsns](../../../relational-databases/system-tables/mspeer-lsns-transact-sql.md) pour récupérer le numéro séquentiel dans le journal de la transaction la plus récente que la base de données **B** a reçue de la base de données **C**.  
  
    2.  Recréez l'abonnement de la base de données **B** à la publication de la base de données **C**, en spécifiant que l'abonnement doit être initialisé en fonction du numéro séquentiel dans le journal (valeur **initialize with backup** pour le paramètre **@sync_type** de [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)). Passez à l'étape b.  
  
    3.  Recréez l'abonnement de la base de données **C** à la publication de la base de données **B**, en spécifiant que l'Abonné possède déjà les données. Passez à l’étape 13.  
  
13. Exécutez les Agents de distribution pour synchroniser les abonnements sur les bases de données **B** et **C**. La restauration est terminée.  
  
#### <a name="msdb-database-publisher"></a>Base de données msdb (serveur de publication)  
  
1.  Restaurez la dernière sauvegarde de la base de données **msdb** .  
  
2.  La sauvegarde restaurée est-elle complète et à jour ? Si elle contient la configuration la plus récente de tous les abonnements et publications, la récupération est terminée. Sinon, passez à l'étape 3.  
  
3.  Recréez le travail de nettoyage de l'abonnement à partir de vos scripts de réplication. La récupération est terminée.  
  
#### <a name="master-database-publisher"></a>Base de données master (serveur de publication)  
  
1.  Restaurez la dernière sauvegarde de la base de données **master** .  
  
2.  Vérifiez que les paramètres et la configuration de réplication de la base de données et ceux de la base de données de publication sont cohérents.  
  
### <a name="databases-at-the-distributor"></a>Bases de données du serveur de distribution  
  
#### <a name="distribution-database"></a>Base de données de distribution  
  
1.  Restaurez la dernière sauvegarde de la base de données de distribution.  
  
2.  Si l'option **sync with backup** a été activée sur la base de données de distribution avant la défaillance, passez à l'étape 3 ; sinon passez à l'étape 4.  
  
     Si l'option est activée, la requête `SELECT DATABASEPROPERTYEX('<DistributionDatabaseName>', 'IsSyncWithBackup')` retourne la valeur 1.  
  
3.  La sauvegarde restaurée est-elle complète et à jour ? Si elle contient la configuration la plus récente de tous les abonnements et publications, la récupération est terminée. Sinon, passez à l’étape 4.  
  
4.  Les informations de configuration dans la base de données de distribution restaurée ne sont pas à jour ou l'option **sync with backup** n'a pas été définie sur la base de données de distribution. (Après la restauration, la base de données de distribution peut ignorer des transactions qui ont été validées sur le serveur de publication, mais qui n'ont pas encore été remises aux Abonnés.) Supprimez et recréez la réplication, puis exécutez la validation.  
  
    1.  Supprimez la configuration de la réplication sur le serveur de publication, le serveur de distribution et les Abonnés, puis recréez la configuration. Lorsque vous recréez les abonnements, spécifiez que l'Abonné possède déjà les données. Passez à l'étape b.  
  
         Pour plus d’informations sur la suppression de la réplication, consultez [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
         Pour plus d'informations sur la manière de spécifier que l'Abonné possède déjà les données, consultez [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md).  
  
    2.  Marquez toutes les publications pour validation. Réinitialisez tous les abonnements qui n'ont pas pu être validés. La récupération est terminée.  
  
         Pour plus d'informations sur la validation, consultez [Validate Replicated Data](../../../relational-databases/replication/validate-replicated-data.md). Pour plus d’informations sur la réinitialisation, consultez [Réinitialiser des abonnements](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="msdb-database-distributor"></a>Base de données msdb (serveur de distribution)  
  
1.  Restaurez la dernière sauvegarde de la base de données **msdb** .  
  
2.  La sauvegarde restaurée est-elle complète et à jour ? Si elle contient la configuration la plus récente de tous les abonnements et publications, la récupération est terminée. Sinon, passez à l'étape 3.  
  
3.  Supprimez la configuration de la réplication sur le serveur de publication, le serveur de distribution et les Abonnés, puis recréez la configuration. Lorsque vous recréez les abonnements, spécifiez que l'Abonné possède déjà les données. Passez à l’étape 4.  
  
     Pour plus d’informations sur la suppression de la réplication, consultez [sp_removedbreplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
     Pour plus d'informations sur la manière de spécifier que l'Abonné possède déjà les données, consultez [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md).  
  
4.  Marquez toutes les publications pour validation. Réinitialisez tous les abonnements qui n'ont pas pu être validés. La récupération est terminée.  
  
     Pour plus d'informations sur la validation, consultez [Validate Replicated Data](../../../relational-databases/replication/validate-replicated-data.md). Pour plus d’informations sur la réinitialisation, consultez [Réinitialiser des abonnements](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="master-database-distributor"></a>Base de données master (serveur de distribution)  
  
1.  Restaurez la dernière sauvegarde de la base de données **master** .  
  
2.  Vérifiez que les paramètres et la configuration de réplication de la base de données et ceux de la base de données de publication sont cohérents.  
  
### <a name="databases-at-the-subscriber"></a>Bases de données de l'Abonné  
  
#### <a name="subscription-database"></a>Base de données d'abonnement  
  
1.  Est-ce que la dernière sauvegarde de base de données d'abonnement est plus récente que le paramètre de rétention minimal de la distribution sur la base de données de distribution ? (Cela détermine si le serveur de distribution possède encore toutes les commandes requises pour mettre l'Abonné à jour.) S'il en contient, passez à l'étape 2. Si non, réinitialisez l'abonnement. La récupération est terminée.  
  
     Pour déterminer le paramètre de rétention maximale de la distribution, exécutez [sp_helpdistributiondb](../../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md) et récupérez la valeur de la colonne **max_distretention** (exprimée en heures).  
  
     Pour plus d'informations sur la réinitialisation d'un abonnement, consultez [Reinitialize a Subscription](../../../relational-databases/replication/reinitialize-a-subscription.md).  
  
2.  Restaurez la dernière sauvegarde de la base de données d'abonnement. Passez à l’étape 3.  
  
3.  Si la base de données d'abonnement contient uniquement des abonnements par émission de données, allez à étape 4. Si la base de données d'abonnement contient des abonnements par extraction, posez-vous les questions suivantes : les informations d'abonnement sont-elles à jour ? La base de données inclut-elle toutes les tables et options définies au moment de la défaillance ? Si tel est le cas, passez à l’étape 4. Si non, réinitialisez l'abonnement. La récupération est terminée.  
  
4.  Pour synchroniser l'Abonné, exécutez l'Agent de distribution. La récupération est terminée.  
  
     Pour plus d’informations sur l’exécution de l’Agent de distribution, consultez [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) ou [Concepts des exécutables de l’Agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
#### <a name="msdb-database-subscriber"></a>Base de données msdb (Abonné)  
  
1.  Restaurez la dernière sauvegarde de la base de données **msdb** . L'Abonné utilise-t-il les abonnements par extraction ? Si non, la restauration est terminée. S'il en contient, passez à l'étape 2.  
  
2.  La sauvegarde restaurée est-elle complète et à jour ? Si elle contient la configuration la plus récente de tous les abonnements par extraction, la récupération est terminée. Sinon, passez à l'étape 3.  
  
3.  Supprimez et recréez les abonnements par extraction. Lorsque vous recréez les abonnements, spécifiez que l'Abonné possède déjà les données. La restauration est terminée.  
  
     Pour plus d'informations sur la suppression des abonnements, consultez [Subscribe to Publications](../../../relational-databases/replication/subscribe-to-publications.md).  
  
     Pour plus d'informations sur la manière de spécifier que l'Abonné possède déjà les données, consultez [Initialize a Subscription Manually](../../../relational-databases/replication/initialize-a-subscription-manually.md).  
  
#### <a name="master-database-subscriber"></a>Base de données master (Abonné)  
  
1.  Restaurez la dernière sauvegarde de la base de données **master** .  
  
2.  Vérifiez que les paramètres et la configuration de réplication de la base de données et ceux de la base de données de publication sont cohérents.  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarde et restauration des bases de données SQL Server](../../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Sauvegarder et restaurer des bases de données répliquées](../../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
 [Configurer la distribution](../../../relational-databases/replication/configure-distribution.md)   
 [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Subscribe to Publications](../../../relational-databases/replication/subscribe-to-publications.md)   
 [Initialiser un abonnement](../../../relational-databases/replication/initialize-a-subscription.md)   
 [Synchroniser les données](../../../relational-databases/replication/synchronize-data.md)  
  
  
