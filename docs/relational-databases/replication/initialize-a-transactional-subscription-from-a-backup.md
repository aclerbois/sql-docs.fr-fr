---
title: Initialiser un abonnement transactionnel à partir d’une sauvegarde | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
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
- manual subscription initialization [SQL Server replication]
- subscriptions [SQL Server replication], initializing
- initializing subscriptions [SQL Server replication], without snapshots
- transactional replication, backup and restore
- backups [SQL Server replication], transactional replication
ms.assetid: d0637fc4-27cc-4046-98ea-dc86b7a3bd75
caps.latest.revision: 36
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 39a7659298ce2efd2daf7c96aa337a019619609f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="initialize-a-transactional-subscription-from-a-backup"></a>Initialiser un abonnement transactionnel à partir d’une sauvegarde
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Bien qu'un abonnement à une publication transactionnelle soit en général initialisé à l'aide d'un instantané, il est possible d'initialiser un abonnement à partir d'une sauvegarde en utilisant les procédures stockées de réplication. Pour plus d’informations, consultez [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
### <a name="to-initialize-a-transactional-subscriber-from-a-backup"></a>Pour initialiser un Abonné transactionnel à partir d'une sauvegarde  
  
1.  Pour une publication existante, vérifiez que la publication prend en charge la capacité d’initialiser à partir d’une sauvegarde en exécutant [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) sur la base de données de publication du serveur de publication. Notez la valeur de **allow_initialize_from_backup** dans le jeu de résultats.  
  
    -   Si la valeur est **1**, la publication prend en charge cette fonctionnalité.  
  
    -   Si la valeur est **0**, exécutez [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) au niveau du serveur de publication sur la base de données de publication. Spécifiez la valeur **allow_initialize_from_backup** pour **@property** et la valeur **true** pour **@value**.  
  
2.  Pour une nouvelle publication, exécutez [sp_addpublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) au niveau du serveur de publication sur la base de données de publication. Spécifiez la valeur **true** pour **allow_initialize_from_backup**. Pour plus d’informations, consultez [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
    > [!WARNING]  
    >  Afin d'éviter les données d'abonnés manquantes, lorsque vous utilisez **sp_addpublication** avec `@allow_initialize_from_backup = N'true'`, utilisez toujours `@immediate_sync = N'true'`.  
  
3.  Créez une sauvegarde de la base de données de publication à l’aide de l’instruction [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
4.  Restaurez la sauvegarde sur l’Abonné à l’aide de l’instruction [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
5.  Exécutez la procédure stockée [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) sur la base de données de publication du serveur de publication. Spécifiez les paramètres suivants :  
  
    -   **@sync_type** – valeur de **initialize with backup**.  
  
    -   **@backupdevicetype** – type d'unité de sauvegarde : **logical** (valeur par défaut), **disk**ou **tape**.  
  
    -   **@backupdevicename** – unité de sauvegarde logique ou physique à utiliser pour la restauration.  
  
         Pour une unité logique, spécifiez le nom de l'unité de sauvegarde qui a été spécifié quand **sp_addumpdevice** a été utilisé pour créer l'unité.  
  
         Pour une unité physique, spécifiez un chemin d'accès complet et un nom de fichier, tels que `DISK = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\BACKUP\Mybackup.dat'` ou `TAPE = '\\.\TAPE0'`;  
  
    -   (Facultatif) **@password** – mot de passe qui a été fourni lorsque le jeu de sauvegarde a été créé ;  
  
    -   (Facultatif) **@mediapassword** – mot de passe qui a été fourni lorsque le support de sauvegarde a été formaté ;  
  
    -   (Facultatif) **@fileidhint** – identificateur pour le jeu de sauvegarde à restaurer. Par exemple, la valeur **1** indique le premier jeu de sauvegarde sur le support de sauvegarde et la valeur **2** le second jeu de sauvegarde ;  
  
    -   (Facultatif pour les périphériques à bandes) **@unload** – spécifiez la valeur **1** (valeur par défaut) si la bande doit être déchargée du lecteur une fois la restauration terminée et **0** si elle ne doit pas être déchargée.  
  
6.  (Facultatif) Pour un abonnement par extraction, exécutez [sp_addpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md) et [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md) au niveau de l’abonné sur la base de données d’abonnement. Pour plus d’informations, consultez [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
7.  (Facultatif) Démarrez l'Agent de distribution. Pour plus d'informations, consultez [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) ou [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Copier des bases de données avec la sauvegarde et la restauration](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)   
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
  
  
