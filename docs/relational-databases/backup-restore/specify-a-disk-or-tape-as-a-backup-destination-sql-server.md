---
title: Spécifier un disque ou une bande comme destination de sauvegarde (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- backup devices [SQL Server], tapes
- backing up databases [SQL Server], tapes
- database backups [SQL Server], tapes
- backup devices [SQL Server], disks
- disk backup devices [SQL Server]
- database backups [SQL Server], disks
- backing up databases [SQL Server], disks
- backups [SQL Server], creating
- tape backup devices, backing up
ms.assetid: e391f452-ed8c-4b40-b846-ac3881271b94
caps.latest.revision: 30
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 69b01a20f429c1c9612a1a64c0ddc2a57bca3243
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-a-disk-or-tape-as-a-backup-destination-sql-server"></a>Spécifier un disque ou une bande comme destination de sauvegarde (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment spécifier un disque ou une bande comme destination de sauvegarde dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!NOTE]  
>  La prise en charge des unités de sauvegarde sur bande sera supprimée dans une prochaine version de SQL Server. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour spécifier un disque ou une bande comme destination de sauvegarde, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Les autorisations BACKUP DATABASE et BACKUP LOG reviennent par défaut aux membres du rôle serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_backupoperator** .  
  
 Des problèmes de propriété et d'autorisations sur le fichier physique de l'unité de sauvegarde sont susceptibles de perturber une opération de sauvegarde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de lire et d'écrire sur l'unité ; le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute doit avoir des autorisations d'écriture. Toutefois, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), qui ajoute une entrée pour une unité de sauvegarde dans les tables système, ne vérifie pas les autorisations d’accès au fichier. De tels problèmes pour le fichier physique de l'unité de sauvegarde peuvent n'apparaître que lorsque la ressource physique est sollicitée au moment de la sauvegarde ou de la restauration.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-specify-a-disk-or-tape-as-a-backup-destination"></a>Pour spécifier un disque ou une bande comme destination de sauvegarde  
  
1.  Après la connexion à l'instance appropriée du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], dans l'Explorateur d'objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données**puis, selon la base de données, sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit sur la base de données, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**. La boîte de dialogue **Sauvegarder la base de données** s'affiche.  
  
4.  Dans la section **Destination** de la page **Général** , cliquez sur **Disque** ou sur **Bande**. Pour sélectionner les chemins d'accès à 64 lecteurs de bande ou de disque au maximum contenant un seul support de sauvegarde, cliquez sur **Ajouter**.  
  
     Pour supprimer une destination de sauvegarde, sélectionnez-la, puis cliquez sur **Supprimer**. Pour afficher le contenu d'une destination de sauvegarde, sélectionnez-la, puis cliquez sur **Sommaire**.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-specify-a-disk-or-tape-as-a-backup-destination"></a>Pour spécifier un disque ou une bande comme destination de sauvegarde  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Dans l'instruction [BACKUP](../../t-sql/statements/backup-transact-sql.md) , spécifiez le fichier ou l'unité et son nom physique. Cet exemple sauvegarde la base de données `AdventureWorks2012` dans le fichier disque `Z:\SQLServerBackups\AdventureWorks2012.Bak`.  
  
```  
USE AdventureWorks2012;  
GO  
BACKUP DATABASE AdventureWorks2012  
TO DISK = 'Z:\SQLServerBackups\AdventureWorks2012.Bak'  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)   
 [Sauvegarder des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [Définir une unité de sauvegarde logique pour un fichier de disque &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)   
 [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)   
 [Définir une unité de sauvegarde logique pour un lecteur de bande &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
  
