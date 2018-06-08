---
title: Afficher les propriétés et le contenu d’une unité de sauvegarde logique (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- displaying backup content
- viewing backup content
- database backups [SQL Server], viewing content
- backing up databases [SQL Server], viewing content
- backing up databases [SQL Server], properties
- displaying backup properties
- backup devices [SQL Server], viewing information
- viewing backup properties
- database backups [SQL Server], properties
ms.assetid: 3a309074-e816-454d-b6c3-fcfdde0cbf74
caps.latest.revision: 22
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 191eefe8086c7f093a6f21b6283d912d8061ee67
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-the-properties-and-contents-of-a-logical-backup-device-sql-server"></a>Afficher les propriétés et le contenu d'une unité de sauvegarde logique (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment afficher les propriétés et le contenu d'une unité de sauvegarde logique dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour afficher les propriétés et le contenu d'une unité de sauvegarde logique, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Security"></a> Sécurité  
 Pour plus d’informations sur la sécurité, consultez [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md).  
  
####  <a name="Permissions"></a> Permissions  
 Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures, l'obtention d'informations relatives à un jeu de sauvegarde ou une unité de sauvegarde requiert l'autorisation CREATE DATABASE. Pour plus d’informations, consultez [GRANT – octroi d’autorisations de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-the-properties-and-contents-of-a-logical-backup-device"></a>Pour afficher les propriétés et le contenu d'une unité de sauvegarde logique  
  
1.  Après la connexion à l'instance appropriée du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], dans l'Explorateur d'objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Objets serveur**, puis **Unités de sauvegarde**.  
  
3.  Cliquez sur l’unité, puis cliquez avec le bouton droit sur **Propriétés**; la boîte de dialogue **Unité de sauvegarde** s’affiche.  
  
4.  La page **Général** affiche le nom de l'unité et la destination, soit un périphérique à bandes, soit un chemin de fichier.  
  
5.  Dans le volet **Sélectionner une page** , cliquez sur **Contenu du support**.  
  
6.  Le volet de droite affiche les volets de propriétés suivants :  
  
    -   **Support**  
  
         Informations sur la séquence du support (le numéro de la séquence du support et de la famille, et l'identificateur du miroir, s'il existe) et la date et l'heure de la création du support.  
  
    -   **Support de sauvegarde**  
  
         Informations sur le support de sauvegarde : nom et description du support, s'ils existent, et nombre de familles dans ce support.  
  
7.  La grille **Jeux de sauvegarde** affiche des informations sur le contenu du support de sauvegarde.  
  
> [!NOTE]  
>  Pour plus d’informations, consultez [Page Contenu du support](../../relational-databases/backup-restore/backup-device-media-contents-page.md).  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-view-the-properties-and-contents-of-a-logical-backup-device"></a>Pour afficher les propriétés et le contenu d'une unité de sauvegarde logique  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Utilisez l'instruction [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) . Cet exemple retourne des informations sur l'unité de sauvegarde logique `AdvWrks2008R2Backup` .  
  
```sql  
USE AdventureWorks2012 ;  
RESTORE LABELONLY  
   FROM AdvWrks2008R2Backup ;  
GO  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)  
  
  
