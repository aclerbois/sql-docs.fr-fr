---
title: Nouveautés du moteur de base de données - SQL Server 2016 | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: high-availability
ms.component: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- what's new [SQL Server Database Engine]
- Database Engine [SQL Server], what's new
ms.assetid: 8f625d5a-763c-4440-97b8-4b823a6e2439
caps.latest.revision: 431
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: fc31efe5f3e78a80061d47149c56bf942cd7e8b7
ms.sourcegitcommit: 38f8824abb6760a9dc6953f10a6c91f97fa48432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="whats-new-in-database-engine---sql-server-2016"></a>Nouveautés du moteur de base de données - SQL Server 2016
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

Cette rubrique récapitule les améliorations introduites dans la version [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)].  Les nouvelles fonctionnalités et améliorations augmentent les capacités et la productivité des architectes, des développeurs et des administrateurs qui conçoivent, développent et maintiennent des systèmes de stockage de données.

Pour découvrir les nouveautés des autres composants SQL Server, consultez [Nouveautés de SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md).

> [!NOTE]
>  [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] est une application 64 bits. L’installation 32 bits n’est plus disponible, même si certains éléments s’exécutent en tant que composants 32 bits.

#### <a name="try-it-out"></a>À votre tour d’essayer

- Pour télécharger [!INCLUDE[ssSQL15](../includes/sssql15-md.md)], accédez au **[Centre d’évaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016)**![télécharger](../analysis-services/media/download.png "télécharger").

- Vous avez un compte Azure ?  Cliquez **[ici](https://azure.microsoft.com/en-us/services/virtual-machines/sql-server/)** pour lancer une machine virtuelle déjà équipée de [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].

> [!NOTE]
> Pour obtenir les notes de publication actuelles, consultez [Notes de publication de SQL Server 2016](../sql-server/sql-server-2016-release-notes.md).
  
## <a name="sql-server-2016-service-pack-1-sp1"></a>SQL Server 2016 Service Pack 1 (SP1)  
-  La syntaxe de `CREATE OR ALTER <object>` est désormais disponible pour les [procédures](../t-sql/statements/create-procedure-transact-sql.md), les [vues](../t-sql/statements/create-view-transact-sql.md), les [fonctions](../t-sql/statements/create-function-transact-sql.md) et les [déclencheurs](../t-sql/statements/create-trigger-transact-sql.md).
-   La prise en charge d’un modèle d’indicateurs de requête plus générique a été ajoutée : `OPTION (USE HINT('<hint1>', '<hint2>'))`. Pour plus d’informations, consultez [Indicateurs de requête (Transact-SQL)](../t-sql/queries/hints-transact-sql-query.md).  
- La DMV [sys.dm_exec_valid_use_hints](../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md) a été ajoutée aux indicateurs de liste.  
- La DMV [sys.dm_exec_query_statistics_xml](../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md) a été ajoutée pour retourner des statistiques temporaires XML de plan d’exécution de requêtes.  
- La DMV [sys.dm_db_incremental_stats_properties](../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md) a été ajoutée aux statistiques incrémentielles pour la table spécifiée.  
- La colonne `instant_file_initialization_enabled` a été ajoutée à [sys.dm_server_services](../relational-databases/system-dynamic-management-views/sys-dm-server-services-transact-sql.md).  
- La colonne `estimated_read_row_count` a été ajoutée à [sys.dm_exec_query_profiles](../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md).  
-  Les colonnes `sql_memory_model` et `sql_memory_model_desc` ont été ajoutées à [sys.dm_os_sys_info](../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) pour fournir des informations sur le modèle de verrouillage des pages de mémoire.
-  Les éditions qui prennent en charge un certain nombre de fonctionnalités ont été développées. Cela comprend l’ajout de la sécurité de niveau ligne, d’Always Encrypted, du masquage dynamique des données, de l’audit de la base de données, d’OLTP en mémoire et de plusieurs autres fonctionnalités à toutes les éditions. Pour plus d’informations, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../sql-server/editions-and-supported-features-for-sql-server-2016.md).   
-  [sp_refresh_parameter_encryption](../relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md) permet au chiffrement Toujours actif de mettre à jour les métadonnées lorsque des objets chiffrés à l’aide du chiffrement Toujours actif sont redéfinis.  


##  <a name="Feature"></a> SQL Server 2016 RTM
Cette section contient les sous-sections suivantes :

