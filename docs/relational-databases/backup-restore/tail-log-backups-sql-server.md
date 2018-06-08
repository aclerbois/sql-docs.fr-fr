---
title: Sauvegardes de la fin du journal (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- backing up [SQL Server], tail of log
- transaction log backups [SQL Server], tail-log backups
- NO_TRUNCATE clause
- backups [SQL Server], log backups
- tail-log backups
- backups [SQL Server], tail-log backups
ms.assetid: 313ddaf6-ec54-4a81-a104-7ffa9533ca58
caps.latest.revision: 55
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 70b11ff22c345737e95e32695cbe187d846cac00
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tail-log-backups-sql-server"></a>Sauvegardes de la fin du journal (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique s'applique uniquement à la sauvegarde et la restauration des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] employant le mode de récupération complète ou le mode de récupération utilisant les journaux de transactions.  
  
 Une *sauvegarde de la fin du journal* capture tous les enregistrements de journal qui n’ont pas encore été sauvegardés (la *fin du journal*) pour empêcher toute perte de travail et préserver la continuité de la séquence de journaux de transactions consécutifs. Avant de pouvoir récupérer une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aux date et heure les plus récentes, vous devez sauvegarder la fin de son journal des transactions. La sauvegarde de la fin du journal est la dernière sauvegarde intéressante dans le plan de récupération de la base de données.  
  
> **REMARQUE :** les scénarios de restauration ne nécessite pas tous une sauvegarde de la fin du journal. Vous n'avez pas besoin d'une sauvegarde de la fin du journal si le point de récupération est contenu dans une sauvegarde de journal antérieure. En outre, une sauvegarde de la fin du journal est inutile si vous déplacez ou remplacez (par écrasement) la base de données et ne souhaitez pas la restaurer à un point donné après la sauvegarde la plus récente.  
  
   ##  <a name="TailLogScenarios"></a> Scénarios qui nécessitent une sauvegarde de la fin du journal  
 Nous vous recommandons d'effectuer une sauvegarde de la fin du journal dans les scénarios suivants :  
  
-   Si la base de données est en ligne et que vous envisagez d'effectuer une opération de restauration de la base de données, commencez par sauvegarder la fin du journal. Pour éviter une erreur de base de données en ligne, vous devez utiliser... l’option WITH NORECOVERY de l’instruction [BACKUP](../../t-sql/statements/backup-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
-   Si une base de données est hors connexion, ne démarre pas et que vous devez restaurer la base de données, sauvegardez d'abord la fin du journal. Comme il est exclu d'effectuer une transaction à ce stade, l'utilisation de WITH NORECOVERY est facultative.  
  
-   Si une base de données est endommagée, essayez d'effectuer une sauvegarde de la fin du journal à l'aide de l'option WITH CONTINUE_AFTER_ERROR de l'instruction BACKUP.  
  
     Sur une base de données endommagée, la sauvegarde de la fin du journal n'aboutit que si les fichiers journaux ne sont pas endommagés, la base de données est en mesure de prendre en charge cette sauvegarde de la fin du journal, et la base de données ne contient pas de modifications journalisées en bloc. Si une sauvegarde de la fin du journal ne peut pas être créée, toutes les transactions validées après la dernière sauvegarde de fichier journal sont perdues.  
  
 Le tableau suivant résume les options BACKUP NORECOVERY et CONTINUE_AFTER_ERROR.  
  
|Option BACKUP LOG|Commentaires|  
|-----------------------|--------------|  
|NORECOVERY|Utilisez NORECOVERY chaque fois que vous envisagez de poursuivre une opération de restauration sur la base de données. NORECOVERY fait passer la base de données en état de restauration. Ceci permet d'éviter des modifications dans la base de données après la sauvegarde de la fin du journal. Le journal sera tronqué sauf si l’option NO_TRUNCATE ou COPY_ONLY est aussi spécifiée.<br /><br /> **Important :**  Évitez d’utiliser NO_TRUNCATE, sauf si la base de données est endommagée.|  
|CONTINUE_AFTER_ERROR|Utilisez CONTINUE_AFTER_ERROR uniquement si vous sauvegardez la fin d'une base de données endommagée.<br /><br /> Si vous sauvegardez la fin du journal sur une base de données endommagée, certaines métadonnées capturées normalement dans des sauvegardes de journaux sont parfois indisponibles. Pour plus d’informations, consultez [Sauvegardes de la fin du journal avec des métadonnées de sauvegarde incomplètes](#IncompleteMetadata), dans cette rubrique.|  
  
##  <a name="IncompleteMetadata"></a> Sauvegardes de la fin du journal avec des métadonnées de sauvegarde incomplètes  
 Les sauvegardes de la fin du journal capturent la fin du journal même si la base de données est hors connexion ou endommagée, ou s'il y manque des fichiers de données. Cela peut aboutir à des métadonnées incomplètes à partir des commandes d'informations de restauration et de **msdb**. Mais seules les métadonnées sont incomplètes ; le journal capturé est complet et exploitable.  
  
 Si une sauvegarde de la fin du journal contient des métadonnées incomplètes, dans la table [backupset](../../relational-databases/system-tables/backupset-transact-sql.md) , **has_incomplete_metadata** a la valeur **1**. De plus, dans le résultat de [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), **HasIncompleteMetadata** a la valeur **1**.  
  
 Si les métadonnées d’une sauvegarde de la fin du journal sont incomplètes, la plupart des informations relatives aux groupes de fichiers sont absentes de la table [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md) au moment de la sauvegarde de la fin du journal. La plupart des colonnes de la table **backupfilegroup** ont la valeur NULL ; seules les colonnes suivantes ont une signification :  
  
-   **backup_set_id**  
-   **filegroup_id**  
-   **type**  
-   **type_desc**  
-   **is_readonly**  
  
##  <a name="RelatedTasks"></a> Tâches associées  
 Pour créer une sauvegarde de la fin du journal, consultez [Sauvegarder le journal des transactions lorsque la base de données est endommagée &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md).  
  
 Pour restaurer une sauvegarde du journal des transactions, consultez [Restaurer une sauvegarde du journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md).  
    
## <a name="see-also"></a> Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Sauvegardes de type copie seule &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)   
 [Appliquer les sauvegardes du journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)    
 [Guide d’architecture et gestion du journal des transactions SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)
  
