---
title: Journal des transactions (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- transaction logs [SQL Server], about
- databases [SQL Server], transaction logs
- logs [SQL Server], transaction logs
ms.assetid: d7be5ac5-4c8e-4d0a-b114-939eb97dac4d
caps.latest.revision: 65
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 877c48ecbe9befaf0bb04f34a866dd8fee6671bc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="the-transaction-log-sql-server"></a>Journal des transactions (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Chaque base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un journal des transactions qui enregistre toutes les transactions et les modifications apportées par chacune d’entre elles.
  
Le journal des transactions est un composant essentiel de la base de données. En cas de défaillance du système, vous aurez besoin de ce journal pour rétablir votre base de données à un état cohérent. 

Pour plus d’informations sur l’architecture du journal des transactions et les structures internes, consultez [Guide d’architecture et gestion du journal des transactions SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md).

> [!WARNING] 
> Le journal des transactions ne doit être ni supprimé ni déplacé, sauf si vous comprenez pleinement les conséquences d’une telle opération. 

> [!TIP]
> Différents points de contrôle créent des points de référence connus et fiables à partir desquels vous pouvez commencer à appliquer les journaux des transactions lors de la récupération d'une base de données. Pour plus d’informations, consultez [Points de contrôle de base de données (SQL Server)](../../relational-databases/logs/database-checkpoints-sql-server.md).  
  
## <a name="operations-supported-by-the-transaction-log"></a>Opérations prises en charge par le journal des transactions  
 Le journal des transactions prend en charge les opérations suivantes :  
  
-   Récupération des transactions individuelles.  
-   Récupération de toutes les transactions incomplètes au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
-   Restauration par progression d'une base de données, d'un fichier, d'un groupe de fichiers ou d'une page jusqu'au point de défaillance  
-   Prise en charge de la réplication transactionnelle  
-   Prise en charge de la haute disponibilité et de solutions de récupération d'urgence : [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], mise en miroir de bases de données et copie des journaux de transaction.

### <a name="individual-transaction-recovery"></a>Récupération des transactions individuelles
Si une application envoie une instruction `ROLLBACK`, ou si le moteur de base de données détecte une erreur telle que la perte de la communication avec un client, les enregistrements du journal permettent de restaurer les modifications effectuées par une transaction incomplète. 

### <a name="recovery-of-all-incomplete-transactions-when-includessnoversionincludesssnoversion-mdmd-is-started"></a>Récupération de toutes les transactions incomplètes au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
En cas de défaillance d’un serveur, il peut arriver que certaines modifications effectuées dans les bases de données n’aient jamais pu être écrites de la mémoire tampon vers les fichiers de données ou proviennent de transactions incomplètes dans les fichiers de données. Lorsqu'elle démarre, une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute une récupération de chaque base de données. Chaque modification enregistrée dans le journal qui risque de ne pas avoir été répercutée dans les fichiers de données est récupérée. Chaque transaction incomplète trouvée dans le journal des transactions est ensuite restaurée afin de préserver l'intégrité de la base de données. 

### <a name="rolling-a-restored-database-file-filegroup-or-page-forward-to-the-point-of-failure"></a>Restauration par progression d’une base de données, d’un fichier, d’un groupe de fichiers ou d’une page jusqu’au point de défaillance
Après un incident matériel ou une défaillance de disque touchant les fichiers de base de données, vous pouvez restaurer la base de données jusqu'au point de défaillance. Restaurez tout d'abord la dernière sauvegarde complète et différentielle de la base de données, puis restaurez la séquence suivante des sauvegardes des journaux des transactions jusqu'au point de défaillance. 

Quand vous restaurez chaque sauvegarde de journal, le moteur de base de données réapplique toutes les modifications enregistrées dans le journal pour restaurer par progression toutes les transactions. Quand la dernière sauvegarde de journal a été restaurée, le moteur de base de données utilise les informations des journaux pour restaurer toutes les transactions qui n’ont pas été terminées à ce stade. 

### <a name="supporting-transactional-replication"></a>Prise en charge de la réplication transactionnelle
L’Agent de lecture du journal surveille le journal des transactions de chaque base de données configurée pour la réplication transactionnelle et copie les transactions devant être répliquées à partir du journal des transactions dans la base de données de distribution. Pour plus d’informations sur la réplication transactionnelle, consultez [Fonctionnement de la réplication transactionnelle](http://msdn.microsoft.com/library/ms151706.aspx).

### <a name="supporting-high-availability-and-disaster-recovery-solutions"></a>Prise en charge des solutions de récupération d’urgence et de haute disponibilité
Les solutions à serveur de secours, [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], les mises en miroir de base de données et les copies des journaux de transactions dépendent fortement du journal des transactions. 

Dans un **scénario [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]**, chaque mise à jour d’une base de données, le réplica principal, est immédiatement reproduite dans des copies distinctes et complètes de la base de données, les réplicas secondaires. Le réplica principal envoie immédiatement chaque enregistrement de journal aux réplicas secondaires, qui appliquent les enregistrements de journal entrants aux bases de données du groupe de disponibilité, avec une restauration par progression continue. Pour plus d’informations, consultez [Instances de cluster de basculement AlwaysOn](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)

Dans un **scénario de copie des journaux de transactions**, le serveur principal envoie le journal des transactions actif de la base de données primaire vers une ou plusieurs destinations. Chaque serveur secondaire restaure le journal dans sa base de données secondaire locale. Pour plus d’informations, consultez [À propos de la copie des journaux de transaction](../../database-engine/log-shipping/about-log-shipping-sql-server.md). 

Dans un **scénario de mise en miroir de base de données**, toutes les mises à jour d’une base de données, à savoir la base de données principale, sont immédiatement reproduites dans une copie distincte complète de la base de données, la base de données miroir. L'instance du serveur principal envoie chaque enregistrement du journal immédiatement à l'instance du serveur miroir, qui applique ces enregistrements à la base de données miroir en la restaurant constamment par progression. Pour plus d’informations, consultez [Mise en miroir de bases de données](../../database-engine/database-mirroring/database-mirroring-sql-server.md).

##  <a name="Characteristics"></a>Caractéristiques du journal des transactions

Caractéristiques du journal des transactions [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] : 
-  Le journal des transactions est mis en œuvre sous la forme d'un fichier ou d'un ensemble de fichiers distinct dans la base de données. Le cache du journal est géré indépendamment du cache des tampons des pages de données, ce qui se traduit par un code simple, rapide et robuste dans le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Pour plus d’informations, consultez [Architecture physique du journal des transactions](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch).
-  Le format des enregistrements et des pages du journal ne suit pas obligatoirement celui des pages de données.
-  Le journal des transactions peut être implémenté dans plusieurs fichiers. Les fichiers peuvent être configurés pour croître automatiquement en définissant la valeur `FILEGROWTH` pour le journal. Le risque d'insuffisance de l'espace dans le journal des transactions et la surcharge administrative sont ainsi réduits. Pour plus d’informations, consultez [Options de fichiers et de groupes de fichiers &#40;Transact-SQL&#41; ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).
-  Le mécanisme de réutilisation de l'espace des fichiers journaux est rapide et a une incidence minimale sur le débit des transactions.

Pour plus d’informations sur l’architecture du journal des transactions et les structures internes, consultez [Guide d’architecture et gestion du journal des transactions SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md).

##  <a name="Truncation"></a> Troncation du journal des transactions  
La troncation du journal libère de l'espace dans le fichier journal pour que le journal des transactions puisse le réutiliser. Vous devez régulièrement tronquer le journal des transactions pour l’empêcher de remplir l’espace imparti. Plusieurs facteurs peuvent retarder la troncation du journal. Il est donc important de surveiller sa taille. Certaines opérations peuvent faire l'objet d'une journalisation minimale afin de réduire leur impact sur la taille des journaux de transactions.  
 
La troncation du journal supprime les [fichiers journaux virtuels](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) inactifs du journal des transactions logique d’une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce qui libère de l’espace dans le journal logique de façon à ce qu’il soit réutilisé par le journal des transactions physique. Si un journal des transactions n’est jamais tronqué, il finit par occuper tout l’espace disque alloué aux fichiers journaux physiques.  
  
Pour éviter de manquer d’espace, à moins que la troncation du journal soit retardée pour une raison quelconque, la troncation se produit automatiquement après les événements suivants :  
  
- En mode de récupération simple, après un point de contrôle.  
- En mode de récupération complète ou de récupération utilisant les journaux de transactions, si un point de contrôle s'est produit depuis la sauvegarde précédente, la troncation se produit après une sauvegarde de fichier journal (sauf s'il s'agit d'une sauvegarde de copie uniquement).  
  
 Pour plus d’informations, consultez [Facteurs pouvant retarder la troncation du journal](#FactorsThatDelayTruncation), plus loin dans cette rubrique.  
  
> [!NOTE]
> La troncation du journal ne réduit pas la taille du fichier journal physique. Pour réduire la taille physique d'un fichier journal physique, vous devez réduire le fichier journal. Pour plus d'informations sur la réduction de la taille du fichier journal physique, consultez [Gérer la taille du fichier journal des transactions](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md).  
> Toutefois, gardez à l’esprit les [facteurs pouvant retarder la troncation du journal](#FactorsThatDelayTruncation). Si l’espace de stockage est à nouveau nécessaire après une réduction de journal, le journal des transactions croît de nouveau, introduisant une surcharge au niveau des performances pendant les opérations d’accroissement du journal.
  
##  <a name="FactorsThatDelayTruncation"></a> Factors that can delay log truncation  
 Lorsque les enregistrements de journal restent actifs longtemps, la troncation du journal des transactions est retardée et le journal des transactions peut se remplir entièrement, comme nous l’avons déjà mentionné dans cette longue rubrique.  
  
> [!IMPORTANT]
> Pour plus d’informations sur la façon de répondre à un journal des transactions saturé, consultez [Résoudre les problèmes liés à un journal des transactions saturé&#40;Erreur SQL Server 9002&#41;](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md).  
  
 En réalité, la troncation du journal peut être différée pour différentes raisons. Le cas échéant, découvrez ce qui empêche de tronquer le journal en interrogeant les colonnes **log_reuse_wait** et **log_reuse_wait_desc** de l’affichage catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Le tableau suivant décrit les valeurs possibles de ces colonnes.  
  
|Valeur log_reuse_wait|Valeur log_reuse_wait_desc|Description|  
|----------------------------|----------------------------------|-----------------|  
|0|NOTHING|Il existe un ou plusieurs [fichiers journaux virtuels réutilisables](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch).|  
| 1|CHECKPOINT|Aucun point de contrôle n’est apparu depuis la dernière troncation du journal ou le début du journal n’est pas encore allé au-delà d’un [fichier journal virtuel](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch). (Tous les modes de récupération)<br /><br /> Il s'agit d'une raison courante de retarder la troncation du journal. Pour plus d’informations, consultez [Points de contrôle de base de données &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).|  
|2|LOG_BACKUP|Une sauvegarde du journal est requise avant que le journal des transactions puisse être tronqué. (Mode de récupération complète ou mode de récupération utilisant les journaux de transactions uniquement)<br /><br /> Lorsque la sauvegarde de journal suivante est terminée, l'espace du journal peut devenir réutilisable.|  
|3|ACTIVE_BACKUP_OR_RESTORE|Une sauvegarde de données ou une restauration est en cours (tous les modes de récupération).<br /><br /> Si une sauvegarde des données empêche la troncation du journal, l'annulation de l'opération de sauvegarde peut résoudre le problème immédiat.|  
|4|ACTIVE_TRANSACTION|Une transaction est active (tous les modes de récupération) :<br /><br /> Une transaction longue peut exister au démarrage de la sauvegarde du fichier journal. Dans ce cas, libérer l'espace peut requérir une autre sauvegarde du fichier journal. Notez que les transactions longues empêchent la troncation du journal dans tous les modes de récupération, notamment le mode de récupération simple, où le journal des transactions est généralement tronqué sur chaque point de contrôle automatique.<br /><br /> Une transaction est différée. Une *transaction différée* est en fait une transaction active dont la restauration est bloquée à cause d'une ressource indisponible. Pour plus d’informations sur les causes des transactions différées et la manière de les faire sortir de l’état différé, consultez [Transactions différées &#40;SQL Server&#41;](../../relational-databases/backup-restore/deferred-transactions-sql-server.md).<br /> <br /> Les transactions à long terme peuvent également remplir le journal des transactions de tempdb. La base de données tempdb est implicitement utilisée par les transactions utilisateur pour les objets internes tels que les tables de travail pour le tri, les fichiers de travail pour le hachage, les tables de travail de curseur et la gestion de version de ligne. Même si la transaction utilisateur inclut uniquement les données de lecture (requêtes `SELECT`), des objets internes peuvent être créés et utilisés dans des transactions utilisateur. Ensuite, le journal des transactions tempdb peut être rempli.|  
|5|DATABASE_MIRRORING|La mise en miroir de bases de données est interrompue ou, en mode haute performance, la base de données miroir se trouve derrière la base de données principale de manière significative. (Mode de récupération complète uniquement)<br /><br /> Pour plus d’informations, consultez [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).|  
|6|REPLICATION|Durant les réplications transactionnelles, les transactions liées aux publications ne sont pas encore remises à la base de données de distribution. (Mode de récupération complète uniquement)<br /><br /> Pour plus d'informations sur la réplication transactionnelle, consultez [SQL Server Replication](../../relational-databases/replication/sql-server-replication.md).|  
|7|DATABASE_SNAPSHOT_CREATION|Un instantané de base de données est créé. (Tous les modes de récupération)<br /><br /> Il s'agit d'une raison courante et habituellement brève du retard de la troncation du journal.|  
|8|LOG_SCAN|Une analyse du journal se produit. (Tous les modes de récupération)<br /><br /> Il s'agit d'une raison courante et habituellement brève du retard de la troncation du journal.|  
|9|AVAILABILITY_REPLICA|Un réplica secondaire d'un groupe de disponibilité applique les enregistrements du journal des transactions de cette base de données à une base de données secondaire associée. (Mode de récupération complète)<br /><br /> Pour plus d’informations, consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).|  
|10|—|Usage interne uniquement|  
|11|—|Usage interne uniquement|  
|12|—|Usage interne uniquement|  
|13|OLDEST_PAGE|Si une base de données est configurée pour utiliser des points de contrôle indirects, la page la plus ancienne dans la base de données peut être plus ancienne que le [numéro séquentiel dans le journal (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) du point de contrôle. Dans ce cas, la page la plus ancienne peut retarder la troncation du journal. (Tous les modes de récupération)<br /><br /> Pour plus d’informations sur les points de contrôle indirects, consultez [Points de contrôle de base de données &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).|  
|14|OTHER_TRANSIENT|Cette valeur n'est pas utilisée actuellement.|  
  
##  <a name="MinimallyLogged"></a> Opérations pouvant faire l’objet d’une journalisation minimale  
La*journalisation minimale* implique de ne journaliser que les informations obligatoires pour pouvoir récupérer la transaction sans prendre en charge la récupération jusqu’à une date et heure. Cette rubrique identifie les opérations qui sont journalisées au minimum en [mode de récupération](../backup-restore/recovery-models-sql-server.md) utilisant les journaux de transactions (ainsi qu’en mode de récupération simple, sauf quand une sauvegarde est en cours).  
  
> [!NOTE]
> La journalisation minimale n'est pas prise en charge pour les tables à mémoire optimisée.  
  
> [!NOTE]
> En [mode de récupération complète](../backup-restore/recovery-models-sql-server.md), toutes les opérations en bloc sont entièrement journalisées. Cependant, vous pouvez minimiser la journalisation d'un ensemble d'opérations en bloc en faisant temporairement passer la base de données en mode de récupération utilisant les journaux de transactions pour les opérations en bloc. La journalisation minimale est plus efficace que la journalisation complète et réduit la possibilité pour une opération en bloc de grande envergure d'occuper l'espace disponible du journal des transactions pendant une transaction en bloc. En revanche, si la base de données est endommagée ou perdue lors de la journalisation minimale, vous ne pouvez pas récupérer la base de données jusqu'au point de défaillance.  
  
 Les opérations suivantes, qui sont entièrement journalisées en mode de récupération complète, font l'objet d'une journalisation minimale en modes simple et de récupération utilisant les journaux de transactions :  
  
-   Opérations d’importation en bloc ([bcp](../../tools/bcp-utility.md), [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et [INSERT... SELECT](../../t-sql/statements/insert-transact-sql.md)). Pour plus d'informations sur les conditions dans lesquelles la journalisation d'une importation en bloc dans une table est minimale, consultez [Prerequisites for Minimal Logging in Bulk Import](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).  
  
Quand la réplication transactionnelle est activée, les opérations `BULK INSERT` sont entièrement journalisées, même dans le mode de récupération utilisant les journaux de transactions.  
  
-   Opérations [SELECT INTO](../../t-sql/queries/select-into-clause-transact-sql.md).  
  
Lorsque la réplication transactionnelle est activée, les opérations SELECT INTO sont entièrement journalisées, même dans le mode de récupération utilisant les journaux de transactions.  
  
-   Mises à jour partielles vers des types de données de valeur élevée, en incluant la clause `.WRITE` dans l’instruction [UPDATE](../../t-sql/queries/update-transact-sql.md) pendant l’insertion ou l’ajout de nouvelles données. Notez que la journalisation minimale n'est pas utilisée quand des valeurs existantes sont mises à jour. Pour plus d’informations sur les types de données de valeur élevée, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).  
  
-   Instructions[WRITETEXT](../../t-sql/queries/writetext-transact-sql.md) et [UPDATETEXT](../../t-sql/queries/updatetext-transact-sql.md) lors de l'insertion ou de l'ajout de nouvelles données dans les colonnes de type données **text**, **ntext**, et **image** . Notez que la journalisation minimale n'est pas utilisée quand des valeurs existantes sont mises à jour.  
  
    > [!WARNING]
    > L’emploi des instructions `WRITETEXT` et `UPDATETEXT` est **déprécié** : évitez de les utiliser dans les nouvelles applications.  
  
-   Si la base de données est en mode simple ou de récupération utilisant les journaux de transactions, certaines opérations DDL avec index sont journalisées au minimum qu'elles soient exécutées hors connexion ou en ligne. Les opérations journalisées minimales impliquant les index sont les suivantes :  
  
    -   Opérations[CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) (vues indexées comprises).  
  
    -   Opérations[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) REBUILD ou DBCC DBREINDEX.  
  
        > [!WARNING]
        > L’instruction `DBCC DBREINDEX` est **dépréciée** : ne l’utilisez pas dans de nouvelles applications.  
  
    -   Reconstruction d’un nouveau segment de mémoire [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md) (le cas échéant). La désallocation de pages d’index pendant une opération `DROP INDEX` est **toujours** entièrement journalisée.
  
##  <a name="RelatedTasks"></a> Tâches associées  
 **Gestion du journal des transactions**  
  
-   [Gérer la taille du fichier journal des transactions](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md)  
  
-   [Résoudre les problèmes liés à un journal des transactions saturé &#40;erreur SQL Server 9002&#41;](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)  
  
 **Sauvegarde du journal des transactions (mode de récupération complète)**  
  
-   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
 **Restauration du journal des transactions (mode de récupération complète)**  
  
-   [Restaurer une sauvegarde de journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
[Guide d’architecture et gestion du journal des transactions SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)   
[Contrôler la durabilité d’une transaction](../../relational-databases/logs/control-transaction-durability.md)   
[Conditions requises pour une journalisation minimale dans l’importation en bloc](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)   
[Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
[Points de contrôle de base de données &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md)   
[Afficher ou modifier les propriétés d’une base de données](../../relational-databases/databases/view-or-change-the-properties-of-a-database.md)   
[Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)  
[Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)    
[sys.dm_db_log_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)  
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)    
  
  