-   [Index columnstore](#columnstore)
-   [Configurations étendues à la base de données](#scopedconfiguration)
-   [OLTP en mémoire](#InMemory)
-   [Optimiseur de requête](#QueryOptimizer)
-   [Statistiques des requêtes dynamiques](#LiveStats)
-   [Magasin de requêtes](#QueryStore)
-   [Tables temporelles](#TT)
-   [Sauvegardes distribuées vers le Stockage Blob Microsoft Azure](#StripedBackupToAzure)
-   [Sauvegardes d’instantanés de fichiers vers le Stockage Blob Microsoft Azure](#FileSnapshotBackup)
-   [Gestion de sauvegarde](#ManagedBackup)
-   [Base de données tempdb](#multipleTempDB)
-   [Prise en charge de JSON intégrée](#ForJson)
-   [PolyBase](#bkPolyBase)
-   [Stretch Database](#stretch)
-   [Prise en charge du codage UTF-8](#UTF8)
-   [Nouvelles valeurs par défaut de taille de base de données et de croissance automatique](#DefaultDB)
-   [Améliorations de Transact-SQL](#TSQL)
-   [Améliorations des vues système](#SystemTable)
-   [Améliorations de la sécurité](#Security)
-   [Améliorations en matière de haute disponibilité](#HighAvailability)
-   [Améliorations apportées à la réplication](#Repl)
-   [Améliorations apportées aux outils](#Tools)

## <a name="columnstore-indexes"></a>Index columnstore

Cette version propose des améliorations des index columnstore, notamment des index columnstore non cluster actualisables et des index columnstore sur des tables en mémoire, ainsi que bien d’autres nouveautés destinées à l’analytique opérationnelle.

- Un index non cluster columnstore en lecture seule est actualisable après la mise à niveau.  Il n’est pas nécessaire de reconstruire l’index pour qu’il soit actualisable.

- Les performances des requêtes analytiques sur les index columnstore ont été améliorées, en particulier pour les agrégats et les prédicats de chaîne.

- Les vues de gestion dynamique (DMV) et les événements XEvent bénéficient d’une meilleure capacité de prise en charge.

Pour plus d’informations, consultez les rubriques suivantes dans la section [Guide des index columnstore](../relational-databases/indexes/columnstore-indexes-overview.md) de la documentation en ligne :

- [Synthèse des fonctionnalités des index columnstore en fonction des versions](~/relational-databases/indexes/columnstore-indexes-what-s-new.md) : inclut les nouveautés.

- [Chargement de données d’index columnstore](../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)

- [Performances des requêtes d’index columnstore](~/relational-databases/indexes/columnstore-indexes-query-performance.md)

- [Prise en main de columnstore pour l’analytique opérationnelle en temps réel](../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)

- [Index columnstore pour l’entreposage des données](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)

- [Défragmentation des index columnstore](~/relational-databases/indexes/columnstore-indexes-defragmentation.md)


## <a name="database-scoped-configurations"></a>Configurations étendues à la base de données


La nouvelle instruction [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) vous permet de contrôler certaines configurations de votre base de données particulière. Les paramètres de configuration affectent le comportement de l’application.

La nouvelle instruction est disponible à la fois dans [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] et [!INCLUDE[sqldbesa](../includes/sqldbesa-md.md)].



## <a name="in-memory-oltp"></a>OLTP en mémoire


### <a name="storage-format-change"></a>Changement de format de stockage

Le format de stockage des tables optimisées en mémoire a changé entre SQL Server 2014 et 2016. Pour les opérations de mise à niveau et d’attachement/restauration à partir de SQL Server 2014, le nouveau format de stockage est sérialisé et la base de données redémarre une fois pendant la récupération de la base de données.

- [Mettre à niveau vers SQL Server 2016](../database-engine/install-windows/upgrade-sql-server.md)


### <a name="alter-table-is-log-optimized-and-runs-in-parallel"></a>Exécution en parallèle et optimisation pour la journalisation d’ALTER TABLE

Maintenant que vous exécutez une instruction ALTER TABLE sur une table optimisée en mémoire, seules les modifications de métadonnées sont écrites dans le journal. Cela réduit considérablement les E/S du journal. De plus, la plupart des scénarios ALTER TABLE s’exécutent désormais en parallèle, ce qui peut réduire considérablement la durée de l’instruction.

- Pour les exceptions non parallèles, notamment les LOB, consultez [Modification des tables optimisées en mémoire](../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md).



### <a name="statistics"></a>Statistiques

Les [statistiques des tables optimisées en mémoire](../relational-databases/in-memory-oltp/statistics-for-memory-optimized-tables.md) sont à présent mises à jour automatiquement. En outre, la méthode de l’échantillonnage est désormais prise en charge pour collecter les statistiques, ce qui vous permet d’éviter la méthode de l’analyse complète qui est plus coûteuse.


### <a name="parallel-and-heap-scan-for-memory-optimized-tables"></a>Analyse parallèle et de segment de mémoire pour les tables optimisées en mémoire

Les tables optimisées en mémoire et les index des tables optimisées en mémoire prennent désormais en charge l’analyse parallèle. Les requêtes analytiques sont donc plus performantes.

Par ailleurs, l’analyse de segment de mémoire est prise en charge et peut être effectuée en parallèle. Dans le cas d’une table optimisée en mémoire, une analyse de segment de mémoire fait référence à l’analyse de toutes les lignes d’une table à l’aide de la structure de données de segment de mémoire utilisée pour le stockage des lignes. Pour une analyse de table complète, l’analyse de segment de mémoire est plus efficace que l’utilisation d’un index.

### <a name="transact-sql-improvements-for-memory-optimized-tables"></a>Améliorations de Transact-SQL pour les tables optimisées en mémoire

Il existe plusieurs éléments Transact-SQL qui n’étaient pas pris en charge pour les tables optimisées en mémoire dans SQL Server 2014 et qui le sont désormais dans SQL Server 2016 :


- Les contraintes et les index UNIQUE sont pris en charge.


- Les références FOREIGN KEY entre les tables optimisées en mémoire sont prises en charge.
  - Ces clés étrangères peuvent référencer uniquement une clé primaire et non pas une clé unique.


- Les contraintes CHECK sont prises en charge.

- Un index non unique accepte des valeurs NULL dans sa clé.

- Les TRIGGER sont pris en charge sur les tables optimisées en mémoire.
  - Seuls les déclencheurs AFTER sont pris en charge. Les déclencheurs INSTEADOF ne sont pas pris en charge.
  - Tout déclencheur sur une table optimisée en mémoire doit utiliser WITH NATIVE_COMPILATION.

- Prise en charge complète de l’ensemble des pages de codes et des classements SQL Server avec des index et d’autres artefacts dans les tables optimisées en mémoire et les modules T-SQL compilés en mode natif.


- Prise en charge de la [modification des tables optimisées en mémoire](../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md) :
  - Index ADD et DROP. Modification de la valeur bucket_count (nombre de compartiments) des index de hachage.
  - Modifications de schéma : ajouter/supprimer/modifier des colonnes ; ajouter/supprimer une contrainte.

- Une table optimisée en mémoire peut désormais avoir plusieurs colonnes dont la longueur combinée est supérieure à la longueur de page de 8 060 octets. Une table avec trois colonnes de type `nvarchar(4000)`en est un exemple. Dans ces exemples, certaines colonnes sont stockées hors ligne. Vos requêtes ignorent totalement si une colonne est sur une ligne ou hors ligne.

- [Les types LOB (Large Object)](../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md) `varbinary(max)`, `nvarchar(max)` et `varchar(max)` sont désormais pris en charge dans les tables optimisées en mémoire.


Pour obtenir des informations générales, consultez :

- [Les constructions Transact-SQL ne sont pas prises en charge par l’OLTP en mémoire](../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)
- [Fonctionnalités SQL Server non prises en charge pour l’OLTP en mémoire](~/relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)



### <a name="performance-and-scaling-improvements"></a>Améliorations des performances et des capacités

- La taille des données n’est plus limitée. Voir [Estimer les besoins en mémoire des tables mémoire optimisées](~/relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md).

- Plusieurs threads simultanés sont maintenant responsables de la [conservation sur disque des modifications apportées aux tables optimisées en mémoire](../relational-databases/in-memory-oltp/scalability.md).

- Prise en charge des plans parallèles pour l’[accès aux tables optimisées en mémoire à l’aide de Transact-SQL interprété](../relational-databases/in-memory-oltp/accessing-memory-optimized-tables-using-interpreted-transact-sql.md).


### <a name="enhancements-in-sql-server-management-studio"></a>Améliorations apportées à SQL Server Management Studio

- L’opération [Déterminer si un tableau ou une procédure stockée doit être déplacée vers l’OLTP en mémoire](../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md) n’a plus besoin de la configuration de collecteurs de données ou de l’entrepôt de données de gestion. Vous pouvez désormais exécuter le rapport directement sur une base de données de production.

- L’[applet de commande PowerShell pour l’évaluation de la migration](../relational-databases/in-memory-oltp/powershell-cmdlet-for-migration-evaluation.md) permet d’évaluer l’adéquation de la migration de plusieurs objets dans une base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

- Générez des listes de contrôle de migration en cliquant avec le bouton droit sur une base de données et en sélectionnant Tâches > Générer des listes de contrôle de migration OLTP en mémoire.

### <a name="cross-feature-support"></a>Prise en charge étendue à plusieurs fonctionnalités

- Prise en charge de l’utilisation du contrôle de version du système temporel avec OLTP en mémoire. Pour plus d’informations, consultez [Tables temporelles avec version gérée par le système avec des tables optimisées en mémoire](../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md).

- Prise en charge des magasins de requêtes pour le code compilé en mode natif à partir de charges de travail OLTP en mémoire. Pour plus d’informations, consultez [Utilisation du magasin de requêtes avec l’OLTP en mémoire](../relational-databases/performance/using-the-query-store-with-in-memory-oltp.md).

- [Sécurité au niveau des lignes dans les tables optimisées en mémoire](../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md#rls)

- [À l’aide de MARS &#40;Multiple Active Result Set&#41;](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md), les connexions peuvent désormais accéder aux tables optimisées en mémoire et aux procédures stockées compilées en mode natif.

- Prise en charge de [Transparent Data Encryption (TDE)](../relational-databases/security/encryption/transparent-data-encryption.md). Si une base de données est configurée pour ENCRYPTION, les fichiers inclus dans le [groupe de fichiers optimisés en mémoire](../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md) sont désormais aussi chiffrés.

Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).


## <a name="query-optimizer"></a>Optimiseur de requête
### <a name="compatibility-level-guarantees"></a>Garanties de niveau de compatibilité
Lorsque vous mettez à niveau votre base de données vers SQL Server 2016, vous n’observerez aucun changement de plan si vous continuez à utiliser votre ancien niveau de compatibilité (par exemple, 120 ou 110). Les nouvelles fonctionnalités et améliorations liées à l’optimiseur de requête seront disponibles uniquement au dernier niveau de compatibilité. 
### <a name="trace-flag-4199"></a>Indicateur de trace 4199
En général, il est inutile de recourir à l’indicateur de trace 4199 dans SQL Server 2016, puisque la plupart des comportements de l’optimiseur de requête contrôlés par cet indicateur de trace sont activés inconditionnellement au dernier niveau de compatibilité (130) dans SQL Server 2016.
### <a name="new-referential-integrity-operator"></a>Nouvel opérateur d’intégrité référentielle
Une table peut référencer au maximum 253 autres tables et colonnes en tant que clés étrangères (références sortantes). [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] fait passer de 253 à 10 000 le nombre limite des autres tables et colonnes pouvant référencer des colonnes dans une table unique (références entrantes). Pour connaître les restrictions associées, consultez [Create Foreign Key Relationships](../relational-databases/tables/create-foreign-key-relationships.md). Un nouvel opérateur d’intégrité référentielle, qui effectue les vérifications d’intégrité référentielle en place, est disponible (au niveau de compatibilité 130). Celui-ci améliore les performances globales pour les opérations UPDATE et DELETE sur les tables qui présentent un grand nombre de références entrantes, ce qui permet de disposer de nombreuses références entrantes. Pour plus d’informations, voir [Query Optimizer Additions in SQL Server 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/23/query-optimizer-additions-in-sql-server/)(Améliorations apportées à l’optimiseur de requête dans SQL Server 2016).
### <a name="parallel-update-of-sampled-statistics"></a>Mise à jour parallèle des statistiques échantillonnées
L’échantillonnage des données pour générer des statistiques est désormais effectué en parallèle (au niveau de compatibilité 130) afin d’améliorer les performances de collecte des statistiques. Pour plus d’informations, voir [Update Statistics](../t-sql/statements/update-statistics-transact-sql.md).
### <a name="sublinear-threshold-for-update-of-statistics"></a>Seuil sous-linéaire pour la mise à jour des statistiques
La mise à jour automatique des statistiques est désormais plus intensive pour les tables volumineuses (au niveau de compatibilité 130). Le seuil de déclenchement de la mise à jour automatique des statistiques est de 20 %. À compter de SQL Server 2016, pour les tables volumineuses, ce seuil (toujours un pourcentage) commencera à diminuer à mesure que le nombre de lignes augmente dans la table. Vous n’aurez plus besoin de définir l’indicateur de trace 2371 pour réduire le seuil. 
### <a name="other-enhancements"></a>Autres améliorations
La requête INSERT d’une instruction INSERT SELECT est multithread ou peut présenter un plan parallèle (au niveau de compatibilité 130). Pour disposer d’un plan parallèle, l’instruction INSERT … SELECT doit utiliser l’indicateur TABLOCK. Pour plus d’informations, voir [Parallel Insert Select](https://blogs.msdn.microsoft.com/sqlcat/2016/07/06/sqlsweet16-episode-3-parallel-insert-select/)(Instruction INSERT SELECT parallèle).

## <a name="live-query-statistics"></a>Statistiques sur les requêtes dynamiques
 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] offre la possibilité de visualiser le plan d’exécution dynamique d’une requête active. Ce plan de requête active fournit des informations en temps réel sur le processus d’exécution des requêtes à mesure que les commandes basculent d’un opérateur de plan de requête à un autre. Pour plus d’informations, voir [Live Query Statistics](../relational-databases/performance/live-query-statistics.md).

## <a name="query-store"></a>Magasin de requêtes
Le magasin de requêtes est une nouvelle fonctionnalité qui fournit aux administrateurs de base de données des informations sur le choix du plan de requête et sur les performances. Elle simplifie la résolution des problèmes de performances en vous permettant de trouver rapidement les différences de performances provoquées par un changement de plan de requête. La fonctionnalité capture automatiquement l'historique des requêtes, des plans et des statistiques d'exécution et les conserve à des fins de consultation. Elle sépare les données en périodes, ce qui vous permet de voir les modèles d'utilisation de base de données et de comprendre à quel moment le changement de plan de requête a eu lieu sur le serveur. Le magasin de requêtes, qui utilise une boîte de dialogue [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] pour présenter les informations, vous permet de forcer la requête vers l’un des plans de requête sélectionnés. Pour plus d’informations, voir [Monitoring Performance By Using the Query Store](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).


## <a name="temporal-tables"></a>Tables temporelles
[!INCLUDE[ssSQL15](../includes/sssql15-md.md)] prend désormais en charge les tables temporelles avec version gérée par le système. Une table temporelle est un nouveau type de table qui fournit des informations correctes sur des faits stockés à tout moment. Chaque table temporelle se compose en fait de deux tables : l’une pour les données actuelles et l’autre pour les données d’historique. Quand les données dans la table sont remplacées par les données actuelles, le système garantit le stockage des valeurs précédentes dans la table d’historique. Des constructions d’interrogation sont fournies pour ne pas exposer cette complexité aux utilisateurs. Pour plus d’informations, voir [Temporal Tables](../relational-databases/tables/temporal-tables.md).

## <a name="backups"></a>Sauvegardes

### <a name="striped-backups-to-microsoft-azure-blob-storage"></a>Sauvegardes distribuées vers le Stockage Blob Microsoft Azure
Dans [!INCLUDE[ssSQL15](../includes/sssql15-md.md)], la sauvegarde SQL Server vers une URL à l’aide du service de stockage d’objets blob Microsoft Azure prend maintenant en charge les jeux de sauvegarde distribuée à l’aide d’objets blob de blocs pour prendre en charge une taille maximale de sauvegarde de 12,8 To. Pour obtenir des exemples, consultez [Code Examples](../relational-databases/backup-restore/sql-server-backup-to-url.md#Examples).

### <a name="file-snapshot-backups-to-microsoft-azure-blob-storage"></a>Sauvegardes d’instantanés de fichiers vers le Stockage Blob Microsoft Azure
 Dans [!INCLUDE[ssSQL15](../includes/sssql15-md.md)], la sauvegarde SQL Server vers une URL prend désormais en charge l’utilisation d’instantanés Azure pour sauvegarder des bases de données dans lesquelles tous les fichiers de base de données sont stockés à l’aide du service de stockage d’objets blob Microsoft Azure. Pour plus d’informations, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).

### <a name="managed-backup"></a>Gestion de sauvegarde
Dans [!INCLUDE[ssSQL15](../includes/sssql15-md.md)], la gestion de sauvegarde SQL Server vers Microsoft Azure utilise le nouveau stockage d’objets blob de blocs pour les fichiers de sauvegarde. La gestion de sauvegarde a également bénéficié de nombreuses modifications et améliorations.

-   Prise en charge des planifications de sauvegardes automatisées et personnalisées.

-   Prise en charge des sauvegardes de bases de données système.

-   Prise en charge des bases de données utilisant le mode de récupération simple.

 Pour plus d'informations, consultez [Gestion de sauvegarde SQL Server sur Microsoft Azure](../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)

> [!NOTE]
>  Pour [!INCLUDE[ssSQL15](../includes/sssql15-md.md)], ces nouvelles fonctionnalités de gestion de sauvegarde ne disposent pas encore d’une interface utilisateur correspondante dans [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)].

## <a name="tempdb-database"></a>Base de données tempdb
 Plusieurs améliorations ont été apportées à tempdb :

-   Les indicateurs de trace 1117 et 1118 ne sont plus nécessaires pour tempdb. Si vous avez plusieurs fichiers de base de données tempdb, ils continuent tous de croître en même temps en fonction des paramètres de croissance. En outre, toutes les allocations dans tempdb utilisent des extensions uniformes.

-   Par défaut, le programme d’installation ajoute autant 8 fichiers tempdb ou autant que le nombre de processeurs, la valeur la plus petite étant retenue.

-   Pendant l’installation, vous pouvez configurer le nombre de fichiers de base de données tempdb, la taille initiale, la croissance automatique et le positionnement des répertoires à l’aide du nouveau contrôle d’entrée de l’interface utilisateur figurant dans la section Configuration du moteur de base de données - TempDB de l’Assistant Installation de SQL Server.

-   La taille initiale par défaut est de 8 Mo et la croissance automatique par défaut est de 64 Mo.

-   Vous pouvez spécifier plusieurs volumes pour les fichiers de base de données tempdb. Si plusieurs répertoires sont spécifiés, les fichiers de données tempdb sont répartis entre les répertoires selon le principe du tourniquet (round robin).

## <a name="built-in-json-support"></a>Prise en charge de JSON intégrée
SQL Server 2016 offre une prise en charge intégrée de l’importation et l’exportation de JSON ainsi que de l’utilisation des chaînes JSON. Cette prise en charge intégrée inclut les instructions et fonctions suivantes.

-   Mettez les résultats de la requête au format JSON, ou exportez JSON, en ajoutant la clause **FOR JSON** à une instruction **SELECT** . Par exemple, utilisez la clause **FOR JSON** pour déléguer le formatage de la sortie JSON produite par vos applications clientes à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Mettre les résultats de requête au format JSON avec FOR JSON &#40;SQL Server&#41;](../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md).

-   Convertissez des données JSON en lignes et colonnes, ou importez JSON, en appelant la fonction du fournisseur d’ensembles de lignes **OPENJSON**. Utilisez **OPENJSON** pour importer des données JSON dans SQL Server ou convertir des données JSON en lignes et colonnes dans le cas d’une application ou d’un service qui ne peut pas pour l’instant consommer directement les données JSON. Pour plus d’informations, consultez [Convertir des données JSON en lignes et colonnes avec OPENJSON &#40;SQL Server&#41;](../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md).

-   La fonction **ISJSON** teste si une chaîne contient un JSON valide. Pour plus d’informations, consultez [ISJSON &#40;Transact-SQL&#41;](../t-sql/functions/isjson-transact-sql.md).

-   La fonction **JSON_VALUE** extrait une valeur scalaire à partir d’une chaîne JSON. Pour plus d’informations, consultez [JSON_VALUE &#40;Transact-SQL&#41;](../t-sql/functions/json-value-transact-sql.md).

-   La fonction **JSON_QUERY** extrait un objet ou un tableau à partir d’une chaîne JSON. Pour plus d’informations, consultez [JSON_QUERY &#40;Transact-SQL&#41;](../t-sql/functions/json-query-transact-sql.md).

-   La fonction **JSON_MODIFY** met à jour la valeur d’une propriété dans une chaîne JSON et retourne la chaîne JSON mise à jour. Pour plus d’informations, consultez [JSON_MODIFY &#40;Transact-SQL&#41;](../t-sql/functions/json-modify-transact-sql.md).

## <a name="polybase"></a>PolyBase
 PolyBase vous permet d’utiliser des instructions T-SQL pour accéder à des données stockées dans Hadoop ou Azure Blob Storage et les interroger de manière ad hoc. Il vous permet également d’interroger des données semi-structurées et de joindre les résultats à des jeux de données relationnelles stockés dans SQL Server. Optimisé pour les charges de travail d’entreposage de données, PolyBase est destiné à des scénarios d’analyse de requête.

 Pour plus d’informations, consultez [Guide de PolyBase](../relational-databases/polybase/polybase-guide.md).

## <a name="stretch-database"></a>Stretch Database
 Stretch Database est une nouvelle fonctionnalité dans [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] qui fait migrer vos données d’historique en toute sécurité et de façon transparente vers le cloud Microsoft Azure. Vous pouvez accéder de façon transparente à vos données SQL Server, que celles-ci soient locales ou étendues vers le cloud. Définissez la stratégie qui détermine où les données sont stockées et SQL Server gère le déplacement des données en arrière-plan. La table entière est toujours en ligne et peut toujours être interrogée. De plus, Stretch Database ne nécessite aucune modification des requêtes ou applications existantes : l’emplacement des données est totalement transparent pour l’application. Pour plus d'informations, consultez [Stretch Database](../sql-server/stretch-database/stretch-database.md).
 
## <a name="support-for-utf-8"></a>Prise en charge du codage UTF-8
[bcp Utility](../tools/bcp-utility.md), [BULK INSERT](../t-sql/statements/bulk-insert-transact-sql.md) et [OPENROWSET](../t-sql/functions/openrowset-transact-sql.md) prennent désormais en charge la page de codes UTF-8. Pour plus d’informations, consultez ces rubriques et [Créer un fichier de format &#40;SQL Server&#41;](../relational-databases/import-export/create-a-format-file-sql-server.md).

## <a name="new-default-database-size-and-autogrow-values"></a>Nouvelles valeurs par défaut de taille et de croissance automatique de base de données
Nouvelles valeurs pour la base de données model et valeurs par défaut pour les nouvelles bases de données (basées sur model). La taille initiale des fichiers journaux et de données est désormais de 8 Mo. La croissance automatique par défaut des fichiers journaux et de données est maintenant de 64 Mo.


## <a name="transact-sql-enhancements"></a>Améliorations de Transact-SQL
De nombreuses améliorations ont été apportées pour prendre en charge les fonctionnalités décrites dans les autres sections de cette rubrique. Les améliorations supplémentaires suivantes sont disponibles.
- L’instruction TRUNCATE TABLE autorise désormais la troncation de partitions spécifiées. Pour plus d’informations, consultez [TRUNCATE TABLE &#40;Transact-SQL&#41;](../t-sql/statements/truncate-table-transact-sql.md).
- [ALTER TABLE &#40;Transact-SQL&#41;](../t-sql/statements/alter-table-transact-sql.md) permet désormais d’effectuer de nombreuses actions de modification de colonne pendant que la table reste disponible.
- La vue de gestion dynamique (DMV) de l’index de recherche en texte intégral [sys.dm_fts_index_keywords_position_by_document &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-position-by-document-transact-sql.md) retourne l’emplacement des mots clés dans des documents. Cette DMV a également été ajoutée dans [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] SP2 et [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] SP1.
- Un nouvel indicateur de requête **NO_PERFORMANCE_SPOOL** peut empêcher l’ajout d’un opérateur spool à des plans de requête. Les performances peuvent s’en trouver améliorées quand plusieurs requêtes simultanées sont exécutées avec des opérations spool. Pour plus d’informations, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../t-sql/queries/hints-transact-sql-query.md).
- L’instruction [FORMATMESSAGE &#40;Transact-SQL&#41;](../t-sql/functions/formatmessage-transact-sql.md) a été améliorée pour accepter un argument msg_string.
- La taille maximale de la clé d’index pour les index NON CLUSTER a été augmentée à 1 700 octets.
- Une nouvelle syntaxe DROP IF a été ajoutée pour les instructions DROP liées à AGGREGATE, ASSEMBLY, COLUMN, CONSTRAINT, DATABASE, DEFAULT, FUNCTION, INDEX, PROCEDURE, ROLE, RULE, SCHEMA, SECURITY POLICY, SEQUENCE, SYNONYM, TABLE, TRIGGER, TYPE, USER et VIEW. Pour plus de détails, consultez les rubriques correspondant à chaque syntaxe.
- L’option MAXDOP a été ajoutée à [DBCC CHECKTABLE &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-checktable-transact-sql.md), [DBCC CHECKDB &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) et [DBCC CHECKFILEGROUP &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-checkfilegroup-transact-sql.md) pour spécifier le degré de parallélisme.
- SESSION_CONTEXT peut désormais être défini. Inclut la fonction [SESSION_CONTEXT &#40;Transact-SQL&#41;](../t-sql/functions/session-context-transact-sql.md), la fonction [CURRENT_TRANSACTION_ID &#40;Transact-SQL&#41;](../t-sql/functions/current-transaction-id-transact-sql.md) et la procédure [sp_set_session_context &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md).
- Les extensions analytiques avancées permettent aux utilisateurs d’exécuter des scripts écrits dans un langage pris en charge comme R. [!INCLUDE[tsql](../includes/tsql-md.md)] prend en charge R en introduisant la procédure stockée [sp_execute_external_script &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) et l’[option de configuration de serveur external scripts enabled](../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md). Pour plus d’informations, consultez [SQL Server R Services](../advanced-analytics/r-services/sql-server-r-services.md).
- Également pour prendre en charge R : possibilité de créer un pool de ressources externes. Pour plus d’informations, consultez [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/create-external-resource-pool-transact-sql.md).  Nouveaux affichages catalogue et nouvelles vues de gestion dynamique ([sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md) et [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)). Des arguments supplémentaires sont disponibles pour [sp_execute_external_script &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) et [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/create-workload-group-transact-sql.md). Des colonnes supplémentaires sont ajoutées à certains affichages catalogue de Resource Governor et DMV existants.
- La syntaxe [CREATE USER](../t-sql/statements/create-user-transact-sql.md) a été améliorée avec l’option ALLOW_ENCRYPTED_VALUE_MODIFICATIONS pour prendre en charge la fonctionnalité Always Encrypted. Pour plus d’informations, consultez [Migrer des données sensibles protégées par Always Encrypted](../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).
- Les fonctions [COMPRESS &#40;Transact-SQL&#41;](../t-sql/functions/compress-transact-sql.md) et [DECOMPRESS &#40;Transact-SQL&#41;](../t-sql/functions/decompress-transact-sql.md) convertissent des valeurs vers et depuis l’algorithme GZIP.
- Les fonctions [DATEDIFF_BIG &#40;Transact-SQL&#41;](../t-sql/functions/datediff-big-transact-sql.md) et [AT TIME ZONE &#40;Transact-SQL&#41;](../t-sql/queries/at-time-zone-transact-sql.md) et la vue [sys.time_zone_info &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-time-zone-info-transact-sql.md) sont ajoutées pour prendre en charge les interactions de date et d’heure.
- Il est désormais possible de créer des informations d’identification au niveau de la base de données (en plus des informations d’identification au niveau du serveur précédemment disponibles). Pour plus d’informations, consultez [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../t-sql/statements/create-database-scoped-credential-transact-sql.md).
- Huit nouvelles propriétés sont ajoutées à [SERVERPROPERTY &#40;Transact-SQL&#41;](../t-sql/functions/serverproperty-transact-sql.md) : InstanceDefaultDataPath, InstanceDefaultLogPath, ProductBuild, ProductBuildType, ProductMajorVersion, ProductMinorVersion, ProductUpdateLevel et ProductUpdateReference.
- La longueur d’entrée maximale de 8 000 octets pour la fonction [HASHBYTES &#40;Transact-SQL&#41;](../t-sql/functions/hashbytes-transact-sql.md) est supprimée.
- Les nouvelles fonctions de chaîne [STRING_SPLIT &#40;Transact-SQL&#41;](../t-sql/functions/string-split-transact-sql.md) et [STRING_ESCAPE &#40;Transact-SQL&#41;](../t-sql/functions/string-escape-transact-sql.md) sont ajoutées.
- Options de croissance automatique : l’indicateur de trace 1117 est remplacé par les options AUTOGROW_SINGLE_FILE et AUTOGROW_ALL_FILES d’ALTER DATABASE. L’indicateur de trace 1117 n’a aucun effet. Pour plus d’informations, consultez [Options de fichiers et de groupes de fichiers ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md) et la nouvelle colonne is_autogrow_all_files de [sys.filegroups &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md).
- Allocation d’extensions mixtes : lors de l’allocation par défaut des 8 premières pages d’un objet dans le cadre de bases de données utilisateur, les extensions de pages mixtes sont remplacées par des extensions uniformes. L’indicateur de suivi 1118 est remplacé par l’option SET MIXED_PAGE_ALLOCATION d’ALTER DATABASE. L’indicateur de suivi 1118 n’a aucun effet. Pour plus d’informations, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-set-options.md) et la nouvelle colonne `is_mixed_page_allocation_on` de [sys.databases &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

### <a name="transact-sql-improvements-for-natively-compiled-modules"></a>Améliorations de Transact-SQL pour les modules compilés en mode natif

Il existe des éléments Transact-SQL qui n’étaient pas pris en charge pour les modules compilés en mode natif dans SQL Server 2014 et qui le sont désormais dans SQL Server 2016 :


- Constructions de requête :
  - UNION et UNION ALL
  - SELECT DISTINCT
  - OUTER JOIN
  - Sous-requêtes dans SELECT


- Les instructions INSERT, UPDATE et DELETE peuvent désormais inclure la [clause OUTPUT](../t-sql/queries/output-clause-transact-sql.md).

- Les LOB peuvent maintenant être utilisés dans une procédure native comme suit :
  - Déclaration de variables.
  - Paramètres d’entrée reçus.
  - Paramètres passés dans des fonctions de chaîne, comme dans LTrim ou Substring, dans une procédure native.


- Les fonctions table inline (instruction unique) (TVF) peuvent désormais être compilées en mode natif.

- Les fonctions scalaires définies par l’utilisateur peuvent désormais être compilées en mode natif.

- Prise en charge accrue d’une procédure native pour appeler des :
  - [Fonctions de sécurité](../t-sql/functions/security-functions-transact-sql.md) intégrées.
  - [Fonctions mathématiques](../t-sql/functions/mathematical-functions-transact-sql.md) intégrées.
  - Fonction intégrée `@@SPID`.


- L’instruction EXECUTE AS CALLER est désormais prise en charge, ce qui signifie que la clause EXECUTE AS n’est plus nécessaire lors de la création d’un module T-SQL compilé en mode natif.


Pour obtenir des informations générales, consultez :

- [Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)
- [Modification de modules T-SQL compilés en mode natif](../relational-databases/in-memory-oltp/altering-natively-compiled-t-sql-modules.md)

## <a name="system-view-enhancements"></a>Améliorations des vues système
- Deux nouvelles vues prennent en charge la sécurité au niveau des lignes. Pour plus d’informations, consultez [sys.security_predicates &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md) et [sys.security_policies &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md).
- Sept nouvelles vues prennent en charge la fonctionnalité de magasin de requêtes. Pour plus d’informations, consultez [Affichages catalogue de magasin de requête &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md).
- 24 nouvelles colonnes sont ajoutées à [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) pour fournir des informations sur les allocations de mémoire.
- Deux nouveaux indicateurs de requête (MIN_GRANT_PERCENT et MAX_GRANT_PERCENT) sont ajoutés pour spécifier des allocations de mémoire. Consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../t-sql/queries/hints-transact-sql-query.md).
- [sys.dm_exec_session_wait_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-session-wait-stats-transact-sql.md) fournit un rapport par session semblable au rapport [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) à l’échelle du serveur.
- [sys.dm_exec_function_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-function-stats-transact-sql.md) fournit des statistiques d’exécution concernant les fonctions à valeurs scalaires.
- À partir de [!INCLUDE[ssSQL15](../includes/sssql15-md.md)], les entrées incluses dans [sys.dm_db_index_usage_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md) sont conservées comme elles l’étaient avant [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)].
- Les informations sur les instructions envoyées à une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peuvent être retournées par la nouvelle fonction de gestion dynamique [sys.dm_exec_input_buffer &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-input-buffer-transact-sql.md).
- Deux nouvelles vues prennent en charge [SQL Server R Services ](../advanced-analytics/r-services/sql-server-r-services.md): [sys.dm_external_script_requests](../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md) et [sys.dm_external_script_execution_stats](../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md). 


## <a name="security-enhancements"></a>Améliorations de la sécurité

### <a name="row-level-security"></a>Sécurité au niveau des lignes
La sécurité au niveau des lignes introduit le contrôle d’accès basé sur le prédicat. Elle propose une évaluation flexible, centralisée et basée sur le prédicat, qui peut prendre en compte les métadonnées (notamment les étiquettes) ou tout autre critère que l’administrateur juge approprié. Le prédicat est utilisé comme critère pour déterminer si l'utilisateur a l'accès approprié aux données en fonction de ses propres attributs. Le contrôle d’accès basé sur l’étiquette peut être implémenté à l’aide du contrôle d’accès basé sur le prédicat. Pour plus d’informations, consultez [Sécurité au niveau des lignes](../relational-databases/security/row-level-security.md).


### <a name="always-encrypted"></a>Always Encrypted
Avec Always Encrypted, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut effectuer des opérations sur des données chiffrées. Qui plus est, la clé de chiffrement réside avec l’application à l’intérieur de l’environnement approuvé du client, et non sur le serveur. Always Encrypted sécurise les données client pour que les administrateurs de base de données n’aient pas accès aux données en texte brut. Le chiffrement et le déchiffrement des données se produisent en toute transparence au niveau du pilote, minimisant ainsi les modifications qui doivent être apportées aux applications existantes. Pour plus d’informations, consultez [Always Encrypted &#40;moteur de base de données&#41;](../relational-databases/security/encryption/always-encrypted-database-engine.md).


### <a name="dynamic-data-masking"></a>Masquage dynamique des données
Le masquage dynamique des données limite l’exposition des données sensibles en les masquant aux utilisateurs sans privilège. Le masquage dynamique des données permet d’empêcher les accès non autorisés à des données sensibles. Pour cela, les clients peuvent indiquer la quantité de données sensibles à exposer avec un impact minimal sur la couche Application. Il s’agit d’une fonctionnalité de sécurité basée sur des stratégies qui masque les données sensibles dans le jeu de résultats d’une requête sur des champs de base de données désignés (les données dans la base de données ne sont pas modifiées). Pour plus d’informations, consultez [Dynamic Data Masking](../relational-databases/security/dynamic-data-masking.md).


### <a name="new-permissions"></a>Nouvelles autorisations
- L’autorisation **ALTER ANY SECURITY POLICY** est disponible dans le cadre de l’implémentation de la sécurité au niveau des lignes.
- Les autorisations **ALTER ANY MASK** et **UNMASK** sont disponibles dans le cadre de l’implémentation de masquage des données dynamiques.
- Les autorisations **ALTER ANY COLUMN ENCRYPTION KEY**, **VIEW ANY COLUMN ENCRYPTION KEY**, **ALTER ANY COLUMN MASTER KEY DEFINITION**et **VIEW ANY COLUMN MASTER KEY DEFINITION** sont disponibles dans le cadre de l’implémentation de la fonctionnalité Always Encrypted.
- Les autorisations **ALTER ANY EXTERNAL DATA SOURCE** et **ALTER ANY EXTERNAL FILE FORMAT** sont visibles dans [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] mais elles s’appliquent uniquement à [!INCLUDE[ssAPS](../includes/ssaps-md.md)] ([!INCLUDE[ssDW](../includes/ssdw-md.md)]).
- Les autorisations **EXECUTE ANY EXTERNAL SCRIPT** sont disponibles dans le cadre de la prise en charge des scripts R.
 - Les autorisations **ALTER ANY DATABASE SCOPED CONFIGURATION** sont disponibles pour autoriser l’utilisation de l’instruction [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

### <a name="transparent-data-encryption"></a>chiffrement transparent des données
- Transparent Data Encryption a été amélioré et prend désormais en charge l’accélération matérielle Intel AES-NI pour le chiffrement. Cela permet de réduire la surcharge du processeur liée à l’activation de Transparent Data Encryption.

### <a name="aes-encryption-for-endpoints"></a>Chiffrement AES pour les points de terminaison
- Le chiffrement par défaut pour les points de terminaison est passé de RC4 à AES.

### <a name="new-credential-type"></a>Nouveau type d’informations d’identification
- Il est désormais possible de créer des informations d’identification au niveau de la base de données (en plus des informations d’identification au niveau du serveur précédemment disponibles). Pour plus d’informations, consultez [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../t-sql/statements/create-database-scoped-credential-transact-sql.md).


## <a name="high-availability-enhancements"></a>Améliorations en matière de haute disponibilité
SQL Server 2016 Standard Edition prend désormais en charge les groupes de disponibilité de base Always On. Les groupes de disponibilité de base prennent en charge un réplica principal et un réplica secondaire. Cette fonctionnalité remplace la technologie de mise en miroir de base de données obsolète pour une haute disponibilité. Pour plus d’informations sur les différences entre les groupes de disponibilité de base et avancés, consultez [Groupes de disponibilité de base &#40;groupes de disponibilité Always On&#41;](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).

L’équilibrage de charge des demandes de connexion de tentative de lecture est désormais pris en charge sur un jeu de réplicas en lecture seule. Auparavant, les connexions étaient toujours dirigées vers le premier réplica en lecture seule disponible dans la liste de routage. Pour plus d’informations, consultez [Configurer l’équilibrage de charge entre des réplicas en lecture seule](../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).

 Le nombre de réplicas prenant en charge le basculement automatique est passé de deux à trois.

 Les comptes de service administrés de groupe sont désormais pris en charge pour les clusters de basculement Always On. Pour plus d’informations, consultez [Comptes de service administrés de groupe](https://technet.microsoft.com/library/hh831782.aspx). Pour Windows Server 2012 R2, une mise à jour est nécessaire pour éviter les temps morts temporaires après un changement de mot de passe. Pour obtenir la mise à jour, consultez [Les services basés sur gMSA ne peuvent pas se connecter après un changement de mot de passe dans un domaine Windows Server 2012 R2](https://support.microsoft.com/kb/2998082/).

 [!INCLUDE[ssHADR](../includes/sshadr-md.md)] prend en charge les transactions distribuées et le DTC sur Windows Server 2016. Pour plus d’informations, consultez [Prise en charge des transactions distribuées](../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md#dtcsupport).

 Vous pouvez désormais configurer le basculement de [!INCLUDE[ssHADR](../includes/sshadr-md.md)] quand une base de données est mise hors connexion. Cette modification nécessite le paramétrage de l’option **DB_FAILOVER** sur **ON** dans les instructions [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../t-sql/statements/create-availability-group-transact-sql.md) ou [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../t-sql/statements/alter-availability-group-transact-sql.md).

Always On prend désormais en charge les bases de données chiffrées. Les Assistants Groupe de disponibilité vous invitent à présent à entrer un mot de passe pour toute base de données contenant une clé principale de base de données : soit quand vous créez un groupe de disponibilité, soit quand vous ajoutez des bases de données ou des réplicas à un groupe de disponibilité existant.

Il est désormais possible de combiner deux groupes de disponibilité figurant dans deux clusters WSFC (Windows Server Failover Cluster) distincts dans un groupe de disponibilité distribué. Pour plus d’informations, consultez [Groupes de disponibilité distribués &#40;groupes de disponibilité Always On&#41;](../database-engine/availability-groups/windows/distributed-availability-groups-always-on-availability-groups.md).

L’amorçage direct permet de répliquer automatiquement un réplica secondaire sur le réseau (contrairement à un amorçage manuel qui nécessite une sauvegarde physique de la base de données cible pour une restauration sur le serveur secondaire). Pour spécifier l’amorçage direct, définissez **SEEDING_MODE=AUTOMATIC** dans les instructions [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../t-sql/statements/create-availability-group-transact-sql.md) ou [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../t-sql/statements/alter-availability-group-transact-sql.md). Vous devez également spécifier **GRANT CREATE ANY DATABASE** avec [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../t-sql/statements/alter-availability-group-transact-sql.md) sur chaque réplica secondaire utilisé avec l’amorçage direct.

**Améliorations des performances** : le débit de la synchronisation des groupes de disponibilité a été augmenté de ~10x grâce à la compression parallèle et plus rapide des blocs de journal sur le réplica principal, à un protocole de synchronisation optimisé et à la décompression parallèle et la restauration par progression des enregistrements de journal sur le réplica secondaire. L’actualisation des réplicas secondaires accessibles en lecture est ainsi accrue et le temps de récupération de la base de données en cas de basculement réduit. Notez que la restauration par progression des tables optimisées en mémoire n’est pas encore parallèle dans SQL Server 2016.

## <a name="replication-enhancements"></a>Améliorations apportées à la réplication
- La réplication des tables optimisées en mémoire est désormais prise en charge. Pour plus d’informations, consultez [Abonnés à la réplication de tables optimisées en mémoire](../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md).
- La réplication est à présent prise en charge dans [!INCLUDE[ssSDSFull](../includes/sssdsfull-md.md)]. Pour plus d’informations, consultez [Replication to SQL Database](../relational-databases/replication/replication-to-sql-database.md).

## <a name="tools-enhancements"></a>Améliorations apportées aux outils

### <a name="management-studio"></a>Management Studio
Télécharger la dernière version de [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)

- [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] prend en charge ADAL (Active Directory Authentication Library), actuellement en développement, pour établir une connexion à Microsoft Azure. Cette technologie remplace l’authentification basée sur les certificats utilisée dans [!INCLUDE[ssSQL14](../includes/sssql14-md.md)][!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)].
- L’installation de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] exige l’installation préalable de .NET 4.6. Quand [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] est installé, le programme d’installation installe automatiquement le .NET 4.6.
- Une nouvelle option de grille de résultats de requête conserve les caractères de retour chariot/saut de ligne lors de la copie ou de l’enregistrement de texte à partir de la grille de résultats. Vous pouvez définir cette option dans le menu Outils/Options.
- Les outils d’administration SQL Server ne sont plus installés à partir de l’arborescence de fonctionnalités principales ; pour plus d’informations, consultez [Installer les outils d’administration SQL Server avec SSMS](http://msdn.microsoft.com/library/af68d59a-a04d-4f23-9967-ad4ee2e63381).
- L’installation de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] exige l’installation préalable du .NET 4.6.1. Quand [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] est installé, le programme d’installation installe automatiquement le .NET 4.6.1.

### <a name="upgrade-advisor"></a>Conseiller de mise à niveau
SQL Server 2016 Upgrade Advisor Preview est un outil autonome qui permet aux utilisateurs de versions antérieures d’exécuter un ensemble de règles de mise à niveau sur leur base de données SQL Server. Les utilisateurs peuvent ainsi identifier les modifications avec rupture, les changements de comportement et les fonctionnalités déconseillées, mais aussi recevoir de l’aide sur l’adoption des nouvelles fonctionnalités telles que Stretch Database.

 Vous pouvez télécharger Upgrade Advisor Preview [ici](https://www.microsoft.com/en-us/download/details.aspx?id=48119) ou l’installer à l’aide de Web Platform Installer.

## <a name="see-also"></a> Voir aussi
[Nouveautés de SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md)
 
[Notes de publication de SQL Server 2016](../sql-server/sql-server-2016-release-notes.md) 
 
[Installer les Outils d’administration SQL Server avec SSMS](http://msdn.microsoft.com/library/af68d59a-a04d-4f23-9967-ad4ee2e63381)