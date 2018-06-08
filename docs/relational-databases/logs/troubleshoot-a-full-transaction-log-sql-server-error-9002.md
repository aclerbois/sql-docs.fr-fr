---
title: Résoudre les problèmes liés à un journal des transactions saturé (erreur SQL Server 9002) | Microsoft Docs
ms.custom: ''
ms.date: 08/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], full
- troubleshooting [SQL Server], full transaction log
- 9002 (Database Engine error)
- transaction logs [SQL Server], truncation
- backing up transaction logs [SQL Server], full logs
- transaction logs [SQL Server], full log
- full transaction logs [SQL Server]
ms.assetid: 0f23aa84-475d-40df-bed3-c923f8c1b520
caps.latest.revision: 59
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 9f3ecb5eb7b1b074e473161f274ab96f4bbaba1b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-a-full-transaction-log-sql-server-error-9002"></a>Résoudre les problèmes liés à un journal des transactions saturé (erreur SQL Server 9002)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit les réactions possibles et émet quelques suggestions qui vous aideront à éviter cette situation dans le futur. 
  
  Quand le journal des transactions est saturé, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] émet une **erreur 9002**. Le journal peut être renseigné quand la base de données est en ligne ou en cours de récupération. Si le journal se remplit tandis que la base de données est en ligne, cette dernière reste en ligne et peut uniquement être lue mais pas mise à jour. Si le journal se remplit en cours de récupération, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] marque la base de données comme RESOURCE PENDING. Dans les deux cas, une intervention de l'utilisateur est nécessaire pour libérer de l'espace disque.  
  
