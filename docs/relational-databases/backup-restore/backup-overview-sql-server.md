---
title: Vue d’ensemble de la sauvegarde (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 07/15/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- tables [SQL Server], backing up data
- backups [SQL Server]
- database backups [SQL Server]
- backup types [SQL Server]
- data backups [SQL Server]
- backing up tables [SQL Server]
- database restores [SQL Server], backups
- backing up [SQL Server], about backing up
- restoring [SQL Server], backup types
- backups [SQL Server], about
- backups [SQL Server], table-level backups unsupported
ms.assetid: 09a6e0c2-d8fd-453f-9aac-4ff24a97dc1f
caps.latest.revision: 84
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: aeba4beeb3e52ba0dffb8df6ed8c860f9c6c8393
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="backup-overview-sql-server"></a>Backup Overview (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique présente le composant de sauvegarde de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La sauvegarde de votre base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est essentielle pour protéger vos données. Cette discussion couvre les types de sauvegardes et les restrictions liées aux sauvegardes. La rubrique présente également les unités et les supports de sauvegarde de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
  
## <a name="terms"></a>Termes
 
 **sauvegarder [verbe]**  
 Copie les données ou les enregistrements de journal d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de son journal des transactions sur une unité de sauvegarde, telle qu'un disque, pour créer une sauvegarde de données ou de journal.  
  
**sauvegarde [nom]**  
 Copie de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui peut être utilisée pour restaurer et récupérer les données après une défaillance. Une sauvegarde des données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est créée au niveau d'une base de données, ou d'un ou de plusieurs de ses fichiers ou groupes de fichiers. Vous ne pouvez pas créer de sauvegardes au niveau des tables. Outre les sauvegardes de données, le mode de récupération complète nécessite la création de sauvegardes du journal des transactions.  
  
**[mode de récupération](../../relational-databases/backup-restore/recovery-models-sql-server.md)**  
 Propriété de base de données qui contrôle la maintenance du journal des transactions sur une base de données. Il existe trois modes de récupération : simple, complète et utilisant les journaux de transactions. Le mode de récupération de base de données détermine les spécifications de sauvegarde et de restauration.  
  
 **[restauration](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)**  
 Processus à plusieurs phases qui copie toutes les données et les pages des journaux à partir d'une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifiée dans une base de données spécifiée, puis restaure toutes les transactions journalisées dans la sauvegarde en appliquant les modifications journalisées pour rétablir un état ultérieur des données.  
  
 ## <a name="types-of-backups"></a>Types de sauvegardes  
  
 **[Sauvegarde de copie uniquement](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)**  
 Sauvegarde d'utilisation particulière qui est indépendante de la séquence normale des sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**sauvegarde de données**   
 Sauvegarde de données dans une base de données complète (sauvegarde de base de données), une base de données partielle (sauvegarde partielle) ou un ensemble de fichiers ou groupes de fichiers (sauvegarde de fichiers).  
  
**[sauvegarde de base de données](../../relational-databases/backup-restore/full-database-backups-sql-server.md)**  
 Sauvegarde d'une base de données. Les sauvegardes complètes de base de données représentent l'intégralité de la base de données à l'issue de l'opération de sauvegarde. Les sauvegardes différentielles contiennent uniquement les modifications apportées à la base de données depuis sa plus récente sauvegarde complète de base de données.  
  
 **[sauvegarde différentielle](../../relational-databases/backup-restore/full-database-backups-sql-server.md)**  
 Sauvegarde de données basée sur la dernière sauvegarde complète d’une base de données complète ou partielle ou d’un ensemble de fichiers de données ou de groupes de fichiers ( *base différentielle*) et qui contient uniquement les extensions de données ayant changé depuis la base différentielle.  
  
 Une sauvegarde différentielle partielle n'enregistre que les extensions de données qui ont changé dans les groupes de fichiers depuis la sauvegarde partielle précédente, appelée la base de la sauvegarde différentielle.  
  
**sauvegarde complète**  
 Sauvegarde de données qui contient toutes les données d'une base de données particulière ou d'un jeu de groupes de fichiers ou de fichiers, ainsi qu'une partie suffisante du journal pour permettre la récupération de ces données.  
  
**[sauvegarde de fichier journal](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)**  
 Sauvegarde des journaux des transactions qui inclut tous les enregistrements des journaux qui n'ont pas été sauvegardés lors d'une sauvegarde de fichier journal précédente. (mode de récupération complète)  
  
 **[sauvegarde de fichiers](../../relational-databases/backup-restore/full-file-backups-sql-server.md)**  
 Sauvegarde d'un ou de plusieurs fichiers ou groupes de fichiers de base de données.  
  
 **[sauvegarde partielle](../../relational-databases/backup-restore/partial-backups-sql-server.md)**  
 Contient des données provenant uniquement de certains des groupes de fichiers dans une base de données, y compris les données du groupe de fichiers primaire, de chaque groupe de fichiers en lecture-écriture, ainsi que, éventuellement, de tout fichier spécifié en lecture seule.  
  
## <a name="backup-media-terms-and-definitions"></a>Termes et définitions des supports de sauvegarde  
  
 **[unité de sauvegarde](../../relational-databases/backup-restore/backup-devices-sql-server.md)**  
 Unité de disque ou de bande sur laquelle les sauvegardes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont écrites et à partir de laquelle elles peuvent être restaurées. Les sauvegardes SQL Server peuvent également être écrites dans un service Stockage Blob Microsoft Azure, et le format d’ **URL** est utilisé pour spécifier la destination et le nom du fichier de sauvegarde. Pour plus d’informations, consultez [Sauvegarde et restauration SQL Server avec le service de stockage d’objets blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).  
  
 **[support de sauvegarde](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)**  
 Une ou plusieurs bandes ou un ou plusieurs fichiers disque sur lesquels une ou plusieurs sauvegardes ont été écrites.  
  
 **[jeu de sauvegarde](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)**  
 Contenu de sauvegarde ajouté à un jeu de supports par une opération de sauvegarde réussie.  
  
 **[famille de supports](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)**  
 Sauvegardes créées sur une seule unité qui n'est pas mise en miroir ou sur un ensemble d'unités en miroir dans un jeu de supports  
  
 **[jeu de supports](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)**  
 Ensemble ordonné de supports de sauvegarde (bandes ou fichiers disque) sur lequel une ou plusieurs opérations de sauvegarde ont été écrites en utilisant un type et un nombre fixes d'unités de sauvegarde.  
  
 **[jeu de supports en miroir](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md)**  
 Plusieurs copies (miroirs) d'un jeu de supports.  
  
##  <a name="BackupCompression"></a> Compression de sauvegarde  
 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] et les versions ultérieures prennent en charge la compression des sauvegardes. [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures peuvent restaurer une sauvegarde compressée. Pour plus d’informations, consultez [Compression de sauvegardes &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-compression-sql-server.md).  
  
##  <a name="Restrictions"></a>  Restrictions des opérations de sauvegarde 
 La sauvegarde peut être effectuée si la base de données est en ligne et en cours d'utilisation. Cependant, les restrictions suivantes existent.  
  
### <a name="cannot-back-up-offline-data"></a>Impossible de sauvegarder des données hors connexion  
 Toute sauvegarde qui fait implicitement ou explicitement référence à des données hors connexion échoue. Voici quelques exemples classiques de cette situation :  
  
-   Vous demandez une sauvegarde complète de la base de données, mais un groupe de fichiers de la base de données est hors connexion. Comme tous les groupes de fichiers sont implicitement inclus dans une sauvegarde complète de base de données, cette opération échoue.  
  
     Pour sauvegarder cette base de données, vous pouvez utiliser une sauvegarde de fichiers et spécifier uniquement les groupes de fichiers en ligne.  
  
-   Vous demandez une sauvegarde partielle, mais un groupe de fichiers en lecture-écriture est hors connexion. Du fait que tous les groupes de fichiers en lecture-écriture sont indispensables pour une sauvegarde partielle, cette opération échoue.  
  
-   Vous demandez une sauvegarde de fichiers spécifiques, mais un fichier n'est pas en ligne. L'opération échoue. Pour sauvegarder les fichiers en ligne, vous pouvez supprimer le fichier hors connexion de la liste des fichiers et recommencer l'opération.  
  
 En règle générale, une sauvegarde de journal aboutit même si un ou plusieurs fichiers de données ne sont pas disponibles. Cependant, si un fichier contient des modifications journalisées en bloc et effectuées en mode de récupération utilisant les journaux de transactions, tous les fichiers doivent être en ligne pour que la sauvegarde aboutisse.  
  
### <a name="concurrency-restrictions"></a>Restrictions d’accès concurrentiel   
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recourt à un processus de sauvegarde en ligne pour permettre qu'une base de données soit sauvegardée alors qu'elle est encore utilisée. Lors d'une sauvegarde, la plupart des opérations sont possibles ; par exemple, les instructions INSERT, UPDATE et DELETE sont autorisées. Cependant, si vous tentez une opération de sauvegarde pendant qu'un fichier de base de données est en cours de création ou de suppression, l'opération de sauvegarde attend que la création ou la suppression soit terminée ou que le délai d'attente de la sauvegarde expire.  
  
 Parmi les opérations qui ne peuvent pas être effectuées lors d'une sauvegarde de base de données ou d'une sauvegarde du journal des transactions, citons :  
  
-   Les opérations de gestion des fichiers telles que l'instruction ALTER DATABASE employées avec l'option ADD FILE ou REMOVE FILE.  
  
-   Les opérations de compactage de base de données ou de fichier. Cela comprend également les opérations de compactage automatique.  
  
-   Si vous tentez de créer ou de supprimer un fichier de base de données pendant qu'une opération de sauvegarde est en cours, la création ou la suppression échoue.  
  
 Si une opération de sauvegarde chevauche une opération de compactage ou de gestion des fichiers, un conflit se produit. Quelle que soit l'opération effectuée la première, la seconde opération attend que le verrou défini par la première opération expire. (Le délai d'expiration est contrôlé par un paramètre d'expiration de la session). Si le verrou est libéré au cours du délai d'expiration, la seconde opération se poursuit. Si le verrou expire, la seconde opération échoue.  
  
##  <a name="RelatedTasks"></a> Tâches associées  
 **Unités et supports de sauvegarde**  
  
-   [Définir une unité de sauvegarde logique pour un fichier de disque &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)  
  
-   [Définir une unité de sauvegarde logique pour un lecteur de bande &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
-   [Spécifier un disque ou une bande comme destination de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/specify-a-disk-or-tape-as-a-backup-destination-sql-server.md)  
  
-   [Supprimer une unité de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)  
  
-   [Définir la date d’expiration d’une sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/set-the-expiration-date-on-a-backup-sql-server.md)  
  
-   [Afficher le contenu d’un fichier ou d’une bande de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [Afficher les fichiers de données et les fichiers journaux dans un jeu de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-data-and-log-files-in-a-backup-set-sql-server.md)  
  
-   [Afficher les propriétés et le contenu d’une unité de sauvegarde logique &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   [Restaurer une sauvegarde à partir d’une unité &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-backup-from-a-device-sql-server.md)  
  
-   [Didacticiel : Sauvegarde et restauration SQL Server dans le service de stockage d'objets blob Windows Azure](~/relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md)  
  
 **Créer une sauvegarde**  
  
> [!NOTE]  
>  Pour réaliser des sauvegardes partielles ou de copie uniquement, vous devez utiliser l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] [BACKUP](../../t-sql/statements/backup-transact-sql.md) avec l’option PARTIAL ou COPY_ONLY, respectivement.  
  
-   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Sauvegarder des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)  
  
-   [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
-   [Sauvegarder le journal des transactions lorsque la base de données est endommagée &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md)  
  
-   [Activer ou désactiver des sommes de contrôle de sauvegarde au cours d’opérations de sauvegarde ou de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server.md)  
  
-   [Spécifier si une opération de sauvegarde ou de restauration continue ou s’arrête après la survenue d’une erreur &#40;SQL Server&#41;](../../relational-databases/backup-restore/specify-if-backup-or-restore-continues-or-stops-after-error.md)  
  
-   [Utiliser Resource Governor pour limiter l’utilisation de l’UC par compression de la sauvegarde &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)  
  
-   [Didacticiel : Sauvegarde et restauration SQL Server dans le service Stockage Blob Microsoft Azure](~/relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md)  
  
## <a name="and-more"></a>Et plus encore 
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)  
  
  
