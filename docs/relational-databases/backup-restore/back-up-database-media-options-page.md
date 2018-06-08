---
title: Sauvegarder la base de données (page Options de support) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- swb.backupdatabase.mediaoptions.f1
- sql13.swb.backupdatabase.mediaoptions.f1
ms.assetid: eff36228-710c-4ed5-9af5-95859575dc0f
caps.latest.revision: 7
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d7103048caf9d4cc66a32065a410d41529110939
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="back-up-database-media-options-page"></a>Sauvegarder la base de données (page Options de support)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez la page  **Options de support** de la boîte de dialogue **Sauvegarder la base de données** pour afficher ou modifier les options de sauvegarde de la base de données.  
  
 **Pour créer une sauvegarde à l'aide de SQL Server Management Studio**  
  
-   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
> [!IMPORTANT]  
>  Vous pouvez définir un plan de maintenance de base de données pour créer des sauvegardes de base de données. Pour plus d’informations, consultez [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md) et [Utiliser l’Assistant Plan de maintenance](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md).  
  
> [!NOTE]  
>  Quand vous spécifiez une tâche de sauvegarde à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez générer le script [!INCLUDE[tsql](../../includes/tsql-md.md)][BACKUP](../../t-sql/statements/backup-transact-sql.md) correspondant en cliquant sur le bouton **Script** et en sélectionnant une destination pour le script.  
  
## <a name="options"></a>Options  
  
### <a name="overwrite-media"></a>Remplacer le support  
 Les options du volet **Remplacer le support** contrôlent la façon dont la sauvegarde est écrite sur le support. Si vous avez sélectionné l'URL (Stockage Windows Azure) comme destination de la sauvegarde dans la page Général de la boîte de dialogue Sauvegarder la base de données, les options sous la section Remplacer le support sont désactivées. Pour remplacer une sauvegarde, utilisez l’instruction Transact-SQL **BACKUP TO URL. WITH FORMAT**. Pour plus d’informations, consultez [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).  
  
 Seule l’option **Sauvegarder sur un nouveau support de sauvegarde et effacer tous les jeux de sauvegarde existants** est prise en charge avec les options de chiffrement. Si vous sélectionnez les options sous la section **Sauvegarder sur le support de sauvegarde existant**, les options de chiffrements sur la page **Options de sauvegarde** sont désactivées.  
  