## <a name="responding-to-a-full-transaction-log"></a>Réagir à un journal des transactions complet  
 La réponse adéquate à un journal des transactions saturé dépend en partie de la ou des conditions qui ont motivé le remplissage du journal. 
 
 Pour découvrir les raisons qui empêchent de tronquer le journal dans une situation donnée, utilisez les colonnes **log_reuse_wait** et **log_reuse_wait_desc** de l’affichage catalogue **sys.database**. Pour plus d’informations, consultez [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Pour obtenir une description des facteurs susceptibles de retarder la troncation du journal, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
> **IMPORTANT**  
>  Si la base de données était en mode de récupération quand l’erreur 9002 s’est produite, récupérez la base de données à l’aide de l’instruction [ALTER DATABASE *nom_base_de_données* SET ONLINE après avoir résolu le problème.](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
 D'autres solutions possibles en cas de saturation du journal des transactions sont les suivantes :  
  
-   Sauvegarde du journal.  
  
-   Libération de l'espace disque pour que le journal puisse croître automatiquement.  
  
-   Déplacement du fichier journal vers une unité dotée d'un espace disque suffisant.  
  
-   Augmentation de la taille du fichier journal.  
  
-   Ajout d'un fichier journal sur un autre disque.  
  
-   Achèvement ou suppression d'une transaction longue.  
  
 Ces solutions sont abordées dans les sections qui suivent. Optez pour une solution adaptée à votre situation.  
  
## <a name="back-up-the-log"></a>Sauvegarder le journal  
 Si vous travaillez en mode de restauration complète ou en mode de récupération utilisant les journaux de transactions et si vous n'avez pas sauvegardé récemment le journal des transactions, la création d'une sauvegarde est ce qui empêche la troncation du journal. Si le journal n’a jamais été sauvegardé, vous **devez créer deux sauvegardes du journal** pour autoriser le [!INCLUDE[ssDE](../../includes/ssde-md.md)] à le tronquer à l’endroit exact de la dernière sauvegarde. Le fait de tronquer le journal permet de libérer de l'espace pour les nouveaux enregistrements de ce dernier. Pour empêcher le journal de se remplir à nouveau, effectuez les sauvegardes régulièrement.  
  
 **Pour créer une sauvegarde du journal des transactions**  
  
> **IMPORTANT**  
>  Si la base de données est endommagée, consultez [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
-   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Backup.SqlBackup%2A> (SMO)  
  
### <a name="freeing-disk-space"></a>Libération d’espace disque  
 Vous pouvez libérer de l'espace sur le disque où est stocké le fichier journal des transactions de la base de données en supprimant ou en déplaçant d'autres fichiers. Ceci permet au système de récupération d'augmenter automatiquement la taille du fichier journal.  
  
### <a name="move-the-log-file-to-a-different-disk"></a>Déplacer le fichier journal vers un autre disque  
 Si vous ne pouvez pas libérer suffisamment d'espace disque sur le lecteur où le fichier journal se trouve actuellement, essayez de déplacer ce fichier sur une autre unité disposant d'espace suffisant.  
  
> **IMPORTANT** Les fichiers journaux ne doivent jamais être placés sur des systèmes de fichiers compressés.  
  
 **Déplacer un fichier journal**  
  
-   [Déplacer des fichiers de bases de données](../../relational-databases/databases/move-database-files.md)  
  
### <a name="increase-log-file-size"></a>Augmenter la taille d’un fichier journal  
 Si le disque du journal dispose d'espace libre, vous pouvez augmenter la taille du fichier journal. La taille maximale pour les fichiers journaux est de deux téraoctets (To) par fichier journal.  
  
 **Augmenter la taille du fichier**  
  
 Si la fonctionnalité de croissance automatique est désactivée, que la base de données est en ligne et que l'espace disque disponible est suffisant, effectuez l'une des opérations suivantes :  
  
-   Augmentez manuellement la taille du fichier pour générer un seul incrément de croissance.  
  
-   Activez la croissance automatique à l'aide de l'instruction ALTER DATABASE pour définir un incrément de croissance différent de zéro pour l'option FILEGROWTH.  
  
> **REMARQUE** Dans les deux cas, si la limite de taille actuelle est atteinte, augmentez la valeur MAXSIZE.  
  
### <a name="add-a-log-file-on-a-different-disk"></a>Ajout d’un fichier journal sur un autre disque  
 Ajoutez un nouveau fichier journal à la base de données d'un autre disque doté d'un espace suffisant à l'aide de l'instruction ALTER DATABASE <nom_base_de_données> ADD LOG FILE.  
  
 **Ajouter un fichier journal**  
  
-   [Ajouter des fichiers de données ou journaux à une base de données](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)  
## <a name="complete-or-kill-a-long-running-transaction"></a>Achever ou supprimer une transaction longue
### <a name="discovering-long-running-transactions"></a>Découverte des transactions de longue durée
Une transaction de très longue durée entraîne la saturation du journal des transactions. Pour rechercher des transactions de longue durée, appliquez une des procédures suivantes :
 - **[sys.dm_tran_database_transactions](../system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md).**
Cet affichage de gestion dynamique retourne des informations sur les transactions au niveau de la base de données. Pour une transaction de longue durée, les colonnes particulièrement intéressantes incluent l’heure du premier enregistrement de journal [(database_transaction_begin_time)](../system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md), l’état actuel de la transaction [(database_transaction_state)](../system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md)et le [numéro séquentiel dans le journal](../backup-restore/recover-to-a-log-sequence-number-sql-server.md) de l’enregistrement initial dans le journal des transactions [(database_transaction_begin_lsn)](../system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md).

 - **[DBCC OPENTRAN](../../t-sql/database-console-commands/dbcc-opentran-transact-sql.md).**
Cette instruction vous permet d'identifier l'ID du propriétaire de la transaction et éventuellement de retrouver la source de la transaction pour y mettre fin dans les règles de l'art (la valider au lieu de la restaurer).

### <a name="kill-a-transaction"></a>Supprimer une transaction
Parfois, il vous suffit de mettre un terme au processus ; vous pouvez avoir à utiliser l’instruction [KILL](../../t-sql/language-elements/kill-transact-sql.md) . Utilisez cette instruction avec précaution, particulièrement lorsque des processus critiques que vous ne voulez pas supprimer sont en cours d’exécution. Pour plus d’informations, voir [KILL (Transact-SQL)](../../t-sql/language-elements/kill-transact-sql.md).

## <a name="see-also"></a>Voir aussi  
[Article de support de la base de connaissances - Le journal des transactions augmente de manière inattendue ou est saturé sur un ordinateur SQL Server](https://support.microsoft.com/en-us/kb/317375) [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Gérer la taille du fichier journal des transactions](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md)   
 [Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)   
 [sp_add_log_file_recover_suspect_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-log-file-recover-suspect-db-transact-sql.md)  
  
  