> [!NOTE]  
>  Pour plus d’informations sur les supports de sauvegarde, consultez [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
 **Sauvegarder sur le support de sauvegarde existant**  
 Sauvegarde la base de données sur le support de sauvegarde existant. L'activation de cette case d'option active trois options.  
  
 Choisissez l'une des options suivantes :  
  
 **Ajouter au jeu de sauvegarde existant**  
 Ajoute le jeu de sauvegarde au support de sauvegarde existant, tout en préservant les sauvegardes antérieures.  
  
 **Remplacer tous les jeux de sauvegarde existants**  
 Remplace les éventuelles sauvegardes antérieures présentes sur le support de sauvegarde existant par la sauvegarde active.  
  
 **Vérifier le nom du support de sauvegarde et la date d'expiration du jeu de sauvegarde**  
 Éventuellement, si vous sauvegardez sur un support de sauvegarde existant, exigez la vérification du nom et de la date d'expiration des jeux de sauvegarde pendant l'opération de sauvegarde.  
  
 **Nom du support de sauvegarde**  
 Si l’option **Vérifier le nom du support de sauvegarde et la date d’expiration du jeu de sauvegarde** est sélectionnée, éventuellement, spécifiez le nom du support de sauvegarde à utiliser pour cette opération de sauvegarde.  
  
 **Sauvegarder sur un nouveau support de sauvegarde et effacer tous les jeux de sauvegarde existants**  
 Utilise un nouveau support de sauvegarde et efface tous les jeux de sauvegarde antérieurs s'y trouvant.  
  
 Si vous cliquez sur cette option, les options suivantes sont activées :  
  
 **Nouveau nom du support de sauvegarde**  
 Éventuellement, entrez un nouveau nom pour le support de sauvegarde.  
  
 **Description du nouveau support de sauvegarde**  
 Éventuellement, entrez une description significative pour le support de sauvegarde. Cette description doit être suffisamment explicite pour indiquer avec précision le contenu du support.  
  
### <a name="reliability"></a>Fiabilité  
 Les options du volet **Journal des transactions** contrôlent la gestion des erreurs par l’opération de sauvegarde.  
  
 **Vérifier la sauvegarde en fin d'opération**  
 Vérifie si le jeu de sauvegarde est complet et que tous les volumes sont lisibles.  
  
 **Effectuer une somme de contrôle avant d'écrire sur le support**  
 La somme de contrôle permet de s'assurer de l'intégrité des supports de sauvegarde avant l'écriture. L'activation de cette option revient à spécifier l'option CHECKSUM dans une instruction BACKUP [!INCLUDE[tsql](../../includes/tsql-md.md)]. Elle peut entraîner une augmentation de la charge de travail et une diminution du débit de la sauvegarde. Pour plus d’informations sur les sommes de contrôle de sauvegarde, consultez [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
 **Continuer lors d'erreurs**  
 La sauvegarde se poursuit même si une ou plusieurs erreurs sont rencontrées.  
  
### <a name="transaction-log"></a>Journal des transactions  
 Les options du volet **Journal des transactions** contrôlent le comportement d’une sauvegarde de journal des transactions. Ces options concernent uniquement le mode de récupération complète ou le mode de récupération utilisant les journaux de transactions. Elles sont activées uniquement si **Journal des transactions** a été sélectionné dans le champ **Type de sauvegarde** de la page [Général](../../relational-databases/backup-restore/back-up-database-general-page.md) de la boîte de dialogue **Sauvegarder la base de données**.  
  
> [!NOTE]  
>  Pour plus d’informations sur les sauvegardes des journaux de transactions, consultez [Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md).  
  
 **Tronquer le journal des transactions**  
 Permet de sauvegarder le journal des transactions puis de le tronquer pour libérer de l'espace. La base de données reste en ligne. Il s'agit de l'option par défaut.  
  
 **Sauvegarder la fin du journal et laisser la base de données dans l'état de restauration**  
 Sauvegarder la fin du journal et laisser la base de données dans l'état de restauration. Crée une *sauvegarde de la fin du journal*, incluant les journaux n’ayant pas encore été sauvegardés (journal actif), généralement en guise de préparation à la restauration d’une base de données. La base de données est inaccessible aux utilisateurs tant qu'elle n'a pas été restaurée dans son intégralité.  
  
 La sélection de cette option revient à spécifier WITH NO_TRUNCATE, NORECOVERY dans une instruction [BACKUP](../../t-sql/statements/backup-transact-sql.md) ([!INCLUDE[tsql](../../includes/tsql-md.md)]). Pour plus d’informations, consultez [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
### <a name="tape-drive"></a>Lecteur de bande  
 Les options du volet **Lecteur de bande** contrôlent la gestion des bandes durant l’opération de sauvegarde. Ces options sont activées uniquement si **Bande** a été sélectionné dans le volet **Destination** de la page [Général](../../relational-databases/backup-restore/back-up-database-general-page.md) de la boîte de dialogue **Sauvegarder la base de données**.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation des périphériques à bandes, consultez, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
 **Décharger la bande après la sauvegarde**  
 La bande est déchargée une fois la sauvegarde terminée.  
  
 **Rembobiner la bande avant de décharger**  
 La bande est automatiquement libérée et rembobinée avant d'être déchargée. Cette option est activée uniquement si **Décharger la bande après la sauvegarde** est sélectionné.  
  
## <a name="see-also"></a> Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)   
 [Sauvegarder des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [Sauvegarder le journal des transactions lorsque la base de données est endommagée &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md)  
  
  
